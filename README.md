# AHT10-X-OLED-7
```arduino=
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <AHT10.h>

double maxx=-100,minn=999;

uint8_t readStatus = 0;
AHT10 myAHT10(AHT10_ADDRESS_0X38);
const uint64_t pipe = 0x00FF; // 設定發送端 pipe 名稱
 

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels

// Declaration for SSD1306 display connected using software SPI (default case):
#define OLED_MOSI   9
#define OLED_CLK   10
#define OLED_DC    11
#define OLED_CS    12
#define OLED_RESET 13
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
  OLED_MOSI, OLED_CLK, OLED_DC, OLED_RESET, OLED_CS);

/*Comment out above, uncomment this block to use hardware SPI
#define OLED_DC     8
#define OLED_CS     10
#define OLED_RESET  9
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
  &SPI, OLED_DC, OLED_RESET, OLED_CS);
*/
void setup() {
  Serial.begin(9600);
  myAHT10.begin();
  if(!display.begin(SSD1306_SWITCHCAPVCC)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  while (myAHT10.begin() != true)
  {
    Serial.println(F("AHT10 not connected...")); 
    delay(5000);
  }
  Serial.println(F("AHT10 initial OK"));
  }
}


void shower(){
 
    display.clearDisplay();
    double t = myAHT10.readTemperature();    //讀取 AHT10 溫度
    double h = myAHT10.readHumidity();   //讀取 AHT10 濕度
    maxx=max(maxx,t);
    minn=min(minn,t);
    Serial.print("Temperatura: ");
    Serial.print(t);
    Serial.print("   Humidity: ");
    Serial.println(h);

    //-------------------

    display.setTextSize(2); // Draw 2X-scale text
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0,0);
    display.print(F("Temp:"));
    display.println(t);
    display.print(F("Humi:"));
    display.println(h);
    display.print(F("maxt:"));
    display.println(maxx);
    display.print(F("mint:"));
    display.println(minn);
    
    display.display();      // Show initial text
  }

void loop(){
  shower();
  delay(500);
  }

```
