# Att&ck Discover & Blue Team

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
