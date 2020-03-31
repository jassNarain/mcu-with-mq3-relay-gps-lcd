#include <LiquidCrystal.h>
#include <SoftwareSerial.h>
#include <TinyGPS.h>
String myindex = "1"; // this number is incremented as we increase the number of GPS modules. 
// let's say if you are using 2 GPS modules, then we will use the same programming but will change the index number to 1, for third nodemcu module we change it to 2 and so on. 
String mylong = ""; // for storing the longittude value
String mylati = ""; // for storing the latitude value
String myvalue = "abc"; 
int state = 0;
const int pin = 11;
float gpslat, gpslon;
char buff[10];

TinyGPS gps;
SoftwareSerial sgps(4, 5);// gps RX with Pin 5,Tx with Pin 4
LiquidCrystal lcd(6,7,8,9,10,12);
void setup()
{
  sgps.begin(9600);
  lcd.begin(16,2);
  pinMode(pin,INPUT);
  pinMode(13,OUTPUT);
  digitalWrite(13,0);
        lcd.print("Alcohol Based ");
      lcd.setCursor(0,1);
      lcd.print("GPS Detection");
      delay(3000);
      lcd.clear();
}


void loop()
{
  while (sgps.available())
  {
    int c = sgps.read();
    if (gps.encode(c))
    {
      gps.f_get_position(&gpslat, &gpslon);
    }
  }
   lcd.print("Latitude:");
      lcd.print(gpslat, 6);
      lcd.setCursor(0,1);
      lcd.print("Longitude:");
      lcd.print(gpslon ,6);
      lcd.setCursor(0,0);
      lcd.print("Latitude:");
      lcd.print(gpslat, 6);
    if (digitalRead(pin) == high && state == 1) {
      digitalWrite(13,1);
      lcd.setCursor(0,0);
      lcd.print("Your Location is");
      lcd.setCursor(0,1);
      lcd.print(" being  shared  ");     
    }
  if (digitalRead(pin) == HIGH)
 {
      state = 1;
    }
  }
