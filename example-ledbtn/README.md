SAMA5D3 Example Pubnub Client
=============================

This client is a fairly trivial C PubNub app that connects your board
to the world via a PubNub channel:

  * Messages are sent whenever the user button (near the ethernet connectors)
    is pressed
  * According to received messages, the blue and red LEDs can be switched

This is implemented using the libevent library and using the standard Linux
kernel hardware interfaces - ``/dev/input/event0`` (for button input) and
``/sys/devices/leds/leds/d*/brightness`` (for LED flashing).

See the comment near the top of ``sama5d3-ledbtn.c`` for details on the
PubNub message format.  To get it running:

  * Run ``make`` to build the C program.

  * The first time after booting up, you need to give your user access
    permissions to the device files that interface the respective hardware:

	sudo chmod a+rw /sys/devices/leds/leds/d*/brightness
	sudo chmod a+r /dev/input/event0

  * To execute the program, run: ``./sama5d3-ledbtn``

  * Go to the PubNub web console, subscribe to the ``sama5d3`` channel
    and start playing around. :-)
