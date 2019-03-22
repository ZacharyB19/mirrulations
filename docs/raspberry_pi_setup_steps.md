NEW CLIENT

 ssh pi@<ip_of_desired_pi> password: mirrulati0ns<#_on_pi_stack>
 
- sudo apt-get install python3
- sudo apt-get install python3-pip
- sudo apt-get install git
- git clone https://github.com/MoravianCollege/mirrulations.git
- cd mirrulations
- nano setup.py
- REMOVE the python>=3.7.2
- REMOVE fakeredis import
- cd src/mirrulations_core/
- nano main.py
- change client_id to client_id= "".join(random.choice(string.asciiuppercase + string.digits) for _ in range(16))
- cd /home/pi
- nano script.py

import os
os.system('python3 home/pi/mirrulations/src/mirrulations_core/main.py')

- sudo nano /lib/systemd/system/mirrulations.service
    - add to file

        [Unit]

        Description=Start Mirrulations on Boot
        After=multi-user.target

        [Service]

        Type=idle
        ExecStart=/usr/bin/python3 /home/pi/script.py

        [Install]

        WantedBy=multi-user.target

- sudo chmod 644 /lib/systemd/system/mirrulations.service
- sudo systemctl daemon-reload
- sudo systemctl enable mirrulations.service