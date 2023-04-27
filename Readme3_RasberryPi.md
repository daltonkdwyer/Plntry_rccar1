This readme shows you how to set up a Rasberry pi. 

There are four components to setting up the Pi:
    1. Downloading dependencies
    2. Run Flask server on a local terminal
    3. Making Ngrok run at startup. The server runs locally (127.0.0.1:5000), and it will need to be exposed to a public IP address
    4. Making Chromium (the Pi's browser) autoboot and go to the right Plntry URL

Firstly, hopefully you can just download the pre-existing image from the current SD card:
    1. [PLACEHOLDER]
        a. COME BACK TO THIS: Wipe the last SD card, and practice re-imaging with the current one
        b. And obvi, store the processed image somewhere (Gdrive) so the next person can just download it
    2. If not, you will need to download the Rasberry Pi imager to install Rasbian (the Raspberry Pi OS) onto your SD card.
        a. Find the instructions here: https://www.raspberrypi.com/documentation/computers/getting-started.html
        b. But if you just want to skip to getting the imager, it is here: https://www.raspberrypi.com/software/


NOTE!!!: You should try to keep the pi's username as 'pi'. In one case you switched to daltonkdwyer, and now can't change back, which is a pain. Pay attention to this in the rest of the tutorial (you've called it out when it's needed)

STEP 1: Downloading dependencies
    1. This part shouldn't be too hard. Can do it from any folder
        a. |pip3 install flask_socketio

STEP 2: Run Flask Websockets Server at startup
    1. You will run a Flask server on a local terminal like usual
        a) Tutorial here: https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-3-systemd
        b) You will use option 3: "systemd"
            - There is one bug in the tutorial. In the command to start the tutorial, you need to put "-u pi" as shown below. 
            - See here, last response: https://forums.raspberrypi.com/viewtopic.php?t=306352
    2. Commands to type:
        a) Go into the systemd folder, and create a new .service doc:
        | sudo nano /lib/systemd/system/app.service
        b) Type in the relevant commands. Remember to use ctrl-x and 'Y' to save the file:        
        [Unit]
        Description=Start the Flask server
        After=multi-user.target
        [Service]
        ExecStart=sudo -u pi python /home/pi/rc_car1/app.py    (OR put in daltonkdwyer instead of pi!!!)
        [Install]
        WantedBy=multi-user.target
        c) Tell systemd to recognize the service with (you will need to enter this command every time you change the file!!!)
        |sudo systemctl daemon-reload
        d) Finally, tell systemd that we want the service to start on boot:
        |sudo systemctl enable app.service
    3. To see if everything's running correctly (USEFUL!!!) type in this:
        |systemctl status app.service


STEP 3: Launch Ngrok on boot
    1. You will launch Ngrok on boot, so it punches out to the right webpage
    2. Firstly, you'll need to download it
        a) https://dashboard.ngrok.com/get-started/setup
            - Email: yahoo, pw: m
        b) You’ll need to use the command to put your authentication token in, as you’re using a feature of the premium offering (which is how you can reuse the same URL, https://plntry33.ngrok.io/ all the time)
        c) Next, you need to move Ngrok into path for some reason:
            - cd into downloads
            - Move to path:
            |mv ngrok /usr/local/bin
                - See here: https://stackoverflow.com/questions/30188582/ngrok-command-not-found_
    3. To make it autostart, for some reason you're using bash. Not systemd
        a) Tutorial here: https://rntlab.com/question/module-9-ngrok-install-and-work/
        b) Install screen:
        | sudo apt install screen
        c) Edit the bashrc folder here:
        | sudo nano /home/pi/.bashrc
            - OR replace the pi with daltonkdwyer as usual
        d) Insert this text at the bottom:
        | screen -d -m /home/pi/./ngrok http --subdomain=plntry33 5000
            - OR replace the pi with daltonkdwyer as usual


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
                Exec=/usr/bin/lxterm -e 'bash /home/pi/rc_car1/browserautostart.sh' (OR /home/daltonkdwyer/rc_car1)
    2. Tell the terminal what to
        a) Create a shell SH file in the rc_car folder:
        |touch browserautostart.sh
        b) Nano in, and insert the following:
        |nano browserautostart.sh
        Paste: chromium-browser https://plntry.herokuapp.com/rc_car1?
        [or whatever the link to the current car is!]