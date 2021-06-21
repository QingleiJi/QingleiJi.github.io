---
layout: page
title: Closed loop controlled 4D printing
description: Increase the precision of 4D printing using closed loop control methods.
img: /assets/pdf/IEEE_TIE_CL4DP/CL4DP.jpg
importance: 1
# category: work
---

The project is funded by the Swedish Research Council (Vetenskapsrådet) with the title '[Closed-Loop 4D Printing with High Precision](https://www.iip.kth.se/sps/current-projects/4d-printing-1.917040){:target="https://www.iip.kth.se/sps/current-projects/4d-printing-1.917040"}'.

Conventional 4D printed actuation is normally performed in open loop, which can lead to uncontrolled and low precise movements. The core methodology of this project is to apply different closed loop control methods to increase the precision of 4D printed actuation.

In the project, we take 4D printed thermal Shape Memory Polymer (SMP), which is a very commonly used thermal stimulated material for 4D printing, to perform the closed loop control studies <d-cite key="ji2020feedback"></d-cite>. The printed shape of SMP is the permanent shape. It can be deformed with external force to another temporary shape when stimulated by temperature above its glass transition temperature $$T_g$$. This temporary shape can be maintained when the temperature is decreased to lower temperature. What's magic about SMP is that it can recover from the temporary shape to the permanent shape when it is heated again above $$T_g$$.

One limitation of SMP introduced actuation is that it only have two states: the permanent shape and the temporary shape. We find that the recovery speed of SMP is influenced by the stimulus temperature. By applying closed loop control over the 4D printed shape, we can then tune the stimulus temperature during the recovery process so that the recovery process of SMP is controlled. We can also terminate the recovery process before the SMP recovers to its permanent shape. As a result we can achieve any arbitrary intermedia shape instead of the two states aforementioned.