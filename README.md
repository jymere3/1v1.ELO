#IMPORTS
import os
import platform
import requests
import threading
from urllib.request import Request, urlopen
import colorama
from colorama import Fore
from rich.console import Console
from rich.table import Table
import json
import time

os.system('title [EloGGs] 1v1.LOL Booster')

if os.name != "nt":
    exit()
 
#1V1URLS
url1 = 'https://securetoken.googleapis.com/v1/token?key=AIzaSyBPrAfspM9RFxuNuDtSyaOZ5YRjDBNiq5I'
player = 'https://us-central1-justbuild-cdb86.cloudfunctions.net/player'
 
#REFRESHCHECK
try:
    config = json.load(open('config.json'))
except:
    print(f'{Fore.RED}ERROR! PLEASE DOWNLOAD THE CONFIG.JSON FILE!')
    exit()

refresh = config["refresh"]

if refresh == "put-here":
    print(f'{Fore.RED}ERROR! PLEASE PUT YOUR REFRESH-TOKEN INTO THE CONFIG.JSON FILE!')
    exit()
else:
    pass

#KEYCHECK
try:
    os.system('cls')
    userkey_input = f'{Fore.RESET}{Fore.RESET}[{Fore.RED}!{Fore.RESET}] Please Enter your Key: {Fore.YELLOW}'
    userkey = input(userkey_input)
    if len(userkey) < 4:
        print(f'{Fore.RED}ERROR! YOUR KEY IS INVALID. (EXPIRED / ASK OWNERS FOR KEY)')
        addstartup()
        exit()
    else:
        pass
    keyurls = requests.get('https://pastebin.com/raw/YOUR-CODE') # put pastebin code here with user key(s)
    keyurl = keyurls.text
    if userkey in keyurl:
        print(f'[{Fore.GREEN}+{Fore.RESET}] AUTHENTICATED...')
        time.sleep(1)
    else:
        print(f'{Fore.RED}ERROR! YOUR KEY IS INVALID. (EXPIRED / ASK OWNERS FOR KEY)')
        exit()
except Exception as e:
    print(f'{Fore.RED}ERROR! : {e}')
    exit()
 
os.system('cls')
 
def ascii():
    os.system('cls')
    print(f"""{Fore.RESET}{Fore.RESET}
    {Fore.RED}╔══════════════════════════════════════╗{Fore.RESET}{Fore.GREEN}
              ╔═╗╦  ╔═╗  ╔═╗╔═╗╔═╗
              ║╣ ║  ║ ║──║ ╦║ ╦╚═╗
              ╚═╝╩═╝╚═╝  ╚═╝╚═╝╚═╝{Fore.RESET}
    {Fore.RED}╚══════════════════════════════════════╝{Fore.RESET}
        {Fore.GREEN}https://github.com/codeuk/eloggs{Fore.RESET}
    """)
 
def printoptions():
    print(f'    {Fore.RESET}[{Fore.RED}+{Fore.RESET}] {Fore.RED}User{Fore.RESET}@{Fore.RED}ELOGG {Fore.RESET}|{Fore.GREEN} Patched Version {Fore.RESET}')
    print(f"""{Fore.RESET}
      [{Fore.GREEN}1{Fore.RESET}] {Fore.RED}Boosting{Fore.RESET}
      [{Fore.GREEN}2{Fore.RESET}] {Fore.RED}Nickname{Fore.RESET}
      [{Fore.GREEN}3{Fore.RESET}] {Fore.RED}Stats{Fore.RESET}
      [{Fore.GREEN}4{Fore.RESET}] {Fore.RED}Themes{Fore.RESET}
    """)
 
ascii()
printoptions()
 
optioninput = f'    {Fore.RESET}[{Fore.RED}+{Fore.RESET}]{Fore.RED} Option: {Fore.GREEN}'
option = input(optioninput)
 
if option == "1":
    {Fore.RESET}
    pass
 
