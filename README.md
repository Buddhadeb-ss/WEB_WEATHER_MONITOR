# WEB_WEATHER_MONITOR
It is ESP32 based web weather monitoring project. implementing the BME280 sensor using I2C protocol to connect and a onboard web host on local wifi to host the website.

**Working**

ESP32 Start

↓

Initialize Peripherals (I2C, OLED, BME280)

↓

Connect to Wi-Fi

↓

Start Web Server

↓

Display IP Address

↓

Read Environmental Data

↓

Update OLED Display

↓

Update Web Dashboard

↓

Continuous Monitoring

**Hardware Components Used**

| Component               | Purpose                                      |
| ----------------------- | -------------------------------------------- |
| ESP32 Development Board | Main microcontroller and web server          |
| BME280 Sensor           | Measures temperature, humidity, and pressure |
| SSD1306 OLED Display    | Displays sensor readings and IP address      |
| Breadboard              | Circuit prototyping                          |
| Jumper Wires            | Electrical connections                       |
| Micro USB Cable         | Power supply and programming                 |
| Wi-Fi Router/Hotspot    | Enables IoT connectivity and web access      |
