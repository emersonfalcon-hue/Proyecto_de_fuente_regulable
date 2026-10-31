#include <Wire.h>
#include <LiquidCrystal_I2C.h>


LiquidCrystal_I2C lcd(0x27, 16, 2);

const int pinAnalogico = A0;


const float R1 = 30000.0; // 30 kΩ
const float R2 = 10000.0; // 10 kΩ

void setup() {
  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("Voltimentro DC");
  lcd.setCursor(0, 1);
  lcd.print("Rango: 0 - 20V");
  delay(2000);
  lcd.clear();
}

void loop() {

  long sumaADC = 0;
  for (int i = 0; i < 20; i++) {
    sumaADC += analogRead(pinAnalogico);
    delay(5);
  }
  float promedioADC = sumaADC / 20.0;


  float voltajeADC = (promedioADC * 5.0) / 1023.0;


  float voltajeEntrada = voltajeADC * ((R1 + R2) / R2);


  if (voltajeEntrada < 0.08) {
    voltajeEntrada = 0.0;
  }



  lcd.setCursor(0, 0);
  lcd.print("Lectura DC:");
  
  lcd.setCursor(0, 1);
  lcd.print("Voltaje: ");
  lcd.print(voltajeEntrada, 2); 
  lcd.print(" V  ");

  delay(300);
}
