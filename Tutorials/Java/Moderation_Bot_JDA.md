# Creating a simple moderation bot

This tutorial assumes you have basic knowledge on JDA. If you don't know how to setup a project, please read [this tutorial](https://github.com/DiscordBotList/DBL-Tutorials/blob/tutorials/Tutorials/Java/Getting%20started%20with%20JDA.md)

## Step 1

Create a new project and add JDA as a dependency (again, if you don't know how refer to the tutorial above)

In this tutorial, we'll be using [JDA-Utilities](https://github.com/JDA-Applications/JDA-Utilities) to handle our commands, so be sure to [add it as a depencency](https://github.com/JDA-Applications/JDA-Utilities/wiki/Setup#using-gradle) as well. Check the latest version [here](https://bintray.com/jagrosh/maven/JDA-Utilities).

Our build.gradle now looks like this:
```gradle
plugins {
    id'java'
    id'application'
    id'com.github.johnrengelman.shadow' version '2.0.1'
}

group 'com.example'
version '1.0-SNAPSHOT'

mainClassName = 'com.example.jda.Bot'

version '1.0'

sourceCompatibility = 1.8

repositories {
    jcenter()
}

dependencies {
    compile 'net.dv8tion:JDA:3.5.0_331'
    compile 'com.jagrosh:jda-utilities:2.0'
}

compileJava.options.encoding = 'UTF-8'
```

## Step 2

Now, we'll use JDA-Utilities' CommandClient to handle our command processing. 

This is what a basic command looks like:
```java
public class ModBot {
    public static void main(String[] args) throws LoginException {
        CommandClientBuilder commandClientBuilder = new CommandClientBuilder();

        //our prefix is !!
        commandClientBuilder.setPrefix("!!");

        //"Type !!help"
        commandClientBuilder.useDefaultGame();
        
        commandClientBuilder.addCommand(new HelloWorldCommand());
        
        new JDABuilder(AccountType.BOT)
                .setToken("your-token-goes-here")
                .addEventListener(commandClientBuilder.build())
                .buildAsync();
    }

    public static class HelloWorldCommand extends Command {
        public HelloWorldCommand() {
            this.name = "helloworld";
            this.aliases = new String[]{"hw"};
            this.help = "says hello";
        }

        @Override
        protected void execute(CommandEvent commandEvent) {
            commandEvent.reply("Hello world!");
        }
    }
}
```

### GuildController

All mod actions in JDA (such as banning, kicking, adding or removing roles, server muting, server deafening, etc) are done through the `GuildController` class.

To get the GuildController for a Guild, call it's `getController()` method.

### RestActions

All JDA methods that interact with discord's [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer) return an instance of the `RestAction` class.

A RestAction represents a request that will be done. However, the request will be executed only when you call `queue()`, `complete()`, or `submit()` on the RestAction object.
If you don't call one of those methods, the action will **never be executed** and your bot will not work.

Whenever possible, try using the `queue()` method and it's [overloads](https://beginnersbook.com/2013/05/method-overloading/), as they do not [block](https://en.wikipedia.org/wiki/Blocking_(computing)) the current [thread](https://en.wikipedia.org/wiki/Thread_(computing)), so other operations aren't slowed down.

### Kick

For now, let's make a kick command. It'll be executed as `!!kick @Person`, where Person is who you want to kick. For this command, we'll use the `Message#getMentionedMembers()` and `GuildController#kick` methods.

This is what our kick command looks like:

```java
@Override
protected void execute(CommandEvent commandEvent) {
    Guild guild = commandEvent.getGuild();

    //if we're not in a guild we can't kick anyone
    if(guild == null) {
        commandEvent.reply("You must run this command in a server");
        return;
    }
    
    Member author = commandEvent.getMessage().getMember();
    
    //the author can't kick people
    if(!author.hasPermission(Permission.KICK_MEMBERS)) {
        commandEvent.reply("You don't have permission to kick people!");
        return;
    }

    List<Member> mentionedMembers = commandEvent.getMessage().getMentionedMembers();

    if(mentionedMembers.isEmpty()) {
        commandEvent.reply("You must mention who you want to be kicked");
        return;
    }

    guild.getController().kick(mentionedMembers.get(0)).queue(success->{
        commandEvent.reply("Successfully kicked " + mentionedMembers.get(0).getUser().getName());
    }, error->{
        commandEvent.reply("Unable to kick " + mentionedMembers.get(0).getUser().getName() + ": " + error);
    });
}
```

You might be wondering what the `x->{}` syntax is. If you don't know, they're [lambda expressions](https://www.tutorialspoint.com/java8/java8_lambda_expressions.htm), which are similar to javascript's closures or python's lambdas.

### Ban

The way to ban is almost equal to the way you kick, but there's one additional argument you must give to the `GuildController#ban()` method, which is how far the banned person's messages will be deleted.

The implementation of this command is left as an exercise for you.

### Softban

A softban is basically a ban and unban, to kick and delete an user's messages. Let's implement one now by modifying the ban command:

```java
guild.getController().ban(member, 7 /* delete all messages from the member in the last week */).queue(done->{
    guild.getController().unban(member.getUser()).queue(done2->{
        commandEvent.reply("Softbanned " + member.getUser().getName() + "#" + member.getUser().getDiscriminator());
    }, error->{
        commandEvent.reply("Error unbanning: " + error);
    };
}, error->{
    commandEvent.reply("Error banning: " + error);
});
```

### Mute

To mute, we'll need a Mute role. For now, we'll just give the person the first role named `Muted` we find:

```java
Guild guild = ...
GuildController controller = guild.getController();

List<Member> mentionedMembers = commandEvent.getMessage().getMentionedMembers();

if(mentionedMembers.isEmpty()) {
    commandEvent.reply("You must mention who you want to be kicked");
    return;
}

Member toMute = mentionedMembers.get(0);
  
Role muteRole = guild.getRoles().stream().filter(r->r.getName().equals("Muted")).findFirst().orElse(null);
  
if(muteRole == null) {
    commandEvent.reply("No role named 'Muted' found");
    return;
}

controller.addSingleRoleToMember(toMute, muteRole).queue(success->{
    commandEvent.reply("Successfully muted " + toMute.getUser().getName());
}, error->{
    commandEvent.reply("Unable to mute " + toMute.getUser().getName() + ": " + error);
});
```

This example uses the java [stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html) and [optional](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) APIs

Again, the actual command implementation is left as an exercise for the reader.

### Tempmute

We'll mute someone for 1 hour, then unmute. Let's change our mute command:

```java
controller.addSingleRoleToMember(toMute, muteRole).queue(success->{
    commandEvent.reply("Successfully muted " + toMute.getUser().getName());
    controller.removeSingleRoleFromMember(toMute, muteRole).queueAfter(1, TimeUnit.HOURS, success2->{
        commandEvent.reply("Successfully unmuted " + toMute.getUser().getName());
    }, error->{
        commandEvent.reply("Unable to unmute " + toMute.getUser().getName() + ": " + error);
    });
}, error->{
    commandEvent.reply("Unable to mute " + toMute.getUser().getName() + ": " + error);
});
```

This uses the [`RestAction#queueAfter`](https://github.com/DV8FromTheWorld/JDA/blob/907f766537a18b610ed8a2cedf95cf6754cf50ee/src/main/java/net/dv8tion/jda/core/requests/RestAction.java#L535-L566) method, which means our pending unmutes will be lost when the bot is restarted. To keep track of them even across restarts, we'd need to save them in a database (do *not* save data in json files, that always backfires), but that's out of the scope of this tutorial.

### Curse Word Filter

To create our filter, we'll need to add a new listener to our JDA object. Let's create a new class and extend `ListenerAdapter`:
```java
public class CurseWorldFilter extends ListenerAdapter {
    
}
```

Now, let's override the `onGuildMessageReceived(GuildMessageReceivedEvent)` method:
```java
@Override
public void onGuildMessageReceived(GuildMessageReceivedEvent event) {
        
}
```


Let's also add a curse word list:
```java
public class CurseWorldFilter extends ListenerAdapter {
    private static final List<String> CURSE_WORDS = Arrays.asList(
            "insert", "curse", "words", "here", "but", "only", "in", "lower", "case", "please"
    );

    @Override
    public void onGuildMessageReceived(GuildMessageReceivedEvent event) {
        
    }
}
```

Now, let's see if there's a curse word and delete the message if it has one:
```java
public class CurseWorldFilter extends ListenerAdapter {
    private static final List<String> CURSE_WORDS = Arrays.asList(
            "insert", "curse", "words", "here", "but", "only", "in", "lower", "case", "please"
    );

    @Override
    public void onGuildMessageReceived(GuildMessageReceivedEvent event) {
        //get the raw content (what the user typed) and convert to lower case
        //this is why we have to put curse words as lower case in the list above
        String message = event.getMessage().getContentRaw().toLowerCase();
        
        //for each curse word in the curse words list
        for(String curseWord : CURSE_WORDS) {
            //check if the message contains it
            if(message.contains(curseWord)) {
                //the message contains a curse word
                //let's see if we can delete it
                
                //if we don't have permission to delete messages we'll just warn and return
                if(!event.getGuild().getSelfMember().hasPermission(event.getChannel(), Permission.MESSAGE_MANAGE)) {
                    System.out.println("No permission to delete messages in #" + event.getChannel().getName());
                    return;
                }
                
                //try deleting it
                event.getMessage().delete().queue(ddone->{
                    //if it was deleted, warn the user
                    event.getChannel().sendMessage(event.getAuthor().getAsMention() + ", you cannot say that!").queue();
                }, error->{
                    //if we got an error deleting it print it
                    System.out.println("Error deleting message with curse word");
                    error.printStackTrace();
                });
                return;
            }
        }
    }
}
```

If we run our bot now, we'll notice it completely ignores curse words. That's because we didn't register the listener. Now let's go back to our main class and change `.addEventListener(commandClientBuilder.build())` to `.addEventListener(commandClientBuilder.build(), new CurseWorldFilter())` so our listener is registered.

Run the bot again and you'll notice it's now detecting curse words and deleting them.
