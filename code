#include <WiFi.h>
#include <HTTPClient.h>

#include <Wire.h>
#include "MAX30105.h"
#include "heartRate.h"

#include <OneWire.h>
#include <DallasTemperature.h>
#include <MPU6050.h>

#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// ================= WIFI =================
const char* ssid = "HealthMonitor";
const char* password = "12345678";
const char* serverName = "http://192.168.4.1/update";

// ================= PINS =================
#define SDA_PIN 21
#define SCL_PIN 22
#define BUTTON_PIN 15
#define BUZZER_PIN 25
#define ONE_WIRE_BUS 18

// ================= OLED =================
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

// ================= SENSORS =================
MAX30105 heartSensor;
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature tempSensor(&oneWire);
MPU6050 mpu;

// ================= VARIABLES =================
float bodyTemp = 0;
volatile int bpm = 0;

bool fallFlag = false;
bool faintFlag = false;
bool manualAlert = false;

unsigned long lastSendTime = 0;
unsigned long lastDisplayTime = 0;

TaskHandle_t heartTask;

// ================= HEART RATE TASK =================
void heartRateTask(void *pvParameters)
{
  const TickType_t delayTime = pdMS_TO_TICKS(10);
  unsigned long lastBeat = 0;

  while (1)
  {
    long irValue = heartSensor.getIR();

    if (irValue > 5000)
    {
      if (checkForBeat(irValue))
      {
        unsigned long now = millis();
        unsigned long delta = now - lastBeat;

        if (delta > 300)
        {
          float beatsPerMinute = 60000.0 / delta;

          if (beatsPerMinute < 200 && beatsPerMinute > 30)
          {
            bpm = (int)beatsPerMinute;
          }
        }

        lastBeat = now;
      }
    }
    else
    {
      bpm = 0;
    }

    vTaskDelay(delayTime);
  }
}

// ================= SETUP =================
void setup()
{

  Serial.begin(115200);

  pinMode(BUTTON_PIN, INPUT_PULLUP);
  pinMode(BUZZER_PIN, OUTPUT);

  Wire.begin(SDA_PIN, SCL_PIN);

  // OLED
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  display.setCursor(0,20);
  display.println("SYSTEM STARTING");
  display.display();

  // WiFi
  WiFi.begin(ssid,password);

  while(WiFi.status()!=WL_CONNECTED)
  {
    delay(200);
  }

  // Sensors
  tempSensor.begin();
  tempSensor.setResolution(10);

  mpu.initialize();

  if(!heartSensor.begin(Wire))
  {
    Serial.println("MAX30102 ERROR");
    while(1);
  }

  heartSensor.setup(60,4,2,100,411,4096);
  heartSensor.setPulseAmplitudeRed(0x0A);
  heartSensor.setPulseAmplitudeIR(0x1F);

  display.clearDisplay();
  display.setCursor(0,20);
  display.println("DEVICE READY");
  display.display();

  // Start heart task
  xTaskCreatePinnedToCore(
    heartRateTask,
    "HeartRate",
    4096,
    NULL,
    2,
    &heartTask,
    0);
}

// ================= LOOP =================
void loop()
{

// ---------- Manual Emergency Button ----------
static unsigned long lastButton = 0;

if(digitalRead(BUTTON_PIN)==LOW && millis()-lastButton>400)
{
  manualAlert = !manualAlert;
  lastButton = millis();
}

// ---------- Temperature ----------
tempSensor.requestTemperatures();
bodyTemp = tempSensor.getTempCByIndex(0);

// ---------- Fall Detection ----------
int16_t ax,ay,az;
mpu.getAcceleration(&ax,&ay,&az);

long accel = abs(ax)+abs(ay)+abs(az);

if(accel > 50000)
fallFlag = true;
else
fallFlag = false;

// ---------- Faint Detection ----------
if(bpm < 40 && bpm > 0)
faintFlag = true;
else
faintFlag = false;

// ---------- Buzzer ----------
if(fallFlag || faintFlag || manualAlert)
digitalWrite(BUZZER_PIN,HIGH);
else
digitalWrite(BUZZER_PIN,LOW);

// ---------- Send Data ----------
if(millis()-lastSendTime > 2000)
{
  if(WiFi.status()==WL_CONNECTED)
  {
    HTTPClient http;

    String url = String(serverName) +
    "?temp=" + String(bodyTemp) +
    "&bpm=" + String(bpm) +
    "&fall=" + String(fallFlag ? "1":"0") +
    "&faint=" + String(faintFlag ? "1":"0") +
    "&alert=" + String(manualAlert ? "1":"0");

    http.begin(url);
    http.GET();
    http.end();
  }

  lastSendTime = millis();
}

// ---------- OLED ----------
if(millis()-lastDisplayTime > 500)
{
  display.clearDisplay();

  display.setCursor(0,0);
  display.println("HEALTH MONITOR");

  display.setCursor(0,15);
  display.print("Temp: ");
  display.print(bodyTemp,1);
  display.println(" C");

  display.setCursor(0,30);
  display.print("Heart: ");
  display.print(bpm);
  display.println(" BPM");

  display.setCursor(0,45);

  if(fallFlag)
  display.println("FALL DETECTED");

  else if(faintFlag)
  display.println("FAINT DETECTED");

  else if(manualAlert)
  display.println("MANUAL ALERT");

  else
  display.println("STATUS NORMAL");

  display.display();

  lastDisplayTime = millis();
}

}
