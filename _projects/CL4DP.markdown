---
layout: page
title: Closed loop controlled 4D printing
description: Increase the precision of 4D printing using closed loop control methods.
img: /assets/pdf/IEEE_TIE_CL4DP/CL4DP.jpg
importance: 1
# category: work
---

## Background
The project is funded by the Swedish Research Council (Vetenskapsrådet) with the title '[Closed-Loop 4D Printing with High Precision](https://www.iip.kth.se/sps/current-projects/4d-printing-1.917040){:target="https://www.iip.kth.se/sps/current-projects/4d-printing-1.917040"}'.

Conventional 4D printed actuation is normally performed in open loop, which can lead to uncontrolled and low precise movements. The core methodology of this project is to apply different closed loop control methods to increase the precision of 4D printed actuation.

## Methods
In the project, we take 4D printed thermal Shape Memory Polymer (SMP), which is a very commonly used thermal stimulated material for 4D printing, to perform the closed loop control studies [[1]](#1). The printed shape of SMP is the permanent shape. It can be deformed with external force to another temporary shape when stimulated by temperature above its glass transition temperature $$T_g$$. This temporary shape can be maintained when the temperature is decreased to lower temperature. What's magic about SMP is that it can recover from the temporary shape to the permanent shape when it is heated again above $$T_g$$.

One limitation of SMP introduced actuation is that it only have two states: the permanent shape and the temporary shape. We find that the recovery speed of SMP is influenced by the stimulus temperature. By applying closed loop control over the 4D printed shape, we can then tune the stimulus temperature during the recovery process so that the recovery process of SMP is controlled. We can also terminate the recovery process before the SMP recovers to its permanent shape. As a result we can achieve any arbitrary intermedia shape instead of the two states aforementioned.

## Results
To solve the above closed loop control problem, we started with building a data-driven model for the recovery process of SMP [[1]](#1). Based on the model, a simple Proportional–Integral (PI) controller is developed and implemented. Good control results are achieved which proves the feasibility of the method. Then, an optimal controller developed using Q-learning method is also applied and outperforms the PI controller in terms of time efficiency.

## Conclusions
With model based controllers, robust and fast shape control of SMP can be realized. However, it should be noted that the model is not always accurate and the control environment is not always perfect. In many 4D printing studies the acquisitions of the actuation model are hard. Thus we start to think if we can perform model-free control of the SMP by learning the control policy directly from the experimental knowledge. This is the next step we are working on.


## References
<a id="1">[1]</a> 
Q. Ji et al., "[Feedback control for the precise shape morphing of 4D Printed Shape Memory Polymer](https://ieeexplore.ieee.org/abstract/document/9280377){:target="https://ieeexplore.ieee.org/abstract/document/9280377"}," in IEEE Transactions on Industrial Electronics.