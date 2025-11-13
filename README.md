IoT MPU6050 Real-Time Measurement Project

This repository contains all the code for a complete, end-to-end IoT prototype for capturing and visualizing 6-axis motion data (acceleration + gyroscope) in real-time.

The system is built on a "Sensor-Gateway-Dashboard" architecture that runs entirely on your local network, requiring no internet connection or cloud services.

System Architecture

The data flows from the sensor to your browser in three stages:

Sensor Node (ESP32 + MPU6050):

Reads 6-axis data from the MPU6050 sensor.

Connects to your local Wi-Fi.

Publishes the data as a JSON payload to an MQTT topic.

Gateway (Raspberry Pi):

Runs a Mosquitto MQTT Broker to receive the data.

Runs a Python Flask App that:

Subscribes to the MQTT topic.

Caches the latest data from all nodes.

Serves a simple REST API (/api/nodes).

Dashboard (Web UI):

A simple HTML/CSS/JS single-page application.

Runs in your browser.

Fetches data from the Raspberry Pi's API every 2 seconds.

Dynamically displays the measurements for all connected nodes.

Folder Structure

/ESP32_Node: Contains the PlatformIO project for the ESP32 firmware.

/RaspberryPi_Gateway: Contains the Python Flask backend and the HTML/CSS/JS frontend.

Hardware Requirements

Sensor Node:

ESP32 Development Board (e.g., ESP32-WROOM-32)

MPU6050 Accelerometer/Gyroscope Sensor

Breadboard and Jumper Wires

Gateway:

Raspberry Pi (Any model 3B+ or newer is recommended)

microSD Card (8GB+)

Power Supply

Network:

A Wi-Fi router (for your local network)

Setup Instructions

Step 1: Sensor Node (ESP32)

Wiring: Connect the MPU6050 to your ESP32:

VCC -> 3.3V

GND -> GND

SDA -> D21 (GPIO 21)

SCL -> D22 (GPIO 22)

Open in PlatformIO: Open the /ESP32_Node folder in VS Code with the PlatformIO extension installed.

Configure: Open ESP32_Node/include/config.h and set:

WIFI_SSID: Your Wi-Fi network name.

WIFI_PASS: Your Wi-Fi password.

MQTT_BROKER_IP: The IP address of your Raspberry Pi (you'll get this in the next step).

Upload: Plug in your ESP32 and click the "PlatformIO: Upload" button.

Step 2: Gateway (Raspberry Pi)

Install OS: Install Raspberry Pi OS on your SD card and boot up your Pi.

Connect to Wi-Fi: Ensure your Pi is on the same Wi-Fi network as your ESP32.

Find its IP Address: Open a terminal on the Pi and run ifconfig. Note the wlan0 inet address (e.g., 192.168.1.107). Go back to Step 1 and put this IP in your config.h file and re-upload to the ESP32.

Install Mosquitto (MQTT Broker):

sudo apt update
sudo apt install mosquitto mosquitto-clients -y
sudo systemctl enable mosquitto
sudo systemctl start mosquitto


Install Python Dependencies:

sudo apt install python3-flask python3-paho-mqtt


Copy Gateway Code: Copy the entire /RaspberryPi_Gateway folder onto your Raspberry Pi (e.g., into the /home/pi/ directory).

Run the Gateway:

cd /path/to/RaspberryPi_Gateway
python3 gateway_app.py


You should see it connect to the MQTT broker and start the Flask server on port 5000.

Step 3: View the Dashboard

On your laptop or phone (on the same Wi-Fi network), open a web browser.

Go to http://<YOUR_PI_IP_ADDRESS>:5000 (e.g., http://192.168.1.107:5000).

You should see the dashboard! If your ESP32 is running, its data card will appear within a few seconds.
