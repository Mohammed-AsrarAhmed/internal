9.
```
C++ 
#include <WiFi.h> 
#include <HTTPClient.h>        

#include <WiFiClientSecure.h>
#include <DHTesp.h>            

const char* ssid = "Wokwi-GUEST"; 
const char* password = ""; 

const char* server = "api.thingspeak.com"; 
const int   channelID = 1234567;              
const char* writeAPIKey = "XXXXXXXXXXXXXXXX";

#define DHT_PIN 15 
#define LED_ALERT_PIN 2 
 
#define TEMP_HIGH_THRESHOLD  30.0 
#define TEMP_LOW_THRESHOLD   18.0 
#define HUM_HIGH_THRESHOLD   80.0 
 
DHTesp dht; 
WiFiClientSecure client;
 
void setup() { 
  Serial.begin(115200); 
  pinMode(LED_ALERT_PIN, OUTPUT); 
  digitalWrite(LED_ALERT_PIN, LOW); 

  dht.setup(DHT_PIN, DHTesp::DHT22); 
 
  WiFi.begin(ssid, password); 
  Serial.print("Connecting to WiFi..."); 
  while (WiFi.status() != WL_CONNECTED) { 
    delay(500); 
    Serial.print("."); 
  } 
  Serial.println("\nWiFi connected"); 
  Serial.print("IP address: "); 
  Serial.println(WiFi.localIP()); 
 
  client.setInsecure(); 
} 
 
void loop() { 
  static unsigned long lastSend = 0; 
  if (millis() - lastSend < 20000) {
    delay(100); 
    return; 
  } 
  lastSend = millis(); 
 
  TempAndHumidity data = dht.getTempAndHumidity(); 
  if (dht.getStatus() != DHTesp::ERROR_NONE) { 
    Serial.println("DHT read error!"); 
    return; 
  } 
 
  float temp = data.temperature; 
  float hum = data.humidity; 
 
  Serial.printf("Temp: %.1f °C | Humidity: %.1f %%\n", temp, hum); 
 
  int alert = 0; 
  String alertMsg = "Normal"; 
  digitalWrite(LED_ALERT_PIN, LOW); 
 
  if (temp > TEMP_HIGH_THRESHOLD) { 
    alert = 1; 
    alertMsg = "HIGH TEMP"; 
    digitalWrite(LED_ALERT_PIN, HIGH); 
  } else if (temp < TEMP_LOW_THRESHOLD) { 
    alert = 1; 
    alertMsg = "LOW TEMP"; 
    digitalWrite(LED_ALERT_PIN, HIGH); 
  } else if (hum > HUM_HIGH_THRESHOLD) { 
    alert = 1; 
    alertMsg = "HIGH HUMIDITY"; 
    digitalWrite(LED_ALERT_PIN, HIGH); 
  } 
 
  Serial.println("Alert: " + alertMsg); 
 
  if (WiFi.status() == WL_CONNECTED) { 
    HTTPClient http; 
    String url = "https://" + String(server) + "/update?api_key=" + 
writeAPIKey + 
                 "&field1=" + String(temp, 1) + 
                 "&field2=" + String(hum, 1) + 
                 "&field3=" + String(alert); 
 
    http.begin(client, url);
 
    int httpCode = http.GET();
 
    if (httpCode > 0) { 
      Serial.printf("ThingSpeak Response: %d\n", httpCode); 
      if (httpCode == 200) { 
        Serial.println("Data sent successfully!"); 
      } 
    } else { 
      Serial.println("HTTP request failed"); 
    } 
    http.end(); 
  } else { 
    Serial.println("WiFi lost"); 
  } 
 
  delay(100);
}
```
10.
```
import network
import urequests
import time
import machine

ssid = "Wokwi-GUEST"
password = ""

server = "api.thingspeak.com"
channel_id = 1234567
write_api_key = "XXXXXXXXXXXXXXXX"

trigger_pin = machine.Pin(15, machine.Pin.OUT)
echo_pin = machine.Pin(14, machine.Pin.IN)
led_alert_pin = machine.Pin(13, machine.Pin.OUT)

DISTANCE_CLOSE_THRESHOLD = 10.0

def connect_wifi():
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, password)
    print("Connecting to WiFi...")
    while not wlan.isconnected():
        time.sleep(0.5)
        print(".", end="")
    print("\nWiFi connected")
    print("IP address:", wlan.ifconfig()[0])

def measure_distance():
    trigger_pin.off()
    time.sleep_us(2)
    trigger_pin.on()
    time.sleep_us(10)
    trigger_pin.off()

    while echo_pin.value() == 0:
        signal_off = time.ticks_us()
    while echo_pin.value() == 1:
        signal_on = time.ticks_us()

    time_passed = signal_on - signal_off
    distance = (time_passed * 0.0343) / 2  # round trip
    return distance

connect_wifi()

last_send = 0

while True:
    if time.time() - last_send < 20:
        time.sleep(1)
        continue

    last_send = time.time()

    distance = measure_distance()
    print(f"Distance: {distance:.1f} cm")

    alert = 0
    alert_msg = "Normal"
    led_alert_pin.off()

    if distance < DISTANCE_CLOSE_THRESHOLD:
        alert = 1
        alert_msg = "OBJECT TOO CLOSE"
        led_alert_pin.on()

    print("Alert:", alert_msg)

    url = f"https://{server}/update?api_key={write_api_key}&field1={distance:.1f}&field2={alert}"

    try:
        response = urequests.get(url)
        print("ThingSpeak Response:", response.status_code)
        response.close()
    except Exception as e:
        print("HTTP request failed:", e)

    time.sleep(1)

```
11.
```
String secretKey = "K";

String encrypt(String data) {
  String encrypted = "";
  for (int i = 0; i < data.length(); i++) {
    encrypted += (char)(data[i] ^ secretKey[0]);
  }
  return encrypted;
}

String decrypt(String data) {
  return encrypt(data);
}

void setup() {
  Serial.begin(115200);

  randomSeed(analogRead(0));
  int temp = random(25, 35);

  String sensorData = "TEMP:" + String(temp);
  String encryptedData = encrypt(sensorData);
  String decryptedData = decrypt(encryptedData);

  Serial.println("Original Data : " + sensorData);
  Serial.println("Encrypted Data : " + encryptedData);
  Serial.println("Decrypted Data : " + decryptedData);
}

void loop() {
}

```
12.
```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";

const char* mqttServer = "broker.hivemq.com";
const int mqttPort = 1883;

const char* deviceID = "ESP32_Device_001";
const char* mqttUser = "iot_user_demo";
const char* mqttPassword = "iot_pass_demo";

WiFiClient espClient;
PubSubClient client(espClient);

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message received: ");
  for (unsigned int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();
}

void connectToWiFi() {
  Serial.print("Connecting to WiFi");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWiFi Connected");
}

void connectToMQTT() {
  while (!client.connected()) {
    Serial.print("Authenticating device... ");
    if (client.connect(deviceID, mqttUser, mqttPassword)) {
      Serial.println("SUCCESS");
      client.subscribe("iot/auth/demo");
    } else {
      Serial.print("FAILED, rc=");
      Serial.println(client.state());
      delay(2000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  connectToWiFi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    connectToMQTT();
  }
  client.loop();

  client.publish("iot/auth/demo", "Authenticated IoT Device Access Granted");
  delay(5000);
}

```
