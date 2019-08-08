Introducing a way to play your mpd music from just about anywhere.

[mobile firefox screenshot](screenshot-mobileff.png "mobile screenshot on firefox")

[mobile chrome screenshot](screenshot-mobilechrome.png "mobile screenshot on chrome")

Features:

  * Stream music from mpd http output to any device which supports html5
  * Display album art
  * Display song info
  * Works locally, on a LAN, or over an ssh connection
  * able to toggle full-screen display. Simply touch the album art to toggle full-screen status.

Get started:

  * make sure http output is enabled in your mpd conf. The system default for most debian-based distros is for mpd to run as the system folder, so if you haven't configured your mpd, it's probably in /etc/mpd.conf. Here's what you want to add: 

    `audio_output {
    	type		"httpd"
    	name		"mpd radio"
    	encoder		"lame"		# optional, vorbis or lame
    	port		"8000"
    	bind_to_address "0.0.0.0"               # optional, IPv4 or IPv6
    	bitrate		"128"			# do not define if quality is defined
    	format		"44100:16:1"
    	max_clients     "0"                     # optional 0=no limit
    }`

  * if you had to change your mpd.conf, you'll also need to restart mpd. If your config is /etc/mpd.conf, use:
      sudo systemctl restart mpd.service

  * get my mpd_what script, and set it to run periodically - you can do this with conky, or you can run it periodicaly with a shell script, like this, using Ctrl+C to quit it later:

    #!/bin/bash
    while true; do
      ~/bin/mpd_what -q >/dev/null 2>&1 &
      sleep 5
    done

  * Now, add a symbolic link to your `last` to your coverart dir. If your coverart dir is ~/Pictures/coverart, then you'd do this:

      ln -s ~/.config/mpd_what/last ~/Pictures/coverart/last

  * Now, copy play.htm to your coverart dir:

      cp play.htm ~/Pictures/coverart

  * Now, if your coverart dir doesn't already serve http, here's how you can make it do that:

      cd ~/Pictures/coverart && python3 http.server 8888

  * The above command serves HTTP on TCP port 8888 on your computer. Now, get your ip address:

      ip addr | grep 'inet ' | grep -v 127.0.0.1 | awk '{print $2}' | sed 's#/.*##'

  * Now point your phone or tablet to this html file. If the ip address you got in the last step was 192.168.1.101, and you are using python to serve http on port 8888, you'd use this url:

      http://192.168,101:8888/play.htm
    
