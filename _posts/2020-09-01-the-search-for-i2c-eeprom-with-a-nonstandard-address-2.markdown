---
layout: post
title: "Finding Fire TV Serial Number over Bluetooth Low Energy"
date: "2020-09-08 23:20:00 -0500"
img: /img/20200908-Fire/FireError.jpeg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [bluetooth, ble]
---

A few months ago I ordered a Fire TV Stick 4K, and ended up receiving 2 by mistake. I did contact customer support to offer to send it back but Mohammad at Amazon said I can just keep it as a gesture of good faith, due to "the circumstances" (this was early on in the COVID pandemic). Lucky me, right? Well....

I kind of forgot about the second stick for a few days but once I got it hooked up to an extra TV I was met with this error screen:
![](/assets/img/20200908-Fire/FireError.jpeg){:class="img-responsive"}
*Image courtesy Amazon forum*

Okay, that’s weird. I did the usual unplug, wait, and re-plug to no avail. I did some minor googling and Amazon’s troubleshooting didn’t work. I gave up. 

A few months later I tried again, found more possible solutions including blocking internet access with my router, changing router DNS settings, using a mobile hotspot and no dice. Gave up. 

This time I just told myself it must be blocked on Amazon’s side. They had to be looking at this device as lost or unaccounted for because of the shipping mixup and blacklisted it. (Spoiler: I was right). I started a chat with Amazon tech support to plead my case. I explained that there was a shipping error and that I have this "bricked" device and they told me they can fix it if I gave them the serial number. 

Fun Fact: there’s no physical serial numbers printed on these devices. The only place they’re found is in the settings menu (unreachable for me) and the box (which I threw away months ago). Another dead end. 

## Bluetooth Low Energy
I’ve been playing with BLE lately on a project where I’m triggering my Canon M50 with an ESP32 dev module. This is done with GATT attributes over BLE. In my experimentation I’ve been using an iOS app called [BLE Scanner](https://apps.apple.com/us/app/ble-scanner-4-0/id1221763603) to look at devices and their GATT attributes. I had a hunch that the Fire stick uses BLE to sync and communicate with the remote and wouldn’t you know it, it popped up on the scanner. 
![](/assets/img/20200908-Fire/FireSerial.png){:class="img-responsive"}

In this app, you can select a BLE device and analyze its attributes. Even write to attributes if the device allows. We’re interested in the ::Device Information:: service which has UUID `0x180A`. This is a common service ID that you’ll find on almost every BLE device. For example, on my Apple Watch the Device Information service shows the Model Name and Manufacturer Name, and the app shows the values for those attributes in both HEX and ASCII formats. 
![](/assets/img/20200908-Fire/AWSerial.png){:class="img-responsive"}

## Fire TV BLE Attributes and Serial Number
If you connect to a Fire TV with the GATT analyzer, you’ll see that Amazon shares a few more details than Apple. You can find hardware revision, firmware version, and of course, the serial number. ![](/assets/img/20200908-Fire/FireSerialHex.jpeg){:class="img-responsive"}

The serial number is way longer than an Amazon serial number, and that’s because it needs to be converted from HEX to ASCII. (For some reason this one didn’t show the ASCII translation like the Apple Watch page did). 

## Back to Amazon, Un-brick the Fire TV
Once more I chatted with Amazon support, this time with serial number in hand. A very nice agent was able to confirm that the device had never been registered through a legitimate purchase and therefore was locked. He did what was necessary to reset it and immediately the update completed as needed!