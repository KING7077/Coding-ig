```py
import discord
from discord.ext import commands, pages
from embeds_Stuff import *

bot = commands.Bot(command_prefix='!!')


class Help(commands.HelpCommand):
    def __init__(self):
        super().__init__()
        self.pages = [new_embed_1, new_embed_2, new_embed_3, new_embed_4]

    def get_pages(self):
        return self.pages

    async def send_bot_help(self, mapping):
        # here, you can use a custom paginator if you want
        # I have a few embeds saved up in another file
        # you can of course use the embeds along with the paginator buttons
        paginator = pages.Paginator(pages=self.get_pages())
        paginator.add_button(pages.PaginatorButton(
            "first", label=None, emoji="<:first:914408236877238292>", style=discord.ButtonStyle.green))  # first page paginator button
        paginator.add_button(pages.PaginatorButton(
            "prev", label=None, emoji="<:prev:914409708117450782>'", style=discord.ButtonStyle.green))  # prev page paginator button
        paginator.add_button(pages.PaginatorButton(
            "next", label=None, emoji="<:next:914410408486527036>'", style=discord.ButtonStyle.green))  # next page paginator button
        paginator.add_button(pages.PaginatorButton(
            "last", label=None, emoji="<:last:914410740918677514>'", style=discord.ButtonStyle.green))  # last page paginator button

        # context is the context of the command, like ctx in a normal command
        # starting the paginator, as you would in a normal command
        await paginator.send(self.context)

    # this is a method of commands.HelpCommand, which sends the help message for a command for a specified command
    async def send_command_help(self, command: commands.Command):
        # the command parameter is a commands.Command object
        # every command has a help aka description, a name and a list of aliases, which would be specified in the command decorator
        # so you can access them via command.help, command.name and command.aliases, and put them in an embed if you wish to do so
        # also, every command has a signature, which is basically an example usage
        # for example, v!ban <member> [reason]. This is a command signature for a ban command with the member parameter and the reason parameter
        # if your command has aliases, it would look like so: clean_prefix[command.name|command.aliases[0]|command.aliases[1].....]
        embed = discord.Embed(title=self.get_command_signature(
            command), description=command.help, colour=discord.Colour.blue())
        if command.aliases:
            # adding fields to the embed in case of aliases
            embed.add_field(name="Alias(es)", value=", ".join(command.aliases))

        # sending the embed to the channel
        await self.context.send(embed=embed)

    async def send_error_message(self, error):
        # this is a method of commands.HelpCommand, which sends the error message for a command
        # the error parameter is a string, hence can be put into an embed
        # this is useful for when a command is not found, or when a command is not found in a category, and so on
        # of course, you can customize the embed description
        embed = discord.Embed(title="Error", description=error)
        await self.context.send(embed=embed)


"""
There are many advantages to using the commands.HelpCommand class, over using a custom help command
most tutorials ask you to do bot.remove_command('help'), but then, you do not get the advantages of the in-built commands.HelpCommand
1. commands.HelpCommand has useful methods like send_bot_help and send_command_help
2. The functionality of send_command_help, send_group_help and send_cog_help isn't available if you do bot.remove_command('help')
3. send_error_message is another useful method that can be used here, and can help you construct a better help command

All in all, the better way to construct a help command for your bot is by subclassing commands.HelpCommand as it has many extra functionalities.
I decided to use custom embeds for the send_bot_help as I already had the embeds, but there may be better ways to do the same thing
"""


@bot.command(name='ping', help='Pong!', aliases=['pong'])
async def ping(ctx):
    await ctx.send('Pong!')

bot.help_command = Help()
bot.run('ODU0OTgwMDY2NTkzMzQxNDQw.YMr0PA.NVjYy6UmAM0h2464T4igrgKJJM8')

# Hope you got some information on how to use the commands.HelpCommand class from here
```
