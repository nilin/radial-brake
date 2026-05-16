# Radial Brake



Radial brake dampens the radial component of the update vector, in my experiments by a factor `OUTWARD_SCALE_FACTOR = 0.5`. The cleanest definition is as follows. Apply the usual update `w := w_prev+dw` to the weights. Then re-scale `w` to `w_brake` such that 

`||w_brake|| = ||w_prev|| + OUTWARD_SCALE_FACTOR * (||w||-||w_prev||)`,

if `||w||>||W_prev||`. otherwise `OUTWARD_SCALE_FACTOR` is replaced by `INWARD_SCALE_FACTOR`.
The procedure is related to [AdamP](https://arxiv.org/abs/2006.08217) (roughly `OUTWARD_SCALE_FACTOR = 0`) and [hyperball](https://arxiv.org/abs/2010.02916), roughly (`OUTWARD_SCALE_FACTOR = INWARD_SCALE_FACTOR = 0`).


The following drawing shows a definition which agrees to first order.

![Radial brake dampens the radial component of the update vector. Not shown: 2nd order correction.](assets/radial_brake_drawing.jpg)


## Experimental Setting

The plots below are based on [PrimeIntellect's](https://www.primeintellect.ai/) 2930-step autoresearch record [PR300](https://github.com/KellerJordan/modded-nanogpt/pull/300) from the [modded nanogpt track 3](https://github.com/KellerJordan/modded-nanogpt/tree/master/records/track_3_optimization). This record inherits from my 2990-step record PR294 where I introduced the radial brake. The implementation in these experiments differs slightly but agrees to first order and should not give materially different results.

## Weight Norms

![Line plot of weight Frobenius RMS q50 over training steps for no brake no weight decay, no brake with weight decay 0.025, and brake with no weight decay.](assets/frob2d_rms_q50.png)

Median per-tensor dimension-normalized Frobenius RMS. 

## Condition Proxy

![Line plot of estimated condition proxy q100 over training steps for the three radial brake comparison runs.](assets/s4_condition_proxy_q100.png)

Maximum estimated condition number proxy based on the logged Schatten-4-style statistic (Schatten 4-norm/2-norm).

## Late Validation Loss

![Line plot of validation loss from step 2500 for no brake no weight decay, no brake with weight decay 0.025, and brake with no weight decay.](assets/late_val_from_2500.png)

Validation loss from step 2500 onward.
