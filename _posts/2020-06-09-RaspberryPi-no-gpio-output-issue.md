---
title: "Raspberry Pi 4B no GPIO output in C program: Solved!"
description: I encountered the problem that the new Raspberry Pi can't output normally when programming with C. The issue and effective solution are not found together anywhere else. Now it's here.
date: 2020-06-09
layout: post
---

I am using the latest Raspberry Pi 4B board and programming using *C*. It is found that when using the [WiringPi](http://wiringpi.com/) library to control the GPIO output (digital/pwm), there is no actual voltage output while the program can just compile and run. On the other hand, the GPIO works normally if program with *Python* using the [RPi.GPIO](https://pypi.org/project/RPi.GPIO/) module.

The problem can be solved by updating the *WiringPi* to 2.52. You can not automatically get this version using the *apt-get* command as follows as it is not an official release yet.

{% highlight csharp linenos %}
sudo apt-get install wiringpi
{% endhighlight %}

To update to 2.52, do the following:

{% highlight csharp linenos %}
cd /tmp
wget https://project-downloads.drogon.net/wiringpi-latest.deb
sudo dpkg -i wiringpi-latest.deb
{% endhighlight %}

Then, have fun with your [blink.c](http://wiringpi.com/examples/blink/)!
{% highlight c linenos %}
#include <wiringPi.h>
int main (void)
{
  wiringPiSetup () ;
  pinMode (0, OUTPUT) ;
  for (;;)
  {
    digitalWrite (0, HIGH) ; delay (500) ;
    digitalWrite (0,  LOW) ; delay (500) ;
  }
  return 0 ;
}
{% endhighlight %}

This post will be useless when *WiringPi* 2.52 or later versions become an official release.

[Reference page](http://wiringpi.com/wiringpi-updated-to-2-52-for-the-raspberry-pi-4b/) for updating *WiringPi*.

[Raspberry Pi Pinout](https://pinout.xyz/pinout/wiringpi) in different schemes.

