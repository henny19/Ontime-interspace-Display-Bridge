# **Ontime Display Bridge**

A hardware bridge that connects the modern **Ontime** browser-based stage timer to legacy **Interspace Industries** numeric displays.

## **📖 Overview**

This project allows you to breathe new life into legacy RS232-based numeric displays (commonly used in AV and live events) by driving them directly from an **Ontime** instance.

The device acts as a "Man-in-the-Middle" injector. It sits inline with the 12V XLR power typically used by these displays, tapping the power for itself while injecting a new data signal derived from the Ontime WebSocket API.

**Copyright (c) 2025 technicaleventservices.com**

## **✨ Features**

* **Wireless Connectivity:** Connects to Ontime over WiFi (supports WSS/SSL).  
* **Zero-Config Startup:** Uses a Captive Portal (WiFiManager) for on-site configuration of WiFi credentials and Ontime server details—no coding required on show site.  
* **Legacy Protocol Support:** Generates the specific 9600 baud, swapped-nibble hex protocol required by Interspace displays.  
* **Smart Color Logic:** Automatically maps Ontime phases to display colors:  
  * Running: **Green**  
  * Warning: **Amber**  
  * Overtime/Negative: **Red**  
* **Local Status Display:** On-board 16x2 LCD shows:  
  * Connection Status & IP Address.  
  * Current Event Title (with scrolling for long titles).  
  * Countdown Time & Playback State (Running/Stopped).  
* **Auto-Reconnect:** Robust connection handling with heartbeat monitoring to ensure reliability during live shows.

## **🛠️ Hardware Requirements**

The circuit is designed to be built on a custom PCB or Veroboard.

* **Microcontroller:** Wemos D1 Mini (ESP8266)  
* **Level Shifter:** MAX3232 Module (RS232 to TTL)  
* **Power Regulation:** Fasizi (or similar) DC-DC Buck Converter (12V Input \-\> 5V Output)  
* **Display:** I2C 1602 LCD Screen (Address 0x27)  
* **Connectors:**  
  * 1x XLR 3-Pin Female (Input/Power Source)  
  * 1x XLR 3-Pin Male (Output to Display)

### **Wiring Concept**

The device is powered by the 12V rail from the legacy controller (or a 12V PSU) on XLR Pin 3\.

* **Pin 1 (GND):** Common Ground.  
* **Pin 2 (Data):** The input data line is **disconnected** (capped off) to stop the original signal. The new signal from the MAX3232 is injected into the Output XLR Pin 2\.  
* **Pin 3 (12V):** Passthrough power, tapped to feed the 5V regulator.

## **💻 Software Setup**

### **Prerequisites**

* **Arduino IDE:** Version 2.0 or newer.  
* **ESP8266 Board Definition:** Version 3.1.2 or stable equivalent.  
  * *Manager URL:* http://arduino.esp8266.com/stable/package\_esp8266com\_index.json

### **Required Libraries**

Install these via the Arduino Library Manager:

| Library | Author | Purpose |
| :---- | :---- | :---- |
| **WiFiManager** | tzapu | Captive portal for WiFi/Server config |
| **WebSockets** | Markus Sattler | WSS client for Ontime API |
| **Arduino\_JSON** | Arduino | Parsing API payloads |
| **LiquidCrystal I2C** | Frank de Brabander | Driving the local LCD |

### **Board Settings**

* **Board:** LOLIN(WEMOS) D1 R2 & mini  
* **Upload Speed:** 921600  
* **Frequency:** 80 MHz

## **⚙️ Configuration**

On first boot (or if the configured WiFi is missing):

1. The LCD will display **"Connecting WiFi... Failed\! Reset."**  
2. The device will create a WiFi Hotspot named **Ontime\_Setup**.  
3. Connect to this hotspot with your phone or laptop.  
4. A portal should open automatically (or visit 192.168.4.1).  
5. Enter your:  
   * **Venue WiFi SSID & Password**  
   * **Ontime Domain** (e.g., ontime.my-event.com)  
   * **Ontime Port** (usually 443 for SSL)  
6. Click **Save**. The device will reboot and connect.

## **📄 License**

This project is licensed under the MIT License \- see the LICENSE file for details.

*Developed by technicaleventservices.com*
