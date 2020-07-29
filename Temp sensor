#include <LiquidCrystal.h>
LiquidCrystal lcd (12,10,4,5,6,7);
int PIRpin = 11;
int LED = 9;
int pirRead = 0;
int tempPin = A0; // Because body temperature is analog signal.
int tempC; 
int speakerPin = 2;
int warn = 392; //when the temp is inappropriate.
int numMax = 3; //replay the 'warn' sound when the temp is invalid.
int rLedPin = 13;
int gLedPin = 8;
int bLedPin = 9;

byte degree[] ={
  0b00110,
  0b01001,
  0b01001,
  0b00110,
  0b00000,
  0b00000,
  0b00000,
  0b00000}; // º as a symbor of user

   
void setup() {
  pinMode(rLedPin, OUTPUT);
  pinMode(gLedPin, OUTPUT);
  pinMode(bLedPin, OUTPUT);
  pinMode(PIRpin, INPUT);
  pinMode(speakerPin, OUTPUT);
  
  digitalWrite(rLedPin, LOW);
  digitalWrite(gLedPin, LOW);
  digitalWrite(bLedPin, LOW);
  
  lcd.begin(16,2);
  Serial.begin(9600);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Please take your");
    lcd.setCursor(0,1);
    lcd.print("body temperature");
  delay(1000);
} // Display the sentence on LCD monitor while no motion detected.



void RGBLED_Display(int Num) {
  switch (Num) {
    case 0 :
    digitalWrite(rLedPin, LOW);
    digitalWrite(gLedPin, LOW);
    digitalWrite(bLedPin, LOW);
    break;
    
    case 1 :
    digitalWrite(rLedPin, HIGH);
    digitalWrite(gLedPin, LOW);
    digitalWrite(bLedPin, LOW);
    break;
    
    case 2 :
    digitalWrite(rLedPin, LOW);
    digitalWrite(gLedPin, HIGH);
    digitalWrite(bLedPin, LOW);
    break;
    
    case 3 :
    digitalWrite(rLedPin, LOW);
    digitalWrite(gLedPin, LOW);
    digitalWrite(bLedPin, HIGH);
    break;
  }
}
 

void loop() {
  pirRead = digitalRead(PIRpin);
  RGBLED_Display(0);
  if(pirRead == HIGH) {
    digitalWrite(LED, HIGH);
    double getTempC = analogRead(tempPin);
    float voltage = getTempC*5000.0/1024.0;
    float tempC = (voltage-500)/10.0;
    Serial.println("Current Temp. : "+ (String)tempC + "C");
      lcd.setCursor(0,0);
      lcd.print("Your temp: ");
      lcd.setCursor(0,2);
      lcd.createChar(0,degree);
      lcd.setCursor(11,0);
      lcd.print(tempC);
      lcd.write((byte)0);
      lcd.write("C  ");
    if (tempC<37.2) {
      lcd.setCursor(0,2);
      lcd.print("      PASS        ");
      RGBLED_Display(2);
    }
    else if (tempC<40.0) {
      lcd.setCursor(0,2);
      lcd.print(" TEMP TOO HIGH  ");
      tone(speakerPin,warn,1000);
      RGBLED_Display(1);
    }
    else {
      lcd.setCursor(0,0);
      lcd.print(" Invalid value. ");
      lcd.setCursor(0,2);
      lcd.print(" Please recheck ");
      RGBLED_Display(3);
      while(numMax) {
      tone(speakerPin,warn,500);
      delay(500);
        numMax--;
      }
      numMax = 3;
    }
    delay(1000);
    }  

  else {
    digitalWrite(LED, LOW);
    delay(1000);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Please take your");
    lcd.setCursor(0,1);
    lcd.print("body temperature");
  }
}
