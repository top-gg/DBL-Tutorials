# Setting up your development work space

## Introduction

So you’ve seen all these amazing bots such as Misaki, Dyno, Luca and Mee6, and you want to try your own hand at creating your own Discord Bot and you have no idea where to start?

Well dear reader! You're in luck, this tutorial will teach you the utmost basics to get your development environment set up to writing your first line of code and beyond!

If you're reading this tutorial guide, then you've chosen to use **Discord.js**, personally speaking I believe that you've made the correct choice.

>***NOTE:*** _I'm running Microsoft Windows 10 x64, please make sure you download the correct version for your Operating System_

## Node

Okay, before we can do anything you should go download [node.js](https://nodejs.org/en/), we need to grab the "LTS" which stands for `Long Term Support`, at the time of this tutorial it's version 8.9.4

![Node 8.9.4 LTS](https://i.imgur.com/DInISCi.png)

Click the LTS button and save the installer somewhere you can remember, then navigate to the location and run the installer.

You should be greeted with this screen.

![Install Node](https://i.imgur.com/SqClxwE.png)

All you need to do right now is follow the on screen instructions until you hit this screen...

![PATH](https://i.imgur.com/al8slRU.png)

Click on the bottom option **Add to PATH** and it should pull up the following list, make sure you select the one that is highlighted, this step is **VITAL** to the entire process.

![Click This](https://i.imgur.com/qlmceuf.png)

Let me explain what that option does.

It adds the `node` and `npm` to the windows PATH variable, which means you can run those as commands in the command prompt from any location on your machine, this is extremely vital for what you want to do.

Alright, once node has finished installing go a head and close the installer and anywhere on your desktop, right click and select **either** `Open command window here` or `Open PowerShell window here`

![Powershell](https://i.imgur.com/2zqiYHw.png)

Once you've got the terminal of your choice open, type `node -v` to tell you what version you've got installed.

![Node Version](https://i.imgur.com/HI56OLW.png)

As you can see, node installed correctly, but if it didn't follow the previous steps.

## Software

Now we need to choose which software to use, now the problem is there's a fair few choices of software out there, but there's 3 free programs that are extremely popular... did I mention they're free?

I personally use [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, it's an amazing bit of software, the other free popular programs are [Atom](https://atom.io/) by GitHub, and [Sublime Text Editor 3](https://www.sublimetext.com/3), ultimately it's up to you which software you pick to install.

Now, there's a very very good reason as to why _"Notepad++"_ isn't on that list.

# BECAUSE IT'S NOT PROGRAMMING SOFTWARE

Alright now I've got that out of the way, I will explain why, programs such as Visual Studio Code, Atom and Sublime Text Editor 3 come with something called [_intellisense_](https://code.visualstudio.com/docs/editor/intellisense) in some form, it's extremely handy it can auto-complete, offer suggestions on what you're wanting to do aka code-hinting, whilst notepad++ does not, it's just a glorified step up from the infamous notepad that comes with Windows.

If you highlight a curly brace, square bracket or regular bracket, it's partner will be highlighted. Believe me when I say that feature can be a life safer and if you install a linter (which we will be covering next), your code _should_ be as problem free as the software can make it.

Again as I previously stated I use Visual Studio Code so I will show you how to install and correctly configure an Eslinter extension.

On the left hand side, you should see a square symbol, that's the extensions button, click it, and in the search box type in "Eslint", click the one by `Dirk Baeumer`

![Click here](https://i.imgur.com/ILttrY0.png)
![Type eslint here](https://i.imgur.com/GozyzKS.png)

>***NOTE:*** _I already have the extension installed, but that button will say `Install` for you_

![Click Install](https://i.imgur.com/VVyprXH.png)

Okay, now that will do for the eslinter for now. We will cover the important step in the next tutorial which is correctly configuring it for your project.

## Process Management

Now you've installed Node (as well as NPM), and you've installed a code editor, the last thing we should install is a process manager.

I bet you're asking why you need a process manager in the first place, well it's simple really.

I personally use [nodemon](https://www.npmjs.com/package/nodemon), and when ever you save a file it will automatically restart the process, and with Node you need to restart the process with every save otherwise the changes you've made won't be loaded.

There are many great process managers out there, such as PM2 and Forever, those are fantastic for production (_finished bots that are deployed_), but not so great for development.

Which is why I use nodemon, all you need to do to install it is open up your terminal again, but this time with Administrative permissions and type `npm i -g nodemon`

![Feel free to ignore those warnings](https://i.imgur.com/Nf88t3C.png)

Alright, if you get a screen similar to the above then it's installed just fine, and I guess that covers this tutorial.

Congratulations, you've successfully set up your development environment, in the next tutorial we'll get into the best bits, actually creating your bot!