# Creating a bot with JDA

We'll build a really basic bot with the [JDA](https://github.com/DV8FromTheWorld/JDA) discord API wrapper.

In this tutorial, we'll use the [IntelliJ IDEA](https://www.jetbrains.com/idea/) IDE, created by Jetbrains.

This tutorial assumes you have a JDK 8+ installed and the JAVA_HOME environment variable is set to it.

## Step 1

[Download](https://www.jetbrains.com/idea/download/#section=windows) and install IDEA (The community edition is enough)

## Step 2

Once you open IDEA, you'll see this screen ![Yes I know it's outdated](https://this-is.definitely-not-a-sketchy.host/34b8ea3359.png)

Click on `Create New Project`, then select `Gradle` and mark just `Java`, like this ![Screenshot](https://this-is.definitely-not-a-sketchy.host/57a7a714c4.png)

## Step 3

Choose a group and artifact ID, they can be anything you want, but usually the group id is the reverse of a domain you own, so `mywebsite.com` becomes `com.mywebsite` and the artifact id is an identifier for the project, such as `my-jda-bot` ![Screenshot](https://this-is.definitely-not-a-sketchy.host/04c5ac9953.png)

You don't need to change any gradle settings, but personally I like to enable auto import. ![Screenshot](https://this-is.definitely-not-a-sketchy.host/27cb66091a.png)

After that, choose a project name and where to save it and then click on `Finish`. ![Screenshot](https://this-is.definitely-not-a-sketchy.host/a3ad3b3e78.png)

## Step 4

Wait for gradle to finish configuring your project, and you should see a screen like this ![Screenshot](https://this-is.definitely-not-a-sketchy.host/495400670e.png)

Open build.gradle and let's add JDA as a dependency ([check the latest version here](https://bintray.com/dv8fromtheworld/maven/JDA/)) ![build.gradle](https://this-is.definitely-not-a-sketchy.host/05ba05e357.png)

If you get a dialog like this showing up, click `Import Changes` (it won't show up if you enabled auto import) ![Dialog](https://this-is.definitely-not-a-sketchy.host/71e1a51042.png)


## Step 5

Now, we'll create our main class. Open in the file viewer `src/main` and right click on `java`, then go to New -> Java Class ![Screenshot](https://this-is.definitely-not-a-sketchy.host/1c9e5a3a13.png)

Give your main class a name and click on `OK` ![Screenshot](https://this-is.definitely-not-a-sketchy.host/22228eed1b.png)

Now, we'll create a main method (hint: type `psvm` until a popup shows and then hit enter) ![Screenshot](https://this-is.definitely-not-a-sketchy.host/ff16bc80ed.png)

## Step 6

Now, we'll create our JDA instance, using the JDABuilder class ![Screenshot](https://this-is.definitely-not-a-sketchy.host/7e282317d3.png)

To continue, you need a discord bot token, which you can get on the [applications](https://discordapp.com/developers/applications/me) page

Add the token to your JDABuilder using the `setToken(String)` method ![Screenshot](https://this-is.definitely-not-a-sketchy.host/e1417b49e0.png)

## Step 7

It's finally time to log in to discord with your bot! To build the JDA instance and connect to discord, simply call the method `JDABuilder#buildAsync()` ![Screenshot](https://this-is.definitely-not-a-sketchy.host/c0407e9e5a.png)

You're probably wondering why that line is red. If we hover the mouse above it, we'll see a message explaining what's wrong ![Screenshot](https://this-is.definitely-not-a-sketchy.host/8268931668.png)

An exception, huh? For now, we'll just declare the main method as `throws LoginException` ![Screenshot](https://this-is.definitely-not-a-sketchy.host/0c2e7fbbaa.png)

Now, click the green play button to the left of our class name and select `Run` ![Screenshot](https://this-is.definitely-not-a-sketchy.host/9fa9f3ef30.png)

You'll see something like this being printed to your console ![Screenshot](https://this-is.definitely-not-a-sketchy.host/8b5ebd238c.png)

The first 6 lines are because we don't have any [slf4j](https://www.slf4j.org) implementations on our project, but we can ignore those *for now*.

The next 3 lines tell us that JDA has successfully logged in to discord, and is ready to receive messages. But our bot doesn't do anything right now.

## Step 8

To have our bot do something, we need to add a listener to our JDA instance. For now, let's have our main class extend `ListenerAdapter` and override the onMessageReceived method ![Screenshot](https://this-is.definitely-not-a-sketchy.host/500234a501.png)

Now, let's make it reply with `Pong!` if the message is `!ping` ![Screenshot](https://this-is.definitely-not-a-sketchy.host/b02ec3af10.png)

To finalize, we register a new instance of our main class in the JDABuilder ![Screenshot](https://this-is.definitely-not-a-sketchy.host/c324da1c24.png)

## Step 9

Now, when we re-run out bot, it'll respong with `Pong` when we run `!ping`

![Screenshot](https://this-is.definitely-not-a-sketchy.host/526bc0191a.png)

## Step 10

Now that you know how to build a basic bot, we need to export it into a runnable jar file. To do so, we'll use the gradle shadow plugin. Here's how our build.gradle looks now ![Screenshot](https://this-is.definitely-not-a-sketchy.host/8a3e236c46.png)

To export our project, open the gradle tab on the right and double click on Tasks->shadow->shadowJar ![Screenshot](https://this-is.definitely-not-a-sketchy.host/5b5c4e3ceb.png)

The file will be in PROJECT_ROOT/build/libs, and can be ran with the `java` command: `java -jar ourjar.jar`

## Note

If you don't want to risk your bot entering into infinite message loops, add this to your listener ![Screenshot](https://this-is.definitely-not-a-sketchy.host/c7d9de0f9b.png)
