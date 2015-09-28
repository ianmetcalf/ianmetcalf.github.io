---
layout: post
title: Talking to hardware with Node.js
---

As part of a project to gather heating data on my house and better control
when the boiler and pumps operate I needed a way to talk to hardware.
Specifically Dallas Onewire temperature sensors and IO modules. I had worked
with these devices before using an Atmel AVR microcontroller and drivers I
had written in C. This time though I wanted to leverage I higher level
language so I decided to try using Node.js on a Rasperry PI.

The Onewire network consists of a single master device and multiple slave
devices all connected to the same bus. Each slave has a unique rom assigned
by the manufacturer. The master initiates communication and data is passed
back and forth by holding the bus high or low during specific time windows.
The need for exact timing means that the protocol requires low level support.

I have found that the easiest way to handle this is with a dedicated chip
that takes care of the timing issues and communicates with a Raspberry PI
over the much more common I2C protocol. Luckily someone has written a
[i2c module](https:www.npmjs.com/package/i2c) with native bindings that work
on the Raspberry PI.

The first step was to create a [base module](https://www.npmjs.org/package/ds2482)
that exposes an api for sending onewire commands via the bridge chip. Then
to create a [temperature module](https://www.npmjs.org/package/ds2482-temperature)
and [IO module](https://www.npmjs.org/package/ds2482-io) that handle the
specific funtionallity of the corresponding slave devices.

With the drivers complete and working I am able to poll the temperature sensors
and control the IO pins. The next step will be to create an application that
uses the data to better control the boiler and pumps.
