## TimeTrack

Data populated from these scripts is meant to be visualized at <https://timetrack.slgotting.com>


## Getting Started

### Install packages with

`pip install timetrack-slg`


### Setup up systemd to automatically run script to track active window

> :warning: | Make sure to change username to your username
> :---: | :---
> :warning: | Also your pip install might be at a location other than `/home/username/.local/bin/timetrack-slg`
> :info: | Use `whereis timetrack-slg` to find its "binary" location
> :info: | The -s switch is set to .9766 because this is the interval I found gives me close to or exactly 1 run per second.
> :info: | Your processing speed is likely different, simply run it for 5 minutes at the default `-s 1` setting and calculate your precise interval with `count / 300`
> :info: | Get your DISPLAY variable with `env | grep DISPLAY`

`
echo '[Unit]
Description=Simple time logger
After=multi-user.target

[Service]
User=username
Type=simple
Restart=always
Environment="DISPLAY=:0"
ExecStart=/home/username/.local/bin/timetrack-slg -s .9766

[Install]
WantedBy=multi-user.target' >> /etc/systemd/system/timetrack-slg.service
`
