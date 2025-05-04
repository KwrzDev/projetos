import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True  # essencial para comandos de texto

bot = commands.Bot(command_prefix='.', intents=intents)


# Evento de inicialização
@bot.event
async def on_ready():
    print(f"🤖 Bot conectado como {bot.user}")


# Comando .ajuda
@bot.command(name='ajuda')
async def ajuda(ctx):
    embed = discord.Embed(
        title="Help | Informativos",
        description="**Comando**\n*Descrição do comando*\n`Outros modos de usar`",
        color=discord.Color.yellow()
    )
    embed.add_field(name=".ip", value="Status do servidor principal da **Rede Light**.\n`.status`", inline=False)
    embed.add_field(name=".invite", value="Sistema de Invites.", inline=False)
    embed.add_field(name=".ping", value="Meu tempo de resposta.", inline=False)
    embed.add_field(name=".serverinfo", value="Vê as informações do servidor do Discord.\n`.sinfo`", inline=False)
    embed.add_field(name=".site", value="Link direto das lojas da **Rede Light**.\n`.loja`", inline=False)
    embed.add_field(name=".tutoriais", value="Tutoriais recomendados pela rede.\n`.tuto`", inline=False)
    embed.add_field(name=".userinfo", value="Vê as informações de alguém ou de si mesmo se não mencionar ninguém.\n`.uinfo`", inline=False)
    embed.set_footer(text="Rede Light")
    await ctx.send(embed=embed)


# Comando .ip (ou .status)
@bot.command(name='status')
async def status(ctx):
    await ctx.send("🌐 O servidor da **Rede Light** está online!")

@bot.command(name='ip')
async def ip(ctx):
    await status(ctx)


# Comando .invite
@bot.command(name='invite')
async def invite(ctx):
    await ctx.send("🔗 Link de convite: https://discord.gg/seuservidor")


# Comando .ping
@bot.command(name='ping')
async def ping(ctx):
    latency = round(bot.latency * 1000)
    await ctx.send(f"🏓 Pong! Latência: {latency}ms")


# Comando .serverinfo (ou .sinfo)
@bot.command(name='serverinfo')
async def serverinfo(ctx):
    guild = ctx.guild
    embed = discord.Embed(title="📊 Informações do servidor", color=discord.Color.green())
    embed.add_field(name="Nome", value=guild.name, inline=False)
    embed.add_field(name="ID", value=guild.id, inline=False)
    embed.add_field(name="Membros", value=guild.member_count, inline=False)
    if guild.icon:
        embed.set_thumbnail(url=guild.icon.url)
    await ctx.send(embed=embed)

@bot.command(name='sinfo')
async def sinfo(ctx):
    await serverinfo(ctx)


# Comando .site (ou .loja)
@bot.command(name='site')
async def site(ctx):
    await ctx.send("🛒 Loja da Rede Light: https://seudominio.com/loja")

@bot.command(name='loja')
async def loja(ctx):
    await site(ctx)


# Comando .tutoriais (ou .tuto)
@bot.command(name='tutoriais')
async def tutoriais(ctx):
    await ctx.send("📚 Tutoriais da Rede Light: https://seudominio.com/tutoriais")

@bot.command(name='tuto')
async def tuto(ctx):
    await tutoriais(ctx)


# Comando .userinfo (ou .uinfo)
@bot.command(name='userinfo')
async def userinfo(ctx, member: discord.Member = None):
    member = member or ctx.author
    embed = discord.Embed(title="👤 Informações do usuário", color=discord.Color.orange())
    embed.add_field(name="Nome", value=str(member), inline=True)
    embed.add_field(name="ID", value=member.id, inline=True)
    embed.add_field(name="Criado em", value=member.created_at.strftime("%d/%m/%Y"), inline=False)
    embed.add_field(name="Entrou no servidor em", value=member.joined_at.strftime("%d/%m/%Y") if member.joined_at else "Desconhecido", inline=False)
    if member.avatar:
        embed.set_thumbnail(url=member.avatar.url)
    await ctx.send(embed=embed)

@bot.command(name='uinfo')
async def uinfo(ctx, member: discord.Member = None):
    await userinfo(ctx, member)


# ⚠️ IMPORTANTE: Substitua pelo seu token verdadeiro aqui
bot.run("")
