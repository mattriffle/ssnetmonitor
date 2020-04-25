# ssnetmonitor

A super simple network monitor written in Python, suitable for a home Raspberry Pi or similar.

It was developed partially on an Ubuntu 18.04 LTS machine, and finished on
a Raspberry Pi running Raspbian Buster.  I expect it is mostly universal to
*NIX operating systems.

## Installation

Requires Python 3 and the PyYAML package.  For my Raspbian Buster install, I 
had to install pip and then PyYAML:

```
sudo apt-get install python3-pip
sudo pip3 install PyYAML
```

Clone the git rep, update the YAML file, and most likely if you "just run it"
you'll get a log file in the same working directory.  But you probably want
to update the YAML a bit.

## How To Use

At the moment, the script runs strictly in the foreground, and can be managed
by something like supervisord or systemd (I'm using the latter).

See the sample YAML file for configuration options -- I recommend leaving the
list of external hosts to ping (which represent the highly-available public
DNS servers of Google, Cloudflare, Quad9, and OpenDNS).

You can optionally fill in the local IP of your home gateway, and the public
IP address assigned to your modem.  This allows the monitor to better 
determine *where* a failure is occurring.

Lastly, you can point to where it should log results, and change the amount of
time the script sleeps between checks.

## What It Does

It pings, in turn, your gateway (if configured), your modem (if configured), 
and two hosts from the external list.  If either of the internal checks or
both of the external checks fail, it logs such.

If we make it to the end of the list without errors and we were in a 'down'
condition, it logs the return to uptime.  Then it returns to the top of the
loop, we sleep, and we do it again.  Every 60th run through the script it sends
an 'ALIVE' message to the log.

## Potential Future

My itches are scratched already, but it's possible I might update the script
later to optionally handle daemonization itself, as an educational exercise.
It's also possible that notification of outages through some means (once
the outage is over because, well, you know...) could be added.

Patches welcome?

Mostly, this script exists for two reasons: 1) My ISP can be unreliable and
I've often wanted a firm record of its ups and downs, and 2) as someone who
has done a lot of Perl programming, I wanted to do something *useful* in
Python, and to work on thinking like a Python programmer, rather than trying
to directly translate Perl idioms.

## Author

Matt Riffle -- mattr (at) riffnet.com

## License

Licensed under the [MIT License](LICENSE)

