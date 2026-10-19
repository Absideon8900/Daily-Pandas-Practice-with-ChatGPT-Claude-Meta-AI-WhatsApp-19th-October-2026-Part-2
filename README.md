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
