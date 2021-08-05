
 
# What does eZ-MM-SPOT do?
### Will write some fun stuff here soonz <3
  



        All of these overrides are required. Values are up to you.
        I recommend you keep FASTBB and SLOWBB at what I have provided you.
        Also for initlizing you may start Z_TIMER with any value, it will automatically adjust properly. 
        Start with quote balance less than MIN_VOLUME_TO_SELL 

        Wallet Size Recommendation: 10-20k
        Any wallet can be accomodated for really by adjusting the TL_TIERS and TIMER_MULT to ease the wallet burn.

## Contact me for the telegram group link.
- telegram: **@zy0nBear**
- email: zy0n.bear@protonmail.com
-----------

# "FAST_BB"
### This is the period of the Fast Bollinger Band
- Value Type: `Whole Number`
- Example Value: `"FAST_BB": 30`
    
# "SLOW_BB"    
### This is the period of the Slow Bollinger Band
- Value Type: `Whole Number`
- Example Value: `"SLOW_BB": 200`
 -----------

# "STATIC_DELAYS"
- Value Type: `Integer Array - seperated by ','` **(comma)**
- Example Value: `"STATIC_DELAYS": "80,120,140,160"`
### What this does 
- Sets a value for the corresponding TL tier.
So in this example for tl 20 the delay would be 80, tl 40 delay would be 120, tl 80 delay would be 160 and tl 100 delay would be 160

# "TL_TIERS"
### Controls the TIER setup of TL based on the location of fastBB.mid compared to the Slow Bollinger Band zones.
- Value Type: `Integer Array, seperated by ', '` **(comma+space)**
- Example Value: `"TL_TIERS": "20, 40, 80, 100"`
    
### More Details:
        zone 1        fastBB.mid > slowBB.high it would use 20
        zone 2        fastBB.mid < slowBB.high && fastBB.mid > slowBB.mid it would use 40
        zone 3        fastBB.mid < slowBB.mid && fastBB.mid > slowBB.low it would use 80
        zone 4        fastBB.mid < slowBB.low it would use 100
 -----------

# "MAX_SPOT_SELL"
### Number of allowed sell orders to be in the orderbooks. Enabled by QUANTUM_BREAK
- Value Type: `Whole Number`
- Example Value: `"MAX_SPOT_SELL": 1`
    
# "MAX_SPOT_BUY"
### Number of allowed buy orders to be in the orderbooks.  Enabled by QUANTUM_BREAK
- Value Type: `Whole Number`
- Example Value: ` "MAX_SPOT_BUY": 1`
    
# "QUANTUM_BREAK"
### This activates restricting of the order books and enables my super smart CANCEL_SPREAD logic.
- Value Type: `Boolean`
- Example Value: `"QUANTUM_BREAK": true`
### More Details:
- When true, it only allows MAX_SPOT_SELL # of sell orders to be active at one time, and MAX_SPOT_BUY # of buy orders to be active at one time
### A bit about this CANCEL_SPREAD logic:
        Based on the current orderBooks ask spreads from the previous candles low, and bid spreads from the previous candles high.
        What this means is:
        When buy or sell order count becomes greater than MAX_SPOT_BUY or MAX_SPOT_SELL it sets a 
        cancel spread small enough to close the excess orders.
        When BUYING or SELLING is displayed as DISABLED in the console readout:
        it will attempt to cancel any orders placed on the books pertaining to whichever is disabled.
        When this limit is not reached it sets cancel spread to keep the current orders active.
        When QUANTUM_BREAK is not active, it will set cancel spread based off of calculated gain.
        
# "UBER_CANCEL"
### This forces all orders to be canceled, once either MAX_SPOT_BUY or MAX_SPOT_SELL # of orders has been exceeded
- Value Type: `Boolean`
- Example Value: `"UBER_CANCEL": true`
-----------
    
# "PROFIT_SNAKE"
### When active (a value larger than 0) it represents the gain value at which you wish to override dancing style sell criteria.
- Value Type: `Whole Number`
- Example Value: ` "PROFIT_SNAKE": 1.5` 
  - Meaning if position comes into 1.5% profit and dancing style is still saying don't sell. We allow selling.

# "SLOWDOWN_SNAKE": true,
### When Enabled, this applies the current STATIC_DELAY between PROFIT_SNAKE sell orders.
- Value Type: `Boolean`
- Example Value: `"UBER_CANCEL": true`
    
# "PROFIT_CLIPPER"
### When active (a value larger than 0) it represents the number of candles to take into consideration of low. 
- Value Type: `Whole Number`
- Example Value: `"PROFIT_CLIPPER": 3`
  - When pair.Ask drops below the lowest value of the last 3 candleslow values
  - STOP_LIMIT is triggered, and the position is closed.
### THIS FEATURE ONLY WORKS WHEN IN PROFIT. 
  -There are no additional safety checks to determine how close you are to negative gain. Caution.

