# TDB Allotment Dashboard - AI Coding Guide

## Project Overview
A **Streamlit-based dental allotment dashboard** that displays real-time scheduling for dental procedures. The app reads from an Excel file (`Putt Allotment.xlsx`) and provides live updates with status tracking, doctor assignments, and operational management.

## Architecture & Key Components

### Data Flow
1. **Excel Source** → Read via `pd.read_excel()` with sheet `"Sheet1"`
2. **Data Processing** → Time format conversion, status filtering, ongoing/upcoming calculations
3. **UI Rendering** → Streamlit `st.data_editor()` for live editing
4. **Auto-save** → Changes immediately written back to Excel via `pd.ExcelWriter()`

### Core Data Model
- **Patient records**: Name, In/Out times, procedure, assigned doctor/staff
- **Status values**: "WAITING", "ARRIVED", "ON GOING", "CANCELLED"
- **Time storage**: Decimal format in Excel (e.g., `9.30` = 09:30), converted to `HH:MM` strings for UI
- **Assignments**: Doctor (DR.), operation theater (OP 1-4), support staff (FIRST/SECOND/Third)
- **Tasks**: SUCTION/CLEANING checkboxes (stored as "✓" or empty)

## Critical Implementation Patterns

### Time Handling (Complex & Project-Specific)
Excel stores times as decimals (e.g., `9.30` for 09:30). The app handles multiple formats:
- `dec_to_time()` - Converts Excel decimal/string → "HH:MM" display format
- `safe_str_to_time_obj()` - Converts "HH:MM" → Python `time` object for pickers
- Time comparisons: Convert to minutes since midnight (`time_to_minutes()`)
- **Always handle overnight cases**: If `Out_min < In_min`, add 1440 (24h in minutes)

### Data Synchronization
- **Change detection**: Use MD5 hash of raw DataFrame to detect Excel file updates
- **Session state tracking**: `st.session_state.prev_hash`, `prev_ongoing`, `prev_upcoming` prevent duplicate notifications
- **Auto-refresh**: 60-second interval via `st_autorefresh()` + manual "🔄 Refresh" button
- **Instant save pattern**: After `st.data_editor()` edits, write directly to Excel before `st.rerun()`

### Status-Based UI Logic
- **Ongoing filtering**: `(In_min <= current_min) & (current_min <= Out_min)` + exclude CANCELLED/DONE/SHIFTED
- **Upcoming filtering**: Next 15 minutes window (`In_min > current_min & In_min <= current_min + 15`)
- **Toast notifications**: Triggered only on NEW transitions (compare with previous state sets)
- **Row styling**: CSS borders by status (green=ON GOING, blue=DONE, red=CANCELLED, amber=ARRIVED)

### Multi-Select Data Editing
Three separate `st.data_editor()` instances with different key/scope:
1. **Upcoming section** - Next 15 minutes (immutable except picker times)
2. **Ongoing section** - Currently happening procedures  
3. **Full schedule** - All patients with all fields editable
4. **Per-OP tabs** - Filtered by operation theater

Edit detection: Compare `edited_all is not None and not edited_all.equals(display_all)`

### Checkbox Storage
- **Display as**: Python `bool` in editor
- **Stored as**: "✓" (True) or "" (False) in Excel
- **Conversion**: `str_to_checkbox()` handles both formats + NaN

## Developer Workflow

### Running the App
```bash
# Install dependencies
pip install -r requirements.txt  # or: pandas openpyxl streamlit streamlit-autorefresh

# Run the dashboard
streamlit run app.py

# Auto-opens at http://localhost:8501
```

### Excel File Requirements
- **Location**: Same directory as `app.py`
- **Name**: `Putt Allotment.xlsx` (checked at startup)
- **Sheet**: "Sheet1"
- **Required columns**: Patient Name, In Time, Out Time, Procedure, DR., OP, FIRST, SECOND, Third, CASE PAPER, SUCTION, CLEANING, STATUS

### Common Tasks

**Adding a new field**: 
1. Add column to Excel
2. Include in `display_*` DataFrames (e.g., `display_all`)
3. Configure in `st.data_editor()` column_config if it needs special handling
4. Update Excel write-back logic in the auto-save block

**Changing time format**:
- Modify `dec_to_time()` for input parsing
- Update `column_config` TimeColumn format in `st.data_editor()`
- Excel still stores as decimals via `time_obj_to_str()` conversion

**Adding new status values**:
- Update `"STATUS"` SelectboxColumn options in all `st.data_editor()` calls
- Update CSS color logic in `get_status_background()`
- Consider notification logic in change detection section

## Color & Theme System
All colors centralized in `COLORS` dict (lines 26-35):
- Primary: white bg, dark text
- Accents: Brown (#99582f), Beige (#c9bbb0)
- Status indicators: Green (ongoing), Blue (done), Red (cancelled), Amber (arrived)
- All CSS uses variable references for consistency

## Dependencies & External Integrations
- **Streamlit**: UI framework, state management
- **Pandas/openpyxl**: Excel read/write
- **streamlit-autorefresh**: 60-sec auto-refresh
- **datetime/timezone**: IST time (UTC+5:30)
- **hashlib**: MD5 hashing for change detection

## Common Pitfalls to Avoid
- ❌ Forgetting to handle `pd.NA` in time comparisons → causes filtering failures
- ❌ Not clearing empty patient rows before saving → leaves garbage in Excel
- ❌ Comparing DataFrames with `.equals()` before converting types → always str() non-text columns first
- ❌ Missing `drop=True` in `.reset_index()` → introduces extra index column
- ❌ Not updating `st.session_state.prev_*` sets → duplicate notifications on refresh
