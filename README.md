lightsequence  working but instructions are not yet complete.
=============

Code to toggle relays via RasPi GPIO using MIDI

This is more of a bank of knowledge for me to recreate this project should anything happen to corrupt my set up.  But ya'll are more than welcome to the info.

Derived from lightorgan.c which is used to control GPIO pins with midi playback in ALSA.  The source and instructions are available at: http://chivalrytimberz.wordpress.com/2012/12/03/pi-lights/


lightorgan8.c  - Modified to only use 1st eight pitch of scale rather than approximating across the entire 12 pitch scale.

lightsequence.c - modified further to allow user to indicate 8 or 16 relay/pin control.  Uses midi channels 0 for 8ssr and 1 for 16ssr.

Starting from scratch on the Raspi, you need to install:

	Raspi OS (Raspbian, Debian, etc.) (www.raspberrypi.org/downloads)
	timidity (apt-get install timidity)
	ALSA Devel libs (should already be present)
	Gordon's wiringPi library (https://projects.drogon.net/raspberry-pi/wiringpi/)
(git clone git://git.drogon.net/wiringPi)
		Build it from the wiringPi directory:
			<code>cd wiringPi
			./build</code>
		Test it:
			gpio -v
			gpio readall

Need GIT - (sudo apt-get install git-core)

Optionally, if you want the original lightorgan.  You will need subversion to grab a copy.
	subversion (apt-get install subversion)
	lightorgan (svn checkout http://pi-lightorgan.googlecode.com/svn/trunk/ pi-lightorgan-read-only)

Modify the /etc/timidity/timidity.cfg file.  Uncomment these lines and restart Timidity.

opt EFresamp=d          #disable resampling
opt EFvlpf=d            #disable VLPF
opt EFreverb=d          #disable reverb
opt EFchorus=d          #disable chorus
opt EFdelay=d           #disable delay
opt anti-alias=d        #disable sample anti-aliasing
opt EWPVSETOZ           #disable all Midi Controls
opt p32a                #default to 32 voices with auto reduction
opt s32kHz              #default sample frequency to 32kHz


Hardware:

Pin chart at: https://projects.drogon.net/raspberry-pi/wiringpi/pins/

Connect pins as appropriate to your setup.

MIDI file creation:

You should be able to use any midi file but the purpose of this program is to design a specific light sequence.

I've been using the open source program MidiEditor to drop notes into patterns.  Sounds terrible when played but gives me a complete control of the pattern.


Test files included in the repo:

midilighttest8.mid - Tests playing first 8 notes on Channel 0.  Cycles from 0 to 7 in progressively faster scales.  1/4 second notes through 1/32 second notes.

midilighttest16.mid - Eight notes played on Channels 0 & 1.  Untested until I get another SSR board.
