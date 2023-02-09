# Autosense, Dynamic ISF/ICR and Adjust Basal
## Auto-sensitivity Mode
- Note to check if AF affects autosens

Auto-sensitivity (autosense or autosens) reviews your last 8 hrs and 24 hrs of data every loop cycle (5 min) and determines whether you have been reacting more or less sensitively to insulin. It then makes conservative temporary adjustments to your basal rates, blood sugar target, and ISF.

Example:
Autosense finds Bill has been running more sensitive to insulin lately. In the last 24 hours, he has been 2X more sensitive to insulin, whereas in the last 8 hours, he has been 3X more sensitive to insulin.

Autosense then takes the more conservative calculated sensitivity. In this example, the more conservative value is obtained from the 8 hour window because by assuming Bill is 3X more as opposed to 2X more sensitive to insulin, the system will be posed to give less insulin.

If you are using autotune, autosense will use your calculated autotune ICR, ISF and basal rates as its baseline as opposed to your set values.

Note that autosense does not look at meals and make adjustments to your ICR. It only looks at your sensitivity to insulin and adjust ISF/basal rates/blood sugar targets accordingly.

## Dynamic ISF
Autosense was thought by many to be too conservative and slow at making changes. Dynamic ISF is a drop-in replacement for autosense's ISF calculation formula, with the goal of making it more aggressive. If you find that you have high ISF variability throughout the day, you can turn this feature on.

Dynamic ISF takes into consideration a new variable called "Adjustment Factor" which affects its aggressiveness. If dynamic ISF is too aggressive, you can decrease this number by 0.05-0.1 points to make it more meek. Likewise, increase this number if you still feel dynamic ISF is not aggressive enought.

Note: Dynamic ISF is temporarily disabled and the system reverts to autosense if either "High Temptarget Raises Sensitivity" or "Exercise Mode" is enabled and a high temporary target has been set by the user.

### Advanced information
Autosense determines an ratio (autosens.ratio) and alters your ISF in the following manner:

- New ISF = (profile set ISF)/(autosens.ratio)

Example: Bill has an ISF of 3 in his settings. The system finds Bill has been more resistant to insulin lately and needs to increase his insulin. It calculates Bill to have an autosens.ratio of 1.1 (note that a larger autosens.ratio results in you having a lower, more aggressive ISF):

When autosense goes to adjust the ISF, it does the following calculation:

- 3 UL/mmol / 1.1 = 2.73 UL/mmol

Bill is now temporarily set to have an ISF of 2.73.

Dynamic ISF (using the default logarithmic algorithm in FreeAPS X) uses an alternative formula to calculate the autosens.ratio for ISF adjustments:

- autosens.ratio = profile.sens * AF * TDD * log((BG/peak)+1) / 1800
- New ISF = (profile set ISF)/(autosens.ratio)

This formula takes into consideration your profile set ISF (profile.sens in mg/dl) current blood glucose (BG in mg/dl), total daily dose (TDD over the last 24 hours), insulin peak effect (peak activity normally is 120 min) and a new variable called adjustment factor (AF) that allows for user tuning of Dynamic ISF/CR.

## Dynamic CR
This is an experimental feature that alters the carb ratio (CR) based on current blood sugar and total daily dose (TDD). Unlike ISF, ICR by default is not altered by autosense and changed every Loop cycle. Using dynamic CR will result in a dramatic change in how ICR is calculated by FreeAPS X, and will result in it being modified every 5 min. Dynamic CR uses a similar formula as Dynamic ISF as described above:

- autosens.ratio = profile.sens * AF * TDD * log((BG/peak)+1) / 1800
- New CR = (profile set CR)/(autosens.ratio)

If you find your CR changes dramatically day to day and FreeAPS X is not providing adequate bolus recommendations, you can test this feature. Note that FreeAPS X is already making modifications to your recommended boluses without this feature enabled.

Note:
If the autosens.ratio is greater than 1, the following formula is used to make the calculated CR less aggressive: 

- new.autosens.ratio = (autosens.ratio - 1)/2 + 1 

## Adjust Basal
Adjust basal replaces autosense's formula for adjusting basal rates, with one dependent on total daily dose (TDD) of insulin. Turn on this setting to give basal adjustments more agility. If you have high variability of TDD day-to-day, keep this setting off.

### Advanced Information
Normally a new basal rate is set by autosens:

- New basal profile = current basal profile * autosens.ratio

Adjust basal replaces the autosens.ratio with its own autosens.ratio calculated as such:

- autosens.ratio = (weighted average of TDD)/(2 week average of TDD)
- New basal profile = current basal profile * autosens.ratio

See "Weighted Average of TDD" setting to understand how this variable is calculated.

As an example:

Bill has a TDD of 55 U over the last 24 hours. He has a 14 day TDD average of 48 U. He has set his "Weighted average of TDD" in preferences to 0.7. His current profile basal rate is 1 U/h.

- Weighted average of TDD = 0.7 * 55 U + 0.3 * 48 U = 52.9 U
- basal.autosens.ratio = 52.9 U / 48 U = 1.10
- New basal profile = 1 U/h * 1.10 = 1.10 U/h