# "PROFIT_TRAIL"
### When active (a value larger than 0) it represents in a WHOLE number, the % away from the highest gain reached to initate profit stop_loss 
- Value Type: `Number`
- Example Value: `"PROFIT_TRAIL": 23.2`
  - This acts as a 'Trailing System' for using STOP_LOSS in profit.

# "HEAVY_BAGS"
### HEAVY_BAGS and PANIC_BAG_DROP both must be ABOVE 0 for this feature to ACTIVATE.
### When active (a value larger than 0) it represents the % of used wallet at which we allow the triggering of PANIC LOSS SELLING
- Value Type: `Number`
- Example Value: `"HEAVY_BAGS": 32.3`
  - What this means is if your position grows to a size larger than 32.3% of your wallet balance, we will enable stop_loss protection on this pair.
  - This is triggered by reaching PANIC_BAG_DROP negative gain. 
### **PLEASE NOTE THIS WILL CAUSE IMMEDIATE LOSS OF A PORTION OF YOUR WALLET BALANCE WHEN CONDITIONS ARE MET**

# "PANIC_BAG_DROP"
### When active (a value larger than 0) it represents the % of **negative gain** reached at which to drop the HEAVY_BAGS sized bags. 
- Value Type: `Number`
- Example Value: `"PANIC_BAG_DROP": 5`
  - This will trigger STOP_LOSS selling when your position's **NEGATIVE GAIN** is larger than 5

-----------

# "SOFT_DUMP"
### This value creates a 'NO-BUY-ZONE' at SOFT_DUMP Percent of Price below FAST_BB low band.
- Value Type: `Number`
- Example Value: `"SOFT_DUMP": 2`
- If the lower band of the FAST_BB is at 100, it will stop letting GUNBOT place orders if pair.Ask drops below 98
    
# "HARD_DUMP"
### This value creates a 'NO-BUY-ZONE' at HARD_DUMP Percent of Price below SLOW_BB low band.
- Value Type: `Number`
- Example Value: `"HARD_DUMP": 5`
  - If the lower band of the SLOW_BB is at 100, it will stop letting GUNBOT place orders if pair.Ask drops below 95

# "FRUITYMODE"
### When Enabled, this forces GUNBOT to only allow buying below (lastBuyRate - (pair.atr * FRUIT_SPREAD))
### This allows orders to not bunch up in the same 'price zone'
`OPTIONAL OVERRIDE`
- Value Type: `Boolean`
- Example Value: `"FRUITYMODE": true`
# "FRUITY_RSI"
### RSI value to allow buying below
- Value Type: Number
- Example Value: `"FRUITY_RSI": 30`
  - This will only allow buying to happen when RSI is below 30
  - STATIC_DELAYS are not taken into account when FRUITY_RSI allows trading.

# "FRUIT_SPREAD"
### FRUIT_SPREAD * current ATR value = spread between buying. 
- Value Type: `Number`
- Example Value: `"FRUIT_SPREAD": 2`
  - This will keep a spread of pair.atr * FRUIT_SPREAD between your last order and your next.
  - STATIC_DELAYS are taken into account when the bot is making trades based on FRUIT_SPREAD.

# "FRUIT_EXPIRES"
### **REQUIRED if using FRUITYMODE** This givea an expiration timer to FRUITYMODE, it allows buying to resume 'above your last buy rate' after FRUIT_EXPIRES hours.
- Value Type: `Number`
- Example Value: `"FRUIT_EXPIRES": 2.3`
-----------

# "EUCAMODE"
### When Enabled, this forces GUNBOT to only allow selling above (lastSellRate + (pair.atr * EUCA_SPREAD))
### This allows orders to not bunch up in the same 'price zone'
`OPTIONAL OVERRIDE`
- Value Type: `Boolean`
- Example Value: `"EUCAMODE": true`
# "EUCA_RSI"
### RSI value to allow buying below
- Value Type: Number
- Example Value: `"EUCA_RSI": 60`
  - This will only allow selling to happen when RSI is above 60
  - STATIC_DELAYS are not taken into account when EUCA_RSI allows trading.

# "EUCA_SPREAD"
### EUCA_SPREAD * current ATR value = spread between buying. 
- Value Type: `Number`
- Example Value: `"EUCA_SPREAD": 2`
  - This will keep a spread of pair.atr * EUCA_SPREAD between your last order and your next.
  - STATIC_DELAYS are taken into account when the bot is making trades based on EUCA_SPREAD.

# "EUCA_EXPIRES"
### **REQUIRED if using EUCAMODE** This givea an expiration timer to EUCAMODE, it allows buying to resume 'below your last sell rate' after EUCA_EXPIRES hours.
- Value Type: `Number`
- Example Value: `"EUCA_EXPIRES": 2.3`
-----------

# "ZY0N_ABP"
### Is a secondary break-even calculator.
- Value Type: `Boolean`
- Example Value: `"ZY0N_ABP": true`
    
# "ABP_DEBUG"
### only enable if I ask you to. 
- Value Type: `Boolean`
- Example Value: `"ABP_DEBUG": false`
    
