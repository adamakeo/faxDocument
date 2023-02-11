# Preferences
Note: FreeAPS X is the iPhone app. It utilizes the OpenAPS algorithm for determining insulin dosing.

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
For optimum performance, the FreeAPS X app should be driven by the continuous glucose monitor (CGM) so the Loop cycle starts with the most recent glucose information available, updates the glucose prediction and then sends commands to the pump, if needed, to modify insulin delivery.

When the phone is locked, a mechanism is required to “wake” up the app out of background mode so it can keep looping.

A Bluetooth connection is used by FreeAPS X to perform this waking from background while the phone is locked.  This is called the heartbeat.

Best case – this comes from the CGM (this is the case with Dexcom where app is on the Looper’s phone)
Second best case – this comes from the RileyLink device (for Eros or Medtronic)
With DASH pods, there is no reliable heartbeat that works all of the time when the phone is locked

This setting allows you to view your hearbeat on your watch.

--- Need to confirm this setting description ---

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
Enabling this setting allows SMBs to be delivered if your blood sugar is predicted to go above target. 

SMBs will remain on if you have a low temporary target set, but will be fully disabled if a high temporary target is set (unless "Allow SMB With High Temptarget" is enabled).

There are limitations on the size of SMBs. <a href = "https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html#understanding-super-micro-bolus-smb">See the OpenAPS documentation for more information.</a>

### Max Delta-BG Threshold SMB
THis is a safety limiter that looks at the difference between your last two blood glucose readings. If the difference is large, FreeAPS X suspects them to be incorrect and will suspend SMB delivery accordingly.

You can adjust how much of a difference should be allowed before SMBs are delivered. 30% is advised for closed loop.

### Enable SMB With COB
SMBs will be enabled if you currently have carbs on board (COB) to help you deal with meal spikes. This feature should be enabled if you want to use UAM.

If you already have "Enable SMB Always" on, this feature is redundent and does not need to be configured.

### Enable SMB with Temptarget
SMBs will be enabled if you have set a lower blood sugar target temporarily. This will allow you to reach your target faster.

If you already have "Enable SMB Always" on, this feature is redundent and does not need to be configured.

### Enable SMB After Carbs
SMBs will be enabled if you had carbs within the last 6 hours to help with meal spikes.

If you already have "Enable SMB Always" on, this feature is redundent and does not need to be configured.

### Allow SMB With High Temptarget
This is a safety limiter that prevents SMBs from occuring when you set a temporary blood glucose target higher than your normal target range.

This setting does limit "Enable SMB Always."

### Enable SMB With High BG
This allows SMBs to occur above a certain blood glucose level. Some individuals with variable sensitivity may find that SMBs can cause low blood sugars and rollercoasters when near their target. 

If you are in closed loop and rely heavily on UAM (i.e. you do not bolus for your meals) you should keep this feature disabled so FreeAPS X can provide you with the necessary insulin if you are predicted to go high. Else if you are currently within your target blood glucose range, SMBs will not be delivered.

If you already have "Enable SMB Always" on, this feature is redundent and does not need to be configured.

### ... When Blood Glucose is Over (mg/dl)
See the above setting for more information. This allows you to configure the target at which SMBs will be enabled.

### Enable UAM
With this option enabled, the SMB algorithm can recognize unannounced meals. This is helpful if you forget to tell FreeAPS X about your carbs or estimate your carbs wrong and the amount of entered carbs is wrong or if a meal with lots of fat and protein has a longer duration than expected. Without any carb entry, UAM can recognize fast glucose increasements caused by carbs, adrenaline, etc, and tries to adjust it with SMBs. This also works the opposite way: if there is a fast glucose decreasement, it can stop SMBs earlier.

### Max SMB Basal Minutes
Max SMB Basal minutes is one of the major limiters of the size of SMBs. For more information on what limit's SMB size, please view<a href = "https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html#understanding-super-micro-bolus-smb"> the OpenAPS documentation.</a> SMBs are limited by the amount of scheduled basal insulin in your profile settings.

Example: Bill has a basal profile setting of 1.5 U/hr. He has a "Max SMB Basal minutes" setting of 30 min:

- 1.5 U / 60 min * 30 min = 0.75 U

Each of Bill's SMBs are thus limited to a maximum of 0.75 U by Max SMB Basal Minutes. 

If you find that FreeAPS X is giving insignificant boluses every 5 minutes, it may be being limited by your Max IOB or Max SMB Basal Minutes. First, confirm your basal rates are adequate. Then you can experiment with increasing this number so FreeAPS X is able to provide larger SMBs for more rapid blood sugar control.

### Max UAM SMB Basal Minutes
See "Max SMB Basal Minutes" above and<a href = "https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html#understanding-super-micro-bolus-smb"> the OpenAPS documentation</a> for more information on the limiters of SMBs. 

