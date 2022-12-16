# PeopleSense-Proj
![Project Logo](https://github.com/GShatrawJr/CSC131-CalTrans-Project/blob/a6ff61eb07f03abcc1cef30f093efeb5f0c5a77c/Resources/PeopleSense%20Logo.png)

# Project Description
This project is for the PeopleSense Website 2.0, which adds significant features such as a single unified landing page for the previously separate dashboards, which after log in sends the user to the correct dashboard based on their usergroup. The new dashboard features Grafana based data visualizations including current utilization, historical time series utilization, and real time GPS maps with train locations. All usergroups are defined using the AWS Cognito system, and thus only specific users get acces to the dashboards based on the agencies they are afilliated with, currently CCJPA, SJJPA, or PeopleSense Employees.

# Setup & Usage
1. Download and install VirtualBox https://www.virtualbox.org/wiki/Downloads
2. Download Ubuntu ISO image https://ubuntu.com/download/desktop
3. Create a new virtual machine using the Ubuntu ISO image and use settings that make sense for your environment.
4. Once the virtual machine is booted, you may want to view things in full screen. This can be done by:
   * Selecting **“Devices”** and then **“Insert Guest Additions CD image”**.
   * It should automatically run but if it doesn’t, run the **“autorun.sh”** file stored on the CD image and wait for it to complete.
   * Once finished, restart the virtual machine and hit **“Ctrl f”** to view things in full screen.
5. Launch a new terminal and update packages: **sudo apt update**
   * The user might need to be added to the sudoers file:
   * Type **su** and enter the password
   * Type **sudo usermod -a -G sudo "username"** with your username
   * Restart the machine, open a new terminal, and re-enter the **sudo apt update** command.
6. Install docker https://docs.docker.com/engine/install/ubuntu/
7. Clone the PeopleSense project repo  

>**Note:**
>    
>* The carbon-relay configs are in the carbon-relay folder at PeopleSense-Proj/PeopleSense/containers/carbon-relay/ 
>* The grafana dashboard api key should be set in the carbon-relay-ng.conf file for metrics to be pushed to the dashboard.  
>* The main.py script expects a .env file with the PeopleSense API key and other items like so:
>    
>    API_KEY="api key"    
>    API_URL="url"  
>    CARBON_PICKLE_PORT=2013  
>    CARBON_SERVER=127.0.0.1  
>    DEBUG=no  
>    
>If Debug is set to "debug", the fake data generator is used instead of the PeopleSense API  
>The Carbon Pickle Port can be changed as long as it matches the carbon-relay-ng.conf file, the same goes for the ip address
  

8. Run the **setup.sh** script in PeopleSense-Proj/PeopleSense/containers/
9. Open the Grafana dashboard to see incomming metrics!

# How it works

<img src="/diagram/diagram.svg"/>

1. Passenger counts are obtained and stored into AWS dynamoDB after going through a PeopleSense prorietary network.  
2. The PeopleSense middleware (this project) software queries the PeopleSense API on a fixed interval (for now) to retrieve timeseries data regarding trian occupancy as well as latitude and longitude.  
3. The data is then pushed to carbon-relay which will go ahead and forward it to a Grafana dashboard.  
4. Lastly, the PeopleSense frontend will access and display Grafana dashboard tiles for an improved user experience

# Utilities
## devfilter.py
Used for testing Bluetooth device filtering locally. Devices are blacklisted based on the UUIDs they provide. See https://www.bluetooth.com/specifications/assigned-numbers/ for lists of UUIDs and their meaning.

To use, first ensure you have Python 3.10 or later installed, along with the `bluez` and `bluez-utils` packages. Then run `python devfilter.py path_to_blacklist`, where `path_to_blacklist` is a text file listing blacklisted UUIDs. An example is provided in `utils/devfilter/blacklist.txt`. The program will scan for nearby Bluetooth devices and keep a running count of blacklisted devices.
# Timeline

<img src="/diagram/timeline.png"/>

# Testing (Comming Soon!)
