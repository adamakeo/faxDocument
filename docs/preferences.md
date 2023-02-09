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

Note that remote configurations with announcement-type events can only be performed every 10 minutes.

### Recommended Insulin Fraction
Recommended insulin fraction is a safety feature built into FreeAPS X. By default, FreeAPS X calculates an "insulin required" value when bolusing for carbs that is half of the insulin actually needed to deal with said meal. FreeAPS X then delivers the remaining insulin via SMBs as the blood sugar starts to rise.

Recommended insulin fraction allows you to alter the amount initially delivered. At its default (1.5), it results in 75% of the required meal bolus being delivered before the meal. You can increase or decrease this to alter the insulin delivered prior to the meal.

### Skip Bolus screen after carbs
After entering carbs, a mealtime bolus will not be suggested or delivered.

### Display HR on Watch
Displays your heart rate on your iWatch FreeAPS X app


For Loopers Using Libre as  CGM
What is a heartbeat and why should it come from a CGM?
For optimum performance, the app should be driven by the continuous glucose monitor (CGM) so the Loop cycle starts with the most recent glucose information available, updates the glucose prediction and then sends commands to the pump, if needed, to modify insulin delivery.

When the phone is locked, a mechanism is required to “wake” up the app out of background mode so it can keep that loop symbol a nice green color.

A Bluetooth connection is used by Loop to perform this waking from background while the phone is locked.  This is called the heartbeat.

Best case – this comes from the CGM (this is the case with Dexcom where app is on the Looper’s phone)
Second best case – this comes from the RileyLink device (for Eros or Medtronic)
With DASH pods, there is no reliable heartbeat that works all of the time when the phone is locked
In order to solve this problem, the folks who work with Libre sensors make the Bluetooth connection available to Loop. This is an area where more progress may happen, but for now, it is xDrip4iOS that has this feature working with Loop. Follow the heartbeat instructions below to get best performance using Libre and allow continued looping while phone is locked.



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
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> and <a href="/autotune">Autotune</a> before adjusting this setting.

This setting determines the maximum ratio autosense can use for its adjustments. Increasing this value allows autosense to make more aggressive adjustments to your basal profile, ISF, and target blood glucose.

If you have Dynamic ISF and/or Dynamic CR, this setting will also limit their ability to make more aggressive adjustments.

If you have autotune enabled, this setting also limits its ability to make more aggressive adjustments to your ICR, basal profile and ISF.

### Autosens Min
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> and <a href="/autotune">Autotune</a> before adjusting this setting.

This setting determine the minimum ratio autosense can use for its adjustments. Decreasing this value allows autosense to make less aggressive adjustments to your basal profile, ISF, and target blood glucose.

If you have Dynamic ISF and/or Dynamic CR, this setting will also limit their ability to make less aggressive adjustments

If you have autotune enabled, this setting also limits its ability to make less aggressive adjustments to your ICR, basal profile and ISF.

## Dynamic Settings
### Enable Dynamic ISF
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

Dynamic ISF is a more aggressive alternative to autosense's ISF adjustment algorithm. Turn this on if you believe yourself to be highly resistant to insulin at some points in the day and autosense does not adequately alter your ISF to deal with it.

### Enable Dynamic CR
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

Dynamic CR alters your ICR every loop cycle based on your current blood glucose and TDD of insulin. Turn it on if you experience your ICR changes day-to-day or at different blood glucose levels and FreeAPS X is not consistently suggesting appropriate boluses. You should rule out other causes for this first including inadequate carb counting or inappropriate profile ICR.

### Adjustment Factor
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

Adjustment Factor (AF) allows one to bias the Dynamic ISF and Dynamic CR (if they are enabled) towards more or less aggressive results. Maintaining this value at 1 keeps Dynamic ISF/CR at its default. Increasing AF above 1 will result in Dynamic ISF/CR outputting more aggressive values, while decreasing it below 1 will bias the output towards less aggressive values.

Example: Bill has Dynamic CR on. His Dynamic CR is calculated to be 1:4 by FreeAPS X based on his current blood glucose, TDD and his set ISF. 

But Bill decides to set his AF to 1.2 because he has found recently that Dynamic CR has not been giving him aggressive enough numbers. FreeAPS X acts accordingly, and increases his CR to something above 1:4 instead (ex: 1:3.5). Note that this is a simplified example. See the section on Dynamic CR for more information.

