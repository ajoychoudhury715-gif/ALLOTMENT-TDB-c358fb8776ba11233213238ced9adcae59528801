# 🎨 UI Customization Quick Reference

## What Was Changed

### ✅ Fixed Issues
1. **Status bar overflow** - Cards now use `overflow: hidden` with proper border-radius
2. **Button text wrapping** - All buttons have `white-space: nowrap` + fixed padding
3. **Inconsistent spacing** - Unified gap system across all containers
4. **Visual hierarchy** - Enhanced shadows, colors, and sizing

### ✅ Added Features
1. **Design tokens** - CSS variables for all colors, shadows, and spacing
2. **Status badges** - Chip-style status indicators with color coding
3. **Premium metric cards** - Hover effects, better typography
4. **Enhanced alert cards** - Gradient backgrounds, better icons
5. **Smooth transitions** - All interactive elements have subtle animations

---

## Where to Find Code

### Main Styling
📂 File: `app.py` (lines 910-1150 in `render_compact_dashboard()`)

### Documentation
📄 File: `UI_DESIGN_SYSTEM.md` - Complete design system guide  
📄 File: `CSS_COMPONENTS.md` - Copy-paste ready components

---

## Quick Customization Guide

### Change Primary Color

In the CSS `<style>` block:
```css
:root {
    --primary-dark: #1f3a5f;      /* Change this */
    --primary-light: #2e86c1;     /* Change this */
}
```

**For example, to make it green:**
```css
:root {
    --primary-dark: #065f46;      /* Dark green */
    --primary-light: #10b981;     /* Light green */
}
```

### Change Status Colors

```css
:root {
    --status-ongoing: #10b981;    /* Green → change here */
    --status-waiting: #f59e0b;    /* Orange → change here */
    --status-arrived: #3b82f6;    /* Blue → change here */
    --status-done: #8b5cf6;       /* Purple → change here */
    --status-cancelled: #ef4444;  /* Red → change here */
}
```

### Adjust Card Shadows

```css
:root {
    --shadow-sm: 0 2px 8px rgba(15, 23, 42, 0.08);      /* Light shadow */
    --shadow-md: 0 8px 20px rgba(15, 23, 42, 0.12);     /* Medium shadow */
    --shadow-lg: 0 16px 40px rgba(15, 23, 42, 0.16);    /* Heavy shadow */
}
```

**To make shadows stronger:**
```css
--shadow-md: 0 12px 32px rgba(15, 23, 42, 0.20);
```

### Change Border Radius

```css
:root {
    --radius-sm: 8px;     /* Change small radius */
    --radius-md: 12px;    /* Change medium radius */
    --radius-lg: 16px;    /* Change large radius */
    --radius-xl: 20px;    /* Change extra-large radius */
}
```

**For more rounded (pill-shaped):**
```css
--radius-md: 16px;
--radius-lg: 24px;
--radius-xl: 32px;
```

---

## Component-Specific Tweaks

### Dashboard Header

**Location:** Line ~970 in `app.py`

```python
st.markdown("<div class='dash-title'>THE DENTAL BOND</div>", unsafe_allow_html=True)
st.markdown("<div class='dash-subtitle'>Real-time Scheduling Management System</div>", unsafe_allow_html=True)
```

**Customization:**
```python
st.markdown("<div class='dash-title'>YOUR CLINIC NAME</div>", unsafe_allow_html=True)
```

CSS for title:
```css
.dash-title {
    text-align: center;
    color: var(--primary-dark);
    font-size: 32px;          /* Change size */
    font-weight: 900;         /* Change weight (700-900) */
    letter-spacing: -0.5px;   /* Change spacing */
}
```

### Metric Cards

**Location:** Line ~1030 in `app.py`

To change the number of columns:
```python
# Current: 6 columns (2 rows of 3)
# Change metrics_html grid to:
grid-template-columns: repeat(4, minmax(0, 1fr));  # 4 columns instead
```

To change card height:
```css
.metric-card {
    min-height: 90px;  /* Change this */
}
```

### Status Badges

**Location:** CSS section around line 1130

To make badges bigger:
```css
.status-badge {
    padding: 6px 12px;   /* Increase padding */
    font-size: 13px;     /* Increase font size */
}
```

To change colors:
```css
.status-ongoing {
    background: rgba(34, 197, 94, 0.15);  /* Change green shade */
    color: #22c55e;                        /* Change text color */
}
```

### Buttons

**Location:** Control buttons at line ~1045

To change button height:
```css
.controls-row .stButton > button {
    height: 44px !important;  /* Change to 48px, 52px, etc */
}
```

To change button colors:
```css
button[kind="primary"] {
    background: linear-gradient(135deg, #3b82f6, #1e40af) !important;  /* Blue instead of navy */
}
```

### Search Bar

**Location:** Line ~1060