This limits the size of SMBs UAM can deliver when it detects a meal, by the amount of scheduled basal insulin in your profile settings. You can configure this setting to make UAM more or less aggressive against meal spikes. Note that SMBs are also limited by your Max IOB and DeliveryRatio.

### SMB DeliveryRatio
This is a safety limiter. FreeAPS X determines how much insulin is required to get you back within target range. If SMB is enabled, FreeAPS X then delivers an SMB, that defaults to half the required insulin.

This setting allows you to boost or reduce what fraction of the required insulin is delivered in a single SMB. It is recommended you look at your basal profile, Max SMB basal minutes, Max UAM SMB Basal Minutes, and Max IOB before you adjust this setting.

### SMB Interval
The minimum interval between SMB boluses. SMBs will be delivered at this rate or less as needed.

### Bolus Increment
The minimum amount of insulin that can be bolused by FreeAPS X via an SMB. This is determined by your pump hardware.

## OpenAPS Targets Settings
### High Temptarget Raises Sensitivity
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

Normally FreeAPS X assumes your sensitivity will be lower with higher blood sugar levels. During periods of exercise, some people may instead experience increased sensitivity to insulin. With this feature enabled, setting a high temporary target will decrease the autosens.ratio being utilized for ISF and basal adjustments, resulting in less insulin being delivered overall. This scales with the temporary target set; higher and higher temp targets lead to lower and lower insulin delivery in the form of basal rates and corrections. 

Note that this setting disables Dynamic ISF at high temp targets.

### Low Temptarget Lowers Sensitivity
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

When planning to have a heavy meal, you may want to set a low temporary target to avoid high blood sugar spikes. You may also want FreeAPS X to deliver more insulin during this time to prevent meals from spiking too high. Enabling this feature will increase your autosens.ratio, which is utilized for ISF and basal adjustments, resulting in greater insulin delivery. This will allow FreeAPS X to better deal with post-prandial spiking.

### Sensitivity Raises Target
When performing autosense and insulin dosing calculations, FreeAPS X uses a target blood glucose that is by default the lower value in your target range.

