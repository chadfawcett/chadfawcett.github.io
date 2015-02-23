---
layout:     post
title:      Bathroom Doors Sensor
date:       2015-02-23 17:15:00
summary:    At work, there are only two bathrooms with approximately 40 people in the building. It seems whenever anyone gets up from their desks and walks over to the bathroom, they are always in use. The solution was simple, build a door monitoring system that integrated with our chat application.
categories: raspberrypi node express
---

### The Problem

The idea came up in conversation while we were complaining about how often the bathrooms are in use when we get up from our desks to use them. I believe it was Dan that said there should be some sort of app that we could use from our desks to see if the doors are open or not. We sat on the idea for a few months until we had a few more people come to the office and the bathroom situation got even worse. It was at this point I decided this app needed to be made.

**Note**: Before you start calling us lazy, I should tell you we are in fact not lazy, but rather just efficient with our time. We don't want to waste our time walking back and forth to the bathrooms when we could be working towards making this world a better place! Or you know, flying our [quadcopters](https://instagram.com/p/yNrHRwIGcu/).

### The Solution

The hardware is pretty straightforward. I stopped by the dollar store on the way home one day and picked up a couple door/window magnet sensors. The sensors simply beep if a door/window is opened. The beeping had no use to me, but the magnetic reed switch was perfect. I took apart the sensors and soldered on some connectors to the reed switch.

![Sensor Wiring](/images/sensor-wiring.jpg)

In order to get the status of the doors onto the web, where it would be accessable by an app, I would normally use an Arduino with an Ethernet shield. For this project, I needed it to be wireless, and I wanted to work with NodeJS so I opted to use my RaspberryPi that has been lying around forever. I was able to wire up the reed switches to some I/O pins on the pi to simply reed when the magnets were close or not (indicating when the door is open or closed). I set up Node on the Pi and was able to find a nice [module](https://github.com/rakeshpai/pi-gpio) that interfaced with the I/O pins. I actually had to do quite a bit of digging around to find a module that I liked. The Pi uses very weird pin numbering (2 versions actually) which made it difficult to use these modules. The one I linked to above (pi-gpio) does a nice job of hiding this, and simply uses the physical pin numbers on the device.

![Sensor Prototyping](/images/sensor-prototyping.jpg)

Rather than having to do some magic with the firewall to allow our app to interface with the Pi, I made it so that the Pi is a client and just udpates to a server when it senses something change. The app then checks the server when someone requests the status of the doors.

![RaspberryPi](/images/pi.jpg)
![Sensor](/images/sensor.jpg)

Once all this was wired up and ready to go, we needed to have some way for the user to interact with it. This is where [Slack](https://slack.com) comes in. I have been using Slack since I started at my current job. I was introduced to it from day one and I really liked it. My boss, Jonathan, had recently made a Slack team for the Kamloops Innovation Centre(KIC) so the whole office space could communicate in one convenient spot (Instead of constantly having annoying conversations on our office mailing list). The acceptance of Slack around the office was mostly possitive, but some people were reluctant to give it a real chance as they didn't see a reason to use it. So Jonathan and I decided we would give them a reason to use Slack at the office by integrating the door sensors into the chat.

Jonathan wrote a script that works with [Hubot](https://hubot.github.com) and we have access to the bot in our KIC Slack chat. All we have to do is either tag `@kicbot` or message the bot directly with something along the lines of `Bathrooms?` and the bot will check the server to see what the status of the doors are, and then inform you if you should go.

![Kicbot](/images/slack-bathrooms.png)

This was implemented just before I left the country for 4 months. So currently I have no use for it, but that's okay because I wasn't the one needing persuading to use Slack.
