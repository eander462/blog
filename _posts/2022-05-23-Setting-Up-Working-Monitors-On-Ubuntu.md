---
title: "Making Monitors Work-Ubuntu Edition"
author: "Erik Andersen"
date: "2022-05-23T12:29:41+08:00"
output: 
  html_document:
    toc: true
    toc_float: true
    keep_md: yes
---



## Intro




This blog is going to focus on the important issues. You know, the real down to earth stuff that affects everyone. *I'm pretty sure it's a writing rule not to start a sentence with "and" but it's a stupid rule so only change it if you want to* And not just first world issues either, we will practice inclusiveness. To demonstrate my commitment, this first post will be all about making sure your dual monitor setup runs smoothly when you switch to linux. 

*This sentence is very long, you could break it up into two* If you're anything like me (and I assume you are), you took a machine learning class in college, and realized windows doesn't let you run code in parallel as UNIX based systems, so you *very* sneakily convinced you dad you needed a new 500 GB ssd to dual boot linux on. Then, as soon as you booted it up, the OS decided that your secondary monitor should really be the main one: *very* frustrating. Power user that you are (you swapped to linux after all), you used all of your skills from years of using windows, booted up the [Nvidia menu](https://forum.manjaro.org/t/how-do-you-use-nvidia-x-server-settings-exactly/96678), toggled the switch that swaps your default monitor, then calmly went about your business. Now your monitors were working perfectly, and you were all ready to be a new linux hacker until... you restarted your computer and it swapped back. This was when you first learned that in linux many settings aren't persistent like they are on windows, and *gasp* this is a [feature](https://stackoverflow.com/questions/15977171/why-when-i-restart-the-terminal-the-environment-variables-are-restarted){target="_blank"}! *When you say this is a feature do you mean that the settings aren't persistent? It's a little confusing*

I have two goals for this post. *colon here?* Nominally, it is about how to use Nvidia settings correctly on Ubuntu 20.04 and above, but the more important message I hope to convey is how to how to go about trouble shooting a linux system **when you first come from windows.** *This is a little clunky* There is extensive documentation online of just about everything you can do on a linux system. But, for a brand new user, its not very helpful. You have to know what you want to do, how to look it up, and then what to do with that knowledge. In the long-run, this will be a much more useful skill than knowing the [xrandr](https://www.x.org/releases/X11R7.5/doc/man/man1/xrandr.1.html)[^1] command is the correct one to swap your display settings. Next time, you (and I) won't have my blog post with the perfect answer available[^2], so knowing the basics of how to problem solve in linux will be necessary. Hadley Wickham preaches having a [scientific mindset](https://adv-r.hadley.nz/introduction.html#meta-techniques) when doing data science; you should *have/practice* the same when working in linux.

[^1]: Note: Since the first draft, I have found a different (possibly easier) way to solve this for Nvidia cards. The solution is linked [here](https://askubuntu.com/questions/379483/nvidia-x-server-settings-lost-on-every-reboot)

[^2]: I definitely will be SEO boosting this thing, so it will appear over Stack overflow for anyone trying to solve this issue

## Solution[^3]

[^3]: Note: this probably isn't the best solution, but it works, and its one I discovered myself, so it fits the spirit of this blog.

I don't want to be accused on my very first blog post of being one of those cooking blogs where they bury the recipe after their whole life story (ignore the three paragraphs above), so I will skip to the punchline and start out telling you the commands to make your monitors work correctly. 

### What's the command?

The command we'll be using is **xrandr**. Xrandr stands for *resize, rotate, and reflect extension*. This is a powerful command that lets you tinker with pretty much any setting you could think of for your monitors. Let's look at the output first to see what we're dealing with.^[^4]

[^4]: I don't think there's any vulnerable info here from my pc, but if there is please don't hack me


```bash
xrandr -q
```

```
## Screen 0: minimum 8 x 8, current 3840 x 1080, maximum 32767 x 32767
## DP-0 connected 1920x1080+0+0 (normal left inverted right x axis y axis) 597mm x 336mm
##    1920x1080     60.00 + 143.98*  119.98   119.93    59.94    50.00  
##    1680x1050    119.99    59.95  
##    1440x900     119.85    59.89  
##    1280x1024    119.96    75.02    60.02  
##    1280x720      59.94    50.00  
##    1024x768      75.03    70.07    60.00  
##    800x600       75.00    72.19    60.32    56.25  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    72.81    59.94    59.93  
## DP-1 disconnected (normal left inverted right x axis y axis)
## HDMI-0 connected primary 1920x1080+1920+0 (normal left inverted right x axis y axis) 598mm x 336mm
##    1920x1080     60.00*+  59.94    50.00  
##    1280x1024     75.02    60.02  
##    1280x720      60.00    59.94    50.00  
##    1152x864      75.00  
##    1024x768      75.03    60.00  
##    800x600       75.00    60.32  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    59.94    59.93  
## DP-2 disconnected (normal left inverted right x axis y axis)
## DP-3 disconnected (normal left inverted right x axis y axis)
## DP-4 disconnected (normal left inverted right x axis y axis)
## DP-5 disconnected (normal left inverted right x axis y axis)
## USB-C-0 disconnected (normal left inverted right x axis y axis)
```

It looks confusing but don't worry, there's only one bit of information we need from here: the monitor names.[^5] The monitor names are the strings on the left called DP-0 or HDMI-0. I have a lot of options, but only those two are connected. As you can see my HDMI-0 is the primary monitor, which is wrong. I want it to be DP-0. To switch it I run the following command.

[^5]: You can also use the following command to extract the names directly, but I find regular expressions a bit confusing, so I currently prefer to do it manually. xrandr | grep -e " connected"

```bash
xrandr --output DP-0 --primary
```


```
## Screen 0: minimum 8 x 8, current 3840 x 1080, maximum 32767 x 32767
## DP-0 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 597mm x 336mm
##    1920x1080     60.00 + 143.98*  119.98   119.93    59.94    50.00  
##    1680x1050    119.99    59.95  
##    1440x900     119.85    59.89  
##    1280x1024    119.96    75.02    60.02  
##    1280x720      59.94    50.00  
##    1024x768      75.03    70.07    60.00  
##    800x600       75.00    72.19    60.32    56.25  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    72.81    59.94    59.93  
## DP-1 disconnected (normal left inverted right x axis y axis)
## HDMI-0 connected 1920x1080+1920+0 (normal left inverted right x axis y axis) 598mm x 336mm
##    1920x1080     60.00*+  59.94    50.00  
##    1280x1024     75.02    60.02  
##    1280x720      60.00    59.94    50.00  
##    1152x864      75.00  
##    1024x768      75.03    60.00  
##    800x600       75.00    60.32  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    59.94    59.93  
## DP-2 disconnected (normal left inverted right x axis y axis)
## DP-3 disconnected (normal left inverted right x axis y axis)
## DP-4 disconnected (normal left inverted right x axis y axis)
## DP-5 disconnected (normal left inverted right x axis y axis)
## USB-C-0 disconnected (normal left inverted right x axis y axis)
```

It swapped! For many of you, this is all the info you need, but for beginning linux users (like me mostly), this still isn't perfect. We know how to swap the primary monitor, but we still have to run this script on every time on startup. There are a number of this to try that I'll discuss later on, but to start out with I'll show you the option I settled for. We're going to make our command into a script, then put that script in our **[.bashrc](https://www.journaldev.com/41479/bashrc-file-in-linux)** file.

### How to make it run[^6]

[^6]: While writing this post, I figured out a better solution to running the script on startup, see the final section.

We could just put the command directly into the .bashrc file, which would probably be fine since it's only a single line, but it is better practice[^7] to make script files, then have .bashrc call the scripts. So, let's make the script. 

[^7]: Source my dad, a 60 year old senior dev.

We'll start by creating a text file that contains the command from above. 


```bash
# Go to the demonstration directory

cd ../playground

# This line assigns assigns the command from above to the file monitor.txt. I have to echo it and put it in quotes, or bash will put the output from the command into the text file. 

echo "xrandr --output DP-0 --primary" > monitor.txt

# Now we print the text file to the screen to check it worked

cat monitor.txt

# Note, you can just edit a text file directly with vim or nano, but this lets me show the whole process without making a gif.
```

```
## xrandr --output DP-0 --primary
```

Great! Now we have a text file[^8] containing our command, but its not executable yet so it won't work. We have to work with linux file permissions^[I recommend chapter 9 of [this](https://nostarch.com/tlcl2) book for a good explanation of how the linux permissions system works if you are curious to learn more.] to make it work. I won't go in depth here, but the spark notes versions should be enough for our purposes. 

[^8]: Fun fact about linux, [file extensions](https://medium.com/@smohajer85/file-extensions-in-linux-c619690941c4) don't *usually* matter. We could have named our file monitor.png, and it would still work exactly the same except for in a few edge cases. It is good practice to give your file a logical extension name though to avoid confusion.

So what are file permissions? Simply, they are a way for the computer to know who is allowed to do what. Linux systems were designed not with personal computers in mind, but rather large centralized computers accessed by many people all the time. The file permission system lets this work without users being able to ruin the whole system for each other. There are three types of access a file can have: read, write, and execute shown by the r, w, and x below.


```bash
# Reset the directory because knitting sets us to the project directory every time

cd ../playground

# The permissions are shown on the left. They look like -rw-rw-r--

ls -lh
```

```
## total 4.0K
## -rwxrwxrwx 1 eander462 eander462 31 May 23 19:30 monitor.txt
```

The permissions are the first thing shown in the second line after the ##. So, our fancy monitor.txt file has permissions -rw-rw-r--[^9]. Very impressive! To make it run, we will have to give execution permission to the file. We'll do that with the **[chmod](http://manpages.ubuntu.com/manpages/trusty/man1/chmod.1.html)** command. There are two ways of telling the command what permissions we want to give, but this is the spark notes version, so see one of the links in this paragraph for more information; all we need to know is that to give every type of user full permission, we use the following command. 

[^9]: Why are the rw's repeated three times? Well, it has to do with [user groups](https://www.redhat.com/sysadmin/manage-permissions). There are three levels of permissions, each can be uniquely given access to a combination of read, write and execute. This isn't important for our purposes here, since this is a harmless command, and we will just give everyone full permission

```bash
# Reset the directory because knitting sets us to the project directory every time

cd ../playground

# Change the permissions. 777 Tells the command that we want to give each user group full permissions

chmod 777 monitor.txt

# See now the permissions give each user access to reading, writing, and executing

ls -lh 
```

```
## total 4.0K
## -rwxrwxrwx 1 eander462 eander462 31 May 23 19:30 monitor.txt
```

And now we've successfully hacked (read: done a simple, standard operation) linux. There's just one more step to get this bad boy running: we need to put it in our .bashrc file[^10], so it executes when we startup our terminal on first boot. 

[^10]: Or .zshrc if you use zshell.

```bash
# Make sure to use >> instead of > like we did last time. This is VERY IMPORTANT. If you just use a single > it will over write your .bashrc file to only say monitor.txt which is definitely what you want.

# Alternatively, you can just directly edit .bashrc using your preffered editor

echo "monitor.txt" >> .bashrc
```

And we're done! Now, when you first run your terminal on startup the monitor.txt program will run and set up your monitors correctly. This may have seemed like a lot,  but there were really only three steps, and remembering how to get a script to run when you boot up your computer is a powerful tool, and will set you well on your way to be a cool linux hacker. *This is a very long sentence*

This isn't the end of the blog. It would be the end of most blogs, but I'm usually left with a feeling that blog writings just know how to do everything, and don't struggle to figure out the complex topics their blogs deal with. I don't want that to be the case here. I was entirely new to linux when I tried to make my monitors work correctly, and it took me three days to work out how to run the few basic steps I've described. In the rest of this post, I want to show you to process of troubleshooting and discovery of the solution. I hope it will be helpful in solving the next issue that comes up.

## But wait! How did you figure that out?

How do we start out troubleshooting a problem? Well, my first instinct was to call my dad and hope he already knew the solution, but he hates talking on the phone, so this solution probably won't work for you. *Annoying*. Guess we'll have to use that [scientific mindset](https://adv-r.hadley.nz/introduction.html#meta-techniques) I talked about earlier.[^11] This section is mostly going to be a narrative of my problem solving process. I learned about many new system features like cron while trying to work it out, so I hope that my learning will be helpful to you (and future me).

[^11]: I've only been a linux user for half a year, but I'm already much better at problem solving than I was at the start. This troubleshooting walk through is going to be at a very basic level, but for a good reason; this tutorial is aimed at past me who spent three days trying to learn linux from scratch and not knowing the most basic of things. A few of the methods I show are going to look dumb, but that is how I learned, so I hope it will be a good resource.

The first step to science (at least computer science) is always to google it. When I google "change primary monitor ubuntu", I get [this](https://askubuntu.com/questions/300670/is-there-any-ability-to-set-my-primary-monitor). Confusingly for a new linux user, not one of the responses give the correct answer, or even mentions using the command line. Coming from windows where you never interact with the terminal, moving away from the safe [gui](https://en.wikipedia.org/wiki/Graphical_user_interface) toggle switches to the scary maroon maze of the command line is a hurdle. I'll save you my pain, but it took a few hours of googling and trying their solutions to learn that I was going to need to use the terminal. 

With the new found power of the command line we'll update the google: ["change primary monitor ubuntu command line"](https://superuser.com/questions/155004/switch-monitors-from-the-command-line). And there we go; the top answer is the solution I came to using xrandr[^12]. Excellent. At this point, I was spinning around in my chair celebrating, scaring my cats away, thinking I was done finally. I copied the command into my terminal[^13], ran it and went on with my day. 

[^12]: A quick warning. I hadn't gotten command line syntax down perfectly at this point, so I typed the command as "xrandr --output  'DP-0' --primary" with DP-0 in quotes. Don't do this. The command runs and does what it is supposed to, but it throws up an error every time which I ignored. When I ran it automatically on startup, the error mattered. The OS refused to launch the desktop environment, and I was stuck trying to tell the OS the error was fine and it could continue the normal startup process. I recommend using the [cheat](https://github.com/cheat/cheat) command line utility for figuring out fiddiliy things like this.

[^13]: Pro(ish) tip: ctrl-c, and ctrl-v don't work in the terminal because the terminal was build before windows ever created those commands. Instead use ctrl-shift-c/v or middle click to copy paste.

Sadly, upon reboot my monitors swapped back and I despaired thinking I would have to type the xrandr command into my terminal every time I booted up the computer. On the advice of my dad, I turned the command into a script in the process I described above. We couldn't initially figure out how to run the script on startup, so for a few week, I had to type "./monitor" every time I booted the system. That was totally unacceptable, so I needed to figure out how to run a script on startup.

### Built in startup

Ubuntu comes with a built-in method for running scripts on startup, so let's start by trying that. To access the startup menu, press the [super key](https://help.ubuntu.com/stable/ubuntu-help/keyboard-key-super.html.en) (the windows key or the command key depending on where your keyboard is from), type in "startup", and push enter.  A GUI will open with three buttons on the right. Press the top-most which says "Add", name the script something helpful, then type in the path to the script we created earlier. Click add, and we should be good. Great let's test it! A quick reboot, and... nothing. Hmm we'll have to try something else. 

> **Update:** This method will work, if you don't neglect to put a **[shebang](https://linuxize.com/post/bash-shebang/)** at the top of your script like I did. The shebang tells the computer what to use to interpret the command with. If we add shebang "#! /bin/bash" to the top of the monitor file, the startup program will run it. This is actually a better solution than the one I present above because it doesn't require running the terminal on startup everytime. I'm going to leave the blog how it is because I think it better fits the theme I'm going for of being scientific and exploring the OS. It took me writing the blog post and retrying all of my potential solutions here to learn this solution. 

### .config folder

After some more quick googling, I decided to try the .config folder. This is a command line process, so I can show you how to do it this time. If we look inside our .config folder, we find that one of the subfolders is called "autostart". Let's look in there. 


```bash
ls ~/.config/autostart
```

```
## monitor.desktop
## nvidia-settings-autostart.desktop
## snake-razer.desktop
```

The nvidia and snake files I have in there are extra additions by me, so we'll focus on the monitor.desktop file. We'll use **[cat](https://man7.org/linux/man-pages/man1/cat.1.html)** to view the contents.


```bash
cat ~/.config/autostart/monitor.desktop
```

```
## [Desktop Entry]
## Type=Application
## Exec=/home/eander462/monitor
## Hidden=false
## NoDisplay=false
## X-GNOME-Autostart-enabled=true
## Name[en_US]=Montior Swap
## Name=Montior Swap
## Comment[en_US]=Swaps the monitor to have the left one be the default
## Comment=Swaps the monitor to have the left one be the default
```

This looks awfully like the command we just put into the startup menu. Turns out, its exactly that! The startup menu puts the scripts to run into this folder. This is a behind the scenes look at how the linux OS actually works. Most operations are reading in data from a text file stored somewhere on the system. Unfortunately, if this is just our input from earlier, its not going to solve our problem^[Except that it did perfectly solve the problem.], so let's move onto cron. 

### Cron

[Cron](https://vitux.com/how-to-schedule-tasks-on-ubuntu-20-04-using-crontab/) is a very helpful system resource that allows users to schedule tasks to happen at certain times. I was directed away from cron immediately in my quest because my dad didn't know that cron supports running programs on reboot, but it turns out it does! Let's try it out. 

This is going to be a very cursory examination of cron because there is much to explore[^14]. To open your crontab write "crontab -e" into the terminal. If you don't have a crontab yet this will create one for you. Inside, we just need to write down what we want the computer to do and when. We want it to execute our monitor script at reboot. To run a task on reboot, we use "@reboot". To specify the script to run, we just give the computer the path. So we put the following into our crontab.

[^14]: [This](https://crontab.guru/) is a helpful resource to schedule your cron jobs.


```bash
@reboot ~/monitor
```

Press ctrl-x to exit cron, press y to save the changes, and we're ready to test it out. One reboot later and... nothing again^[It probably works if we did the shebang properly.] Very frustrating. 

Full disclosure, this process took place over the course of about a month for me. I found solutions that worked well enough, then left it for a while until I felt inspired to troubleshoot some more. Reading it as a blog makes it seem like a very linear process to troubleshoot, but its not. Its a long, frustrating process full of mostly failures.

This brings us to the .bashrc solution I presented above. This works, but has downsides. You have to launch the terminal on startup to flip your monitors, and every time you open a new terminal, it runs the command again which sometimes blacks out the monitors for a few seconds. At the time, I theorized that this was the only solution that worked because of the [x-windows](http://manpages.ubuntu.com/manpages/impish/man1/Xserver.1.html) system. This is what allows a system to have a GUI. I hypothesized that my xrandr command needed this to be running, and that the startup commands ran before x-windows had launched. This turns out not to be the case; the problem was I hadn't put a shebang in the file, so the system didn't know what to run it with. The file executed in the .bashrc file because that file runs everything through bash. 

## Conclusion

Here we are at the end of our troubleshooting journey, and whirlwind tour of various linux features. You are now prepared to go out and be a hacker by delving into the depths of you linux OS. My hope is what you take away from this blog is that linux is complicated, especially for beginners. But, the only way to solve your problems is to experiment. You're (probably) not going to hurt your computer, and even if you do its fairly easy to reload from a prior backup. Don't let the newness *novelty?* scare you, just give it a go and try it out! 





































