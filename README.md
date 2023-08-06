# PiVPN Installation Guide

Requirements: Raspberry Pi, Micro SD Card Reader + Micro SD Card, Keyboard, Monitor, Ethernet Cable, Router, PC(Windows)

DuckDNS(For Dynamic DNS) -> PiVPN -> Portforwarding -> Wireguard

This guide is personally made for my future self to follow along if i forgot how to do it so its a little but scuffed and not as appealing but ill try to make it more followable for others to understand.   
This might not be as good looking as of now but when i have time, ill sureeeeeeely update it to be more appealing and maybe improve some wordings to make it more clear.    


## Preparing the Pi🥧
1) Plug in your sd card reader to your pc 
2) Download Raspberry PI OS Imager from https://www.raspberrypi.com/software/
3) Install Raspberry PI OS Imager. Youre fine to keep pressing next to have everything set as default unless you want to change the directory of where the installed file is saved at.
4) Run the Raspberry PI OS Imager.  
   `Choose OS` -> Raspberry Pi OS (other) -> Raspberry Pi Os Lite (32-bit) (You could choose the suggested Pi OS but ill be using lite 32bit version for less file size and less resource consuming)   
   `Choose Storage` -> (Make sure the storage you are choosing is ur sd card)   
   `Gear Icon` -> Set_hostname: pivpn(or whatever name you want to call it but i like mine pivpn so i can better identify it), Enable SSH, Enable set_username_and_password -> username(whatever username you want i use "pi") -> password(whatever password you want), Enable Set_local_settings, then save it.   
   `Write` -> Just press it when you have done the above
5) While it writes you should prepare your raspberry pi (Plug in your keyboard, monitor, pi ethernet -> your router, not the power just yet maybe plug it in after the write is done and you have ur sd card inserted into your pi)
6) When all the above is done, start your pi up and let it load.
7) Type in your username and press enter, type in your password then press enter(if your password wont display its normal just type it and press enter)
8) if it shows up with something to do with ECDSA Key Fingerprint xxx, Are you sure you want to continue connecting? just type yes
9) When youre in, type in `sudo apt update && sudo apt upgrade -y` to update ur pi and upgrade anything to latest packages etc.
10) now do `sudo reboot` to restart your pi.


## Getting your Pi IP   
There is alot of ways of getting your pi ip, ill provide 2 ways i personally use and find it easy enough.   
1) This is how to get your ip from your pi .
2) after reboot and loggin, type `ip a`, look through the info you should be able to see your ip (usually it should be something like 192.168.x.x/24).
3) Take note of the IP, write it down or type it up on ur windows notepad.exe   
4) ====================================
5) You could also look through ur pi ip from ur router site (due to there being way too many router sites youre gonna have to figure this one out urself) .
6) To get your router sites ip open up cmd on windows, and type in `ipconfig`, look through it and find your default gateway ip (could be something like 192.168.0.1 or 192.168.1.1 or something completely different but those 2 are the more common ones i know)
7) After getting that info, open up a browser and type that in on the search bar then login to your router site(u might have to look up your router site login credential online) 
8) Look through the list of ips and find the one that is connecetd to the pi, then again note that down.



## Installing Putty(For SSH) / Could also use windows cmd instead
1) Go to https://www.putty.org/ to download and install putty on ur pc then run it
2) Fill in `Host name (or ip address)` with ur noted pi ip inorder to connect to it
3) You could also save this by pressing on `default settings` then `save` so u dont need to enter it everytime
4) When all that is done, press `Open`, and then you will be connected to the Pi's SSH
5) ====================================
6) To do it with windows cmd, open up cmd then just type in `SSH username@xxx` (xxx is the ip you noted down), then your password that we set up earlier

   
With all those done, we now could access our pi headlessly from our windows pc. Youre free to unplug ur mouse, keyboard and monitor off your pi to save resources because we will be working on our pc from now on while ur pi is running and is connected to router with ethernet cable.
   

