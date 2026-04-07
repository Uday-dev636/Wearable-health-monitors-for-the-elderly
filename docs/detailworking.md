## Detailed Working

### Step 1: Sensor Reading
- MAX30102 reads heart rate (BPM)
- DS18B20 reads body temperature
- MPU6050 reads acceleration values



### Step 2: Processing (ESP32)
- ESP32 collects all sensor data
- Calculates heart rate
- Reads temperature
- Checks motion values



### Step 3: Fall Detection
- If acceleration is very high
→ Fall is detected



### Step 4: Faint Detection
- If heart rate is very low
→ Faint condition is detected



### Step 5: Manual Alert
- Push button is connected to GPIO 15
- When pressed → Manual alert ON



### Step 6: Buzzer Alert
- If fall OR faint OR button pressed
→ Buzzer turns ON
- Otherwise → Buzzer OFF



### Step 7: Sending Data
- ESP32 connects to ESP8266 WiFi
- Sends data using HTTP request





### Step 8: ESP8266 Server
- Creates WiFi network (HealthMonitor)
- Receives data from ESP32
- Updates values



### Step 9: Web Dashboard
- User connects to WiFi
- Opens browser → http://192.168.4.1
- Displays:
  - Temperature
  - Heart Rate
  - Fall Status
  - Faint Status
  - Alert Status



### Step 10: Output
- OLED shows live data
- Dashboard updates every few seconds
- Alert shown during emergency
