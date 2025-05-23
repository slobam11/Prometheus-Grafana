Prometheus + Grafana monitoring (Node Exporter setup)

This project contains the basic setup for monitoring a system using Prometheus, Grafana, and Node Exporter.

What I Tried
The goal was to set up node_exporter as a systemd service on an Ubuntu server so that Prometheus could collect system metrics for display in Grafana.

Steps I Followed
Downloading and extracting Node Exporter

step 1 : 
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.0/node_exporter-1.8.0.linux-amd64.tar.gz
tar xvf node_exporter-1.8.0.linux-amd64.tar.gz
sudo mv node_exporter-1.8.0.linux-amd64/node_exporter /usr/local/bin/
sudo chmod +x /usr/local/bin/node_exporter
Testing if it's working

Step 2: 
./node_exporter
This step worked without any issues, and node_exporter was available on port 9100.

Creating the systemd service
I created a file:

Step 3: 
sudo nano /etc/systemd/system/node_exporter.service
The content was:

Step 4: 
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=sloba
Group=sloba
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
The problem I encountered
When starting the service:

Step 5: 
sudo systemctl start node_exporter
I got the error:

Step 6: 
node_exporter.service: Failed to determine user credentials: No such process
node_exporter.service: Main process exited, code=exited, status=217/USER
How I solved it
The problem was that the user sloba didn’t exist. I added it:

Step 7:
sudo useradd -m -s /bin/bash sloba
After that, I reloaded the service:

Step 8: 
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
The service started successfully:

Step 9:
sudo systemctl status node_exporter
Conclusion
Node Exporter needs to:

Be an executable file with proper permissions (chmod +x)

Be run by a valid user

Have an absolute path in ExecStart

Be restarted via systemctl daemon-reload whenever you modify the service file
