![Discord](https://discordapp.com/assets/e4923594e694a21542a489471ecffa50.svg)

# Voice channel connections with JDA

Hello there, and welcome to the tutorial on connecting/disconnecting bots to and from voice channels!
Whilst it does not sound very exciting or adventerous, it is an important step in creating music bots. 
You have to learn to crawl before you can learn to run.

## Requirements

There are a couple of requirements you must meet before being able to jump straight into the tutorial. 
This tutorial will not guide you through obtaining them, but it will link you to a few resources you will most likely find useful.

### Requirement 1: Java

This requirement may be quite obvious, but before you can code a bot in Java you will need to have Java installed. 
Java essentially comes in two components: the Java Runtime Environment and the Java Development Kit. 
The Java Runtime Environment, also abbreviated as the JRE is what you most likely have installed on your computer, if at all. 
The JRE allows Java programs to run on your computer.
The Java Development Kit, abbreviated as JDK, contains the tools required to code Java. Installing the JDK will install the JRE, but not vice versa.
**It is important that you have the JDK installed**. If not, please check [Oracle's installation guide](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html).

### Requirement 2: IntelliJ

IntelliJ is the IDE that will be used throughout this tutorial. 
IDE stands for Integrated Development Environment, and they are essentially fancy text editors with built-in tools to help you produce quality code.
IDEs will make your life a lot easier, especially with dependency management softwares, such as Gradle.
JetBrains, the developers of IntelliJ, have a detailed [installation guide](https://www.jetbrains.com/help/idea/install-and-set-up-intellij-idea.html).

### Requirement 3: Gradle

Gradle is the dependency management tool that will be used throughout this tutorial. 
It will ensure that your bot can access JDA files, and that everything will run smoothly throughout deployment.
Please refer to the [official installation guide](https://gradle.org/install/).

## Creating an application

In order to write the code for a Discord bot, you will need to actually create a Discord bot. Luckily, this process is easy peasy!

### Step 0

Make sure you are logged in on the Discord website.

### Step 1

Click on [this link](https://discordapp.com/developers/applications/me) in order to be redirected to Discord's application page. 
You should now see something like this (minus the censor):
![Discord](https://i.imgur.com/ePnR9LI.png)

### Step 2

Click on "New App".
The following should appear on your screen.
![Discord](https://i.imgur.com/Do7pjPg.png)

### Step 3

Fill in the application name, and optionally add a description + avatar. 
I named mine Cyborg.
Once you're done, click on "Create App".

### Step 4

Your new application should say hello by greeting you with the following page:
![Discord](https://i.imgur.com/FgIyqWO.png)

### Step 5

Don't mess around with any of the spooky settings, you want to scroll right down until you see the following:
![Discord](https://i.imgur.com/AthhB7q.png)

### Step 6

Hit that "Create a Bot User" button, and acknowledge the fact that this action is irreversible. 
You should then be prompted with the following (obviously your name and/or discriminator will be different):
![Discord](https://i.imgur.com/pTi9Hzn.png)

Don't check any of the buttons!

### Step 7

Now it's time to invite the bot to your server. Create a brand new server, if you haven't already. 
This will be your super secret (or public - your choice) testing laboratory! 
Inviting your bot may seem difficult at first, but it's actually quite simple.

On your application page, there will be a button called "Generate OAuth2 URL". Click that. 
You will be prompted with the following (ID will vary):
![Discord](https://i.imgur.com/jNstTaC.png)

Make sure to only check the "bot" button for the scopes, and check any permissions you want it to have. 
Then, click on the "copy" button, and paste the contents into your browser URL bar.
Wait until the page loads, select your testing guild, and select "Authorize".

### Step 8

Congratulations, you're done! Make sure to click on "Save changes" at the bottom if you haven't already, and close the tab/window.
In order to go to the applications page again, click [here](https://discordapp.com/developers/applications/me), and then click on the application name.


