---
title: "Evilmouse"
aliases: ["/blog/evilmouse/"]
date: 2026-02-15
showtoc: true
description: "A covert keystroke injector hidden inside a fully functional mouse."
tags: ["hardware", "red-team", "hid"]
tech: ["circuitpython", "rp2040"]
cover:
  image: "/images/evilmouse1.png"
  alt: "Evilmouse"
---
{{< figure src="/images/evilmouse1.png" alt="Evilmouse" width="70%" >}}

Evilmouse is a covert keystroke injector hidden inside a fully functional mouse, similar in concept to a [Rubber Ducky](https://hak5.org/products/usb-rubber-ducky) tool. As soon as it connects, it can autonomously execute commands and begin compromising the system.

## The Idea
These days, everyone that's been through basic job security awareness training knows that a USB stick plugged into a computer is suspicious. A mouse, however, might not appear suspicious at all, especially when its functionality is preserved.

## Materials
| Material | Quantity | Approx. Price |
|---|---|---|
| RP-2040 Zero | 1 | $3 |
| Adafruit 2-Port USB Hub Breakout | 1 | $5 |
| Amazon Basics Mouse | 1 | $6 |
| USB-C Pigtail Cable | 1 | $3 |
| Rosin-core 60/40 Solder |1  | $8 |
| USB-C Data Cable | 1 | $8 |
| Flux Paste | 1 | $6 |
| Kapton Tape | 1 | $5 |
| Dupont Wires | 4 | basically free (~$0.03) |
| **Total** | | **~$44** |

Not bad considering a rubber ducky will cost you around $100.

Also, if you already have some of these soldering materials, it will be much cheaper.

## Building Evilmouse
{{< figure src="/images/evilmouse2.png" alt="Evilmouse" width="90%" >}}
Building the mouse presented several challenges that I had to overcome.

My first problem was that the shell was suprisingly compact for a cheap $6 mouse.
{{< figure src="/images/evilmouse3.png" alt="Evilmouse" width="50%" >}}
As you can see on the top portion of the shell, there is a ton of plastic ribbing and clunky components.
The first thing I had to do was remove some of that, which I achieved using a small cutter on a multi-tool.

After freeing up more space in the shell, I needed to remove the white connector piece from the stock PCB, this can be done **gently** with a small flathead screwdriver.
{{< figure src="/images/evilmouse5.png" alt="Evilmouse" width="50%" >}}

I then began to start programming the RP-2040 Zero, this will perform the actual exploitation.
My original idea was to just run [pico-ducky](https://github.com/dbisu/pico-ducky) inside a mouse, but this isn't directly compatible with the RP-2040 zero.
{{< figure src="/images/evilmouse4.png" alt="Evilmouse" width="50%" >}}
Instead, I opted to flash Circuit Python on it and make my own firmware.
I didnt go too crazy on it, but I just implimented a demo that sends a Windows AV-safe reverse shell to a specified host.
At the time of writing this, I'm also considering making it ducky-script compatible -- anyone reading this is more than welcome to try their hand at it though.

Check out the code [here](https://github.com/NEWO-J/evilmouse/tree/main)

*I take no responsbility for malicious usage, this tool's intention was for education alone.*

After programming the RP2040 chip, I just had to solder the pigtail cable and dupont wires to the USB breakout. This was by a long shot the biggest learning experience for me during this project, I had a little bit of soldering experience before this project, and by experience I mean failing.

It took me around a week to get good enough at soldering through-holes components to be comfortable on the real deal, but it worked out in the end!
{{< figure src="/images/evilmouse6.jpg" alt="Evilmouse" width="70%" >}}

Before the final assembly, its important that we prevent short circuits by adding a piece of kapton tape to the bottom of the components resting on the PCB, this will insulate any electricity from spreading.

{{< figure src="/images/evilmouse8.jpg" alt="Evilmouse" width="70%" >}}

Now all I had to do is connect the dupont wires to the PCB and jiggle it around until I found a configuration that fit the mouse shell. I got the shell to close (and still be functional) with this arrangement:
{{< figure src="/images/evilmouse7.png" alt="Evilmouse" width="70%" >}}
I had to push the usb breakout inwards as I closed the shell for it to work.

## Reverse Shell Demo
Now is the moment you've been waiting for. Time to see it in action:
{{< video src="/videos/evilmouse_demo.mp4" >}}
For anyone confused, I get an admin-level reverse shell sent to laptop B just a few seconds after the mouse is plugged in to laptop A, this essentially means I've fully compromised laptop A just by plugging in a "mouse"... scary world we live in.

For extra stealth, we can take additional measures like hiding the command prompt, or establishing a Windows task for persistence.

Overall, this was a fun project that taught me alot about custom firmware, working with HID libraries and soldering electronics.

If you make your own, I advise you to try and improve upon mine, try adding things like remote activation, advanced windows defender bypasses, or build your code in rust for faster keystrokes.