It is important to understand that AF is not a safety limiter. By increasing the AF, you are are not allowing FreeAPS X to choose both higher and lower values based on your needs. Rather by increasing AF, you are telling the system that ALL of your Dynamically calculated ISF/CR values have not been aggressive enough, and you want the system to make them more aggressive.

The same is true when you lower AF. You are telling the system that ALL dynamically calculated values are too aggressive, and to make them lower.

### Use Sigmoid Function
Dynamic CR and ISF by default use a logarithmic function to perform calculations. Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information. 

This option replaces the logarithmic function with a sigmoid function for Dynamic ISF/CR calculations and alters how AF affects their values (AF becomes akin to a safety limiter unlike how it was used in the logarithmic function).

More information to this section will be added soon. Use this feature at your own risk. Set your AF to half of its current value if you decide to do so, and adjust from there.

### Weighted Average of TDD. Weight of past 24 hours:
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

This ratio is used by "Adjust basal" for its calculations. It allows you to effectively control how dynamic basal adjustments are (if Adjust basal is enabled). You can increase the number to a max of 1 make them more dynamic, and decrease them to a minimum of 0 make them less dynamic. The default of 0.65 means that the system will use 65% of the TDD over the last 24 hours for its calculations, and 35% of TDD over the last 2 weeks.

Example: Bill has a TDD of 55 U over the last 24 hours. He has had a TDD of 48 U over the last 14 days. His Weighted Average is set at 0.65:
- TDD Average = 55 * 0.65 + 48 * 0.35 = 52.55

As you increase the default 0.65 ratio to a higher number, the basal rates will be more so determined by your last 24 hour insulin usage, resulting in more dramatic changes.

### Adjust basal
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

Adjust basal replaces the sensitivity-based formula normally used by autosense for adjusting your basal rates, with one dependent on your TDD of insulin. Use this if FreeAPS X is not by default suggesting adequate basal rates for you.

### Threshold Setting (mg/dl)
The threshold setting is a safety limiter function. If blood sugar at any point is predicted to go below this value, FreeAPS X will suspend insulin delivery (SMBs are halted and Temp Basal of 0 U/hr set) and wait till its algorithms predict otherwise. This setting can be useful if you are experiencing a high number of hypoglycemia events. <a href="https://openaps.readthedocs.io/en/latest/docs/While%20You%20Wait%20For%20Gear/Understand-determine-basal.html?highlight=Safety%20Threshold">Please review the OpenAPS documents if you want a better understanding of how it is used.</a> The threshold setting is by default determined by your blood glucose target setting:

- Lower Target: 90 mg/dl = Threshold 65 mg/dl 
- Lower Target: 100 mg/dl = Threshold 70 mg/dl 
- Lower Target: 110 mg/dl = Threshold 75 mg/dl 
- Lower Target: 130 mg/dl = Threshold 85 mg/dl 


This setting allows you to choose a higher threshold setting than default. Note that you cannot choose something that is lower than the default setting.

Ex: Bill has set a BG target of 110 mg/dl. He has set his threshold to 65 mg/dl in his FreeAPS X preferences. Because FreeAPS X's default threshold setting is 75 mg/dl for 110 mg/dl blood glucose target, Bill's preference will be ignored.

### Enable SMB Always
Enabling this setting allows SMBs to be delivered in the following situations:

- You have carbs on board (COB).
- You entered carbs a minimum of 6 hours ago (even if you have zero COB now).

SMBs will remain on if you have a lower temporary target set (example: you are preparing for a hefty meal), but will be disabled if a higher temporary target is set (example: you are planning to exercise).

There are limitations on the size of SMBs. <a href = "https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html#understanding-super-micro-bolus-smb">See the OpenAPS documentation for more information.</a>



### Enable SMB With High BG
This is a safety limiter that only allows SMBs to occur above a certain blood glucose level. Some individuals with variable sensitivity may find that SMBs can cause low blood sugars and rollercoasters when near their target. 

If you are in closed loop and rely heavily on UAM (i.e. you do not bolus for your meals) you should keep this feature disabled so FreeAPS X can provide you with the necessary insulin if you are predicted to go high. Else if you are currently at a normal BG level, SMBs will not be delivered.

### ... When Blood Glucose is Over (mg/dl)
See the above setting for more information. This allows you to configure the target at which SMBs will be enabled.