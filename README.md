# internet-of-things-path


# task 1:
# connecting esp32 to the internet
This code sets up an ESP32 board to connect to a Wi-Fi network and then sends HTTP GET requests to five different URLs. The response from each URL is checked for a specific string, and depending on the string, the code sets the output pins of the ESP32 board to control a motor in a certain direction or to stop it. The code then waits for 5 seconds before sending the next HTTP request.



# task 2:
This code sets up an output pin on an Arduino board and waits for input from the serial monitor. When input is received, it reads the first byte and extracts the first bit using bitRead(). The value of the first bit is stored in the buttonState variable, and then used to set the output pin HIGH or LOW using digitalWrite(). The code then waits for 1 second before repeating the process.

https://www.tinkercad.com/things/3eUkV3v8HmA?sharecode=dRkKgTGbiAx6Mcx7vYWwah6MwODNVP1Rqgxd_TCOoiE

# task 3:
hdt sensor and detector using esp32 
# sketch.ino 
'''
 #include <WiFi.h>
#include <HTTPClient.h>

#include <DHT.h> 
#define DHTPIN 15 
#define DHTTYPE DHT22 
DHT dht22(DHTPIN, DHTTYPE); 

String URL = "http://ip address /file project /file code.php";

const char* ssid = "name the internet "; 
const char* password = "password "; 

int temperature = 0;
int humidity = 0;

void setup() {
  Serial.begin(115200);
  dht11.begin();   
}

void loop() {
  if(WiFi.status() != WL_CONNECTED) {
    WiFi.mode(WIFI_OFF);
  delay(1000);
  //This line hides the viewing of ESP as wifi hotspot
  WiFi.mode(WIFI_STA);
  
  WiFi.begin(ssid, password);
  Serial.println("Connecting to WiFi");
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
   Serial.println("");
  }
  temperature = dht11.readTemperature(); //Celsius
  humidity = dht11.readHumidity();
  if (isnan(temperature) || isnan(humidity)) {
    Serial.println("Failed to read from DHT sensor!");
    temperature = 0;
    humidity = 0;
  }
  Serial.printf("Temperature: %d °C\n", temperature);
  Serial.printf("Humidity: %d %%\n", humidity);
  String postData = "temperature=" + String(temperature) + "&humidity=" + String(humidity);
  
  HTTPClient http;
  http.begin(URL);
  http.addHeader("Content-Type", "application/x-www-form-urlencoded");
  
  int httpCode = http.POST(postData);
  String payload = "";

  if(httpCode > 0) {
    // file found at server
    if(httpCode == HTTP_CODE_OK) {
      String payload = http.getString();
      Serial.println(payload);
    } else {
      // HTTP header has been send and Server response header has been handled
      Serial.printf("[HTTP] GET... code: %d\n", httpCode);
    }
  } else {
    Serial.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
  }
  
  http.end();  //Close connection

  Serial.print("URL : "); Serial.println(URL); 
  Serial.print("Data: "); Serial.println(postData);
  Serial.print("httpCode: "); Serial.println(httpCode);
  Serial.print("payload : "); Serial.println(payload);
  Serial.println("--------------------------------------------------");
  delay(5000);
} 
'''
