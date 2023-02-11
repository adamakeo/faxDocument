# SMB and UAM

## Super Micro Boluses
Super micro bolus (SMB) gives you small amounts of insulin upfront in the form of a bolus. Your temporary basal rates will be reduced accordingly before or after the SMB is delivered, so you will overall receive the same amount of insulin you would have received with temporary basal rates alone. SMB allows for faster control of blood sugars by providing more insulin up front.

There are limitations on the size of SMBs. <a href = "https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html#understanding-super-micro-bolus-smb">See the OpenAPS documentation for more information.</a>

Notes:
- Please ensure your profile settings are accurate before enabling SMB and UAM. Use autotune over several weeks if you need support with this.
- Do not perform manual correction boluses with SMB and UAM enabled. You will be at a high risk of having a low.

There are numerous settings related to enabling SMBs in FreeAPS X. Below is the presidence of the settings FreeAPS X uses when deciding whether to enable or disable SMB:

- disable SMB when a high temptarget is set (unless allow SMB with High Temptarget is enabled)
- enable SMB/UAM if always-on (unless previously disabled for high temptarget)
- enable SMB/UAM (if enabled in preferences) while we have COB
- enable SMB/UAM (if enabled in preferences) for a full 6 hours after any carb entry
- enable SMB/UAM (if enabled in preferences) if a low temptarget is set

A flow chart will be added here for greater clarity on FreeAPS X SMB decision making.

# Unannounced Meals
Unannounced Meal (UAM) provides an alternative method (in addition to or instead of carb entry) for detecting and safely dosing insulin via SMBs in response to significant BG rises, whether they are due to meals, adrenaline, or any other reason.