To change search input styling:
```css
.search-row input {
    background: rgba(255, 255, 255, 0.8) !important;  /* More opaque */
    border-radius: 8px !important;                     /* Less rounded */
    border: 2px solid #cbd5e1 !important;              /* Thicker border */
}
```

### Data Table

**Location:** CSS around line 1100

To change table header background:
```css
[data-testid="stDataFrameContainer"] thead th {
    background: linear-gradient(135deg, #2e86c1 0%, #1f3a5f 100%) !important;  /* Blue gradient */
}
```

To change row hover color:
```css
[data-testid="stDataFrameContainer"] tbody tr:hover {
    background: rgba(46, 134, 193, 0.1) !important;  /* Change hover color */
}
```

---

## Common Customization Scenarios

### Scenario 1: Darker, More Professional Look

```python
# Change in CSS :root section
--primary-dark: #0f172a;        # Darker navy
--primary-light: #1e3a8a;       # Darker blue
--status-ongoing: #047857;      # Darker green
--shadow-lg: 0 20px 48px rgba(15, 23, 42, 0.2);  # Heavier shadows
```

### Scenario 2: Lighter, Softer Look

```python
--primary-dark: #334155;        # Light gray-blue
--primary-light: #64748b;       # Medium gray-blue
--status-ongoing: #34d399;      # Brighter green
--shadow-md: 0 4px 12px rgba(15, 23, 42, 0.06);  # Lighter shadows
```

### Scenario 3: Vibrant, Modern

```python
--primary-dark: #7c3aed;        # Purple
--primary-light: #a78bfa;       # Light purple
--status-ongoing: #ec4899;      # Pink
--status-waiting: #06b6d4;      # Cyan
--status-arrived: #8b5cf6;      # Violet
```

### Scenario 4: Minimalist, Flat

```python
# Remove shadows entirely
--shadow-sm: none;
--shadow-md: none;
--shadow-lg: none;

# Less rounded
--radius-md: 6px;
--radius-lg: 8px;
--radius-xl: 12px;
```

---

## Testing Your Changes

### Step 1: Make CSS Change
Edit the `<style>` block in the `render_compact_dashboard()` function

### Step 2: Save File
Save `app.py`

### Step 3: Reload Streamlit
The app should auto-reload in the browser (or press R)

### Step 4: Verify
Check that:
- Colors are applied correctly
- No layout shifts
- Buttons still have proper spacing
- Cards still have proper shadows

---

## Debugging

### Issue: Colors not changing
**Solution:** 
1. Make sure you're editing inside the `<style>` tag
2. Clear browser cache (Ctrl+Shift+Delete)
3. Refresh the page

### Issue: Text looks cut off
**Solution:**
Check that `white-space: nowrap` is present on buttons/controls

### Issue: Buttons too big/small
**Solution:**
Change `height` in `.stButton > button` CSS

### Issue: Shadows not visible
**Solution:**
1. Check that `box-shadow` is not commented out
2. Increase shadow spread (second number in shadow)
3. Increase shadow blur (third number in shadow)

---

## Browser DevTools Inspection

### To inspect card styling:
1. Right-click on card → Inspect
2. Find `div[data-testid="stVerticalBlockBorderWrapper"]`
3. Check Styles panel for CSS rules

### To inspect button styling:
1. Right-click on button → Inspect
2. Find `button` element
3. Look for `[kind="primary"]` or `[kind="secondary"]` rules

### To override locally (for testing):
In DevTools Elements tab:
```css
/* Override example */
button {
    height: 60px !important;  /* Test larger buttons */
}
```

---

## Performance Tips

1. **Keep CSS variables** - They're cached and efficient
2. **Use gradients sparingly** - They can impact performance on older devices
3. **Limit animations** - Only use `transition` where needed
4. **Optimize shadows** - Heavy shadows can slow down rendering

---

## File Structure

```
ALLOTMENT-TDB/
├── app.py                          ← Main app (contains styling)
├── UI_DESIGN_SYSTEM.md            ← Full design documentation
├── CSS_COMPONENTS.md              ← Copy-paste components
├── QUICK_REFERENCE.md             ← This file
├── requirements.txt
├── Putt Allotment.xlsx            ← Data file
└── ...other files
```

---

## Next Steps

1. **Read** `UI_DESIGN_SYSTEM.md` for full documentation
2. **Copy** components from `CSS_COMPONENTS.md` as needed
3. **Customize** colors/spacing in the CSS `:root` section
4. **Test** changes in real-time with browser refresh
5. **Deploy** when satisfied with the look

---

## Support

If styling breaks:
1. Check that CSS syntax is valid (use DevTools Styles panel)
2. Ensure all `!important` flags are present where needed
3. Make sure `unsafe_allow_html=True` is set on `st.markdown()`
4. Clear browser cache and refresh

---

**Last Updated:** Jan 14, 2026  
**Version:** 1.0 - Premium UI System
