How I Solved It  : 

relax and enjoy 

The problem was that the user sloba didn’t exist. I added it:

sudo useradd -m -s /bin/bash sloba
After that, I restarted the service:

sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
The service started successfully:


sudo systemctl status node_exporter
Conclusion
Node Exporter must:

Be an executable file with proper permissions (chmod +x)

Be run by a valid user

Have an absolute path in ExecStart

Be restarted using systemctl daemon-reload whenever the service file is modified

Cheers :) 

