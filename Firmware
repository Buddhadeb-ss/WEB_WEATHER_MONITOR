#include <WiFi.h>
#include <WebServer.h>
#include <Wire.h>
#include <Adafruit_BME280.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// WiFi Credentials
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

// I2C Pins
#define I2C_SDA 23
#define I2C_SCL 22

// OLED Settings
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
Adafruit_BME280 bme;
WebServer server(80);

// Sensor Variables
float temperature;
float humidity;
float pressure;
float altitude;

// Webpage Handler
void handleRoot() {

  String html = "<!DOCTYPE html><html><head>";
  html += "<meta http-equiv='refresh' content='5'>";
  html += "<title>ESP32 Weather Station</title>";
  html += "<style>";
  html += "body{font-family:Arial;text-align:center;background:#f4f4f4;}";
  html += ".card{background:white;padding:20px;border-radius:15px;width:350px;margin:auto;margin-top:50px;box-shadow:0 0 15px rgba(0,0,0,0.2);}";
  html += "h1{color:#333;}";
  html += "p{font-size:22px;}";
  html += "</style></head><body>";

  html += "<div class='card'>";
  html += "<h1>ESP32 Weather Station</h1>";

  html += "<p>🌡 Temperature: " + String(temperature, 1) + " °C</p>";
  html += "<p>💧 Humidity: " + String(humidity, 1) + " %</p>";
  html += "<p>📈 Pressure: " + String(pressure, 1) + " hPa</p>";
  html += "<p>⛰ Altitude: " + String(altitude, 1) + " m</p>";

  html += "</div></body></html>";

  server.send(200, "text/html", html);
}

void setup() {

  Serial.begin(115200);

  Wire.begin(I2C_SDA, I2C_SCL);

  // OLED Init
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED not found!");
    while (1);
  }

  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);
  display.setTextSize(1);

  // BME280 Init
  if (!bme.begin(0x76)) { // change to 0x77 if needed

    display.clearDisplay();
    display.setCursor(0, 0);
    display.println("BME280 NOT FOUND");
    display.display();

    while (1);
  }

  // WiFi Connection
  display.clearDisplay();
  display.setCursor(0, 0);
  display.println("Connecting WiFi...");
  display.display();

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("WiFi Connected");
  Serial.println(WiFi.localIP());

  // Start Web Server
  server.on("/", handleRoot);
  server.begin();

  // Show IP Address for 10 seconds
  display.clearDisplay();
  display.setCursor(0, 0);
  display.println("WiFi Connected");
  display.println("");
  display.print("Server IP:");
  display.println(WiFi.localIP());
  display.println("");
  display.println("Open in Browser");
  display.display();

  delay(10000);
}

void loop() {

  // Read Sensor
  temperature = bme.readTemperature();
  humidity = bme.readHumidity();
  pressure = bme.readPressure() / 100.0F;
  altitude = bme.readAltitude(1013.25);

  // OLED Display
  display.clearDisplay();

  display.setTextSize(1);

  display.setCursor(0, 0);
  display.print("IP:");
  display.println(WiFi.localIP());

  display.print("T:");
  display.print(temperature, 1);
  display.println(" C");

  display.print("H:");
  display.print(humidity, 1);
  display.println(" %");

  display.print("P:");
  display.print(pressure, 0);
  display.println(" hPa");

  display.print("A:");
  display.print(altitude, 0);
  display.println(" m");

  display.display();

  server.handleClient();

  delay(1000);
}
