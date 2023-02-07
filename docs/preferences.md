# Preferences

## FreeAPS X

### Glucose Units
Allows you to toggle between mmol/L and mg/dl blood glucose units.

### Remote Control
Allows for remote control of FAX using Nightscout.

**Duplicate Delivery Risk**
<br><span style="color:red";>
We want to highlight a very important risk before you get started.
<br><br>
For safety, always assume a previous remote carb / bolus was delivered despite whether it shows in Nightscout FreeAPS X. For motivation think of the following example:
<br><br>
You send a 5 unit remote bolus.
The bolus is delivered to the Looper.
Nightscout is having a temporary technical issue and doesn't show the bolus was received.
You are watching Nightscout and you don’t see a delivery so you assume it failed.
You send another remote 5 unit bolus.
The second 5 unit bolus is delivered to the Looper (10 Units total).
<br><br>
You can see the danger of sending duplicate bolus/carbs so be careful. If a remote bolus/carb entry doesn’t show in Nightscout/Caregiver, use your own judgment on whether enough time has passed to try again.
</span>

To use, navigate to your connect nightscout instance. Click the lock icon on the top right and enter your API-Secret. Next, click the now visible "+" sign on the top right. A "Log a Treatment" menu will open up. 

To enter carbs, select the "carb correction" event type. Fill in the required blanks and click "Submit Form". FreeAPS X will read any carb entries in Nightscout and adjust insulin delivery as configured.

To configure loop status, pump or to bolus, select the "announcement" event type. In additional notes state one of the following options:

* To bolus, enter "bolus:(amount)" (ex: bolus:0.5).
* To control pump, enter either "pump:suspend" or "pump:resume". 
* To control looping, enter either "looping:true" or "looping:false"
* To control temporary basal rate, enter "tempbasal:(rate),(minutes)" (ex: tempbasal:0,60 to set temporary basal rate at 0 U/hr for 60 minutes)

Note that remote configurations with announcement-type events can only be repeated every 10 minutes.

### Recommended Insulin Fraction
Recommended insulin fraction is a safety feature built into FreeAPS X. By default, FreeAPS X calculates an "insulin required" value when bolusing for carbs that is half of the insulin actually needed to deal with said meal. FreeAPS X then delivers the remaining insulin via SMBs as the blood sugar starts to rise.

Recommended insulin fraction allows you to alter the amount initially delivered. At its default (1.5), it results in 75% of the required meal bolus being delivered before the meal. You can increase or decrease this to alter the insulin delivered prior to the meal.

### Skip Bolus screen after carbs
After entering carbs, a mealtime bolus will not be suggested or delivered.

### Display HR on Watch
Displays your heart rate on your iWatch FreeAPS X app

### Display Statistics
Visual: Displays statistics including Time in range (TIR), coefficent of variance (CV) and estimated A1c at the bottom of the main screen. 

For advanced users: Enabling this settings also allows FreeAPS X to sync your statistics to the Nightscout API.

## Statistics
These settings are purely visual and configure the statistical collection.

### Low Glucose Limit
Sets the lower blood sugar limit for statistical determiniation of time below range (TBR).

### High Glucose Limit
Sets the higher blood sugar limit for statistical determination of time above range (TAR)

### Update every number of minutes
Determines how often the statistics are updated on the homescreen. Also controls how often statistics are uploaded to Nightscout for advanced users.

### Display Loop Cycle statistics
Shows the average number of loops performed over the last 24 hours. Ideally the number should be near 288, the maximum number of loops performed per day. Negative impacters include CPU speed, pump and pod changes, connection issues, etc.

### Override HbA1c unit
The estimate HbA1c statistic is by default given in mmol/mol units. Enabling this toggle converts it to percentage units.

## OpenAPS Main Settings
### Insulin Curve
Enter your insulin type for the appropriate response curve to be used by the algorithm:

- bilinear: IOB curve based on a bilinear activity curve that varies by user’s duration of insulin action setting in their pump.
--insert image of bilinear curve--
- rapid-acting: This is a default setting for Novolog, Novorapid, Humalog, and Apidra insulins. Selecting this setting will result in OpenAPS to use an exponential activity curve with peak activity set at 75 minutes and duration of insulin action set at 300 minutes (5 hours).
- ultra-rapid: This is a default setting for Fiasp. Uses an exponential activity curve with peak activity set at 55 minutes and duration of insulin action set at 300 minutes (5 hours).
-- insert image of exponential curve --

Note that the duration of insulin (DIA) action can be altered in the pump settings section of FreeAPS X. A minimum of 5 hours is required.

<a href="https://www.diabettech.com/insulin/why-we-are-regularly-wrong-in-the-duration-of-insulin-action-dia-times-we-use-and-why-it-matters/">To understand why a higher duration of insulin action is used in FreeAPS X, click to see the following documentation.</a>

### Max IOB
The maximum amount of insulin on board (i.e. in the body). This includes insulin from all sources (basal and bolus) that is automatically delivered by FreeAPS X. Manual boluses are not subjected to this limiter. 

Default is set to zero meaning FreeAPS X can only set temporary basal rates lower that your profile basal rate. I.e. it cannot set temporary basal rates that exceed your profile basal rate in cases of high blood sugar, and it cannot use super micro boluses to control blood sugar.  

You can start by increasing this number to your average mealtime bolus and evaluating its effect. The default recommendation is “average mealbolus + 3x max daily basal” when using super micro boluses.

Ex: If you average mealtime bolus is 6 U, and you have the following basal profile:

- 12am: 1 U/hr
- 6pm: 2 U/hr (this is the "max" basal used) 
- 9pm: 1.5 U/hr 

Your recommended IOB = 6 + 3 * 3 = 15 U. 

If you are insulin resistance, you can continue to increase this number further to allow for greater insulin delivery.

### Max COB
The maximum amount of carbs that FreeAPS X is allowed to dose for. This is a safety feature that protects against erroneous carbohydrate entries that could lead to hypoglycemia episodes.

Choose the maximum amount of carbs you bolus for

### Max Daily Safety Multiplier
Limits the maximum temporary basal rate FreeAPS X is able to use at **any time. The default setting of 3, which is unlikely to need adjustment, allows for a maximum basal rate of 3x the max daily basal.

Ex: If you have the following basal profile:

- 12am: 1 U/hr
- 6pm: 2 U/hr (this is the "max" basal used) 
- 9pm: 1.5 U/hr 

The maximum temporary basal rate that can be set is 2 U/hr * 3 = 6 U/hr

### Current Basal Safety Multiplier 
Limits the maximum temporary basal rate FreeAPS X is able to use at the **current time. The default setting of 4, which is unlikely to need adjustment, allows for a maxium basal rate of 4x the current basal rate. 

Ex: If it is currently 9am and you have the following basal profile:

- 12am: 1 U/hr
- 6pm: 2 U/hr (this is the "max" basal used) 
- 9pm: 1.5 U/hr 

The maximum temporary basal rate that can be set by FreeAPS X at 9am is 1 U/hr * 4 = 4 U/hr

### Autosens Max
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> before adjusting this setting.