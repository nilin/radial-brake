# Radial Brake

A comparison of radial update damping, explicit weight decay, and no radial constraint in the PR300-style training setup.

![Radial brake dampens the radial component of the update vector. Not shown: 2nd order correction.](assets/radial_brake_drawing.jpg)

Radial brake dampens the radial component of the update vector, in my experiments by a factor 0.5. Another correction was applied, normalizing to the original norm + OUTWARD\_SCALE\_FACTOR * OUTWARD\_COMPONENT\_LENGTH, to adjust for outward drift from following tangent directions in a straight line. The procedure is related to [AdamP](https://arxiv.org/abs/2006.08217) (roughly OUTWARD\_SCALE\_FACTOR=0) and [hyperball](https://arxiv.org/abs/2010.02916), roughly (OUTWARD\_SCALE\_FACTOR=INWARD\_SCALE\_FACTOR=0).

## Weight Norms

![Line plot of weight Frobenius RMS q50 over training steps for no brake no weight decay, no brake with weight decay 0.025, and brake with no weight decay.](assets/frob2d_rms_q50.png)

Median per-tensor dimension-normalized Frobenius RMS. 

## Condition Proxy

![Line plot of estimated condition proxy q100 over training steps for the three radial brake comparison runs.](assets/s4_condition_proxy_q100.png)

Maximum estimated condition number proxy based on the logged Schatten-4-style statistic (Schatten 4-norm/2-norm).

## Late Validation Loss

![Line plot of validation loss from step 2500 for no brake no weight decay, no brake with weight decay 0.025, and brake with no weight decay.](assets/late_val_from_2500.png)

Validation loss from step 2500 onward.
