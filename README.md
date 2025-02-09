Python control for LG speaker systems
=====================================

A simple Python API for controlling speakers from LG that can otherwise be controlled via the LG wifi speaker app.

Example use
-----------
Device discovery is out of scope of this project. Use an mdns module such as netdisco to locate devices on your network.

Connect to a speaker at 192.168.1.15:
```
import temescal

speaker = temescal.temescal("192.168.1.15")
```

Connect with a registered callback:
```
import temescal

speaker = temescal.temescal("192.168.1.15", callback=speaker_callback)
```

The callback will be called whenever Temescal receives a packet from the speaker. This may be a response to a sent command or a gratuitous status update triggered by another control event.

Get the current equaliser state:
```
import temescal

speaker = temescal.temescal("192.168.1.15", callback=speaker_callback)
speaker.get_eq()
```

Note that the `get_eq` method does not return any response: the device response will come back through the callback routing.

Set the volume to 20:
```
import temescal

speaker = temescal.temescal("192.168.1.15", callback=speaker_callback)
speaker.set_volume(20)
```

This is not an officially supported Google project.

Keepalive
=========

This fork adds a keepalive feature, designed to determine whether the device is connnected or not. It works by periodically requesting some data from the device; if no response is obtained, it means that the device is offline. The downside of this method is that sending a request to the device will turn the LED screen back on (but it does not affect power consumption).

Device behaviour
================

Power consumption
-----------------

* The device must be turned on at least once using the remote or HDMI CEC to join the WiFi network
* During first boot, ping can reach 5s, then it goes down below 10ms
* When powering on, it will consume 11W for a few minutes then down to 4W
* Then it consumes ~4W continuously, even if "turned off" with the remote or HDMI CEC; even if the LED display is on, consumption stays at 4W -- this device is basically always on

The above information applies at least to SP8YA (my model), feel free to contribute

Open ports
----------

Here is an nmap -A portscan:
```
7000/tcp  open rtsp 
8008/tcp  open http
8009/tcp  open ssl/tcp?
8012/tcp  open tcp?
8443/tcp  open https 
9000/tcp  open ssl/tcp?
9741/tcp  open tcp (used by the mobile app)
9876/tcp  open http?
10001/tcp open ?
10101/tcp open ?
'''
