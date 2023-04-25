This readme shows you how to set up a Rasberry pi.

Firstly, hopefully you can just download the pre-existing image from the current SD card:
    1. [PLACEHOLDER]
        a. COME BACK TO THIS: Wipe the last SD card, and practice re-imaging with the current one
        b. And obvi, store the processed image somewhere (Gdrive) so the next person can just download it
    2. If not, you will need to download the Rasberry Pi imager to install Rasbian (the Raspberry Pi OS) onto your SD card.
        a. Find the instructions here: https://www.raspberrypi.com/documentation/computers/getting-started.html
        b. But if you just want to skip to getting the imager, it is here: https://www.raspberrypi.com/software/

There are three components to setting up the Pi:
    1. Downloading dependencies
    2. Making Chromium (the Pi's browser) autoboot and go to the right Plntry URL
    3. Making Ngrok run at startup. The server runs locally (127.0.0.1:5000), and it will need to be exposed to a public IP address

NOTE: You should try to keep the pi's username as 'pi'. In one case you switched to daltonkdwyer, and now can't change back, which is a pain.

STEP 1: Downloading dependencies
    1. This part shouldn't be too hard
        a. |pip3 install flask_socketio

STEP 2: Chromium Autoboot
    1. You wil xterminal to make a terminal run on startup
        a) Tutorial here: https://forums.raspberrypi.com/viewtopic.php?t=66206 (ctrl-f Ragnar)
        b) Install the right package:
        | sudo apt-get install xterm
        c) Create an autostart directory:
        |mkdir -p ~/.config/autostart
        d) Go into that directory and create a .desktop file
        |cd /home/pi/autostart (or /home/daltonkdwyer/autostart if you didn’t change username) 
        |touch plntry33autostart.desktop
        e) Nano into the file and paste the below
        |nano plntry33autostart.desktop
                [Desktop Entry]
                Encoding=UTF-8
                Name=Terminal autostart
                Comment=Start a terminal and list directory
                Exec=/usr/bin/lxterm -e 'bash /home/pi/rc_car1/browserautostart.sh' (or /home/daltonkdwyer/rc_car1)
    2. Tell the terminal what to
        a) Create a shell SH file in the rc_car folder:
        |touch touch browserautostart.sh




GOT HERE on Sunday: keep going through everything. And make sure it works on the Pi 