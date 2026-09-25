CSU33D03 Computer Networks : Project Code Submission
Group 3: Suhail Jameel, Yash Chhillar, Kanishk Daga
Demo Slot: Wednesday 1 April, 14:55

OVERVIEW
--------
A custom networking protocol built from scratch enabling
bidirectional communication between an offshore wind turbine
and a space station via a LEO satellite relay, using raw
Python UDP sockets.

FOLDER STRUCTURE
----------------
turbine/    Suhail's machine  — Wind Turbine node
relay/      Yash's machine    — Satellite Relay node
station/    Kanishk's machine — Space Station node

Each folder is self-contained and runs independently.
protocol.py is copied into all three folders so each machine
has an identical header definition with no shared dependency.

REQUIREMENTS
------------
Python 3.8 or above (no third party packages required).
All three machines must be on the same network.
Tested on Windows using a mobile hotspot.
University WiFi blocks device-to-device traffic.

SETUP BEFORE RUNNING
--------------------
1. Find the IP address of each laptop (run ipconfig on Windows).

2. In relay/config.py set:
      TURBINE_IP  = Suhail's IP
      STATION_IP  = Kanishk's IP

3. In turbine/pitch_listener.py, yaw_listener.py,
   sensor_sender.py and heartbeat.py set:
      RELAY_IP = Yash's IP

4. In station/command_sender.py and command_sender_auto.py set:
      RELAY_IP = Yash's IP

LAUNCH ORDER (IMPORTANT)
------------------------
Start in this exact order or packets will be lost on startup.

  STEP 1 — Yash runs:    relay/launch.bat
  STEP 2 — Suhail runs:  turbine/launch.bat
  STEP 3 — Kanishk runs: station/launch.bat

Each launch.bat opens the required terminal windows automatically.

HOW TO USE
----------
After all processes are running, go to Kanishk's Command Sender
window and type:

  PITCH:30    sets blade pitch to 30 degrees
  YAW:45      sets blade yaw to 45 degrees
  quit        exits the command sender

WHAT YOU WILL SEE
-----------------
Yash's relay terminal shows [FORWARDED] and [DROPPED] for every
packet passing through the channel. Every 20th command is
deliberately corrupted to demonstrate NACK handling, after which
the station retransmits and the command delivers successfully.

Blackouts trigger automatically every 90 seconds and last 15
seconds. Kanishk's heartbeat monitor will print a WARNING
during this window.

FILES
-----
turbine/
  protocol.py         Shared binary header definition
  pitch_listener.py   Receives PITCH commands, sends ACK
  yaw_listener.py     Receives YAW commands, sends ACK
  sensor_sender.py    Sends WIND/TEMP/RPM every 2 seconds
  heartbeat.py        Sends ALIVE beacon every 5 seconds
  launch.bat          Launches all 4 turbine processes

relay/
  protocol.py         Shared binary header definition
  config.py           Channel parameters (delay, loss, blackout)
  relay.py            8-thread relay with channel simulation
  launch.bat          Launches the relay

station/
  protocol.py            Shared binary header definition
  command_sender.py      Manual command input for demo
  command_sender_auto.py Automated command loop for testing
  sensor_receiver.py     Displays incoming sensor data
  heartbeat_monitor.py   Detects turbine outages
  launch.bat             Launches all 3 station processes

