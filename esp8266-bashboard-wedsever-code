#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

const char* ssid = "HealthMonitor";
const char* password = "12345678";

ESP8266WebServer server(80);

float temperature = 0;
int heartRate = 0;

bool fallDetected = false;
bool faintDetected = false;
bool manualAlert = false;

void updateData() {

if(server.hasArg("temp"))
temperature = server.arg("temp").toFloat();

if(server.hasArg("bpm"))
heartRate = server.arg("bpm").toInt();

if(server.hasArg("fall"))
fallDetected = server.arg("fall") == "1";

if(server.hasArg("faint"))
faintDetected = server.arg("faint") == "1";

if(server.hasArg("alert"))
manualAlert = server.arg("alert") == "1";

server.send(200,"text/plain","OK");
}

void handleRoot() {

String page="";

page+="<!DOCTYPE html><html><head>";
page+="<meta name='viewport' content='width=device-width, initial-scale=1'>";
page+="<meta http-equiv='refresh' content='2'>";

page+="<style>";
page+="body{font-family:Arial;background:#111;color:white;text-align:center;margin:0;}";

page+=".container{padding:20px;}";

page+=".card{background:#1c1c1c;padding:20px;margin:10px;border-radius:10px;}";

page+=".title{font-size:18px;color:#aaa;}";

page+=".value{font-size:30px;font-weight:bold;}";

page+=".alert{color:red;font-weight:bold;font-size:26px;}";

page+=".normal{color:#00ff88;font-size:22px;}";

page+="</style>";

page+="<script>";

page+="function playAlert(){";

page+="var fall=document.getElementById('fall').innerText;";
page+="var faint=document.getElementById('faint').innerText;";
page+="var alert=document.getElementById('alert').innerText;";

page+="if(fall=='FALL DETECTED'||faint=='FAINT DETECTED'||alert=='MANUAL ALERT'){";

page+="var audio=new Audio('https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg');";
page+="audio.play();";

page+="alert('MEDICAL EMERGENCY DETECTED');";

page+="}";

page+="}";

page+="setTimeout(playAlert,500);";

page+="</script>";

page+="</head><body>";

page+="<h1>Health Monitoring Dashboard</h1>";

page+="<div class='container'>";

page+="<div class='card'>";
page+="<div class='title'>Body Temperature</div>";
page+="<div class='value'>"+String(temperature,1)+" °C</div>";
page+="</div>";

page+="<div class='card'>";
page+="<div class='title'>Heart Rate</div>";
page+="<div class='value'>"+String(heartRate)+" BPM</div>";
page+="</div>";

page+="<div class='card'>";
page+="<div class='title'>Fall Detection</div>";

if(fallDetected)
page+="<div id='fall' class='alert'>FALL DETECTED</div>";
else
page+="<div id='fall' class='normal'>NORMAL</div>";

page+="</div>";

page+="<div class='card'>";
page+="<div class='title'>Faint Detection</div>";

if(faintDetected)
page+="<div id='faint' class='alert'>FAINT DETECTED</div>";
else
page+="<div id='faint' class='normal'>NORMAL</div>";

page+="</div>";

page+="<div class='card'>";
page+="<div class='title'>Manual Alert</div>";

if(manualAlert)
page+="<div id='alert' class='alert'>MANUAL ALERT</div>";
else
page+="<div id='alert' class='normal'>OFF</div>";

page+="</div>";

page+="</div>";

page+="</body></html>";

server.send(200,"text/html",page);
}

void setup() {

Serial.begin(115200);

WiFi.softAP(ssid,password);

Serial.println("Dashboard Ready");
Serial.println(WiFi.softAPIP());

server.on("/",handleRoot);
server.on("/update",updateData);

server.begin();
}

void loop() {

server.handleClient();

}
