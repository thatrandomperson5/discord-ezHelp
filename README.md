# discord-ezHelp
ez help command customization for discord.py
Get cool embeded and paginated help commands ez
## Installation

```git
git clone https://github.com/thatrandomperson5/discord-ezHelp.git
```
## Usage
```py
import ezHelp #Import the module


...


class disHelp(ezHelp.Help): #Create a subclass
    def on_page(self, commands, page, last_page):  #When the help command needs a page, gives you the commands on the page(enumerated), the current page and the last page
        embed = discord.Embed(self.context, title=f"Global Chan help [{page}/{last_page}]") #Create the embed
        for x in commands: #For each command
            command = x[1] #Get the command
            index = x[0] #Get the index
            embed.add_field(name=f"{index}) {self.get_command_signature(command)}", value=command.short_doc,inline=False)
        return embed #Return the embed         ^ get the signature, like prefix!commandname <option> [True|False] ^ Get the short description

bot.help_command = disHelp(no_category = 'Informational', timeout=10) #Set the help_command to your help command with the timeout of 10s

```

You should get something like this:
![66995883-522A-4CD6-938E-78C1863A148D](https://user-images.githubusercontent.com/94306853/184012653-d47683af-7d52-407f-b321-e0ccce2395cd.jpeg)


## Advanced usage:
```py

from ezHelp.utils import cogSplit #cogSplit to give you cog info that is able to be paginated
import ezHelp

...

class disHelp(ezHelp.dynHelp):
    def on_page(self, commands, page, last_page):  #On cog command
        commands = list(commands)
        embed = rqEmbed(self.context, title=f"Global Chan {commands.pop(len(commands)-1)[1]} [{page}/{last_page}]")
        
        for x in commands:
            index = x[0]
            command = x[1]
            
            embed.add_field(name=f"{index}) {self.get_command_signature(command)}", value=command.short_doc,inline=False)
        return embed
    def on_main(self, commands, page, last_page): #On main help
        #if self.no_category
        embed = rqEmbed(self.context, title=f"Global Chan [{page}/{last_page}]")
        
        
        name = None
        value = None
        print(commands)
        for x in commands:  #Extracting data, then adding to the embed
            
            index = x[0]
            x = x[1]
            print(x)
            if x["type"] == "cog":
                if name is not None:
                    embed.add_field(name=name, value=value, inline=False)
                if x["value"] is None:
                    name=self.no_category
                else:
                    name = x["value"].qualified_name
                value = ""
            elif x["type"] == "command":
                v: commands.Command = x.get("value")
                value += f"{index})  {v}: `{self.get_command_signature(v)}`\n{v.short_doc}\n"
        embed.add_field(name=name, value=value, inline=False)
        return embed
        
bot.help_command = disHelp(no_category = 'Informational', commands=cogSplit)  #Use the cogsplit pre-processer
```
You should get somehing like this:
![62316029-33FF-44E0-992C-437F14DBE36E](https://user-images.githubusercontent.com/94306853/184146949-26a340f1-72ed-4aee-b6e2-7b7eb709b049.jpeg)
![BC6D2972-7B28-4E04-BFBC-370C7FD012A2](https://user-images.githubusercontent.com/94306853/184146953-ac1efce8-4f6d-475f-a0cc-2f008026f42c.jpeg)