## Preparing Dynamic DNS (Using Duck DNS🦆) 
1) Go to https://www.duckdns.org/ and sign in.
2) Now that you signed in its time to create ur free domain, so enter in the name you want in subdomain section [Image here] then click add domain.
3) After you have done that successfully you should be able to see it below.
4) Head over to https://www.duckdns.org/install.jsp  
   `Operating system` -> Pi  
   `First step - choose a domain` -> (Choose the domain you just created)
5) Now follow the instruction listed on the website.
6) After all that restart and log in to your putty/cmd, then reboot the pi `sudo reboot` just incase.
7) Then restart your putty/cmd again because the SSH would be closed when pi reboots.

## Preparing Static IP/DHCP
1) To make you pi's ip to static you would need to login to your router site(instruction mentioned above)
2) For me i used Static DHCP lease for my Pi (Link the pi's mac address with the current pi ip)
3) If you have any problems with doing this try looking through online to see how to set a static ip or static DHCP for your pi on your router.


## Preparing PiVPN🛡️
1) Go to https://www.pivpn.io/.
2) Copy and paste the installation command onto the putty/cmd. As the time of writing this guide the installation command is `curl -L https://install.pivpn.io | bash` so i will be copying that command in.
3) When all those packages are downloaded it will show up a installation wizard that you have to follow along, you should be able to walkthrough it without any issues but well do it together anyways because there is some things we do need to change.
4) The instruction below is written in time of how the installer would be walking you through, please bare in mind that at later date it could be completely different. 
5) 1 - PiVPN Automated Installer - [Ok]
6) 2 - Static Ip Needed - [Ok]
7) 3 - DHCP Reservation - Choose [Yes] if the ip address matches the static ip you set earlier, else choose [No] then change the ip to the static ip you set earlier.
8) 4 - Local Users - [Ok]
9) 5 - Choose a user - Choose your user by pressing space, arrow keys to move up and down the selection, tab key to loop through sections. For me i chose pi as my user and pressed tab then [Ok]
10) 6 - Installation mode - Again same buttons as above, choose `Wireguard` then [Ok]
11) 7 - Default wireguard port - I personally recommend you to change this port to something different for security purposes, but if youre fine with it or if youre new to all this just keep it as the default `51820` port shown (best to note this port down for later use (portforwarding)). [Ok]
12) 8 - Confirm custom port number - [Yes]
13) 9 - DNS Provider - Choose any DNS theyre all fine but i personally recommend cloudflare(1.1.1.1) or google(8.8.8.8). i would be choosing cloudflare for this guide.
14) 10 - Public Ip or DNS - Remember the DNS Duck Domain Name we made earlier? yh well be using that domain name here so choose `DNS Entry` then [Ok]
15) 11 - PiVPN Setup - Enter in the domain name you created, should be something like `yourdomainname.duckdns.org` then [Ok]
16) 12 - Confirm DNS Name - Look through it and make sure your DNS name is correct then [Ok], or else [No]
17) 13 - Server information - [Ok]
18) 14 - Unattended Upgrades - [Ok]
19) 15 - Unattended Upgrades - [Yes]
20) 16 - Installation Complete! - Read through it then [Ok]
21) 17 - Roboot - [Yes]
22) 18 - Rebooting - [Ok]
23) Itll now reboot so restart ur putty/cmd and log in again
24) Now when all those are done we will now create a couple clients for wireguard to connect to. To do this type in `pivpn add` or `pivpn -a` then press enter
25) Then type in a name for the client, ill just create a temporary client for this guide so ill name it `Client1_test`
26) When that client is setup youre set to go. In the future if u want to use the vpn on multiple devices its good practice to create a client for each of the devices so you could manage them seperately.
27) Remember you could also delete clients, so dont be afraid to create a couple more to play around with.  Do `pivpn help` for a list of available commands for pivpn.
28) you could also do `pivpn -qr` to create a qr code for your mobile wireguard app/apk to scan and connect to. You cant connect to it just yet because we need to do portforwarding but we will teach you how to connect to windows and android devices later on the guide.



