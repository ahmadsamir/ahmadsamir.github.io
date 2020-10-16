---
layout: post
title: NumPad Rebooted
---

If you use the PageUp key a lot (e.g. accessing shell history) and instead of hitting PageUp you hit NumLock? and then it happens several times? the solution for me was to remap the NumLock key to become another PageUp.

This is for systems using Udev (which is now part of systemd), and a USB or PS/2 keyboard; I am not sure this is feasible for laptops, since as far as I know, on a laptop keyboard switching the Numpad on/off can be useful, gives you more keys on the already cramped keyboard.

* First you need to find the "keycode" for the Numlock key; install `evtest`, then run it as root from terminal

* Select the device for your keyboard from the list; note down the device number (e.g. /dev/input/event**8**); this may need a bit of trial and error, usually there are multiple "events" for the same hardware device, e.g. one for handling the regular keyboard keys (A, B, C, Space bar ... etc) and another for handling the multimedia keys on the same keyboard.

* Once you select a device, some info about the keyboard hardware will be printed, on my system I see:  
<pre class="code">
Input driver version is 1.0.1
Input device ID: bus 0x3 vendor 0xd62 product 0xd93f version 0x111
Input device name: "      USB Keyboard"
</pre>
note down the "vendor" (0xd62) and "product" (0xd93f)

* Now, in the terminal, every time you press a key you'll see some info about that key, pressing Numlock I see:
<pre class="code">
Event: time 1602580599.011787, -------------- SYN_REPORT ------------
Event: time 1602580600.611819, type 4 (EV_MSC), code 4 (MSC_SCAN), value 70053
Event: time 1602580600.611819, type 1 (EV_KEY), code 104 (KEY_PAGEUP), value 1
Event: time 1602580600.611819, -------------- SYN_REPORT ------------
Event: time 1602580600.683796, type 4 (EV_MSC), code 4 (MSC_SCAN), value 70053
Event: time 1602580600.683796, type 1 (EV_KEY), code 104 (KEY_PAGEUP), value 0
Event: time 1602580600.683796, -------------- SYN_REPORT ------------
</pre>

the interesting part in the output is *value*, in my case it's **70053**.

* Next create this directory: `/etc/udev/hwdb.d/` (/etc/udev/ should already be there). Inside that directory create a text file e.g. `foo.hwdb` (the extension has to be <i>.hwdb</i>), and open it in a text editor

* The first line in that file, should be something like:  

<code class="code">evdev:input:b*v0D62pD93F*</code>

I constructed that line from the <i>vendor</i> and <i>product</i>, respectively (we noted them down earlier). From /usr/lib/udev/hwdb.d/60-keyboard.hwdb:

<pre>
    #  - Generic input devices match:
    #      evdev:input:bZZZZvYYYYpXXXXeWWWW-VVVV
    #    This matches on the kernel modalias of the input-device, mainly:
    #    ZZZZ is the bus-id (see /usr/include/linux/input.h BUS_*), YYYY, XXXX and
    #    WWWW are the 4-digit hex uppercase vendor, product and version ID and VVVV
    #    is an arbitrary length input-modalias describing the device capabilities.
    #    The vendor, product and version ID for a device node "eventX" is listed
    #    in /sys/class/input/eventX/device/id.
</pre>

The parts we're interested in are *YYYY* and *XXXX*, i.e. the *vendor* and *product*; I set the rest of those hex digits to "*", since for example, it doesn't matter which bus-id is being used.

* The second line:  
<pre class="code">
 KEYBOARD_KEY_70053=pageup    # Remap NumLock to PageUp
</pre> the part after # is a comment

<div class="note"><b>Note:</b> This line starts with one blank space, i.e. it's " KEYBOARD_KEY_70053=pageup", if you remove that space it won't work, it's iffy about syntax (it was the last time I tried anyway, which was quite a while ago); again see /usr/lib/udev/hwdb.d/60-keyboard.hwdb for examples.
</div>

* Save the file, then as root, update the hwdb:

<code class="code">systemd-hwdb update</code>

* To "apply" the changes, again as root:

<code class="code">udevadm trigger /dev/input/eventX</code>

replace *X* with the device number you used previously. If this doesn't work, remove/reinsert the usb cable/receiver from the machine.

* Now try the Numlock key, pressing it should be the same as pressing PageUp.

* To revert this change, simply remove the `foo.hwdb` file and:
<code class="code">
systemd-hwdb update
udevadm trigger /dev/input/eventX
</code>
or remove/reinsert the usb cable/receiver from the machine.

* * *

Of course now you have no way to toggle the Numpad :), so the next logical step is to remap the number keys on the Numpad to always be in number mode i.e. 1 on the Numpad always types 1, 2 on the Numpad always types 2 ...etc, regardless of the state of NumLock. On my system I needed to use:
<pre>
 KEYBOARD_KEY_70062=0
 KEYBOARD_KEY_70059=1
 KEYBOARD_KEY_7005a=2
 KEYBOARD_KEY_7005b=3
 KEYBOARD_KEY_7005c=4
 KEYBOARD_KEY_7005d=5
 KEYBOARD_KEY_7005e=6
 KEYBOARD_KEY_7005f=7
 KEYBOARD_KEY_70060=8
 KEYBOARD_KEY_70061=9
 KEYBOARD_KEY_70055=asterisk
 KEYBOARD_KEY_70063=dot
 KEYBOARD_KEY_70058=enter
 KEYBOARD_KEY_70056=minus
 KEYBOARD_KEY_70057=plus
 KEYBOARD_KEY_70054=slash
</pre>
you simply repeat the above steps for each key.

One other issue that this fixes is booting and then trying to use the Numpad only to find NumLock isn't "on", this way it's always "on" :)


I hope this mini-tutorial was useful.