elif option == "2":
    print()
    nicknameurl = 'https://us-central1-justbuild-cdb86.cloudfunctions.net/player/nickname'
    nickname_input = f'       {Fore.RESET}[{Fore.RED}+{Fore.RESET}] Please enter your desired Nickname: {Fore.YELLOW}'
    nickname = input(nickname_input)
    data = {
        'grant_type': 'refresh_token',
        'refresh_token': refresh
    }
    x = requests.post(url1, data=data)
    wjdataa = x.json()
    try:
        authh = wjdataa['access_token']
    except:
        os.system('cls')
        print(f'{Fore.RESET}[{Fore.RED}-{Fore.RESET}]{Fore.RED} ERROR!{Fore.RESET} Invalid Refresh-Token in config.json, Please get a new one!')
        exit()		
    print()
    print(f'       {Fore.RESET}[{Fore.RED}+{Fore.RESET}] Changing Nickname to [{Fore.GREEN}{nickname}{Fore.RESET}]')
    data = {
        'nickname': nickname
    }
    x = requests.post(nicknameurl, headers={"auth-token": authh}, data=data)
    print()
    print(f'       [{Fore.GREEN}+{Fore.RESET}] Nickname Changed Successfully!')
    time.sleep(3)
 
elif option == "3":
    data = {
        'grant_type': 'refresh_token',
        'refresh_token': refresh
    }
    x = requests.post(url1, data=data)
    wjjdata = x.json()
    try:
        authh = wjjdata['access_token']
    except:
        os.system('cls')
        print(f'{Fore.RESET}[{Fore.RED}-{Fore.RESET}]{Fore.RED} ERROR!{Fore.RESET} Invalid Refresh-Token in config.json, Please get a new one!')
        exit()		
    headerss ={
        'auth-token': authh
    }
    logs = requests.get(player, headers=headerss)
    playerdata = logs.json()
    customrating = playerdata['CustomRating']
    account_nick = playerdata['Nickname']
    boxrating = playerdata['BoxRating']
    try:
        coins = playerdata['HardCurrency']
    except:
        coins = "Never Purchased Coins"
    xp = playerdata['XP']
    kills = playerdata['Stats']['TotalKills']
    deaths = playerdata['Stats']['TotalDeaths']
    totalgames = playerdata['Stats']['TotalGamesPlayed']
    userid = playerdata['ID']
    os.system('cls')
    ascii()
    table = Table(title=f"Account: {account_nick}")
    table.add_column("Stats", style="red")
    table.add_column("Value", style="green")
    table.add_row("Account Name", f"{account_nick}")
    table.add_row("1v1 Elo", f"{customrating}")
    table.add_row("Box Elo", f"{boxrating}")
    table.add_row("Total XP", f"{xp}")
    table.add_row("Total Games", f"{totalgames}")
    table.add_row("Total Kills", f"{kills}")
    table.add_row("Total Deaths", f"{deaths}")
    table.add_row("LOL Coins", f"{coins}")
    table.add_row("Account ID", f"{userid}")
    console = Console()
    console.print(table)
    print()
    restartinput = f"       {Fore.RESET}[{Fore.GREEN}+{Fore.RESET}] Press any key to continue >> "
    restart = input(restartinput)
 
else:
    print()
    print(f'        {Fore.RESET}[{Fore.RED}+{Fore.RESET}] Boosting Themes: {Fore.BLUE}blue{Fore.RESET} / {Fore.RED}red{Fore.RESET} / {Fore.GREEN}green{Fore.RESET}')
    print()
    elotheme_input = f'        {Fore.RESET}[{Fore.RED}+{Fore.RESET}] Please enter what theme you would like: '
    elotheme = input(elotheme_input)
 
    if elotheme == "blue":
        pass
    elif elotheme == "red":
        pass
    elif elotheme == "green":
        pass
    else:
        print()
        print(f"{Fore.RESET}[{Fore.RED}x{Fore.RESET}] {Fore.RED}ERROR - Please enter a valid theme! {Fore.RESET}{Fore.BLUE}blue / {Fore.RED}red{Fore.RESET} / {Fore.GREEN}green{Fore.RESET}")
        exit()
 
 
