Home [CyberZombi3](https://cyberzombi3.github.io/CyberZombi3.co.uk/)

# Registry Monitoring 

Hey guys so I've been looking at Registry monitoring and come up with a list of Registry hives / keys.

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/RegMonitoring/Images/RegKeyMonitoring2.png?raw=true)

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

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/RegMonitoring/Images/RegKeyMonitoring.png?raw=true)

Off the back of any results found you can of course configure alerting via email among others, you can also create a Dashboard containing various bits of information, an example can been seen here just below and also at the top of this post.

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/RegMonitoring/Images/RegKeyMonitoring3.png?raw=true)

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
