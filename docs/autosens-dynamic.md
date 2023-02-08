# Autosense and Dynamic ISF/ICR
## Auto-sensitivity Mode
- Note to check if AF affects autosens

Auto-sensitivity reviews your last 8 hrs and 24 hrs of data every loop cycle (5 min) and determines whether you have been reacting more or less sensitively to insulin. It then makes conservative temporary adjustments to your basal rates, blood sugar target, and ISF.


Example:
Autosense finds Bill has been running more sensitive to insulin lately. In the last 24 hours, he has been 2X more sensitive to insulin, whereas in the last 8 hours, he has been 3X more sensitive to insulin.

Autosense then takes the more conservative calculated sensitivity. In this example, the more conservative value is obtained from the 8 hour window because by assuming Bill is 3X more as opposed to 2X more sensitive to insulin the system will be posed to give less insulin.

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

- new.autosens.ratio = profile.sens * AF * TDD * log((BG/peak)+1) / 1800
- New ISF = (profile set ISF)/(new.autosens.ratio)

This formula takes into consideration your current blood glucose (BG in mg/dl), total daily dose (TDD over the last 24 hours), insulin peak effect (peak activity normally is 120 min) and a new variable called adjustment factor (AF) that allows for user tuning of Dynamic ISF.

## Dynamic CR
This is an experimental feature that alters the carb ratio (CR) based on current blood sugar and total daily dose (TDD). It uses the same formula as Dynamic ISF described above, namely:

- new.autosens.ratio = profile.sens * AF * TDD * log((BG/peak)+1) / 1800
- New CR = (profile set CR)/(new.autosens.ratio)

If you find your CR changes dramatically day to day and FreeAPS X is not providing adequate bolus recommendations, you can test this feature. Note that FreeAPS X is already making modifications to your recommended boluses without this feature enabled.


