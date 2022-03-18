## Step 1: Download the latest node exporter package. You should check the Prometheus downloads section for the latest version and update this command to get that package.

```
cd /tmp
curl -LO https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz
```

# Step 2: Unpack the tarball
```
tar -xvf node_exporter-0.18.1.linux-amd64.tar.gz
```

# Step 3: Move the node export binary to /usr/local/bin
```
sudo mv node_exporter-0.18.1.linux-amd64/node_exporter /usr/local/bin/
```

## Create a Custom Node Exporter Service

# Step 1: Create a node_exporter user to run the node exporter service.
```
sudo useradd -rs /bin/false node_exporter
```

# Step 2: Create a node_exporter service file under systemd.
```
sudo vi /etc/systemd/system/node_exporter.service
```