## Preparing Port Forwarding⏩
1) Log in to your router site (instructions mentioned before)
2) Find your portforwarding page in your router site
3) Below portforwarding instruction varies from router to router so its best for you to look it up how to portforward on your router but ill provide my example below.
4) Create a new portforwarding rule   
   Protocol - Choose UDP
   Service Name - Whatever name you would like to set it to ill be using `PiVPN`   
   Start Port - `51820` (Remember the port we setup earlier from our pivpn installation? yeah use that port for this)
   End port - `51820` (Same as start port)
   Server Ip Address - type in your pi's static ip address that we setup earlier. (If you have start and end ip address that you have to fill in, its the same for both so just type in ur static pi ip for both)
   Active - Some router sites have port active toggles and is set to false as default, so make sure you set this portforwarding rule to active in order for it actually work.
5) That is all for my example, just makesure you set ur portforwarding to active/on, protocol to UDP, port to whatever port you setup before, and ip to the pi's static ip address.
6) When all that is done your portforwarding is done.
   
   

## Preparing Wireguard💂
1) Mobile - Android
2) Download Wireguard from google play store or from wireguard website https://www.wireguard.com/install/ (Choose Download APK File)
3) Install it
4) Go back to your putty/cmd, login to it.
5) Create your client `pivpn add` then enter then followed by name
6) Then get that client qr code `pivpn -qr`
7) Run the wireguard app you just installed and scan the QR code to add it in.
8) Give it a name (whatever name you want to call it).
9) Turn it on
10) Do some testings with ip test and speed test etc to see if everything is fine.
11) ====================================
12) Pc - Windows
13) Download wireguard windows installer from https://www.wireguard.com/install/
14) There is also a portable version of wireguard from https://github.com/DrEm-s/wireguard-windows-portable
15) I recommend the installer instead because the portable version is missing some features, though its good to know there is a portable version out there for the people that is not able to use the installer for whatever reason.
16) Install it
17) Now you need to find the client's CONF file type you created earlier inside your pi's Micro sd card. Ill list 2 examples to demonstrate this.
18) Example 1 - You could use Winscp which could be downloaded from https://winscp.net/ . Download this and install it
    Make sure your pi is running and is connected to the router. Run winscp and type in your `Host name` which is ur pi's static ip, your `User name` which is ur pi's username (in my case its `pi`), your `Password` which is ur password. Just incase you forgot Username and Password are the ones we set in the beginning of this guide when we filled in the `gear icon` username and password information.
19) Now that youre in, go to the directory of your pi `home/pi/configs`. In this directory you would see all the pivpn clients you have made , in my case i would only have 1 named `Client1_test`. Download this client's file by right clicking the file -> download -> save it wherever suits you, i would be saving this on my desktop for easy access.
20) Now that you have that downloaded, thats all u need, just that 1 file.
21) Example 2 - Unplug your pi's power and take out the micro sd card -> plug the micro sd card into your micro sd card reader -> plug that reader into the pc
22) Now that you have the reader plugged in, browse through the directory to find the CONF file i mentioned earlier above. Copy that file to where ever suits you, ill put mine in desktop for easy access.
23) Doesnt matter which example you choose both are fine, you basically just need to get that client CONF file copied over to your PC. In my case if the client `Client1_test` is created for my pc to connect to, i would download/copy the file named `Client1_test` to my desktop for easy access.
24) Now that you have the file, were ready to run wireguard. Find wireguard on your desktop, if you cant find it just search it up in the windows search menu.
25) When you have it open you should be able to see the button `Import tunnel(s) from file`, click on it and select the client CONF file u downloaded/copied over earlier -> after selecting it press open.
26) After that you should be able to see its fully configured and is ready to be activated.
27) Click on `Activate` to run the VPN, click on it again to stop it.

Hurrrrrrray youve done it!    


Links:  
https://www.raspberrypi.com/software/  
https://www.duckdns.org/  
https://www.duckdns.org/install.jsp  
https://www.pivpn.io/  
https://www.putty.org/  
https://winscp.net/  
https://www.wireguard.com/install/   
   

