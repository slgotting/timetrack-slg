## TimeTrack

Time is tracked by logging the active, focused window of the user every second.
The consolidate script is mean to be run on a 5 minute interval on the seconds, and consolidates the raw time log.

You then upload your consolidated time log to <https://timetrack.slgotting.com>, and you can inspect your time usage with a nice graphical UI

## Compatibility

Right now this script is only usable on Linux. Tested working on Ubuntu 18. It should work on any linux distro if the following commands are available:

* xdotool
* wmctrl
* xprintidle

I may add support for Windows in the future but I wanted to get it out as it is right now.

## Getting Started

### Install package with

`pip install timetrack-slg`


To set everything up you can do things the easy way by running in your terminal (after you have installed with pip):
`install-timetracker-slg`

Or the hard way and follow the steps below

2. ### Set up systemd to automatically run script (on boot and always restart on fail)

 :warning: | Make sure to change username to your username
 :---: | :---
 :warning: | Also your python file install might be at a location other than `/home/username/.local/bin/timetrack-slg`. To find location, run `whereis timetrack-slg`. Update ExecStart as necessary
 :information_source: | The -s switch is set to .9766 because this is the interval I found gives me close to or exactly 1 run per second. See ["Calculate your sleep time"](#calculate-your-sleep-time) for information on how to calculate what value you should use.
 :information_source: | Get your DISPLAY variable with `env \| grep DISPLAY`
 :information_source: | Output file `-o /home/username/timetrack-slg/time-log.json` must be a json file
 :information_source: | Run timetrack-slg -h to see all options

:warning: Before running this command make sure you change the necessary variables using the above as guidance to doing so.

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
ExecStart=/home/username/.local/bin/timetrack-slg \
            --output_filepath /home/username/timetrack-slg/time-log.json \
            --sleep_time 0.9766 \
            --time_til_idle 30 \
            --config_filepath /home/username/.config/slg/time-log.yml

[Install]
WantedBy=multi-user.target' | sudo tee /etc/systemd/system/timetrack-slg.service >/dev/null
```

Then tell systemd to start this up at boot with

`sudo systemctl enable timetrack-slg.service`

And start er up with:

`sudo systemctl start timetrack-slg.service`


3. ### Set up cron job to automatically consolidate log

Add this line to your crontab (changing username and paths where necessary):

`*/5 * * * * /home/username/.local/bin/timetrack-consolidate --input_filepath /home/username/timetrack-slg/time-log.json --config_filepath=/home/username/.config/slg/time-log.yml --run_interval 5`

This line consolidates the time-log.json file every 5 minutes so that our timelog filesize doesnt grow too big.


4. ### Success

If you've done everything correctly, you should have a time-log-consolidated.json generated in the input_filepath location.

Admittedly, this is a very unnecessarily complex process right now. I simply wanted to get this out and testable quickly.

(In the future I will be creating a script to do all this for you; that will come with Windows support)


## More Info

### Calculate your sleep time

> 1. Make sure timetrack-consolidate is running in crontab every 5 minutes.
> 2. Run the script without any -s flag set for a while. 10 minutes to be safe. Shouldn't need to do this more than once so find something else to do while waiting.
> 3. Inspect your consolidate file and add up the times of a 5 minute block. The value should be relatively close to 300 (~ 290 - 299). Mine was 293
> 4. Divide number by 300. So in my case I had 293 so ( 293 / 300 ) is .9766, hence the value you see above.
