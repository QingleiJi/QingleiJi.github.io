---
title: What's the minimum sampling time for a Simulink-RaspberryPi system?
description: To monitor and control the running of a Raspberry Pi with MATLAB Simulink module, it is important to set the right sampling time. How should the value be chosen?
date: 2020-06-06
layout: post
---

Simulink offers a very easy-to-use platform to build your systems and then download-run them in external devices. At the same time, Simulink can act as a user interface for observing and controlling in real time the executed system. In most cases, one would expect the system to not only run in real time, but also owns small enough sampling times so that the external signals can be recorded with more details to avoid the [Aliasing](https://en.wikipedia.org/wiki/Aliasing) problem and in some cases the control commands can all be used to regulate the system in time.

Due to the hardware limit, which can be the processer ability or the communication environment, the minimum sampling time is limited to some value. Thus it is vital to get conscious of this value for different devices so that the sampling time can be set correctly. In our prior research, we have been using  *Arduino Mega 2560* as the external device. According to our experience, a minimum sampling time can be set as $$T_s = 0.05\ s$$. In this case, the system to be downloaded to the board can be large enough (under the board memory limit) and the communication is realized through *USB* cable.

As a further test, here, a raspberry pi 4 with 4GB memory is used as the external device. A very simple model as shown in **Figure 1** is running on it while communicating with Simulink running on a PC.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="https://dskqfq.db.files.1drv.com/y4m45zUcTAFmjnpOVdmq25RxY1IZaYgn6l6d_mvZDkKvnHptnmq8p-TVjW46QhQ1Fbr4C0aOpFmRHGPp8cHOEthsA92rRHKXj54ZOpx3gZx9LBNqOX_L3_2-OPXJcoEcd5SOZwfAyKhEwBPwx9djnhSN1PtG5shqy7p2fBYS1Upjwjf18d4-w_7i_64gf_HSQFOaG2QfGMew7ZV2wuF7zVzSA?width=1024&height=540&cropmode=none">
    </div>
</div>
<div class="caption">
    Simulink model used to test the system.
</div>

### Wireless communication *via* router

The Raspberry Pi is firstly linked to the PC with a [Tenda AC10 Router](https://www.tendacn.com/en/product/ac10.html) wirelessly. Different sampling times are used. For each case, the real time passed (noted as the real time cost) is recorded using an independent timer when the system running time is displaying $$10\ s$$. The data are shown in **Table 1**. It is obvious that for $$T_s\leq 0.004\ s$$, which is $$4\ ms$$, there appears a noticeable difference between the system time and the real time, meaning the overrun of the system.

<center><b>Table 1:</b> Time cost for different sampling times through wireless communication.</center>

| Sampling time $$T_s$$ | System running time | Real time cost |
| :-------------------: | :-----------------: | :------------: |
|        0.05 s         |        10 s         |     9.98 s     |
|        0.02 s         |        10 s         |    10.23 s     |
|        0.01 s         |        10 s         |     9.4 s      |
|        0.008 s        |        10 s         |     9.86 s     |
|        0.005 s        |        10 s         |    10.09 s     |
|        0.004 s        |        10 s         |    12.62 s     |
|        0.003 s        |        10 s         |    16.57 s     |
|        0.002 s        |        10 s         |    25.35 s     |
|        0.001 s        |        10 s         |    50.40 s     |

### Wired communication *via* ethernet

To test if it is the communication speed that limits the minimum sampling time, ethernet cable that directly connect the PC and the raspberry pi 4 is used in this case. Similar tests are performed and the data are shown in **Table 2**. Still, the time difference occurs when the sampling time reduces less than $$40\ ms$$. 

<center><b>Table 2:</b> Time cost for different sampling times through wired communication.</center>

| Sampling time $$T_s$$ | System running time | Real time cost |
| :-------------------: | :-----------------: | :------------: |
|        0.05 s         |        10 s         |    10.19 s     |
|        0.02 s         |        10 s         |    10.14 s     |
|        0.01 s         |        10 s         |     9.91 s     |
|        0.008 s        |        10 s         |    10.07 s     |
|        0.005 s        |        10 s         |    10.00 s     |
|        0.004 s        |        10 s         |    12.16 s     |
|        0.003 s        |        10 s         |    16.45 s     |
|        0.002 s        |        10 s         |    24.50 s     |
|        0.001 s        |        10 s         |    49.85 s     |

### Discussion

**Figure 2** plots the data from **Table 1** and **Table 2** in the same figure. Notice that there is difference between wireless and wired communication is almost ignorable. Thus it is reasonable to conclude that the sampling time limit is caused by the computation capability of the microprocessor rather than the communication method. Furthermore, if compared with minimum sampling time of the Arduino board, the one of Raspberry Pi is much smaller, which should probably caused by the tremendous computation improvement of the Cortex-A72 SoC.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="https://dskpfq.db.files.1drv.com/y4moN8DfN4xCoFpcQ4RAazteKR03-GUqKo7MvTg0RcpJkKwDIfFHko2K39DLw-EalbLP1cMVQos9fXhXFbnpVLcZ50WIA3y1oNe6IRCEFvbKDLU9w7yPwA9JG_hIO5s5P7gmDWACNpBr7eXsVNsS2mZ3MS1BKn9uLgZ8kaBzysmrPNU0FSxVNlIjwR-gAVOQ9mvmQ3254675-xid_GyFYuHww?width=1024&height=685&cropmode=none">
    </div>
</div>
<div class="caption">
    Real time cost versus sampling time for different systems.
</div>

### Reference Links

[Overruns on Arduino](https://se.mathworks.com/help/supportpkg/arduino/ug/detect-and-fix-task-overruns-on-arduino-hardware.html)

[Overruns on Raspberry pi](https://se.mathworks.com/help/supportpkg/raspberrypi/ug/detect-and-fix-overruns-on-raspberry_pi-hardware.html)



### Appendix code (Python)

{% highlight python linenos %}
import numpy as np
import matplotlib.pyplot as plt

Sampling_time = [0.05,0.02,0.01,0.008,0.005,0.004,0.003,0.002,0.001]
Real_time_cost_wireless = [9.98,10.23,9.4,9.86,10.09,12.62,16.57,25.35,50.40]
Real_time_cost_wired = [10.19,10.14,9.91,10.07,10.00,12.16,16.45,24.50,49.85]

ax = plt.subplot(111)
ax.plot(Sampling_time,Real_time_cost_wireless, label = 'wireless communication')
ax.plot(Sampling_time,Real_time_cost_wired, label = 'wired communication')
#plt.yscale('log')

plt.xlabel('Sampling time (s)')
plt.ylabel('time (s)')
ax.legend(frameon=False)
ax.set_xlim([0,0.052])
ax.set_ylim([8,30])
# Hide the right and top spines
ax.spines['right'].set_visible(False)
ax.spines['top'].set_visible(False)

ax.yaxis.set_ticks_position('left')
ax.xaxis.set_ticks_position('bottom')
plt.savefig('time_cost_comparision.png', format='png', dpi=300)
plt.show()
{% endhighlight %}