Example: Bill has a target range of 5.5 to 6.0. His target blood glucose is thus 5.5. (Note that Bill's target is not exactly this value; FreeAPS X alters the target via autosense to improve its dosing)

When "Sensitivity Raises Target" is enabled, FreeAPS X will set a higher blood glucose target to base its insulin dosage calculations off of if it detects sensitivity. This can be useful if you find FreeAPS X is too aggressively.

Advanced information:
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

If the autosens.ratio is determined to be <1.0, this setting comes into effect and increases the blood glucose target by a small amount. See the OpenAPS code base for the exact formulas used.

### Resistance Lowers Target
See "Sensitivity Raises Target" for more information. When FreeAPS X detects high insulin resistance, it will set a lower blood glucose target for insulin dosage calculations, providing more insulin overall. This is useful for patients who experience uncontrollable highs.

Advanced information:
Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.

If the autosens.ratio is determined to be >1.0, this setting comes into effect and decreases the blood glucose target by a small amount. See the OpenAPS code base for the exact formulas used.

### Advanced Target Adjustments
Deprecated; autosense has alternative functions for determining if insulin can be safely added when high.

### Exercise Mode
Redundant; same as "High Temptarget Raises Sensitivity". Enabling either feature will provide the desired changed to sensitivity with high temp targets.

### Half Basal Exercise Target
This setting allows you to control the reduction in basal when using either "Exercise Mode" or "High Temptarget Raises Sensitivity." Default is 160 mg/dL meaning basal will be at 50% of your scheduled with a temporary target at 160, 60% at 140, and 75% at 120.

Advanced information:
See openAPS code for more information. The formula used is:

- (halfBasalTarget - 100)/((halfBasalTarget - 100)+(targetBG-100))

Example: Bill has a halfBasalTarget of 160 and has set a temporary target of 120 for his upcoming exercise. Therefore only 75% of his scheduled basal rate will be provided:

- (160 - 100)/((160 - 100) + (120 - 100)) = 0.75

### Wide BG Target Range
By default FreeAPS X ignores your upper value when setting a target range. 

Example: Bill has set a target range of 5.5-6.0. FreeAPS X internally sets it at 5.5-5.5.

This setting allows you to change that behaviour so FreeAPS X targets a desired range instead of your lower value.

Note: This feature is likely to be deprecated from the OpenAPS algorithm soon.

## OpenAPS Other Settings
### Rewind Resets Autosens
Rewind in Medtronic lingo refers to the attachment of a new insulin reservoir and infusion set. For Omnipod users, this means replacing your pod with a new one.

When you change the insulin injection site, you might find your insulin sensitivity is altered based on how well its diffusing into your bloodstream. 

This setting resets autosense's calculated autosens.ratio and forces it to restart anew from the time of the site change to improve your calculated basal rates, sensitivity ratio and target blood glucose.

Please read <a href="/autosens-dynamic">Autosense and Dynamic ISF/ICR</a> for more information.
### Use Custom Peak Time
Allows you to optimize at what time your insulin has maximum activity if the defaults do not work for you.

### Insuln Peak Time
Requires "Use Custom Peak Time" to be enabled. Select a peak activity time point, within the limits set by OpenAPS based on your insulin type.

### Skip Neutral Temps
This is a feature that has been brought from the OpenAPS algoirtm but does not play much role in FreeAPS X. Light sleepers with OpenAPS would find that the notifications every time OpenAPS made a temp basal adjustment, would wake them up from sleep. 

This setting attempts to reduce notifications that are produced by OpenAPS (and FreeAPS X) with the downside of perhaps impacting control and making it harder to see if the system is working.

Recommend to keep this setting disabled.

### Unsuspend if No Temp
After suspending your pump you will be provided a reminder at a chosen time to manually resume your pump. Many people however neglect this reminder and forget to unsuspend, leading to highs.

This feature allows you to use zero temp basals as a way of unsuspending your pump automatically. Prior to suspending your pump, set a 0 U/hr temp basal for the period of time you want the pod to remain suspended. Then suspend the pod. Once the temp basal expires, the pod will be automatically reactivated.

### Suspend Zeros IOB
This allows FreeAPS X to better understand that when a pump supension occurs, no insulin is being delivered to the patient.

FreeAPS X will set a zero temp basal (0 U/hr) during pump suspensions, improving its insulin on board calculations, and thereby its algorithm calculations.

Recommended to keep this setting enabled.

### Bolus Snooze DIA Divisor
Deprecated; Bolus snooze has been removed in latest versions of OpenAPS so this value does not matter.

### Min 5m Carbimpact
This is a fallback setting used by FreeAPS X. If FreeAPS X is unable to tell if carbs are being absorbed from blood sugar readings, it will estimate how many carbs have been absorbed using this setting.

The default value of 8mg/dL/5min assumes carbohydrates will increase blood sugar by 8 mg/dl every 5 minutes. The actual amount of carbohydrates estimated to be absorbed is depended on your calculated carbohydrate sensitivity ratio (CSF = ISF/ICR).

### Autotune ISF Adjustment Fraction
Autotune by default adjust your ISF by 20% each nightly run. This values allows you to make your autotune adjustments less aggressive.

Set this at 1 for the full 20% adjustment.

Advanced information:

-  adjustedISF = adjustmentFraction * autotuneISF + (1-adjustmentFraction) * profileISF
- newISF = ( 0.8 * profileISF ) + ( 0.2 * adjustedISF )

Example: Bill has a profile ISF of 3. Autotune thinks he has a true ISF value of 4. His adjustment fraction is 1.

- adjustedISF = 1 * 4 + (1 - 1) * 3 = 4
- newISF = (0.8 * 3) + (0.2 * 4) = 3.2

Assuming autotune is not being limited by the autosense max and min, Bill's ISF will be set to 3.2 by autotune tonight. Autotune will then repeat the following night, starting with a profileISF = 3.2

### Remaining Carbs Fraction
This is the fraction of carbs that is assumed not to be absorbed yet after 4 hours if carb absorption has not been seen. 

When attempting to measure carbohydrates on board (COB) FreeAPS X may not be fully accurate. This setting is a safety feature that can prevent FreeAPS X from providing insulin for non-existent carbs.

Example: It has been 4 hours since Bill ate 20 carbs. FreeAPS X has been able to calculate that hes absorbed 15 carbs, but cannot account for the 5 other carbs yet. Bill has a remaining carbs fraction of 0.75

- Remaining COB = COB - absorbedCarbs - mealCarbs * (1 - carbsFraction)

- 20 - 15 - 20(1 - 0.75) = 0 

Bill is assumed to have zero carbs on board

Recommend to keep this value at the default of 1 meaning it will not impact FreeAPS X's calculations. This feature is closely tied to "Remaining Carbs Cap."

### Remaining Carbs Cap
This setting is a safety limiter that determines the maximum amount of carbs that are assumed to be absorbed after 4 hours if carb absorption. A minimum of 90 carbs is mandatory for this setting.

Example: Bill eats 150 carbs. After 4 hours, FreeAPS X calculates a COB of 110. It will truncate that number to 90 carbs.

Recommended to keep this value at default unless you know what you are doing.

### Noisy CGM Target Multiplier
If FreeAPS X detects that CGM data has been noisy, it will increase your target blood sugar by a set fraction to avoid you getting low. Default is 30% higher (1.3)

Example: Bill's FreeAPS X has calculated a blood glucose target of 90 mg/dl (5 mmol/L). But Bill has a noisy sensor. He has set his "Noisy CGM Target Multiplier" to 1.3. FreeAPS X will thereby use a target bg of:

- 90 mg/dL * 1.3 = 117 mg/dL

Recommended to keep this value at default of 1.3.