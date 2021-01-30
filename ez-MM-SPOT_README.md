
 
# What does eZ-MM-SPOT do?
  



        All of these overrides are required. Values are up to you.
        I recommend you keep FASTBB and SLOWBB at what I have provided you.
        Also for initlizing you may start Z_TIMER with any value, it will automatically adjust properly. 
        Start with quote balance less than MIN_VOLUME_TO_SELL 

        Wallet Size Recommendation: 10-20k
        Any wallet can be accomodated for really by adjusting the TL_TIERS and TIMER_MULT to ease the wallet burn.

## Contact me for the telegram group link.
- telegram: **@zy0nBear**
- email: zy0n.bear@protonmail.com
# "FAST_BB"
### This is the period of the Fast Bollinger Band
    - Value Type: Whole Number
    - Example Value: `"FAST_BB": 30`
# "SLOW_BB"    
### This is the period of the Slow Bollinger Band
    - Value Type: Whole Number
    - Example Value: `"SLOW_BB": 200`
# "TL_TIERS"
### Controls the TIER setup of TL based on the location of fastBB.mid compared to the Slow Bollinger Band zones.
    - Value Type: Integer Array, seperated by ', ' (comma+space)
    - Example Value: `"TL_TIERS": "20, 40, 80, 100"`
### More Details:
        zone 1        fastBB.mid > slowBB.high it would use 20
        zone 2        fastBB.mid < slowBB.high && fastBB.mid > slowBB.mid it would use 40
        zone 3        fastBB.mid < slowBB.mid && fastBB.mid > slowBB.low it would use 80
        zone 4        fastBB.mid < slowBB.low it would use 100
# "MAX_SPOT_SELL"
### Number of allowed sell orders to be in the orderbooks. Enabled by QUANTUM_BREAK
    - Value Type: Whole Number
    - Example Value: `"MAX_SPOT_SELL": 1`
# "MAX_SPOT_BUY"
### Number of allowed buy orders to be in the orderbooks.  Enabled by QUANTUM_BREAK
    - Value Type: Whole Number
    - Example Value: ` "MAX_SPOT_BUY": 1`
# "QUANTUM_BREAK"
### This activates restricting of the order books and enables my super smart CANCEL_SPREAD logic.
    - Value Type: Boolean
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


# "PROFIT_SNAKE"
### When active( value larger than 0) it represents the gain value at which you wish to override dancing style sell criteria.
    - Value Type: Whole Number
    - Example Value: ` "PROFIT_SNAKE": 1.5` 
- Meaning if position comes into 1.5% profit and dancing style is still saying don't sell. We allow selling.

# "PROFIT_CLIPPER"
### When active( value larger than 0) it represents the number of candles to take into consideration of low. 
    - Value Type: Whole Number
    - Example Value: `"PROFIT_CLIPPER": 3`
- When pair.Ask drops below the lowest value of the last 3 candleslow values
- STOP_LIMIT is triggered, and the position is closed.
### THIS FEATURE ONLY WORKS WHEN IN PROFIT. 
-There are no additional safety checks to determine how close you are to negative gain. Caution.
# "PROFIT_TRAIL"      
- When active( value larger than 0) it represents in a WHOLE number, the % away from the highest gain reached to initate profit stop_loss 
    - Value Type: Number 
    - Example Value: `"PROFIT_TRAIL": 23.2`
- This acts as a 'Trailing System' for using STOP_LOSS in profit.

# "HEAVY_BAGS"
### HEAVY_BAGS and PANIC_BAG_DROP both must be ABOVE 0 for this feature to ACTIVATE.
### When active( value larger than 0) it represents the % of used wallet at which we allow the triggering of PANIC LOSS SELLING
    - Value Type: Number
    - Example Value: `"HEAVY_BAGS": 32.3`
- What this means is if your position grows to a size larger than 32.3% of your wallet balance, we will enable stop_loss protection on this pair.
- This is triggered by reaching PANIC_BAG_DROP negative gain. 
### **PLEASE NOTE THIS WILL CAUSE IMMEDIATE LOSS OF A PORTION OF YOUR WALLET BALANCE WHEN CONDITIONS ARE MET**
# "PANIC_BAG_DROP"
### When active( value larger than 0) it represents the % of **negative gain** reached at which to drop the HEAVY_BAGS sized bags. 
    - Value Type: Number
    - Example Value: `"PANIC_BAG_DROP": 5`
- This will trigger STOP_LOSS selling when your position's **NEGATIVE GAIN** is larger than 5

# "STATIC_DELAYS"
- Value Type: Integer Array - seperated by ', ' **(comma+space)**
- Example Value: `"STATIC_DELAYS": "80,120,140,160"`
### What this does 
- Sets a value for the corresponding TL tier.
So in this example for tl 20 the delay would be 80, tl 40 delay would be 120, tl 80 delay would be 160 and tl 100 delay would be 160


# "ALLOW_MMSR"
### Manually initiates a SR like sell system.
 - Value Type: Boolean
 - It disables buying when this mode is true. so don't forget its on once you reduce.**
    - The reduction happens like this:
        - It takes your current pair bagValue and divides it by MMSR_RATIO
        - So that means if your bags are 10k and you set MMSR_RATIO to 5, it will set 2000 USDT as your TRADING_LIMIT
          - this SR activates also only when your bags are over 60% used and you initiate it by flipping the toggle.

# "MMSR_RATIO"
### This is the integer that your bagValue is divided by to set as TL to use as emergency bag reduction.
- Value Type: Number
- Example Value: `"MMSR_RATIO": 5"`


# "DANCING_STYLE"
This setting determines which trading rules the bot will follow. They are described below.

                                              TRADING STYLES
        pair.Ask is the reference point. 
        Example reading of trading style 1:
                    trading only when pair.Ask is below fastBB.high and pair.Ask is above fastBB.low
     
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


# "Z_TIMER"
- Value Type: Automatic
- Example Value: Don't edit this value.
# "STATS_TIME"  
- Value Type: Automatic
- Example Value: Don't edit this value.
