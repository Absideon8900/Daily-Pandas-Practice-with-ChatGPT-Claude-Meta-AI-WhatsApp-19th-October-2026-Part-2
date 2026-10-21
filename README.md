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

# Solution (Script)
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

backup_df = pd.DataFrame({
    "site_id": [302, 303, 307, 308],
    "rainfall_backup": [210, 640, 370, 455],
    "ndvi_backup": [0.76, 0.87, 0.56, 0.66]
})

risk_lookup = {
    "H": "High",
    "M": "Moderate",
    "L": "Low"
}
working_df = df.copy()
working_df = working_df.drop_duplicates(subset="site_id")
working_df = working_df.merge(backup_df,how="left")
working_df["rainfall_final"] = working_df["rainfall"].combine_first(working_df["rainfall_backup"])
working_df["ndvi_final"] = working_df["ndvi"].combine_first(working_df["ndvi_backup"])
working_df["risk_level"] = working_df["risk_code"].replace(risk_lookup)
working_df["landuse_clean"] = working_df["landuse"].replace({"Forest":"Forest_Area",
                                                             "Urban":"Built-Up"})
working_df = working_df[["site_id",
                         "state",
                         "landuse",
                         "rainfall_final",
                         "ndvi_final",
                         "risk_code","risk_level"]]
high_risk_sites = working_df.copy()
high_risk_sites = high_risk_sites[high_risk_sites["risk_level"] == "High"]
high_risk_sites = high_risk_sites[high_risk_sites["rainfall_final"] <= 500]
print()
print("The Output:")
print()
print(working_df)
print()
print("The Output where the risk level is considered High and the rainfall should be less than or equals to 500")
print()
print(high_risk_sites)
print()
```

# Solution (Output)
```Python
PS C:\Users\hp\Downloads\sql_practice(0)> & C:\Users\hp\AppData\Local\Programs\Python\Python313\python.exe "c:/Users/hp/Downloads/sql_practice(0)/test2.py"

The Output:

   site_id state      landuse  rainfall_final  ndvi_final risk_code risk_level
0      301    WB        Urban           420.0        0.28         H       High
1      302    WB       Forest           210.0        0.78         M   Moderate
2      303    AS       Forest           650.0        0.87         H       High
3      304    AS        Urban           320.0        0.32         L        Low
4      305    OD  Agriculture           520.0        0.42         H       High
5      306    OD       Forest           720.0        0.82         M   Moderate
6      307    WB  Agriculture           370.0        0.58         M   Moderate
7      308    AS        Urban           460.0        0.68         L        Low

The Output where the risk level is considered High and the rainfall should be less than or equals to 500

   site_id state landuse  rainfall_final  ndvi_final risk_code risk_level
0      301    WB   Urban           420.0        0.28         H       High

PS C:\Users\hp\Downloads\sql_practice(0)>
```
