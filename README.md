# Term Structure of CDS Spreads

On the credit default swap market, a series of spreads with corresponding maturities can be obtained every business day.  Let us define the term structure of default swap spreads  , where   denotes a maturity date of a market default swap, and   denotes the corresponding spread (fee rate).  From a term structure of spreads, a term structure of hazard rates   can be calibrated through bootstrapping such that market default swaps are at par.  We assume a stepwise constant hazard rate structure, where each constant   is applied on a period   assuming   is the value date.  

If a term structure of default swap spreads is provided in the first format, calibration is independent on the target default swap we want to price (see https://finpricing.com/lib/FxCompound.html). Time to the value date in years must be divisible by 0.25 so that quarterly payment dates can be generated; otherwise the program will give an error message.  For the first market default swap  ,   can be found by using a root search technique such that the value of this market default swap at the value date is zero, i.e.  .  The calculated   is then applied on the period  .  Another hazard rate  , applied on the period  , can be found such that the value of the second market default swap at the value date is zero, i.e.,  .  We repeat the above procedure until we exhaust all the market default swaps in the term structure file.  A term structure of hazard rates is therefore obtained, denoted by  .

In the second format, maturity dates of the market default swaps are provided.  We first copy all the payment dates of the target default swap to the payment dates used for calibration.  If the target default swap is forward starting, quarterly calibration payment dates will be generated from the first payment date of the target default swap backward to the value date.  If the target default swap finishes before the last maturity date in the term structure file, quarterly calibration payment dates will be generated from the maturity date of the target default swap forward to the last maturity date in the term structure file.  The payment dates used for calibration are therefore built.

We require that each of these maturity dates in the term structure file match one of the generated calibration payment dates.  If one of these maturity dates cannot match any payment date of the target default swap, this maturity date will be replaced by the closest calibration payment date with the corresponding spread unchanged.

It can be seen that if the second format is provided, calibration is dependent on the target default swap.  Regardless which format is provided, we will assume those market default swaps have the same contract specifications as the target default swap, where payment is made at the payment date when default occurs and no accrued fee is allowed in the current model.

The value of the default swap to the buyer, if payment is made at the payment date when default occurs and no accrued fee is allowed, is given by

	 	(1)

where R is the recovery rate, s is the initial fixed fee rate,   is the discount factor,   is a payment date of the target default swap,   is the day count fraction with the given day count convention, and   is the survival density function.

With stepwise constant hazard rates  , if there is only one flat hazard rate   in a payment period  , the integral in equation (1) should be

	 	(2)

where   is the survival probability, and 

	 .	(3)

The survival probability at the value date   is 1.  If there are two flat hazard rates   and   located in  , which indicates that a maturity   of a market default swap falls in the payment period   of the target default swap, then

 

If we assume a flat hazard rate in  , given by

	 

Then, a time-weighted average of hazard rates is obtained

	 .	(4)		

In the model, however, the effective hazard rate at the payment date  , to be used as a flat rate for the payment period  , is determined differently.  If   falls between a calibration payment period  , with corresponding hazard rates  , the effective hazard rate   is linearly interpolated by using

	 .	(5)

It can be found that equations (4) and (5) are equivalent if  .  In other words, as long as the length of each payment period of the target swap, where there are two values of hazard rates, is the same as the length of corresponding payment period of calibration swaps, the model for interpolating hazard rates is acceptable.

The model uses linear interpolation of hazard rates.  This is equivalent to the time-weighted average of hazard rates only when the length of each payment period of the target swap, where there are two values of hazard rates, is the same as the length of corresponding payment period of calibration swaps.  As such, it is required that if there are irregular payment dates in the target default swap, that irregularity must be minimal.  This restriction is imposed on those transactions where time to value date in years is specified in the term structure of hazard rates.  If the time-weighted average of hazard rates is used in future development, this restriction can be removed.

