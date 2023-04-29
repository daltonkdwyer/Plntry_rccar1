0. SUMMARY

    This is a WebRTC architecture. Two browsers will communicate with each other.

    You have written a better diagram here: 


1. HOSTING
    
    1.  The signalling server (you built) is hosted on heroku
        a) Login: 
            Username: dalton.dwyer@yahoo.com
            PW: S13
        b) How to update the code:
            - This should automatically update by Git push
            - You can double check that/change it here: https://dashboard.heroku.com/apps/plntry/deploy/github

    2. The TURN server you got off the shelf. It's made by metered.ca.
        a) Link here: https://dashboard.metered.ca/turnserver/app/63f3ea9be5eb464431ddb2fc
            Login: gmail, password: m
        b) Server logins (need to put into your WebRTC code):
            Username: 999c14afe3cc4008b72f3aa0
            Password: oBpkY5NWEwvTK
        c) All you have to do is feed the IP address of the TURN server into the object

2. SERVER

    1. "plntry_server1.py"
        a) Flask / Websocket server
        b) Sends out the client page
        c) Recieves SDP information from clients and sends it between them so they can establish a connection with each other 


3. CLIENT

    1. 