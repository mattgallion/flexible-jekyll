---
layout: post
title: "Searching for I<sup>2</sup>C EEPROM with a Non-Standard Address"
date: "2020-09-01 17:22:00 -0500"
img: EEPROM.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [PCB, parts, embedded]
---
#### Update: Solution Found! See Solution 1.

## The Problem
A current project of mine requires an I<sup>2</sup>C EEPROM for device fingerprinting/identification, and possibly some data storage later on. The same board also has 8 separate TI TPL0102 digital pots. Those TPL0102 chips use their A0-A2 pins which define the last 3 bits of the address, allowing you to use up to 8 on one bus. Unfortunately the first 4 bits are set to `1010` from the factory... the same first 4 bits used on just about every EEPROM chip on the market.

The TPL0102s are here to stay, so I need to figure out some way to get EEPROM on the board while keeping it simple and cost-effective.

## Potential Solutions
### 1. Find an EEPROM chip with a Non-Standard address
This would be the ideal solution. The NXP PCF85103C starts with address `0010` and would be a good candidate, but it's now obsolete. Searching for alternatives has proven difficult, as it requires digging through DigiKey and searching datasheets one by one to find the address. Some sites have tried to compile I<sup>2</sup>C parts databases but all seem pretty slim, and none include any EEPROM (see: [Adafruit's "The List"](https://learn.adafruit.com/i2c-addresses/the-list), [i2cdevices.org](https://i2cdevices.org/addresses))
#### 🔥 UPDATE: New Chip Found! 🔥
The NXP PCA9501 is just like the PCA9500, but it provides the user with __6__ address pins. This allows __64__ of these IO Expanders (2<sup>6</sup> = 64) to be on a single I<sup>2</sup>C bus.

![PCA9501 Address Configuration](/assets/img/20200901-EEPROM/PCA9501_Address.png){:class="img-responsive"}
### 2. Use an I<sup>2</sup>C address translator.
Not really attractive because of the added cost, but if it's done right there's no extra code required to control it. The LTC4316 is a good candidate for this option.
### 3. I<sup>2</sup>C Bridge
Worse than option 2 but still technically possible. The main board controlling this board already uses I<sup>2</sup>C Bridges to select between daughterboards, so the code already exists. However, daisy chaining these bridges has proven painful in the past and should be avoided.
