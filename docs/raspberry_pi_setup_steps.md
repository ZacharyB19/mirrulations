##Raspberry Pi Setup

###Server Setup
Run the commands and make the file edits in order.


`ssh pi@172.31.228.70`

The password is mirrulati0ns1

`sudo apt-get install python`

`sudo apt-get install python3-pip`

`sudo pip3 install gunicorn`

`sudo apt-get install git`

`git clone https://github.com/MoravianCollege/mirrulations.git`

`cd mirrulations`

`sudo pip3 install -r requirements.txt`

`sudo pip3 install -e .`

`cd src/mirrulations`

`touch config.json`

- ADD to file
	
`{
    "ip": "0.0.0.0",
    "port": "8080",
    "key": <insert api key here>,
    "client_id": "a1vZFvQFuQA3UU6J"
}`

- Save and exit file

`sudo nano /lib/systemd/system/mirrualtions.service`

- ADD to file
	
		[Unit]
		
		Description=Start Mirrulations on Boot
		After=multi-user.target

		[Service]
		
		Type=idle
		ExecStart=/usr/bin/python /home/pi/script.py

		[Install]
		
		WantedBy=multi-user.target
		
- Save and exit file
		
`sudo chmod 644 /lib/systemd/system/mirrualtions.service`

`sudo systemctl daemon-reload`

`sudo systemctl enable sample.service`

`cd ~`

`touch script.py`

`nano script.py`

- ADD to file
	
		import os
		os.system('gunicorn -b 0.0.0.0:8080 --chdir home/pi/		mirrulations/	src/mirrulations endpoints:app')
		
- Save and exit file

`cd mirrulations/src/mirrulations`

`nano docs_filter.py`

- In `add_document_job` change `random_id =''.join(random.choices(string.ascii_letters + string.digits, k=16))` to `random_id=''.join(random.choice(string.asciiuppercase + string.digits) for _ in range(16)`

- In `save_client_log` change `home=os.getenv("HOME")` to `home="/home/pi`

- In `process_docs` add `redis_server` as the first parameter to the `add_document_job` function call

- Save and exit file

`nano doc_filter`

- In `get_file_list` change `home=os.getenv("HOME")` to `home="/home/pi`

- In `process_doc` change `file_list, path = get_file_list(compressed_file, PATHstr, json_data['client_id'])` to `file_list, path = get_file_list(compressed_file, PATHstr, json_data['user'])`
		
`sudo reboot`



###Client Setup
Run the commands and make the file edits in order.

`ssh pi@<ip_of_desired_pi>` 

password: `mirrulati0ns<#_on_pi_stack>`
 
`sudo apt-get install python3`

`sudo apt-get install python3-pip`

`sudo apt-get install git`

`git clone https://github.com/MoravianCollege/mirrulations.git`

`cd mirrulations`

`nano setup.py`

- CHANGE `python_requires>=3.7.2` TO `python_requires>=3.5.3`
- REMOVE fakeredis import
- Save and exit file

`cd src/mirrulations_core/`

`nano __main.py__`

- CHANGE the existing `client_id` to `client_id= ''.join(random.choice(string.asciiuppercase + string.digits) for _ in range(16))`
- Save and exit file

`cd /home/pi`

`nano script.py`

- ADD the following two lines to the top of the file.

`import os`
`os.system('python3 home/pi/mirrulations/src/mirrulations_core/__main.py__')`

- Save and exit file

`sudo nano /lib/systemd/system/mirrulations.service`

- ADD to file

        [Unit]

        Description=Start Mirrulations on Boot
        After=multi-user.target

        [Service]

        Type=idle
        ExecStart=/usr/bin/python3 /home/pi/script.py

        [Install]

        WantedBy=multi-user.target
        
- Save and exit file

`sudo chmod 644 /lib/systemd/system/mirrulations.service`

`sudo systemctl daemon-reload`

`sudo systemctl enable mirrulations.service1`

`sudo reboot`

