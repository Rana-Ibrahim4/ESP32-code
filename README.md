# ESP32-code
### Use http request in ESP32 code to get the data from database
##### steps to use HTTP requests in ESP32 code to get data from a database:

 1. Load the required libraries:
At the beginning of your ESP32 program, load the necessary libraries for making HTTP requests, such as:
ex Git :
```
#include <WiFi.h>
#include <HTTPClient.h>
```
2. Connect to the Wi-Fi network:
Connect the ESP32 to the Wi-Fi network using your connection details:
ex Git :
```
const char* ssid = "your_wifi_ssid";
const char* password = "your_wifi_password";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");
}

```
3. Send an HTTP GET request:
After connecting to the network, use the HTTPClient.h library to send an HTTP GET request to the URL of your database:
ex Git :
```
void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    http.begin("http://your_database_url/data");
    int httpResponseCode = http.GET();
    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println(response);
    } else {
      Serial.print("Error code: ");
      Serial.println(httpResponseCode);
    }
    http.end();
  } else {
    Serial.println("WiFi Disconnected");
  }
  delay(5000);
}

```
4. Process the received data:
After receiving the data from the database, you can process it and perform any other necessary operations.

5. Add security and verification:
As mentioned earlier, it is important to implement good security practices such as using HTTPS, API keys, and input validation to ensure the security of the connection.


### ESP32 Send Temperature and Humidity data to localhost XAMPP 
##### code php
ex Git :
```
<?php

 
 
$hostname = "localhost"; 
$username = "root"; 
$password = ""; 
$database = "project-db"; 

$conn = mysqli_connect($hostname, $username, $password, $database);

if (!$conn) { 
	die("Connection failed: " . mysqli_connect_error()); 
} 

echo "Database connection is OK<br>"; 

$t = 50;
$h = 40;

$sql = "INSERT INTO dht11 (temperature, humidity) VALUES (".$t.", ".$h.")"; 

if (mysqli_query($conn, $sql)) { 
	echo "New record created successfully"; 
} else { 
	echo "Error: " . $sql . "<br>" . mysqli_error($conn); 
}

?>


```
#### Arduinu code 


ex Git :
```
#include <WiFi.h>
#include <HTTPClient.h>

#include <DHT.h> 
#define DHTPIN 19 //D19 
#define DHTTYPE DHT11 
DHT dht11(DHTPIN, DHTTYPE); 

String URL = "http://172.20.10.7/dht11_project/test_data.php"";

const char* ssid = "OPPO A54"; 
const char* password = "OPPO A54"; 

int temperature = 23;
int humidity = 42;

void setup() {
  Serial.begin(115200);

  dht11.begin(); 
  
  connectWiFi();
}

void loop() {
  if(WiFi.status() != WL_CONNECTED) {
    connectWiFi();
  }

  Load_DHT11_Data();
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


void Load_DHT11_Data() {
  //-----------------------------------------------------------
  temperature = dht11.readTemperature(); //Celsius
  humidity = dht11.readHumidity();
  //-----------------------------------------------------------
  // Check if any reads failed.
  if (isnan(temperature) || isnan(humidity)) {
    Serial.println("Failed to read from DHT sensor!");
    temperature = 0;
    humidity = 0;
  }
  //-----------------------------------------------------------
  Serial.printf("Temperature: %d °C\n", temperature);
  Serial.printf("Humidity: %d %%\n", humidity);
}

void connectWiFi() {
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
    
  Serial.print("connected to : "); Serial.println(ssid);
  Serial.print("IP address: "); Serial.println(WiFi.localIP());
}
```
##### In this example, the program connects to the Wi-Fi network, then sends a GET HTTP request to the specified server address. The HTTP response code and the payload received are displayed in the log.

![لقطة شاشة 2024-07-27 115658](https://github.com/user-attachments/assets/54235b36-083d-43ec-a356-79ccdfad701a)


![لقطة شاشة 2024-07-27 115710](https://github.com/user-attachments/assets/0e329273-138e-4052-9cd3-8b8eaee62f34)


![wiring](https://github.com/user-attachments/assets/97ea93ba-17fb-4b1e-bb9f-7308d55b311f)
