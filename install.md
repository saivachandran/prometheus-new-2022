## 1. Update the System

Update the apt package list to prepare the system for further installations.

```
 sudo apt update
```

## 2. Download and Install Prometheus

```
wget https://github.com/prometheus/prometheus/releases/download/v2.27.1/prometheus-2.27.1.linux-amd64.tar.gz
```

## Extract the downloaded archive.

```
tar xvf prometheus-2.27.1.linux-amd64.tar.gz

```

## Change directory to the extracted archive.
```
cd prometheus-2.27.1.linux-amd64
```


## Create the configuration file directory.
```
sudo mkdir -p /etc/prometheus
```

## Create the data directory.
```
 sudo mkdir -p /var/lib/prometheus
```


## Move the binary files prometheus and promtool to /usr/local/bin/.
```
 sudo mv prometheus promtool /usr/local/bin/
 ```

## Move console files in console directory and library files in console_libraries directory to /etc/prometheus/ directory.
```
 sudo mv consoles/ console_libraries/ /etc/prometheus/
 ```

## Move the template configuration file prometheus.yml to /etc/prometheus/ directory
```
sudo mv prometheus.yml /etc/prometheus/prometheus.yml
```

## Verify the installed version of Prometheus.
```
 prometheus --version
 ```

## Verify the installed version of promtool.
```
 promtool --version
 ```



 

