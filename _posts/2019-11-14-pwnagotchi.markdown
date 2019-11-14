---
layout: post
title:  "pwnagotchi_build-v1.sh"
description: My personal experience on assembling my first Pwnagotchi.
---

Every once in a while I find an interesting hardware project, but I never get around to do it. I decided to change that when I found [Pwnagotchi](https://pwnagotchi.ai), a WiFi handshake sniffing "AI" with the likeness of a Tamagotchi.

![pwnagotchi](/img/pwnagotchi.gif)

Pwnagotchi's purpose is to collect all the WPA information it can to be cracked into keys later, either by passively sniffing for them, or by using deauthentication/association attacks. It also uses a neural network to tweak it's own settings to get better at getting material over time.

I decided on using the Pi 0W for the project as it was the only officially supported Pi as the time of writing this, however anything that can run Linux and has a WiFi card that supports monitor mode can run it with a little tweaking. Personally, I wanted my Pwnagotchi to be portable and be able to display its adorable face, so there are a few extra parts beside the Pi I need: a display, batteries, and a case.

<sup>*Check the [Pwnagotchi site](https://pwnagotchi.ai) to learn more, as it goes much more in-depth in how it works than I can.*</sup>

# Table of Contents
- [Display](#display)
- [Batteries](#batteries)
- [Case](#case)
- [Tweaking](#tweaking)
- [Putting the Pieces Together](#putting-the-pieces-together)
- [V2?](#v2)
- [Afterword](#afterword)



# **Display**
There are many display types out there for the Pi, such as LCD, OLED, and E-Paper. I decided on using an E-Paper display due to having the lowest power draw of the bunch, allowing the Pwnagotchi to last as long as possible on a single charge. E-Paper can only display in black and white (and sometimes a third color), however Pwnagotchi only uses 2 colors currently, so there is no downside to the monochrome display. However, there are multiple E-Paper displays designed specifically for the Pi 0W and supported by Pwnagotchi, so I had to sift through them all. There is really only two major concerns for this part that influenced my decision; refresh rate & resolution.

## [Inky pHAT](https://shop.pimoroni.com/products/inky-phat)
![inky-phat](/img/inky-phat.jpg)
The Inky pHAT is a common suggestion for the Pi, coming in at 212x104 pixels across the 3 variations of the display, black/white, black/white/yellow, and black/white/red. However, it has an abysmal **15 second** refresh across the whole lineup. There are some hacks coming to Pwnagotchi to allow it to refresh quicker, however damage over time and visual anomalies are a risk of running it out-of-spec.

## [PaPiRus Zero](https://www.adafruit.com/product/3335)
![papirus-zero](/img/papirus-zero.jpg)
The lowest resolution offering of the bunch at 200x96 pixels, the PaPiRus Zero [apparently can refresh in half a second](https://github.com/PiSupply/PaPiRus#refresh-rates-and-screen-lifespan), making it the fastest display of the bunch. However, Pwnagotchi doesn't update quite that often to take use of that refresh rate. While good, I would prefer at least the resolution of the Inky pHAT.

## [Waveshare E-Ink Display](https://www.waveshare.com/product/2.13inch-e-paper-hat.htm)
![waveshare-eink](/img/waveshare-eink.jpg)
The Waveshare display is not only the recommended display for the Pwnagotchi project, but also the highest spec'd. It has support for partial refresh, a crisp 250x122 resolution, and a fast 2 second full refresh. Like the Inky pHAT, it has yellow and red variants; However, it is most likely using the exact same panel, since both also have the same 15 second refresh and the lower 212x104 resolution.

## Conclusion
I ended up choosing the non-color Waveshare display due to it having an acceptable refresh rate, the highest resolution, and that it is the only officially supported display for Pwnagotchi.



# **Batteries**
To start my look for hardware, I looked for a battery to be able to use the Pwnagotchi portably. However, it was a hard decision as every battery setup for the Pi has its own ups and downs.

My (admittedly flexible) requirements for the battery solution were:

- Allows easily replacing the battery as it loses capacitance through use
- Is low profile (i.e. no longer or wider than the Pi board or the E-paper HAT, and as thin as possible)
- Can power the Pi directly when plugged in without going through the battery to avoid long-term battery damage
- Exposes power level to the Pi to facilitate a safe shutdown when batteries get low

## [JuiceBox Zero](https://juiceboxzero.com)
![juicebox-zero](/img/juicebox-zero.png)
You would be forgiven for mistaking this for a stripped-down Pi initially, the JuiceBox Zero has the same PCB shape and sits on top of the Pi, requiring you to solder it directly to the GPIO. However, connecting directly to the GPIO is a bit annoying as a riser would have to be used to extend the GPIO pins to the screen, adding a lot of empty space. Even if I could find a thin enough battery to fit in that empty space between the Pi and the JuiceBox, heat would be a major concern with the Pi's CPU directly against the battery. Obviously, below the Pi is the best place for the battery system to go...

- Allows easily replacing the battery as it loses capacitance through use ✔️
- Is low profile (i.e. no longer or wider than the Pi board or the E-paper HAT, and as thin as possible) ❌
- Can power the Pi directly when plugged in without going through the battery to avoid long-term battery damage ✔️
- Exposes power level to the Pi to facilitate a safe shutdown when batteries get low ✔️

## [Adafruit PowerBoost 1000](https://www.adafruit.com/product/2465)
![powerboost-1000](/img/powerboost-1000.jpg)
This one was my favorite of the bunch as the board is very small, sits below the Pi, uses removable batteries, and has a low power pin. I even found [a guide](https://github.com/NeonHorizon/lipopi) showing how to wire it to power the Pi. Overall, this seems like it will fit everything I want with a little more effort than most. Then I went to look for batteries....

Finding a Li-Po or Li-Ion battery that fits inside the 65mm x 30mm footprint of the Pi is incredibly hard. The largest battery I could find fitting that footprint is [this on Aliexpress](https://www.aliexpress.com/item/33004531195.html), and while 1000mAh can power my Pi for 3 or so hours, there is an offering that provides more: the PiSugar PowerPack.

- Allows easily replacing the battery as it loses capacitance through use ✔️
- Is low profile (i.e. no longer or wider than the Pi board or the E-paper HAT, and as thin as possible) ✔️
- Can power the Pi directly when plugged in without going through the battery to avoid long-term battery damage ✔️
- Exposes power level to the Pi to facilitate a safe shutdown when batteries get low ✔️

## [PiSugar PowerPack](https://hackaday.io/project/164733-pisugar-battery-for-raspberry-pi-zero)
![pisugar-powerpack](/img/pisugar-powerpack.jpg)
The PiSugar seems to be the most popular battery solution for the Pi Zero, no doubt due to being solderless and not taking up any space above the board, instead opting to attach to the bottom of the board and using springy pins to contact test pads for the USB rail. It comes in both a 1.2A 900mAh version and a 2.4A 1200mAh version (albeit soldered to the board), giving pretty good energy storage for the size. Also, PiSugar provides [3D printable cases on Github](https://github.com/PiSugar/PiSugar), and even provides [additional caps for Pi HATS](https://github.com/PiSugar/pisugar-case-pihat-cap), such as the Waveshare display I chose.

However, the PiSugar always routes power through the battery, meaning that you are using the battery and putting strain on it even if you have the PiSugar plugged in. You can work around this by turning off the PiSugar and plugging into the power port on the Pi, but then you have to wait for the AI to load again, and the power button just stops providing power instead of safely powering down the Pi. Additionally, the Pi will lose power when the charger is unplugged, meaning that once again, you will have to wait for the AI to load.

- Allows easily replacing the battery as it loses capacitance through use ❌
- Is low profile (i.e. no longer or wider than the Pi board or the E-paper HAT, and as thin as possible) ✔️
- Can power the Pi directly when plugged in without going through the battery to avoid long-term battery damage ❌
- Exposes power level to the Pi to facilitate a safe shutdown when batteries get low ❌

## Conclusion
The decision here is really only between the PowerBoost and the PiSugar, as the PowerBoost does everything better than the JuiceBox Zero. I **barely** decided on using the PiSugar due to a lack of a comparable battery for the PowerPack and the ease of use of the PiSugar, even though it is worse on paper.

An honorable mention to the [Pi 0 UPS-Lite V1.1](https://www.aliexpress.com/item/33031469632.html), which is very similar to the PiSugar but only has 1000mAh capacity.



# **Case**
Obviously we need to put a case around our parts to keep them from getting damaged ~~and so it doesn't look like a bomb~~. Luckily, PiSugar provides a case housing a Pi 0W, the PiSugar (obviously), and additional faceplates for HATS such as the Waveshare display I am using. However, I did rebuild the STL's in Solidworks to get a higher poly count.

Now I don't have a 3D printer yet (will be fixing that soon, though), so I had to find a company to print my parts. Shapeways wanted $40+ for just these parts, which felt a little overpriced when I can just get an Ender 3 for ~$200. I kept looking around and found [Treatstock](https://www.treatstock.com), which cost under $20 for everything shipped, which I found much more reasonable. I chose ABS over PLA due to having a higher heat resistance, and seeing a few melted PLA cases in the Pwnagotchi Slack confirmed my worries. My final order came out looking like this:

![case-order](/img/case-order.png)

And in the same day as I ordered my case, the company I chose printed it and sent my pictures to verify the print quality was to my liking, which I thought was fine.

![case-order-image](/img/case-order-image.jpg)

If you are interested in copying my case 1:1, [here is the seller I got mine printed from](https://www.treatstock.com/c/oksharpei-3d), and [here are my high-res STLs](/files/pisugar-hirez.zip).



# **Tweaking**
Now that I had hardware sorted, I could get to work on eeking out as much battery life as I can. [Here is my script](https://github.com/charagarlnad/miscellaneous/blob/master/pwnagotchi-optimize.sh) I ended up with, if you want to use it yourself. To use it, copy it over to the Pi, `chmod +x` it, then run the script as root. Make sure to read the comments included to see missing features as a result of this script! All the optimizations add up to over half the amount of kernel modules loaded and a decent battery life improvement. I might make a more scientific comparison later, but it's at least an extra hour on my PiSugar.

I tried to benchmark AI startup times, however there was nothing beyond margin of error between a UHS-1, UHS-2, and UHS-3 MicroSD card, so any decent quality >=16GB MicroSD card with good random read/write speed should work fine. I would personally recommend the [Samsung EVO Plus](https://www.amazon.com/dp/B0749KG1JK) or the [Sandisk Extreme PLUS](https://www.amazon.com/dp/B06XYDY93V).

To help dissipate heat, I put a [short heatsink](https://www.adafruit.com/product/3084) on the Pi CPU. This may be doing more harm than good because it is pushing the heat closer to the E-Ink display, however I didn't see any heat-related ghosting in my testing. It is quite close to the back of the screen but it *may* fit even with the slimagotchi mod, though I would recommend to put teflon tape on the back of the screen if you try to.

As for plugins, I'm only using [my own discord plugin](https://github.com/charagarlnad/pwnagotchi-discord-plugin) and the included memtemp plugin.



# **Putting the Pieces Together**
After waiting over a month for parts to arrive (mainly the PiSugar), it was time to actually put it all together. *Of course, always test your parts outside of the case to verify they work before putting them in.*

Once I took a closer look at the PiSugar, I noticed a few small tabs on the PCB that weren't removed at the factory; I broke them off with a pair of pliers and a *small* amount of pressure.

![pisugar-tabs](/img/pisugar-tabs.jpg)

After verifying that all my parts worked, I flashed Pwnagotchi to the SD card using [Rufus](https://rufus.ie), and configured it using the [wonderful guide](https://pwnagotchi.ai/configuration/).

Putting the parts in the case was a struggle, as the tolerances are tight enough I had to sand down a few parts for them to fit together nicely. Start by putting in the power button and making sure it has no resistance, and then adding the PiSugar board to keep it in place. Then insert the plastic screws and put the Pi in the other side of the case, and secure it with the included nuts. The washers included are not required when using the PiSugar with a case, so just put them aside. Pop the faceplates on and you are done!

*Of course, this makes it sounds quite easy. It really isn't. Getting the Pi onto the screws took me a while, and adding the nuts is impossible without tweasers in such a small area. Take your time and be careful, if you feel a lot of resistance you probably need to sand down something in the way.*

Tap the power button, and if everything was put together correctly, the power indicators should light up and the screen should update after about a minute. To turn the PiSugar back off, simply double tap the button again.

Here's what my Pwnagotchi looked like after assembling it:

![turing-complete](/img/turing-complete.jpg)

There was a small gap around the screen, so I sanded the lip of the faceplate down to get it to sit flat:

![turing-side](/img/turing-side.jpg)



# **V2?**
As I was writing this, the amount of parts I found kept expanding. At first I only knew about the PiSugar and the Waveshare display, however as I researched I found more and more parts and decided to compare them to see what was the 'best', and kept wanting to build an improved Pwnagotchi.

There are many changes I'm considering, namely a [slimmer screen](https://pwnagotchi.ai/community/#rpi0w-with-waveshare-v2-slim-agotchi), an [external antenna](https://pwnagotchi.ai/community/#adding-an-external-wireless-antenna-to-the-rpi0w), and a more robust power solution using [LiPoPi](https://github.com/NeonHorizon/lipopi). Modifying the PiSugar case to make room for these different parts would be required, and I might as well add a carabiner loop while I'm at it. I might paint the case to add a bit of color and mask printing defects, too.

And while a Pi 4-based Pwnagotchi sounds nice, the much higher power usage and increased heat output means I will probably stick with the Pi 0W and lose out on the 5GHz support of the 4.

# **Afterword**
This was my first real post on my blog or really anywhere at all, and while it feels 'fine' a lot of it feels off to me, I had to rewrite it a ton of times and it still isn't great... Feel free to send me suggestions on what I can change for my next post, as I'm hoping to keep this up since it's something I've always wanted to do, and I've never trusted my own writing skills to be up to it.
