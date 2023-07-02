#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x3F,16,2);  //

const int Trig= 11;
const int Echo= 12;
const int Led_Rojo= 7;
const int Led_Amarillo= 8;
const int Led_Verde= 9;

long tiempoEcho;
long Distancia;

void setup() {
  Serial.begin(9600);
  pinMode(Echo, INPUT);
  pinMode(Trig, OUTPUT);

  pinMode(Led_Rojo, OUTPUT);
  pinMode(Led_Amarillo, OUTPUT);
  pinMode(Led_Verde, OUTPUT);
  digitalWrite(Trig, LOW);

  // Inicializar el LCD
  lcd.init();
  
  //Encender la luz de fondo.
  lcd.backlight();

}

void loop() {
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);

  tiempoEcho = pulseIn(Echo, HIGH); // retorna tiempo en microseg.
  Distancia  = tiempoEcho *0.017;  // distancia en centimentros

  lcd.setCursor(0, 1);
  lcd.print(Distancia);
  lcd.print(" cm    ");
  delay(100);

  if (Distancia > 2000)
  {
    digitalWrite(Led_Verde, HIGH);
    digitalWrite(Led_Amarillo, LOW);
    digitalWrite(Led_Rojo, LOW);
    lcd.setCursor(0, 0);
    lcd.print("Conduzca");
   delay(1000);   
    lcd.clear();
  } 
  if(Distancia < 2000 && Distancia > 500)
  {
    digitalWrite(Led_Verde, LOW);
    digitalWrite(Led_Amarillo, HIGH);
    digitalWrite(Led_Rojo, LOW); 
    lcd.setCursor(0, 0);
    lcd.print("Precaucion");
    delay(1000);   
    lcd.clear();
  }
  if(Distancia < 500)
  {
    digitalWrite(Led_Verde, LOW);
    digitalWrite(Led_Amarillo, LOW);
    digitalWrite(Led_Rojo, HIGH);
    lcd.setCursor(0, 0);
    lcd.print("Frene");
    delay(1000);   
    lcd.clear();    
  }
}
