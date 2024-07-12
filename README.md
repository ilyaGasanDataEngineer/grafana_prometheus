# Installation Guide for Grafana, Prometheus, and Node Exporter

This guide will help you set up Grafana with Docker, configure Prometheus, and install Node Exporter on your server.

## Step 1: Install Docker

First, you need to install Docker on your system. Follow the [official Docker installation guide](https://docs.docker.com/get-docker/) for your operating system.

## Step 2: Set Up Grafana with Docker

1. Create a `docker-compose.yml` file or navigate to your Grafana directory.

2. Add the following content to the `docker-compose.yml` file:

    ```yaml
    version: "3.8"
    services:
      grafana:
        image: grafana/grafana
        container_name: grafana
        restart: unless-stopped
        ports:
         - '3000:3000'
        volumes:
          - grafana-storage:/var/lib/grafana
    volumes:
      grafana-storage: {}
    ```

3. Navigate to the directory containing your `docker-compose.yml` file and run:

    ```bash
    docker-compose up -d
    ```

## Step 3: Set Up Prometheus

1. Navigate to your Prometheus folder:

    ```bash
    cd prometheus
    ```

2. Run Prometheus:
    DOWNLOAD ALL FILES FROM THIS FOLDER https://drive.google.com/drive/folders/1uf6nxVYxN5IXGOTIYwrb1UD3Ktp_HKFq
    ```bash
    ./prometheus
    ```

3. Navigate back to your root directory and then to your Alertmanager folder:

    ```bash
    cd ..
    cd alertmanager
    ./alertmanager
    ```

## Step 4: Setting Up Node Exporter

### Download Node Exporter

1. Download the Node Exporter using the `wget` command:

    ```bash
    wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
    ```

    > **Note:** Ensure you are using the latest version of Node Exporter and the correct architecture build for your server. For the latest releases, check [Prometheus Node Exporter Releases](https://github.com/prometheus/node_exporter/releases).

### Extract the Contents

2. Extract the contents with the following command:

    ```bash
    tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
    ```

### Move the Node Exporter Binary

3. Change to the directory and move the `node_exporter` binary to `/usr/local/bin`:

    ```bash
    cd node_exporter-1.7.0.linux-amd64
    sudo cp node_exporter /usr/local/bin
    ```

4. Clean up by removing the downloaded tar file and its directory:

    ```bash
    rm -rf ./node_exporter-1.7.0.linux-amd64
    ```

### Create a Node Exporter User

5. Create a dedicated user for running Node Exporter:

    ```bash
    sudo useradd --no-create-home --shell /bin/false node_exporter
    ```

6. Assign ownership permissions of the `node_exporter` binary to this user:

    ```bash
    sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
    ```

### Configure the Service

7. To ensure Node Exporter automatically starts on server reboot, configure the systemd service:

    ```bash
    sudo nano /etc/systemd/system/node_exporter.service
    ```

8. Paste the following configuration:

    ```ini
    [Unit]
    Description=Node Exporter
    Wants=network-online.target
    After=network-online.target

    [Service]
    User=node_exporter
    Group=node_exporter
    Type=simple
    ExecStart=/usr/local/bin/node_exporter
    Restart=always
    RestartSec=3

    [Install]
    WantedBy=multi-user.target
    ```

    Save and exit the editor.

### Enable and Start the Service

9. Reload the systemd daemon:

    ```bash
    sudo systemctl daemon-reload
    ```

10. Enable the Node Exporter service:

    ```bash
    sudo systemctl enable node_exporter
    ```

11. Start the service:

    ```bash
    sudo systemctl start node_exporter
    ```

12. To confirm the service is running properly, check its status:

    ```bash
    sudo systemctl status node_exporter.service
    ```

## Step 5: Accessing Grafana and Prometheus

- Open Prometheus in your browser:

    ```
    http://localhost:9090
    ```

- Open Grafana in your browser:

    ```
    http://localhost:3000
    ```

## Step 6: Get Your IP Address

- Determine your server's IP address using the following command:

    ```bash
    ip addr
    ```

## Step 7: Set Up Your Data Source in Grafana

- In Grafana, set your Prometheus data source URL to:

    ```
    http://[your-server-ip]:9090
    ```

Now you have successfully set up Grafana, Prometheus, and Node Exporter!
