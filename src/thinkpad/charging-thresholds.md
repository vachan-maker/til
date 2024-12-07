# Charging Thresholds
To extend the lifespan of your battery, you can define charging thresholds.

A ThinkPad battery can be set for maximum runtime (hours), or for maximum lifespan (years).

[More info](https://support.lenovo.com/us/en/solutions/ht509084-battery-qa)

## Setting the Charging Thresholds
Source: [Reddit](https://www.reddit.com/r/thinkpad/comments/og5anr/comment/h4hpv4k/)
- You  can set the charging start threshold in (At what value or below should charging start):

    `/sys/class/power_supply/BAT0/charge_start_threshold`

- You can set the charging stop threshold in (At what value charging should stop):

    `/sys/class/power_supply/BAT0/charge_stop_threshold`

- I have set the start threshold at 75 and stop threshold at 80 on my laptop
## Optimizing for runtime (hours)
Source: [Lenovo](https://support.lenovo.com/us/en/solutions/ht078208-how-can-i-increase-battery-life-thinkpad-and-lenovo-vbke-series-notebooks)
> The normal default Lenovo Power Manager Battery Maintenance setting keeps the battery fully charged. The setting starts charging when the battery drops below 96%, and stops at 100%. The battery runtime depends on the Power Plan settings.

## Optimizing for lifespan (years)
Source: [Lenovo](https://support.lenovo.com/us/en/solutions/ht078208-how-can-i-increase-battery-life-thinkpad-and-lenovo-vbke-series-notebooks)


> For maximum lifespan when rarely using the battery, set Custom charge thresholds to start charging at 40% capacity and stop at 50%, and keep the ThinkPad cool. The thresholds can be adjusted in the Battery Maintenance settings of Lenovo Power Manager.

> If the battery is used somewhat frequently, set the start threshold at around 85% and stop at 90%. This will still give a good lifespan benefit over keeping the battery charged to 100%.
