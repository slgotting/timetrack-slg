## TimeTrack

Data populated from these scripts is meant to be interacted with at <https://timetrack.slgotting.com>


## Compatibility

Right now this script is only usable on Linux. Tested working on Ubuntu 18. It should work on any linux distro if the following commands are available:

* xdotool
* wmctrl
* xprintidle

I may add support for Windows in the future but I wanted to get it out as it is right now.

## Getting Started

1. ### Install package with

`pip install timetrack-slg`


2. ### Set up systemd to automatically run script (heed warnings)

 :warning: | Make sure to change username to your username
 :---: | :---
 :warning: | Also your pip install might be at a location other than `/home/username/.local/bin/timetrack-slg`. To find location, run `whereis timetrack-slg`
 :information_source: | The -s switch is set to .9766 because this is the interval I found gives me close to or exactly 1 run per second. See ["Calculate your sleep time"](#calculate-your-sleep-time) for information on how to calculate what value you should use.
 :information_source: | Get your DISPLAY variable with `env \| grep DISPLAY`

In your terminal, run:

```
echo '[Unit]
Description=Time tracker
After=multi-user.target

[Service]
User=username
Type=simple
Restart=always
Environment="DISPLAY=:0"
ExecStart=/home/username/.local/bin/timetrack-slg -s .9766 -o /home/username/.config/

[Install]
WantedBy=multi-user.target' | sudo tee /etc/systemd/system/timetrack-slg.service >/dev/null
```


3. ### Set up cron job to automatically consolidate log (heed warnings)




## More Info

### Calculate your sleep time

> 1. Make sure timetrack-consolidate is running in crontab every 5 minutes.
> 2. Run the script without any -s flag set for a while. 10 minutes to be safe. Shouldn't need to do this more than once so find something else to do while waiting.
> 3. Inspect your consolidate file and add up the times of a 5 minute block. The value should be relatively close to 300 (~ 290 - 299). Mine was 293
> 4. Divide number by 300. So in my case I had 293 so ( 293 / 300 ) is .9766, hence the value you see above.
