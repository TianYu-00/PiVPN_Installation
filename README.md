# PiVPN Installation Guide (Windows)

Requirements: Raspberry Pi, SD Card Reader+SD Card, Keyboard, Monitor, Ethernet Cable, Router, PC(Windows)

DuckDNS(For Dynamic DNS) -> PiVPN -> Portforwarding -> Wireguard

Links:  
https://www.raspberrypi.com/software/  
https://www.duckdns.org/  
https://www.duckdns.org/install.jsp  
https://www.pivpn.io/  
https://www.putty.org/  
https://winscp.net/  


Preparing the PI🥧
1) Plug in your sd card reader to your pc 
2) Download Raspberry PI OS Imager from https://www.raspberrypi.com/software/
3) Install Raspberry PI OS Imager. Youre fine to keep pressing next to have everything set as default unless you want to change the directory of where the installed file is saved at.
4) Run the Raspberry PI OS Imager.  
   `Choose OS` -> Raspberry Pi OS (other) -> Raspberry Pi Os Lite (32-bit)   
   `Choose Storage` -> (Make sure the storage you are choosing is ur sd card)   
   `Gear Icon` -> Set_hostname: pivpn(or whatever name you want to call it but i like mine pivpn so i can better identify it), Enable SSH, Enable set_username_and_password -> username(whatever username you want i use "pi") -> password(whatever password you want), Enable Set_local_settings, then save it.   
   `Write` -> Just press it when you have done the above
5) While it writes you should prepare your raspberry pi (Plug in your keyboard, monitor, pi ethernet -> your router, not the power just yet maybe plug it in after the write is done and you have ur sd card inserted into your pi)
6) When all the above is done, start your pi up and let it load all.
7) Type in your username and press enter, type in your password then press enter(if your password wont display its normal just type it and press enter)
8) When youre in, type in `sudo apt update && sudo apt upgrade -y` to update ur pi and upgrade anything to latest packages etc.
9) now do `sudo reboot` to restart your pi.
10) after reboot and loggin, type `ip a`, look through the info you should be able to see your ip (usually it should be something like 192.168.x.x/24).
11) Take note of the IP, write it down or type it up on ur windows notepad.exe

Installing Putty
1) Go to https://www.putty.org/ to download and install putty on ur pc
2) 



Preparing Dynamic DNS (Using Duck DNS🦆) 
1) Go to https://www.duckdns.org/ and sign in.
2) Now that you signed in its time to create ur free domain, so enter in the name you want in subdomain section [Image here] then click add domain.
3) After you have done that successfully you should be able to see it below.
4) Head over to https://www.duckdns.org/install.jsp  
   `Operating system` -> Pi  
   `First step - choose a domain` -> (Choose the domain you just created)
5) Now follow the instruction listed on the website.



Preparing PiVPN🛡️
1) 111



Preparing Port Forwarding⏩




Preparing Wireguard💂
   

