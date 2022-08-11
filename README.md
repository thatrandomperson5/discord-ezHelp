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

from ezHelp.utils import cogSplit #cogSplit to give you cog info
import ezHelp

...

class disHelp(ezHelp.dynHelp):
    def on_page(self, commands, page, last_page):
        commands = list(commands)
        embed = rqEmbed(self.context, title=f"Global Chan {commands.pop(len(commands)-1)[1]} [{page}/{last_page}]")
        
        for x in commands:
            index = x[0]
            command = x[1]
            
            embed.add_field(name=f"{index}) {self.get_command_signature(command)}", value=command.short_doc,inline=False)
        return embed
    def on_main(self, commands, page, last_page):
        #if self.no_category
        embed = rqEmbed(self.context, title=f"Global Chan [{page}/{last_page}]")
        
        
        name = None
        value = None
        print(commands)
        for x in commands:
            
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
        
bot.help_command = disHelp(no_category = 'Informational', commands=cogSplit)
```
