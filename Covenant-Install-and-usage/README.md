# Covenant install and example usage

Covenant Install & Example usage with PowerShell Launcher Updated: Oct 2, 2019 Hey Guys,

So I have been wanting to move away from Metasploit for a little while now and after seeing how our internal Red Team works it was apparent that I needed to open my eyes to more tools. Now I cant afford Cobalt Strike and its doubtful that I would even be granted a copy unless I managed to get it through work, this is due to the triage process that they have to adhere to. So after a little digging and some suggestions from the Red Team, Covenant seemed to be the way to go.

I got Covenant installed and at that point I really wasn’t sure how to use it and after a ton of Googling I still couldn’t really find anything that kinda pointed me in the right direction so I wanted to create a short blog on how to setup a Listener, create a PowerShell Launcher and then hopefully gain your Grunt allowing you to take your attack forward.

Obviously anything you learn here is educational only and anything you do with the knowledge going forward is your responsibility and yours alone. I have my own lab setup so I can test out different tools etc, I would suggest you do the same.

Covenant can be found here -> https://github.com/cobbr/Covenant

Install guide here -> https://github.com/cobbr/Covenant/wiki/Installation-And-Startup

Kali install commands here -> https://dotnet.microsoft.com/download/linux-package-manager/debian9/sdk-2.2.402

Be sure to use DotNet SDK version 2.2 as at the time of writing ver 3.0 will NOT work.

okay so lets get started, first fire up your Kali box and run the following commands in order -

    wget -qO- https://packages.microsoft.com/keys/microsoft.asc 	gpg –dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget -q https://packages.microsoft.com/config/debian/9/prod.list
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
    sudo apt-get update
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install dotnet-sdk-2.2
    git clone –recurse-submodules https://github.com/cobbr/Covenant
    cd Covenant/Covenant
    dotnet build
    dotnet run

That should look something like the below, and by the end look something like this.

Once you’re at this point you should see that Covenant is running and if you navigate over to https://0.0.0.0:7443 you should be good to go. Note that the ip is 0.0.0.0 as Covenant is being ran locally, if this was in AWS for example the ip would be different.

You may need to add an exception so go ahead and do that.

After that you should see the Covenant landing page, happy days your all setup and ready to go, bang in a username and password.

![Image](https://github.com/CyberZombi3/CyberZombi3.co.uk/blob/master/Covenant-Install-and-usage/Images/initial%20logon.png?raw=true)
          
So once logged in you will see all the following options, Listeners, Launchers among others. I’m not going to run through them all, you can look at them all yourself and get to grips with them in your own time.

So what I wanted to go through were the basic steps to get a Grunt running using the PowerShell Launcher, so first we need to create a Listener. Fill in the details, so give it a name and add in your connect address ip, this is the ip of your attacking system.

Next we need to create the Launcher, this time I’m going with the PowerShell Launcher, fill in the options so select your Listener etc etc and what I’m going to do is take hit the Generate button and then copy the Encoded command into a custom script I have. There are multiple ways from here so you could host your launcher from within Covenant, you can down load it and send however you see fit, it’s really up to you.

Encoded command This is then pasted into my script.

And that’s about it really, as suggested above how you have your victim connect to you is personal preference, mostly this would be via a link or phishing email of course.

So for this example I just ran the script on my victim system, again this can be hosted via Covenant or emails etc.

The connection is then activated.

Opening the Grunt provides a whole host of information as seen below.

Then the magic is here in the Interact tab, I have just ran a quick whoami but there are all sorts of tasks already built in and I know that Rastamouse and Cobbr are working hard to create all sorts of awesome new features.

If you have read any of my previous posts you will know that I was working on a Splunk Mitre Att&ck detection app, it looks like the image below and can be downloaded here -> https://github.com/CyberZombi3/Mitre-Attack-Monitoring this is free if you wish to take a look. It’s very high level and would need tuning etc for any work environment however as you can see it would pick up the above method of attack. This should not however be relied on to detect any and all methods of attack.

So I hope this has been of use to someone as always if you want to ask any questions you can find me @CyberZombi3 on the Twitter.

Thanks

CyberZombi3
