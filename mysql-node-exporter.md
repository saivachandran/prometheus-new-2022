# Monitoring MySQL / MariaDB with Prometheus

# Add Prometheus system user and group:
```
sudo groupadd --system prometheus
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
# Download latest release of MySQL exporter:
```
curl -s https://api.github.com/repos/prometheus/mysqld_exporter/releases/latest   | grep browser_download_url   | grep linux-amd64 | cut -d '"' -f 4   | wget -qi -
tar xvf mysqld_exporter*.tar.gz
sudo mv  mysqld_exporter-*.linux-amd64/mysqld_exporter /usr/local/bin/
sudo chmod +x /usr/local/bin/mysqld_exporter
```

# Confirm installation by checking version of mysqld_exporter:
```
mysqld_exporter  --version
```


# Create Prometheus exporter database user

# The user should have PROCESS, SELECT, REPLICATION CLIENT grants:
```
CREATE USER 'mysqld_exporter'@'localhost' IDENTIFIED BY 'StrongPassword' WITH MAX_USER_CONNECTIONS 2;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'mysqld_exporter'@'localhost';
FLUSH PRIVILEGES;
EXIT
```

# Configure database credentials

# Create database credentials file:
```
sudo vim /etc/.mysqld_exporter.cnf
```
```
[client]
user=mysqld_exporter
password=StrongPassword
```
# Set ownership permissions:
```
sudo chown root:prometheus /etc/.mysqld_exporter.cnf
```

# Create a new service file:
```
sudo vim /etc/systemd/system/mysql_exporter.service
```

# Add the following content
```
[Unit]
Description=Prometheus MySQL Exporter
After=network.target


[Service]
Type=simple
Restart=always
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/mysqld_exporter \
--config.my-cnf /etc/.mysqld_exporter.cnf \
--collect.global_status \
--collect.info_schema.innodb_metrics \
--collect.auto_increment.columns \
--collect.info_schema.processlist \
--collect.binlog_size \
--collect.info_schema.tablestats \
--collect.global_variables \
--collect.info_schema.query_response_time \
--collect.info_schema.userstats \
--collect.info_schema.tables \
--collect.perf_schema.tablelocks \
--collect.perf_schema.file_events \
--collect.perf_schema.eventswaits \
--collect.perf_schema.indexiowaits \
--collect.perf_schema.tableiowaits \
--collect.slave_status \
--web.listen-address=0.0.0.0:9104

[Install]
WantedBy=multi-user.target
```

# If your server has a public and private network, you may need to replace 0.0.0.0:9104 with private IP, e.g. 192.168.4.5:9104


# When done, reload systemd and start mysql_exporter service.
```
sudo systemctl daemon-reload
sudo systemctl enable mysql_exporter
sudo systemctl start mysql_exporter
```


# Configure MySQL endpoint to be scraped by Prometheus Server

# Login to your Prometheus server and Configure endpoint to scrape. Below is an example for two MySQL database servers.
```
scrape_configs:
  - job_name: server1_db
    static_configs:
      - targets: ['10.10.1.10:9104']
        labels:
          alias: db1

  - job_name: server2_db
    static_configs:
      - targets: ['10.10.1.11:9104']
        labels:
          alias: db2# 
```
