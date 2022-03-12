## TimeTrack

Data populated from these scripts is meant to be visualized at (<https://timetrack.slgotting.com>)


## Getting Started

### Install packages with

`pip install timetrack-slg`


### Setup up systemd to automatically run script to track active window

>> :warning: | Make sure to change username with your username
> :---: | :---
> :warning: | Also your pip install might be at a location other than `/home/username/.local/bin/timetrack-slg`
> :---: | :---

`
echo '[Unit]
Description=Simple time logger
After=multi-user.target

[Service]
User=username
Type=simple
Restart=always
Environment="DISPLAY=:1"
ExecStart=/home/username/.local/bin/timetrack-slg -s .9766

[Install]
WantedBy=multi-user.target' >> /etc/systemd/system/timetrack-slg.service
`
