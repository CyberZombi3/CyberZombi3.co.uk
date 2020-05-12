Home [CyberZombi3](https://cyberzombi3.github.io/CyberZombi3.co.uk/)

# MitreAtt&ckDashboard

Mitre Att&ck Splunk Dashboard
So I was asked if I could post something about the Splunk Dashboard I have been working on based off of the Mitre Att&ck framework... I have to say it's still very much a work in progress but the basics are starting to take shape now.

A few pre reqs -

Sysmon logging enabled
Powershell logging enabled
Netmon logging enable (not really discussed below but I may cover it some other time)
A Splunk install
A basic knowledge of Splunk .conf files will help

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/MitreAtt&ckDashboard/Images/Splunk%20Dashboard.png?raw=true)

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

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/MitreAtt&ckDashboard/Images/C2C.png?raw=true)

Right if we wanted to create something like above, we fire up Splunk, Navigate over to the main search windows, select dashboards, then create new and then you would have a blank template, from there you go add panel in the top left corner, followed by the panel type on the right hand side.

You then need select what type, for the above look and feel you want to go ahead and select Single Value (the search would also be slightly different for these as you would want to add a count to assist the graphic)

sourcetype="wineventlog:windows powershell" OR EventCode=403 OR EventCode=400 OR EventCode=600 earliest=-1440m latest=now index=main | stats count as Total

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/MitreAtt&ckDashboard/Images/nopsearch2.png?raw=true)

You can then go right ahead and pop in your search terms, for this I one I want to see if certain powershell event codes are being created (see above) *** it's note worthy too that I have blacklisted and white listed a whole bunch of event codes in the lab so it's less license intensive, makes it easier to find things too. If you have a whole bunch of automagic panels be sure to offset the refresh rates as your dashboard and system will just become unresponsive.

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/MitreAtt&ckDashboard/Images/Nop%20Visual.png?raw=true)

You can then confirm your visualization and also your formatting, I don't want to go into too much detail here, you can see that the options are pretty straight forward.

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/MitreAtt&ckDashboard/Images/nopColour.png?raw=true)

And that's all there is to it really, the events in a normal view would look like the below and take up a lot of space so unless you see something of interest you don't really want to be going through potentially 100's of logs.

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/MitreAtt&ckDashboard/Images/nop.png?raw=true)


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

