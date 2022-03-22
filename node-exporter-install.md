# Installing node_exporter

Steps to install node_exporter

1. Add user for node_exporter

   ```
   sudo useradd --no-create-home --shell /bin/false node_exporter
   ```

2. Download node_exporter

   ```
  
   wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz
   ```

3. Extract node_exporter

   ```
   tar xvf node_exporter-0.18.1.linux-amd64.tar.gz
   ```

4. Copy node_exporter to /opt

   ```
   sudo mv node_exporter-0.18.1.linux-amd64 /opt/node_exporter
   sudo chown -R node_exporter:node_exporter /opt/node_exporter
   ```

5. Create service file for systemd

   ```
   sudo nano /etc/systemd/system/node_exporter.service
   ```

6. Fillin as follows:

   ```
   [Unit]
   Description=Node Exporter
   Wants=network-online.target
   After=network-online.target

   [Service]
   User=node_exporter
   Group=node_exporter
   Type=simple
   ExecStart=/opt/node_exporter/node_exporter --collector.systemd

   [Install]
   WantedBy=multi-user.target
   ```

7. Start the service with systemd and verify it runs

   ```
   sudo systemctl daemon-reload
   sudo systemctl start node_exporter 
   sudo systemctl status node_exporter 

   
   ```
8. On the prometheus server, dont' forget to add the static config for the collection of data!


# node exporter dashboard id
```
1860
```
