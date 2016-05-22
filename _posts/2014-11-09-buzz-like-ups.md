---
layout: post
title:  "Buzz like a UPS"
date:   2014-11-09 16:34:56 +0600
categories: IoT
permalink: /:slug
---
This is yet another simple test of a new hardware component that I haven't played with before. Buzzer is an audio device which may be mechanical, electromechanical, or piezoelectric. It can generate from quite awful UPS-like siren to melodious tone; it really depends on how you want to use it. I just wanted to make noise like that of a UPS does when electricity goes off.

I'm using a Buzzer module which has three legs, also a 9V battery, so that it may operate without getting power from the USB and can become a portable device. Here's the connection list that I have made with an Arduino Nano for this:

| Connection From | To |
| ------------- | ------------- |
| 9V positive wire | Arduino's VIN |
| 9V negative wire | Arduino's ground |
| Buzzer module's VCC | Arduino's 5V |
| Buzzer module's ground | Arduino's ground |
| Buzzer module's I/O (in my case middle leg) | Arduino's Digital Pin 13 |

The code that make the Buzzer beep like a UPS is probably even simpler than blinking a LED:

{% highlight scheme %}
int buzzer =  13;  

void setup()  
{        
    pinMode(buzzer, OUTPUT);     
}

void loop()                     
{
    digitalWrite(buzzer, HIGH);
    delay(1000);
    digitalWrite(buzzer, LOW); 
    delay(1000);        
}
{% endhighlight %}

Here’s the final outcome:

![Buzz like a UPS]({{ site.url }}/assets/buzzlikeaups.jpg)

First published on my [previous blog](https://tanzimsaqib.wordpress.com/2014/11/09/buzz-like-a-ups/).