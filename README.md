## 1.창의공학을 통해서 학생들이 학습해야할 내용에 대해 5가지를 적으시오. (창의성 함양, 협업, 소통기술 꼭 포함 되어야 합니다.)
- 창의성과 문제 해결 능력: 학생들은 주어진 공학 과제나 문제를 해결하는 과정에서 창의적인 접근 방법을 개발하고 적용하는 능력을 키워야 합니다. 이를 통해 새로운 아이디어를 발굴하고 혁신적인 솔루션을 개발하는 데 필요한 능력을 향상시켜야 합니다.

- 팀워크와 협업: 학생들은 팀 기반 프로젝트를 통해 협업 능력을 강화해야 합니다. 다양한 역할을 수행하고 서로의 강점을 이용하여 과제를 완료하는 경험을 통해 팀워크와 리더십 능력을 키워야 합니다.

- 효과적인 소통 기술: 학생들은 프로젝트 팀 내에서의 원활한 의사소통을 위한 기술을 개발해야 합니다. 이는 아이디어 공유, 계획 수립, 문제 해결, 프로젝트 상황 업데이트 등 다양한 상황에서 필요합니다.
  
- 하드웨어 및 소프트웨어 통합: 학생들은 하드웨어와 소프트웨어의 상호작용을 이해하고, 실제 시스템을 구축하고 프로그래밍하는 능력을 개발해야 합니다. 이는 마이크로 컴퓨터, 센서, 액추에이터와 같은 하드웨어 구성 요소와 프로그래밍 언어 및 통신 프로토콜에 대한 지식을 포함합니다.

- IoT 및 미래 기술 이해: 학생들은 인터넷 of Things (IoT) 및 미래의 기술 동향에 대한 이해를 개발해야 합니다. 이를 통해 현대 공학 도메인에서 중요한 역할을 하는 기술 및 트렌드를 파악하고 이를 활용하여 혁신적인 프로젝트와 솔루션을 개발할 수 있어야 합니다.

이러한 내용을 통해 학생들은 창의성을 향상시키고 협업 및 소통 능력을 향상시키며, 미래에 필요한 공학 능력을 구축하는데 도움을 받을 것입니다.

## 2.스마트폰으로 집의 전등을 켜고 끄는 시스템을 설계하고 제작하는 과정을 유무선 통신을 포함한 과정을 자세히 설명하시오. (릴레이 추가하면 만점)
<앱인벤터></br>
- 앱 인벤터를 사용하여 스마트폰 앱을 디자인하고 프로그래밍합니다. 전등을 on / off 하는 버튼과 신호를 생성하고 블럭코딩을 합니다. ip주소를 통해 서버연결을 할 수 있도록 하고 아두이노, 프로세싱, 앱인벤터가 구성되면 폰의 앱을 통해 원하는 동작을 할 수 있습니다.


<회로>
> 릴레이 모듈과 아두이노를 연결하기 </br>
릴레이 모듈에는 VCC, GND, IN 제어핀이 있고 이를 아두이노와 릴레이를 연결하고 전등을 릴레이에 연결합니다.

```
[아두이노코드]
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "YourWiFiSSID";  // 여기에 WiFi SSID를 입력하세요.
const char* password = "YourWiFiPassword";  // 여기에 WiFi 비밀번호를 입력하세요.

ESP8266WebServer server(3);  // 포트 번호를 3으로 변경합니다.

// 릴레이 제어 핀 설정
const int relayPin = 2;

void setup() {
  Serial.begin(115200);
  delay(10);

  pinMode(relayPin, OUTPUT); // 릴레이 제어 핀 설정

  // Wi-Fi 연결
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");

  // 웹서버 시작
  server.on("/", HTTP_GET, handleRoot);
  server.begin();
}

void loop() {
  server.handleClient();
}

void handleRoot() {
  String state = server.arg("state"); // 전등 상태 가져오기
  if (state == "on") {
    digitalWrite(relayPin, HIGH); // 릴레이를 켜서 전등 켜기
  } else if (state == "off") {
    digitalWrite(relayPin, LOW); // 릴레이를 끌어서 전등 끄기
  }
  server.send(200, "text/plain", "OK");
}

```
```
[프로세싱코드]
import processing.serial.*;

Serial arduino;  // 아두이노와 시리얼 통신을 위한 객체

void setup() {
  arduino = new Serial(this, "COM3", 9600); // 아두이노 포트 번호와 통신 속도를 설정합니다.
}

void draw() {
  // 아두이노에서 받은 데이터 읽기
  while (arduino.available() > 0) {
    String data = arduino.readStringUntil('\n');
    if (data != null) {
      data = data.trim();
      println("Received from Arduino: " + data);
    }
  }
}

```
## 3.아두이노의 스위치를 읽고 서버로 1초에 1회씩 전송하는 코드와 서버에서 수신하여 1이면 초록색, 2이면 검은색으로 바꾸는 코드를 적으시오.
```
[ 아두이노코드 ]
void setup() {
Serial.begin(9600); // Start serial communication at 9600 bps
pinMode(2, INPUT); // Set pin as input for switch
}

void loop() {
int switchState = digitalRead(2); // Read the state of the switch
Serial.println(switchState); // Send the state of the switch to the serial port
delay(1000);
}
```
```
[프로세싱코드]
import processing.net.*;
import processing.serial.*;

Serial myPort;  
Client c;
String data;

void setup()
{
  size(200,200);
  
  String portName = "COM3"; // 아두이노가 연결된 COM 포트
  myPort = new Serial(this, portName, 9600);
  
  String ip = "192.168.215.247"; // 서버의 IP 주소
  int port =80; // 서버의 포트 번호
  
   c = new Client(this, ip , port);
}

void draw()
{
  
 if(myPort.available() >0)
 {
   data=myPort.readStringUntil('
');
   
   if(data != null)
     c.write(data.trim());
 }
}

void clientEvent(Client c) {
    while (c.available() >0) {
        char serverVal = c.readChar();
        if(serverVal == '1')
            background(color(0,255,0));
        else if(serverVal == '2')
            background(color(0));
    }
}
```
## 4.서버에서 스마트폰으로 1을 보내면 스마트폰의 화면이 푸른색으로, 0을 보내면 붉은 색으로 보내는 코드를 적으시오. 앱 인벤터의 컴포넌트와 내용을 적으시오.

## 5.스마트폰에서 on 버튼을 누르면 서버를 거처 아두이노의 7번 핀의 LED 켜지고, off 버튼을 누르면 7번 핀의 LED가 꺼진다. 스마트폰의 텍스트 박스에 숫자 1부터 8 중에 하나를 넣고, Send 버튼을 누르면 아두이노의 4번 핀에 달린 피에조 스피커가 도,레,~ 시,도가 작동되도록 하시오.