# "CYCLE_FINDER"
### This should be ENABLED, if you are using ZY0N_ABP
### Do not turn off without knowing its purpose. Please contact me for information if you have questions.
- Value Type: `Boolean`
- Example Value: `"CYCLE_FINDER": true`
    
# "CYCLE_MM_RESET"
### THIS SHOULD BE ENABLED, if you are using ZY0N_ABP
### Do not turn off without knowing its purpose. Please contact me for information if you have questions.
- Value Type: `Boolean`
- Example Value: `"CYCLE_MM_RESET": true`

# "TL_PROTECT"
### This DISABLES TRADING_LIMIT from being set to any value that will cause an order.
- Value Type: `Boolean`
- Example Value: `"TL_PROTECT": false`
  - In the last line of my code, before I send the 'overrides' to be written, it checks for this value
  - If it is true, it will set ' '+0 as your TL to prevent a real order from being placed 



# "ALLOW_MMSR"
### Manually initiates a SR like sell system.
 - Value Type: Boolean
 - Example Value: `"ALOW_MMSR": false`
 - It disables buying when this mode is true. so don't forget its on once you reduce.**
    - The reduction happens like this:
        - It takes your current pair bagValue and divides it by MMSR_RATIO
        - So that means if your bags are 10k and you set MMSR_RATIO to 5, it will set 2000 USDT as your TRADING_LIMIT
          - this SR activates also only when your bags are over 60% used and you initiate it by flipping the toggle.

# "MMSR_RATIO"
### This is the integer that your bagValue is divided by to set as TL to use as emergency bag reduction.
- Value Type: `Number`
- Example Value: `"MMSR_RATIO": 5"`


# "DANCING_STYLE"
- Value Type: `Number (valid values) 1-27`
- Example Value: `"DANCING_STYLE": 26"`
This setting determines which trading rules the bot will follow. They are described below.

                                              TRADING STYLES
        pair.Ask is the reference point. 
        Example reading of trading style 1:
                    trading only when pair.Ask is below fastBB.high and pair.Ask is above fastBB.low
-----------
1. unrestricted trading
2. only above fastBB.high or below fastBB.low
3. only above fastBB.high or below fastBB.mid
4. only above fastBB.mid or below fastBB.low
5. only (above fastBB.high and above slowBB.high)  or (below fastBB.low && below slowBB.low)
6. only above fastBB.high or below fastBB.low and slowBB.low
7. only above fastBB.high when In Position and Positive Gain or below fastBB.mid
8. only above fastBB.high when In Position and Positive Gain or below fastBB.low
9. when fastBB.mid is above slowBB.mid
       - we trade below fastBB.mid and above slowBB.mid
    - when fastBB.mid is below slowBB.mid - we trade above fastBB.mid and below slowBB.mid
10. only above fastBB.mid
11. only below fastBB.mid
12. only above slowBB.mid
13. only below slowBB.mid
14. selling only above fastBB.high when In Position and Positive Gain or buying only below fastBB.mid and above fastBB.low when in Negative Gain
15. selling only above fastBB.high when In Position and Positive Gain or buying only below fastBB.high and above fastBB.low when in Negative Gain
16. selling only above fastBB.mid when In Position and Positive Gain or buying only below fastBB.mid and above fastBB.low when in Negative Gain
17. selling only above fastBB.mid when In Position and Positive Gain or buying only below fastBB.high and above fastBB.low when in Negative Gain
18. selling only above fastBB.mid when In Position and Positive Gain or buying only below fastBB.low and market isnt dumping below HARD_DUMP % from slowBB.low when in Negative Gain
19. selling only above fastBB.mid when In Position and Positive Gain or buying only below fastBB.low and below slowBB.low and market isnt dumping below HARD_DUMP % from slowBB.low when in Negative Gain
20. only below fastBB.high and above fastBB.low
21. selling only above fastBB.mid when In Position and Positive gain or buying only below fastBB.mid when market isnt dumping below SOFT_DUMP % from fastBB.low
22. selling only above fastBB.mid when In Position and Positive gain or buying only below fastBB.high when market isnt dumping below SOFT_DUMP % from fastBB.low
23. trading always above the bbFast.low
24. trading always bove the bbSlow.low
25. trading disabled.
26. selling only above fastBB.high when In Position and Positive gain or buying only below fastBB.low when market isnt dumping below SOFT_DUMP % from fastBB.low
27. selling only above fastBB.mid when In Position and Positive gain or buying only below fastBB.mid when market isnt dumping below SOFT_DUMP % from fastBB.low
28. selling only when in Profit, and in Position or buying when Not in Profit
-----------


# OVERRIDES AUTOMATTICALY SET, DO NOT TOUCH THEM
### "Z_TIMER"
### "ZY_STATS"
### "ZY_ITB"
### "MOST_BAL_USED"
 -----------

# "SECRET_SAUCE"
### EXPERIMENTAL FULLY 707l0l
- Value Type: `SECRET Boolean`
- Example Value: `"SECRET_SAUCE": true`
- Does the funk
