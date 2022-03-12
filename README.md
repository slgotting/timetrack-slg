## TimeTrack

Data populated from these scripts is meant to be visualized at <https://timetrack.slgotting.com>


## Compatibility

Right now this script is only usable on Linux. Tested working on Ubuntu 18. Likely works on Ubuntu 20.

I may add support for Windows in the future but I wanted to get it out as it is right now.

## Getting Started

### Install packages with

`pip install timetrack-slg`


### Setup up systemd to automatically run script to track active window

 :warning: | Make sure to change username to your username
 :---: | :---
 :warning: | Also your pip install might be at a location other than `/home/username/.local/bin/timetrack-slg`. To find location, run `whereis timetrack-slg`
 :information_source: | The -s switch is set to .9766 because this is the interval I found gives me close to or exactly 1 run per second. See [here](#calculate-your-run-interval) for information on how to calculate what value you should use.
 :information_source: | Get your DISPLAY variable with `env | grep DISPLAY`

echo '[Unit]
Description=Simple time logger
After=multi-user.target

[Service]
User=username
Type=simple
Restart=always
Environment="DISPLAY=:0"
ExecStart=/home/username/.local/bin/timetrack-slg -s .9766 -o /home/username/.config/

[Install]
WantedBy=multi-user.target' >> /etc/systemd/system/timetrack-slg.service

## More Info

### Calculate your run interval

1. Make sure timetrack-consolidate is running in crontab every 5 minutes.
2. Run the script without any -s flag set for a while. 10 minutes to be safe. Shouldn't need to do this more than once
3. Inspect your consolidate file and add up the times of a 5 minute block. The value should be relatively close to 300 (~ 290 - 299). Mine was 293
4. Divide number by 300. So in my case I had 293 so ( 293 / 300 ) is .9766, hence the value you see above.

> :information_source: | Your processing speed is likely different, simply run it for 5 minutes at the default `-s 1` setting and calculate your precise interval with `count / 300`
