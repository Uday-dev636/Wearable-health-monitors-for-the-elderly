## Pin Connections

### ESP32 Connections

#### MAX30102 (Heart Rate Sensor)
- VCC → 3.3V  
- GND → GND  
- SDA → GPIO 21  
- SCL → GPIO 22  

---

#### MPU6050 (Accelerometer)
- VCC → 3.3V  
- GND → GND  
- SDA → GPIO 21  
- SCL → GPIO 22  

---

#### OLED Display (SSD1306)
- VCC → 3.3V  
- GND → GND  
- SDA → GPIO 21  
- SCL → GPIO 22  

---

#### DS18B20 (Temperature Sensor)
- VCC → 3.3V  
- GND → GND  
- DATA → GPIO 18  
- Add 4.7kΩ resistor between DATA and VCC  

---

#### Push Button (Emergency)
- One side → GPIO 15  
- Other side → GND  

---

#### Buzzer
- Positive → GPIO 25  
- Negative → GND  

---

### ESP8266 (NodeMCU)

- Used as WiFi Access Point and Web Server  
- No external components connected  

---

 Notes

- All devices share common GND  
- All sensors use 3.3V  
- MAX30102, MPU6050, and OLED share same I2C pins (GPIO 21, 22)  
