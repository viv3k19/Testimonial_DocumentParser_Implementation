# DocumentParser_using_OmniParse

This project provides a Python-based implementation of a document parser using the OmniParse library. The script is designed to run in a Colab environment and automates the setup process for working with OmniParse, enabling users to extract structured data from documents.

## Table of Contents

- [Installation](#installation)
- [Setup](#setup)
- [Usage](#usage)
  - [Running the Server](#running-the-server)
  - [Using Cloudflare Tunnels](#using-cloudflare-tunnels)
  - [Using Localtunnel](#using-localtunnel)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Installation

To use this script, you'll need to install the following dependencies:

```python
!git clone https://github.com/adithya-s-k/omniparse.git
# %cd omniparse
# %pwd
# %pip install -e .
# %pip install transformers==4.41.2
```
Update and install necessary packages:
```python
!apt-get update && apt-get install -y --no-install-recommends \
    wget \
    curl \
    unzip \
    git \
    libgl1 \
    libglib2.0-0 \
    curl \
    gnupg2 \
    ca-certificates \
    apt-transport-https \
    software-properties-common \
    libreoffice \
    ffmpeg \
    git-lfs \
    xvfb \
    && ln -s /usr/bin/python3 /usr/bin/python \
    && curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash \
    && wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt install python3-packaging \
    && apt-get install -y --no-install-recommends google-chrome-stable \
    && rm -rf /var/lib/apt/lists/*
```
Download and install ChromeDriver:
```python
!CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) && \
    wget -q -N https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip -P /tmp && \
    unzip -o /tmp/chromedriver_linux64.zip -d /tmp && \
    mv /tmp/chromedriver /usr/local/bin/chromedriver && \
    chmod +x /usr/local/bin/chromedriver && \
    rm /tmp/chromedriver_linux64.zip
```
## Setup
Set environment variables for Chrome and Chromedriver:
```python
import os
os.environ['CHROME_BIN'] = '/usr/bin/google-chrome'
os.environ['CHROMEDRIVER'] = '/usr/local/bin/chromedriver'
os.environ['DISPLAY'] = ':99'
os.environ['DBUS_SESSION_BUS_ADDRESS'] = '/dev/null'
os.environ['PYTHONUNBUFFERED'] = '1'

print("Set up complete")
```
## Usage
### Running the Server
To start the OmniParse server, use the following command:
```python
!python server.py --host 127.0.0.1 --port 8000 --documents --media --web
```
### Using Cloudflare Tunnels
If you want to expose the OmniParse server via Cloudflare tunnels, you can follow these steps:
```python
Download and install Cloudflare tunnels:
```python
!wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
!dpkg -i cloudflared-linux-amd64.deb
```
Run the following code to set up a tunnel:
```python
import subprocess
import threading
import time
import socket

def iframe_thread(port):
    while True:
        time.sleep(0.5)
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        result = sock.connect_ex(('127.0.0.1', port))
        if result == 0:
            break
        sock.close()
    print("\nOmniPrase API finished loading, trying to launch cloudflared (if it gets stuck here cloudflared is having issues)\n")

    p = subprocess.Popen(["cloudflared", "tunnel", "--url", "http://127.0.0.1:{}".format(port)], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    for line in p.stderr:
        l = line.decode()
        if "trycloudflare.com " in l:
            print("This is the URL to access OmniPrase:", l[l.find("http"):], end='')

threading.Thread(target=iframe_thread, daemon=True, args=(8000,)).start()
```
### Start the server:
```python
!python server.py --host 127.0.0.1 --port 8000 --documents --media --web
```
### Using Localtunnel
Alternatively, you can expose the server using Localtunnel:
Install Localtunnel:
```python
!npm install -g localtunnel
```
Set up the tunnel:
```python
import subprocess
import threading
import time
import socket
import urllib.request

def iframe_thread(port):
    while True:
        time.sleep(0.5)
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        result = sock.connect_ex(('127.0.0.1', port))
        if result == 0:
            break
        sock.close()
    print("\Omniparse finished loading, trying to launch localtunnel (if it gets stuck here localtunnel is having issues)\n")

    print("The password/enpoint ip for localtunnel is:", urllib.request.urlopen('https://ipv4.icanhazip.com').read().decode('utf8').strip("\n"))
    p = subprocess.Popen(["lt", "--port", "{}".format(port)], stdout=subprocess.PIPE)
    for line in p.stdout:
        print(line.decode(), end='')

threading.Thread(target=iframe_thread, daemon=True, args=(8000,)).start()
```
Start the server:
```python
!python server.py --host 127.0.0.1 --port 8000 --documents --media --web
```
## Limitations
Resource Intensive: The setup requires a significant amount of resources, including specific software and configurations, which may not be straightforward on all machines.
Cloudflare/Localtunnel Issues: There might be issues when setting up tunnels with Cloudflare or Localtunnel, which could affect accessibility.
## Conclusion
This project provides a comprehensive setup for using OmniParse in a cloud-based environment like Colab. 
By following the steps outlined in this guide, you can successfully install dependencies, set up the server, and expose it through tunneling services for easy access and testing.
