---
layout:     post
title:      Bathroom Doors Sensor
date:       2015-02-23 16:45:00
summary:    At work, there are only two bathrooms for the approximately 40 people in the building. It seems as though every time someone gets up from their desk and walks over to a bathroom it is in use. The solution was simple, build a door monitoring system that integrates with our chat application.
categories: raspberrypi node express
---

### The Problem

The idea came up in a conversation during which we were complaining about the bathrooms often being in use when we leave our desks to use them. I believe it was Dan that said there should be some sort of app that we could use from our desks to see if the bathrooms are vacant or not. We sat on the idea for a few months until we had more people join the office, therefore making bathroom situation even worse. It was at this point I decided this app needed to be made.

**Note**: Before you start calling us lazy, I should tell you we are in fact not lazy, but rather, efficient with our time. We don't want to waste our time walking back and forth to a bathroom when we could be working towards making this world a better place! Or you know, flying our [quadcopters](https://instagram.com/p/yNrHRwIGcu/).

### The Solution

The hardware is pretty straightforward. I stopped by the dollar store on the way home one day and picked up a couple door/window magnet sensors. The sensors simply beep if a door/window is opened. The beeping had no use to me, but the magnetic reed switch was perfect. I took apart the sensors and soldered some connectors onto the switches.

![Sensor Wiring](/images/sensor-wiring.jpg)

In order to get the status of the bathrooms onto the web so that it would be accessable by an app. I would usually use an Arduino with an Ethernet shield but for this project, I needed the solution to be wireless, and to work with NodeJS so I opted to use my RaspberryPi. I was able to wire up the reed switches to the I/O pins on the Pi in order to read when the magnets were close or not (indicating when the door is open or closed). I set up Node on the Pi and was able to find a nice [module](https://github.com/rakeshpai/pi-gpio) that interfaced with the I/O pins. I had to do quite a bit of digging around to find a module that I liked. The Pi uses strange pin numbering (2 versions actually) which made it difficult to use these modules. The one I linked to above (pi-gpio) does a nice job of hiding this, and simply uses the physical pin numbers on the device.

![Sensor Prototyping](/images/sensor-prototyping.jpg)

Rather than having to do some magic with the firewall to allow our app to interface with the Pi, I made it so that the Pi is a client and just udpates a server when it senses a change. The app then checks the server when someone requests the status of the doors.

![RaspberryPi](/images/pi.jpg)
![Sensor](/images/sensor.jpg)

Once all this was wired up and ready to go, we needed to have some way for the user to interact with it. This is where [Slack](https://slack.com) comes in. I have been using Slack since I started at my current job. I was introduced to it from day one and I really liked it. My boss, Jonathan, recently made a Slack team for the Kamloops Innovation Centre(KIC) so the whole office space can communicate in one convenient spot (Instead of having annoying conversations on our office mailing list). The acceptance of Slack around the office was mostly positive, but some people were reluctant to give it a real chance because they saw no reason to use it. Jonathan and I decided we would give them a reason by integrating the door sensors into the chat.

Jonathan wrote a script that works with [Hubot](https://hubot.github.com) and we have access to the bot in our KIC Slack chat. All we have to do is either tag `@kicbot` or message the bot directly with something along the lines of `Bathrooms?` and the bot will check the server to see what the status of the bathroom is, and then informs us if we can use it.

![Kicbot](/images/slack-bathrooms.png)

This was implemented just before I left the country for 4 months. Currently I have no use for it, but that's okay, I didn't need persuasion to use Slack.
