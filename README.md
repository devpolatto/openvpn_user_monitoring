# OpenVPN Status Monitor

This project provides a Python script that listens to the status of an OpenVPN server and extracts information about those logged in. The script continuously reads the OpenVPN status file and updates a JSON file with details about the connected clients. 

## Features

- Monitors the OpenVPN status file for changes.
- Extracts information about connected clients from the status file.
- Stores client information in a JSON file for easy access.
- Can be configured to run as a background service using systemd.

## Requirements

- Python 3.x
- OpenVPN server with status file enabled
- Systemd (if running as a service)

## Installation

1. Clone the repository:

    ```bash
    git clone https://github.com/devpolatto/openvpn_user_monitoring.git
    ```

2. Navigate to the project directory:

    ```bash
    cd openvpn_user_monitoring
    ```

3. Make the script executable
     ```bash
     chmod u+x main
     ```

## Usage

1. Run the script indicating the destination path of the data   obtained from the OpenVPN status file

     ```bash
     ./main -o /home/$USER/ovpn-user.log
     ```

     By default, the script monitors the file ```/var/log/openvpn/openvpn-status.log```, but you can change the path using the ```-f``` argument
     More excutation information, excute ```--help``` in the script


2. Optionally, configure the script to run as a systemd service:

     add script to local libs:

     ```bash
     sudo mkdir -p /usr/lib/openvpn-status-monitor
     sudo cp main /usr/lib/openvpn-status-monitor/main
     ```

     create a symblink:

     ```bash
     ln -s /usr/lib/openvpn-status-monitor/main /usr/local/bin/openvpn-status-monitor
     ```

     Create a service in /etc/systemd/system, For example:
     ```bash
     /etc/systemd/system/openvpn_user_monitor.service
     ```
     Enter the code:
     ```bash
     [Unit]
     Description=OpenVPN Status Monitor
     After=network.target

     [Service]
     ExecStart=openvpn-status-monitor -o /home/$USER/ovpn-user.log
     Restart=always

     [Install]
     WantedBy=multi-user.target
     ```

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

### Thanks!