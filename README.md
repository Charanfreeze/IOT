# IOT
#include <PubSubClient.h>
#include <WiFi.h>
const char* ssid = "Redmi";
const char* password = "12345678";
const char* mqttServer = "m12.cloudmqtt.com";
const char* mqttusername = "iwjxatav";
const char* mqttpassword = "iCJRhvk8G2SQ";
const int mqttport = 11650;
int relay=8;
char temp[10]={40};


WiFiClient espClient;
PubSubClient client(espClient);

void setup()
{
  Serial.begin(9600);
  setup_wifi();
  client.setServer(mqttServer , mqttport);
  while (!client.connected()){
    Serial.println("connecting to mqtt...");

    if (client.connect("ESP32Client", mqttusername , mqttpassword)){

      Serial.println("connected");
    }else{

      Serial.print("failed with state");
      Serial.print(client.state());
      delay(2000);
      }
  }
}
void setup_wifi() {
  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}
void loop() {
  // put your main code here, to run repeatedly:
 //float temp;
Serial.println(temp);
client.publish("relay/test",temp); 
delay(1000);
char far[20] =  {fahrenheit()};
Serial.print("fahrenheit");
Serial.println(far);
client.publish("relay/test",far);
delay(500);
if(far > 90)
  {
    digitalWrite(relay,HIGH);
    client.publish("relay/test","ac on");
    delay(500);
  }
    else
    {
    Serial.println("ac off");
    client.publish("relay/test"," ac off");
    }
}
float fahrenheit()
{
  //float data = temp ;
  char f[20]= {(temp*1.8) + 32};
  //return f;
}
