---
layout: post_page
title: Arduino Serial Communication in Linux
---

I was looking for a way to do some basic serial communication with an Arduino and PC.  The requirements that I was looking for was:

* Easy to adapt to multiple projects
* Little to no code preparation
* Should have some kind of debug mode available

I searched and was able to find some c, c++, c# and other libs for this.  I did experiment with some of them, however, somewhere along the line it dawned on me that there is no reason to use a library for the goals I wanted to achieve.  The Linux file system represents devices as files.  With standard input/output and some Linux craft I could achieve all my goals with little or no effort.

I will not get into the details of these mostly basic Linux commands, if there is a command here you don't recognize you can easily Google it and most of the time the top result will give you all the information you need.

My first attempt at this did fail.  Remember that the device files in Linux are only accessible by the root account by default.

I first tried 'sudo echo "hello world" >> /dev/ttyACM0' but this gave me a permission denied error.  Note: ttyACM0 is the name used for my Ardunio.

The problem here is that 'sudo' is only applied to echo and not the stdout redirect '>>'

To solve this problem we have to use a little program called. 'Tee'.  'Tee' basically takes and stdin and puts it out to a file or stdout.  So if we pipe the stdout to 'Tee' and then have 'Tee' write to the file with sudo permissions we should be good to go.

'echo "hello world" | sudo Tee /dev/ttyACM0' does the trick no permission errors.

Now to see what the Ardunio is sending back to me in real time: 'sudo screen /dev/ttyACM0', there ya go in one terminal you can send any data you want to the Ardunio and in the other terminal window you can use screen to see the return values in real-time.

Of course this is a basic use case, but the principals here can be used in a endless number of ways to use all kinds of software and the stdin/stdout to communicate with the Arduino with little to no effort.