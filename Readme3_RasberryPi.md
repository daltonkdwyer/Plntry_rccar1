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
    2. Run Flask server on a local terminal
    3. Making Ngrok run at startup. The server runs locally (127.0.0.1:5000), and it will need to be exposed to a public IP address
    4. Making Chromium (the Pi's browser) autoboot and go to the right Plntry URL

NOTE: You should try to keep the pi's username as 'pi'. In one case you switched to daltonkdwyer, and now can't change back, which is a pain.

STEP 1: Downloading dependencies
    1. This part shouldn't be too hard. Can do it from any folder
        a. |pip3 install flask_socketio

STEP 2: Run Flask Websockets Server at startupß
    1. You will run a Flask server on a local terminal like usual
        a) Tutorial here: https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-1-rclocal
        b) You will use option 3: "systemd"
            - There is one bug in the tutorial. In the command to start the tutorial, you need to put "-u pi" as shown below. 
            - See here, last response: https://forums.raspberrypi.com/viewtopic.php?t=306352
    2. Commands to type:
        a) Go into the systemd folder, and create a new .service doc:
        | sudo nano /lib/systemd/system/app.service
        b) Type in the relevant commands:
        [Unit]
        Description=Start the Flask server
        After=multi-user.target
        [Service]
        ExecStart=sudo -u pi python /home/pi/rc_car1/app.py
        [Install]
        WantedBy=multi-user.target
        ----------------------
        (OR put in daltonkdwyer in the above, instead of pi!)
    
    2. To see if everything's running correctly (USEFUL) type in this:
        |systemctl status app.service



STEP 3: Launch Ngrok on boot
    1. You will launch Ngrok on boot, so it punches out to the right webpage




STEP 2: Launch Chromium on boot, to the right Plntry webpage
    1. You wil xterminal to make a terminal run on startup
        a) Tutorial here: https://forums.raspberrypi.com/viewtopic.php?t=66206 (ctrl-f Ragnar)
        b) Install the right package:
        | sudo apt-get install xterm
        c) Create an autostart directory:
        |mkdir -p ~/.config/autostart
        d) Go into that directory and create a .desktop file
        |cd /home/pi/.config/autostart (or /home/daltonkdwyer/.config/autostart if you didn’t change username) 
        |touch plntry33autostart.deßsktop
        e) Nano into the file and paste the below
        |nano plntry33autostart.desktop
                [Desktop Entry]
                Encoding=UTF-8
                Name=Terminal autostart
                Comment=Start a terminal and list directory
                Exec=/usr/bin/lxterm -e 'bash /home/pi/rc_car1/browserautostart.sh' (or /home/daltonkdwyer/rc_car1)
    2. Tell the terminal what to
        a) Create a shell SH file in the rc_car folder:
        |touch browserautostart.sh
        b) Nano in, and insert the following:
        |nano browserautostart.sh
        Paste: chromium-browser https://plntry.herokuapp.com/rc_car1?
        [or whatever the link to the current car is!]