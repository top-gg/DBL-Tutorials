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

## Writing code

Now comes the exciting part! Writing the actual code. Creating a bot that can join and leave channels is surprisingly simple.
A large of the code is actually just checking whether certain conditions are met. 
Don't get me wrong -- this is equally as important as the connection part.

# Step 1

Firstly, it's time to create a project. Open up IntelliJ, and you should see the following screen:

![IntelliJ](https://i.imgur.com/tJt2XkS.png)

If you see a project open instead, you can get to this screen by clickin on **File** -> **Close Project**. 

Now, click on "Create New Project". You should be prompted with the following:

![IntelliJ](https://i.imgur.com/oiWxhEM.png)

If you do not see this, make sure to click on the gradle tab/window on the left.
For the additional libraries just check Java, nothing else, and click **Next**.

![IntelliJ](https://i.imgur.com/NbwioVQ.png)

We will be following Java package naming conventions throughout this tutorial, so please [read up on those](https://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html).
For the group ID, enter your package name **without** the project name.
Mine is `de.arraying`.
For the artifact ID, enter your project name. 
My bot is called "Cyborg", hence my project name will be `cyborg`.
For the version, you can put whatever version you like.
Click on **Next**.

![IntelliJ](https://i.imgur.com/faHxbXM.png)

Make sure you check "Use auto-import"! Other than that, you don't need to change anything, although you can use the local gradle distribution if you like. 
Click on **Next**.

![IntelliJ](https://i.imgur.com/zbmGf8w.png)

You can leave everything like it is, and click on **Finish**.

## Step 2

You should now be given a window that looks like this:

![IntelliJ](https://i.imgur.com/fjTNknh.png)

*Note: If you look closely, you can see that my project is actually called CyborgExample.
This is because I already have a project called Cyborg, it is the project from which I am copying the code, step by step.*

It may take several seconds, up to a minute, for the `src` content directories to be created. Be patient.

Open up the `build.gradle` file.
Copy and paste the following content into the file:
```gradle
plugins {
    id'java'
    id'application'
    id'com.github.johnrengelman.shadow' version '2.0.1'
}

mainClassName = 'de.arraying.cyborg.Bot' // IMPORTANT

version '1.0.0'

sourceCompatibility = 1.8

repositories {
    jcenter()
}

dependencies {
    compile 'net.dv8tion:JDA:3.5.0_331'
}

compileJava.options.encoding = 'UTF-8'
```

![IntelliJ](https://i.imgur.com/hNp8c4h.png)

It is very important that you **change the mainClassName**! 
This should be set to the following: `<group>.<artifact>.Bot`
In this case, `<group>` is your group ID and `<artifact>` is your artifact ID.

*Note: This tutorial uses the latest version of JDA, currently at 3.5.0_331. In the future, this may be very outdated!*

You may close the `build.gradle` tab.

Now, right-click `src/java`, and press **New** -> **Package**.

![IntelliJ](https://i.imgur.com/C0eXubR.png)

For the package name, enter `<group>.<artifact>`, just like in the previous step, just without the `.Bot`.

Right-click the newly created package, and press **New** -> **Java Class**.

![IntelliJ](https://i.imgur.com/oyM9Bmb.png)

For the name, enter `Bot`, and click **OK**.

IntelliJ will open up a new tab for you, looking something like this (you may have a comment above the class):

![IntelliJ](https://i.imgur.com/yuPth03.png)

Now's it's time to implement the main function!
This is the entry point for your bot.
This tutorial will use runtime parameters to pass in your Discord bot's token. 
Why? Because hard-coding a token is very bad practise, and creating a configuration file handler would be too much for this tutorial.

Add the following code to your bot:

![IntelliJ](https://i.imgur.com/crlSkYk.png)

Now it's time to add JDA, and make the bot connect to Discord.

![IntelliJ](https://i.imgur.com/pclqLPV.png)

With that done, there's nothing stopping you from doing a test run!
Click on **Run** -> **Run**.

![IntelliJ](https://i.imgur.com/wCgncvL.png)

Select "Bot" from the menu.

![IntelliJ](https://i.imgur.com/iLHHGQj.png)

And everything should run. But wait -- there's an error! 
The program doesn't know what your token is, because you never specified it.

Specifying a runtime parameter is really easy.
Click on **Run** -> **Edit Configurations**.

![IntelliJ](https://i.imgur.com/XAAMmZf.png)

Select the "Bot" tab if it hasn't selected by default.
Now you need to get your token. Open the applications page provided by Discord (mentioned earlier).
Scroll down, to the "Bot" section. Then, click "click to reveal", and copy the token.
Now, paste the token under "Environment variables", like this:

![IntelliJ](https://i.imgur.com/4A6P3qb.png)

Click on **OK**. Click on **Run** -> **Run 'Bot'**. You should get a console output like this (username and discriminator may vary):
```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
SLF4J: Failed to load class "org.slf4j.impl.StaticMDCBinder".
SLF4J: Defaulting to no-operation MDCAdapter implementation.
SLF4J: See http://www.slf4j.org/codes.html#no_static_mdc_binder for further details.
[main] INFO JDA - Login Successful!
[JDA MainWS-ReadThread] INFO WebSocketClient - Connected to WebSocket
[JDA MainWS-ReadThread] INFO JDA - Finished Loading!
Logged in as Cyborg#6668!
```

For now, we can ignore the SLF4J warnings.
If you hop over to your testing server, you will notice that your bot has come online! Witchcraft!

### Step 3

You have a working bot that can come online on Discord, but it doesn't connect to voice channels - yet!
That's where this last step comes in. 

Right-click your package, and click on **New** -> **Java Class**.

![IntelliJ](https://i.imgur.com/L9f3VJq.png)

Enter "Handler" as the name, and hit **OK**.

Make the class extend "ListenerAdapter":

![IntelliJ](https://i.imgur.com/wwEnfZb.png)

Go back to `Bot.java`, and add the following line to your JDA code:

![IntelliJ](https://i.imgur.com/91NPAMe.png)

This will register your event handler. Go back to `Handler.java` and add the following method:

![IntelliJ](https://i.imgur.com/Cu0wxvM.png)

You have an event handler that handles the event, and you have a method that handles messages.
Now it's time to add an insanely basic command handler.
The command handler will presume that your prefix is `!`.
Quality bots have a command executor, with command objects, something left out in this tutorial.

![IntelliJ](https://i.imgur.com/l3JxXoh.png)

Add the code (with the checks!) that connects to the user's voice channel:

![IntelliJ](https://i.imgur.com/2TK98Vj.png)

Add the code (again, with the checks!) that disconnects from any voice channel:

![IntelliJ](https://i.imgur.com/OluvvkW.png)

If your code is not yet terminated, you need to do so by clicking the red stop button in the top right corner:

![IntelliJ](https://i.imgur.com/zlatPM1.png)

Run the code (**Run** -> **Run 'Bot'**), and test the functionality!

![Discord](https://i.imgur.com/xRVQ5r0.gif)

![Discord](https://i.imgur.com/2zWapHt.gif)
