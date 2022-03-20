# Click on the archive to download it, or run a simple wget command to download the file.
```
 wget https://github.com/prometheus/alertmanager/releases/download/v0.18.0/alertmanager-0.18.0.linux-amd64.tar.gz
 ```
# Extract the files from the archive.
```
 tar xvzf alertmanager-0.18.0.linux-amd64.tar.gz
```
# change directory
```
cd alertmanager-0.18.0.linux-amd64/
```
 Starting the AlertManager as a service

# In order to start the AlertManager as a service, you are going to move the executables to the /usr/local/bin folder.
```
sudo mv amtool alertmanager /usr/local/bin
```

# For the configuration files, create a new folder in /etc called alertmanager.
```
 sudo mkdir -p /etc/alertmanager
 sudo mv alertmanager.yml /etc/alertmanager
```

# Create a data folder at the root directory, with a prometheus folder inside.
```
 sudo mkdir -p /data/alertmanager
 ```
 
#  Next, create a user for your upcoming service.
```
 sudo useradd -rs /bin/false alertmanager
```

# Give permissions to your newly created user for the AlertManager binaries.
```
sudo chown alertmanager:alertmanager /usr/local/bin/amtool /usr/local/bin/alertmanager
```

# Give the correct permissions to those folders recursively.
```
sudo chown -R alertmanager:alertmanager /data/alertmanager /etc/alertmanager/*
```
 
