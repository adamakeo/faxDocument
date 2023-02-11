# Basal, ISF and ICR

## Basal Profile
Basal profile contains your scheduled basal rates which controls how much insulin is delivered at each hour of the day. It is important to understand that these settings are not taken verbaitam by when looping. They will be adjusted on the fly, being replaced with temporary basal rates throughout the day, based on your current blood sugar reading. Your set values will also be altered by autosense and autotune to a certain degree if FreeAPS X does not believe them to be quite accurate.

Still, your basal profile values should be near your true value without relying on FreeAPS X's algorithm. Safety limiters could prevent FreeAPS X from automatically selecting the appropriate dose for you if your scheduled basal is too inaccurate.

Note that temporary basal rates higher than your profile basal rate cannot be used if Max IOB is set at 0.

## Insulin Sensitivity Factor
ISF, also called insulin correction factor (ICF), refers to the amount of blood glucose in mmol/L one unit of insulin can neutralize.

Example: Bill has a 1:4 ISF (this is also written in shorthand as an ISF of 4). This means 1 U of rapid insulin will bring Bill's sugar down 4 mmol/L.

Like basal rates, ISF is not used verbaitam by FreeAPS X, but is modified over time as data on the patient is collected. Still, it is important to set ISF as close to accurate as possible for FreeAPS X to function well.

## Insulin Carbohydrate Ratio
ICR refers to the amount of carbohydrates one unit of insulin is able to neutralize. 

Example: Bill has a 1:10 ICR (aka an ICR of 10). If Bill has 20 carbs with lunch, he will need 2 U of rapid inslin to neutralize it.

ICR is not changed as drastically as basal rates or ISF unless Dynamic CR is enabled. As always, your ICR must be as accurate as possible for best FreeAPS X function.
