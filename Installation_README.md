Install Instructions
===

linux/macOS
===
1. Navigate to a location to where you wish zyb0t to live.
2. And place zyon-fresh.zip there.
3. Enter zybot directory.
4. Give zyb0t-linux permission to execute
5. COMPLETE your zybotconfig.js.

### the codez:
1. `cd ~`
2. `unzip zyon-fresh.zip`
3. `cd zybot`
4. `chmod u+x zyb0t-*`
5. `nano zybotconfig.js`
### **make necessary edits to config file**

# "WRITE_TO_CONFIG"  
  - This enables or disables zyb0T from saving to your gunbot config.js
# "telegram_username"
 - This is your telegram username, do not include the @ symbol               
# "telegram_chatId"
 - This is your user specific chat id. It can be found by going to @myidbot
# "telegram_token"
 - This is your bot_token from @BotFather
# "telegram_debug_chatID"
 - IGNORE THIS for right now, it was initially placed to 'display' chat_id from above, within the console to 'get' it instead of using @myidbot
# "gunbot_directory"
  - This is the location where gunbot lives.
  - What zybot looks at:
    - state files within /json
    - config.js

# "client_key":
 - This is your license key, it will be used to generate your machine specific license.txt 

# "exchanges":
```javascript
    "binance": {
        //pair zybot will look at
        "USDT-BTC": "MM_SPOT" 
                    // which strat zyb0t will preform
    },
```
  - This is the similar format as which you would find within your gunbot config.js pairs section
  - zyb0T will only run strategies upon the pairs you list within this zybotconfig.js
    - **Secondary note**: 'only run strategies...' the telegram /pairedit feature will still allow you to 'manage' all pairs within your gunbot config.

# Example zybotconfig.js
```json
    {
      "WRITE_TO_CONFIG": true,                      
      "telegram_username": "zy0nbear",
      "telegram_chatId": 123456789,
      "telegram_token": "1233456789:zbcdefghijklmnopwrstuvwxyz1234567890",
      "telegram_debug_chatID": false,
      "gunbot_directory": "/home/zy0n/spot-beta",
      "client_key": "205febe26eac4c13112fa66c595262039d2b54a9abf68a8e9c2292535904cd1f94c94cea5b",
      "exchanges": {
        "binance": {
          "USDT-BTC": "MM_SPOT"
        },
        "binanceFutures": {
          "USDT-BTC": "LIFEVEST"
        }
      }
    }
```



Okay heres some fun stuff for you all, to make life easier for managing your VPS
===

linux/macOS
===========


# These use the app 'screen' for managing your application processes

- Make sure its installed before adding and running these helper functions.

- Navigate to your user home directory
        cd ~
- Look in the directory, Locate file .bashrc or .zshrc
- NOTE: .zsh is the equivalent to .bashrc on macOS

        ls -a | grep .*rc
Also look for .bash_aliases (this is really where you should be adding this stuff, but you will only find on linux and older macOS builds)
===
        ls -a | grep .bash_aliases

- Open that file in your favorite cli-text editor (nano,vi.. blah blah)
        nano ~/.bash_aliases

# GBHOME 
 - Edit to be the location at which gunbot lives
# ACHOME 
- Edit to be the location at which zybot lives

~/.bash_aliases file
===
```bash
        GBHOME=/home/path/to/gunbot
        GBFILE=gunthy-linux
        ACHOME=/home/zybot
        ACFILE=zyb0t-linux
        launchgb () { echo LAUNCHING GUNBOT && cd $GBHOME && screen -d -m -L -S spot ./$GBFILE;}
        launchac () { echo LAUNCHING zyb0T && cd $ACHOME && screen -d -m -L -S zyon ./$ACFILE;}
        gbconf () { cd $GBHOME && nano config.js;}
        gbac () { cd $GBHOME && nano autoconfig.json;}
        gbpv () { cd $GBHOME && nano autoconfig-pairVariables.json;}
        watchgb () { cd $GBHOME && tail -f screenlog.0;}
        watchac () { cd $ACHOME && tail -f screenlog.0;}
        edita () { nano ~/.bash_aliases;}
        resrc () { source ~/.bashrc; }
        killgb () { echo TERMINATING GUNBOT && pkill -f gunthy-linux; }
        killac () { echo TERMINATING zyb0T && pkill -f zyb0t-linux; }
```
### exit nano, by pressing ctrl + s to save, then ctrl + x to exit
        source ~/.bashrc


REAL IMPORTANT NOTES PLEASE READ
===
        It is important that if you are using ~/.bash_aliases for this that you 'source ~/.bashrc'   
        NOT I REPEAT NOT NOT NOT ~/.bash_aliases
        You may soley edit ~/.bashrc and place the functions at the bottom of that file, 
        ~/.bashrc automatically loads ~/.bash_aliases if the file is avaialbe, 
        this is where users 'store' these types of things.

- Now you may use the commands, heres what they do:
- If you try a command, and it says permission denied: try running with sudo, ie:   sudo launchgb
- You may do them at any directory of your VPS

# launchgb
- This command starts gunbot under a screen named 'spot'
- to manually view this screen, type: 
      screen -r spot
# watchgb
- This opens up a tail process and allows you to 'scroll' the view of your gunbots screenout log.
  **press ctrl + c to exit**
# killgb
- This will kill your current gunbot processes
# launchac
 - Starts up zyb0t under a screen named 'zyon'
 - to manually view this screen, type: 
        screen -r zyon
# watchac
- This opens up a tail process and allows you to 'scroll' the view of your zybot screenout log.
**press ctrl + c to exit**
# killac
- This will kill your zyb0t process.
# gbconf
- This opens up your gunbot config.js into nano editor
  - exit nano, by pressing 
**ctrl + s to save, then ctrl + x to exit**
# gbac 
- This opens up your gunbot autoconfig.json file into nano editor
  - exit nano, by pressing 
**ctrl + s to save, then ctrl + x to exit**
# gbpv
- This opens up your gunbot autoconfig-pairVariables.json file into nano editor
  - exit nano, by pressing 
**ctrl + s to save, then ctrl + x to exit**

Script Editor HELPER functions
===
# edita
- This opens up ~/.bash_aliases in nano editor.
# resrc
- This MUST be used to force any changes made to ~/.bash_aliases to become active.
- Use this if you ADD something to ~/.bash_aliases, and desire to 'use it'
- It will only load on its own when you 'close terminal' and open a fresh.