os.system('cls')

refresh = config["refresh"]
print()
if elotheme == "red":
    refresh_input = f"    {Fore.RESET}[{Fore.RED}+{Fore.RESET}] {Fore.RED}Your Refresh-Token Has been Loaded from the JSON File! Starting Booster...{Fore.RESET}"
elif elotheme == "green":
    refresh_input = f"    {Fore.RESET}[{Fore.GREEN}+{Fore.RESET}] {Fore.GREEN}Your Refresh-Token Has been Loaded from the JSON File! Starting Booster...{Fore.RESET}"
else:
    refresh_input = f"    {Fore.RESET}[{Fore.BLUE}+{Fore.RESET}] {Fore.CYAN}Your Refresh-Token Has been Loaded from the JSON File! Starting Booster...{Fore.RESET}"
 
print(refresh_input)
 
url1 = 'https://securetoken.googleapis.com/v1/token?key=AIzaSyBPrAfspM9RFxuNuDtSyaOZ5YRjDBNiq5I'
url = 'https://us-central1-justbuild-cdb86.cloudfunctions.net/player/updateProgressAndStats?gameMode=1v1_Competitive&isVictory=true&killsCount=0&deathsCount=0&isCompetitive=true&isBox=false'
player = 'https://us-central1-justbuild-cdb86.cloudfunctions.net/player'

def log():
    data = {
        'grant_type': 'refresh_token',
        'refresh_token': refresh
    }
    x = requests.post(url1, data=data)
    wjdataa = x.json()
    try:
        authh = wjdataa['access_token']
    except:
        os.system('cls')
        print(f'{Fore.RESET}[{Fore.RED}-{Fore.RESET}]{Fore.RED} ERROR!{Fore.RESET} Invalid Refresh-Token in config.json, Please get a new one!')
        exit()		
    headerss ={
        'auth-token': authh
    }
    logs = requests.get(player, headers=headerss)
 
def elo():
    data = {
        'grant_type': 'refresh_token',
        'refresh_token': refresh
    }
    x = requests.post(url1, data=data)
    wjdata = x.json()
    auth = wjdata['access_token']
    while True:
        headers ={
            'auth-token': auth
        }
        try:
            x = requests.get(url, headers=headers)
        except:
            print(f'{Fore.RED} ERROR - RATELIMIT, NON-FATAL (CONTINUING)')
        wjjdata = x.json()
        if wjdata == '{}':
            print('Auth-token expired! Loading a new one...')
            elo()
        else:
            if elotheme == "red":
                try:
                    print(f"{Fore.RESET}[{Fore.RED}+{Fore.RESET}] {Fore.RED}" + str(wjjdata['CustomRating']))
                except:
                    print(f"{Fore.RESET}[{Fore.RED}ERROR{Fore.RESET}] RATELIMIT - NON FATAL - WAITING")
                    time.sleep(10)
            if elotheme == "green":
                try:
                    print(f"{Fore.RESET}[{Fore.GREEN}+{Fore.RESET}] {Fore.GREEN}" + str(wjjdata['CustomRating']))
                except:
                    print(f"{Fore.RESET}[{Fore.RED}ERROR{Fore.RESET}] RATELIMIT - NON FATAL - WAITING")
                    time.sleep(10)
            else:
                try:
                    print(f"{Fore.RESET}[{Fore.BLUE}+{Fore.RESET}] {Fore.CYAN}" + str(wjjdata['CustomRating']))
                except:
                    print(f"{Fore.RESET}[{Fore.RED}ERROR{Fore.RESET}] RATELIMIT - NON FATAL - WAITING")
                    time.sleep(10)
log()
os.system('cls')
 
# Start boosting
t = threading.Thread(target=elo).start()
t1 = threading.Thread(target=elo).start()
t2 = threading.Thread(target=elo).start()
t3 = threading.Thread(target=elo).start()
t4 = threading.Thread(target=elo).start()
t5 = threading.Thread(target=elo).start()
