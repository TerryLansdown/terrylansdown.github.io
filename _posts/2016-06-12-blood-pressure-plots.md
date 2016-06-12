---
layout: page
subheadline: Graphing Blood Pressure with Pythonista
title: Pythonista Pressure
date: 2016-06-09T12:28
author: Terry Lansdown
tags:
- python
- iOS

teaser: I was looking for an easy way to graph Blood Pressure on iOS and…

header: yes
permalink:
---

Long story short, but I've noticed my blood pressure has been increasing in recent years, ho-hum. As a long time caffeine addict, who probably drinks a little too much and enjoys salty food, I have only myself to blame. Setting aside any work pressures that might be adding to that. So, in an effort to deal with it before medication is required. I've started to try to modify diet and track any progress. A bit of reading and the Quack suggests change can be relatively swift.

How could I add and visualise data when on the move? Maybe I could do something in [Pythonista][1], I seem to remember being interested in a post by [Dr Drang][2] graphing Apple sales data using Python. I started to track my Blood Pressure and build up a bit of data. Here it is:

![Moving Average Blood Pressure  Graph][3]

If you're interested, I'm calling the script from [Drafts][4] on iOS, using a string with 'known' values; 1-3 for Systolic, 4-6, Diastolic, and 7-8 for Pulse, e.g., 1408060. No error handling as I'm in charge of the data. Here's the code:

Setting things up.

```
# coding: utf-8
# To call script from Drafts, use the following URL as URL Action:
# pythonista://bp/add-blood-pressure-data?action=run&argv=[[Draft]]

import matplotlib.pyplot as myPlot
import numpy
import sys
import console
import ast
import pickle
console.clear()

global systolic, diastolic, pulse
systolic = () # data[0]
diastolic = () # data[1]
pulse = () # data[2]
data = [systolic,diastolic,pulse]
```
Reading previous data.

```
file=open('blood-pressure-data.pkl','rb')
data = pickle.load(file)
file.close()
```
Getting the new values and writing them to a data file.

```
newData = (sys.argv[1])
newSys = (0)
newDia = (0)
newPulse = (0)	
newSys = '(' + newData[:3] +',)'
newDia = '(' + newData[3:5] + ',)'
newPulse = '(' + newData[5:] + ',)'
newSys = ast.literal_eval(newSys)
newDia = ast.literal_eval(newDia)
newPulse = ast.literal_eval(newPulse)
data[0] = data[0] + newSys
data[1] = data[1] + newDia
data[2] = data[2] + newPulse

file=open('blood-pressure-data.pkl','wb')
pickle.dump(data, file)
file.close()
```

Graphing them, the plot shows both smoothed average values and the raw data (faded out slightly).

```
N = 5 # size of moving average
systolic = data[0]
diastolic =  data[1]
pulse =  data[2]

# Create smooth moving averages
weights = numpy.ones(N) / N
sys_sma = numpy.convolve(weights, systolic) [N-1:-N+1]
dia_sma = numpy.convolve(weights, diastolic) [N-1:-N+1]
pulse_sma = numpy.convolve(weights, pulse) [N-1:-N+1]
t = numpy.arange(N - 1, len(systolic))

# The Raw Values
myPlot.plot(systolic[N-1:], alpha = 0.1)
myPlot.plot(diastolic[N-1:], alpha = 0.1)
myPlot.plot(pulse[N-1:], alpha = 0.1)

systolic_label = myPlot.plot(sys_sma, label = "Systolic")
diastolic_label =  myPlot.plot(dia_sma, label = "Diastolic")
pulse_label = myPlot.plot(pulse_sma, label = "Pulse")

myPlot.title('Blood Pressure\n')
myPlot.legend(loc=3)
myPlot.show()
```

You can view (or download) the whole script [here][5].

[1]: http://omz-software.com/pythonista/
[2]: http://leancrew.com/all-this/2015/04/moving-averages-and-the-ipad
[3]: https://dl.dropboxusercontent.com/u/317465/assets/2016/bp.jpg
[4]: http://agiletortoise.com/drafts/index.html
[5]: https://dl.dropboxusercontent.com/u/317465/assets/2016/add-blood-pressure-data.py