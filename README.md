# Daily Pandas Practice with ChatGPT, Claude, Meta AI (WhatsApp) 19th October 2026 Part 2
<b>Pandas Challenge - Round 3:</b></br>
<b>Environmental Data Cleaning</b></br>

<b>Step 1: Create the datasets</b></br>
<b>Dataset A - Main monitoring stations</b></br>
```Python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "site_id": [301, 302, 303, 304, 305, 306, 303, 307, 308],
    "state": ["WB", "WB", "AS", "AS", "OD", "OD", "AS", "WB", "AS"],
    "landuse": [
        "Urban", "Forest", "Forest", "Urban", "Agriculture",
        "Forest", "Forest", "Agriculture", "Urban"
    ],
    "rainfall": [420, np.nan, 650, 320, 520, 720, 650, np.nan, 460],
    "ndvi": [0.28, 0.78, np.nan, 0.32, 0.42, 0.82, np.nan, 0.58, 0.68],
    "risk_code": ["H", "M", "H", "L", "H", "M", "H", "M", "L"]
})
```
Notice that <code>stid_id</code>303 is duplicated, and some rainfall and NDVI values are missing.</br>

<b>Dataset B - Backup observations</b></br>
```Python
backup_df = pd.DataFrame({
    "site_id": [302, 303, 307, 308],
    "rainfall_backup": [210, 640, 370, 455],
    "ndvi_backup": [0.76, 0.87, 0.56, 0.66]
})
```

<b>Dataset C - Risk-code lookup</b></br>
```Python
risk_lookup = {
    "H": "High",
    "M": "Moderate",
    "L": "Low"
}
```
<b>Step 2: Your 7 tasks</b></br>

<b>Challenge tracker</b></br>
<ul>
    <li><b>Create a working copy</b></li>
    Create <code>working_df = df.copy()</code>. Keep the original <code>df</code> unchanged.
    <li><b>Remove duplicate stations</b></li>
    Create a duplicate-free Dataframe using <code>drop_duplicates()</code>. Keep the first occurrence of each <code>site_id</code>.
    <li><b>Fill missing values</b></li>
    Use <code>combine_first()</code> to fill the missing rainfall and NDVI values from <code>backup_df</code>. Merge the backup data first using <code>site_id</code>.
    <li><b>Create final columns</b></li>
    Create <code>rainfall_final</code> and <code>ndvi_final</code>, giving priority to the main dataset's value and using the backup values only where the main values are missing.
    <li><b>Use map()</b></li>
    Create a <code>risk_lable</code> column by mapping <code>risk_code</code> through <code>risk_lookup</code>. Think about what happens to an unmapped code.
    <li><b>Use replace()</b></li>
    Create a separate column named <code>landuse_clean</code> where <code>Forest</code> becomes <code>Forest_Area</code> and <code>Urban</code> becomes  <code>Built-up</code>. Leave other land-use categories unchanged.
    <li><b>Filter the cleaned data</b></li>
    Create <code>high_risk_sites</code> containing rows where <code>risk_label</code> is <code>high</code> AND <code>rainfall_high</code> is at least 500. Use the clean dataset.
</ul>
