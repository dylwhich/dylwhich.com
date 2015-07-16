---
layout: post
title:  "Home Automation Rundown"
date:   2015-07-15 23:44:50
categories: idiotic homeautomation
---

# Stuff That's Automated #

The house is becoming more and more automated each day. Yesterday, I
threw together some terrible bash to control our air conditioners and
shoved them into [idiotic] with the eventual goal of... well,
automating them. Here's a quick list of some stuff in the house that's
currently automated and integrated with idiotic:

* All the lights, with X10 switches and lamp modules
* The garage door
* The projector screen in the garage
* The furnace
* Most of the air conditioners
* Motion sensors in all the common areas
* DHT22 Temperature/Humiditiy sensors
* An illuminance sensor

Currently, idiotic doesn't have support for multiple instances. It's
mostly done though and it's next on the todo-list after a __ton__ of
cleanup. So currently all of the remote sensors and outputs are being
reported to and controlled by idiotic through our
[openhalper] script on a bunch of raspberry pis. As soon
as distribution is done, openhalper will be retired and the last
remnant of [OpenHAB] will be gone forever. All of the smarts,
however, are controlled by idiotic.

## Motion Sensors ##

They're probably the most useful and convenient part of the house (or
at least would be if we were less prone to sitting still in the dark
for long periods of time). They're hooked up to the lights in the
kitchen, living room, laundry room, and hallway. They turn on when
somebody walks by and turn off after 5 minutes (2 minutes for the tiny
hallway) if nobody has gone by since. They also disable themselves if
it's daytime, as determined by approximate sunset time and readings
from the illuminance sensor (to make sure the lights stay on during
overcast days, storms, or solar eclipses).

## Door Sensors ##

Actually this one is the most useful, or at least the most used. We
have a sliding bathroom door and put a simple magnetic sensor on it,
and added a rule to turn the light on when it closes. There are two
magnets, one at the end and one at the edge of the frame, so it tends
to trigger halfway through closing which makes up for the slight delay
in turning the light on. It's pretty nice.

## Air Conditioners ##

I didn't get around to reverse engineering the stupid closed API until
last night, so there aren't any rules for them yet. They'll probably
get hooked up to the motion, temperature, and humidity sensors, and
(when it's set up) presence detection. I plan on replacing my crappy
bash script and python wrapper with an actual module that does
intelligent things with requests instead of subprocesses.

For the actual controllers, we're using [thinkeco modlets]
that BG&amp;E sent us __for free__ (in exchange for letting them have
control during peak hours or whatever sometimes). They have their own
thermostat and phone home to the server over WiFi. They're also
[ZigBee] devices that come with ZigBee remotes, but we don't have
a ZigBee hub

## Furnace ##

The furnace is a relay hooked up to a raspberry pi. It will be
controlled by the temperature sensors, once the rules get
written. Which will probably not happen until the first cold Winter
day when we realize that the furnace isn't doing anything.


## The Garage ##

The garage is fun because it has lots of moving things. There's the
motorized projector screen, which can be raised and lowered as one
might expect.  The garage door can also be controlled and monitored
using the existing closed-ness sensor on the opener. We use this for a
'close' button that makes sure the door is closed no matter what state
it's in beforehand.

For movie-watching, there's a 'scene' that can be activated in order
to close the garage door, lower the projector screen, and turn the
lights out. The eventual goal is to hook this up to [CEC] and
<strike>XBMC</strike> [Kodi] so that it automatically does this when the
projector turns on, and undo it all when it turns off.

There's also a similar scene set up in the living room, which just
disables the lights.

## Lights ##

The lights are controlled mostly with [X10 WS13A switches][switches]
and various old appliance and lamp modules. The controller is the
now-discontinued TW523 module and [an arduino][x10-arduino] and a
[raspberry pi][x10-arduino-controller]. Currently idiotic controls
this over the HTTP API, but it will be absorbed by idiotic.

# The State of idiotic #

Idiotic is pretty close to being in what I would consider to be
alpha. It's missing some final distribution infrastructure and has a
really crappy web interface, and there is no persistence method
implemented yet (most of the internals are there though). I'm
currently on a cleanup run, partially because I think a lot of it is
gross and partially because it will reduce the amount of changes that
the public API will have later.

Once that cleanup is done, the most important goal is distribution,
then persistence. At that point all the fundamentals are there, and
most of the major development will be done as modules. It's getting
there!!! Ahhhhhhhhh

[idiotic]: https://github.com/umbc-hackafe/idiotic
[openhalper]: https://github.com/umbc-hackafe/openhalper
[openhab]: https://github.com/openhab/openhab
[modlets]: http://shop.thinkecoinc.com/products/wifi-smartac-kit#.VaalTnqUzGc
[zigbee]: https://www.zigbee.org
[cec]: https://en.wikipedia.org/wiki/CEC
[kodi]: https://kodi.tv
[switches]: https://amazon.com/gp/aw/d/B001L7AN4Y
[x10-arduino-controller]: https://github.com/umbc-hackafe/x10-arduino-controller
[x10-arduino]: https://github.com/umbc-hackafe/x10-controller
