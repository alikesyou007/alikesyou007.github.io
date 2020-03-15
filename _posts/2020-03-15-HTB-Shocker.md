---
layout: post
title: HTB-Shocker Write-Up
---
![Lame](/images/ShockerInfoCard.JPG)    

Reconnaissance
======

I used nmap to see what ports/services were showing as up and running.

![Nmap_scan](/images/Shockernmap.png)

The scan showed that there is an Apache web server running on port 80 with ssh running on port 2222.

I decided to go to the website to see if there was anything else I could glean from it that would help me solve this.

![Website_pull](/images/Shockerwebsite.png)

Not much there so it was time to enumerate.

Enumeration
======

I ran a Gobuster scan for common directories first to see what's out there.

![GoBuster_scan](/images/Shockergobuster1.png)

The interesting directory here is the /cgi-bin directory since that is the directory that holds custom scripts for the web server. I decided to run gobuster from that directory with common script extensions to see if there was anything interesting that showed up. 

![GoBuster_scan2](/images/Shockergobuster2.png)

The user.sh (bash script) stuck out to me so I decided to navigate to the webpage and pull down the script.

![User_script](/images/Shockeruserscript.png)

Not much there but perhaps this webserver is vulnerable to the Shellshock vulnerability? I know that shellshock targets bash scripts. 

Exploitation
======

I did a searchsploit search for “Shellshock” to see if there was an exploit I could use to gain a reverse shell. 

![Searchsploit_SS](/images/Shockersearchsploit.png)

I sifted through a couple of the exploits and then did a google search trying to find the manual way to exploit Shellshock and came across this website. https://www.sevenlayers.com/index.php/125-exploiting-shellshock 

It was an awesome write-up on the vuln and helped me to understand how to get the reverse shell.
  
![SS_Exploit](/images/Shockerexploit.png)

I first attempted to see if I could get to the box via curl. Once I was able to successfuly get in, I went for the reverse shell and got it

Post-Exploitation
======

Once I figured out I was ‘shelly’ I found the user flag.

![SS_PostExploit](/images/Shockeruserflag.png)

In order to become root, I knew that I would have to escalate the privileges of user ‘shelly.’ So I did a sudo -l to figure out what she had sudo privileges for and then use GTFOBins to escalate privileges to root. From there, I was able to get the root flag.

![SS_rootflag](/images/Shockerrootflag.png)


Results Analysis
======

This was my first time exploiting shellshock and I did it without using Metasploit. Metasploit is cool to use but at times, I just want a better undertstanding of what exactly Metasploit is doing. A couple of years ago, the Shellshock vulnerability was a big thing and I surmise that it remains a vulnerability still for some systems. 

All in all, I did a scan to find open ports, found an interesting directory, found the bash script and then exploited it to get into the box as the user and exploited the user's sudo privileges to ‘perl’ to get root.
  
  


