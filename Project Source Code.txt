#include <LiquidCrystal.h>

int Contrast = 100;
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
// Sensor 1
const int trigPin1 = A1; // Pino TRIG do sensor ultrassônico 1
const int echoPin1 = A0; // Pino ECHO do sensor ultrassônico 1
const int ledRed1 = A2;   // Pino do LED vermelho para sensor 1
const int ledGreen1 = A3; // Pino do LED verde para sensor 1

// Sensor 2
const int trigPin2 = A5; // Pino TRIG do sensor ultrassônico 2
const int echoPin2 = A4; // Pino ECHO do sensor ultrassônico 2
const int ledRed2 = 10;   // Pino do LED vermelho para sensor 2
const int ledGreen2 = 9; // Pino do LED verde para sensor 2

void setup() {
  // Configurações do sensor 1
  pinMode(trigPin1, OUTPUT); // Define o pino TRIG como saída
  pinMode(echoPin1, INPUT);  // Define o pino ECHO como entrada
  pinMode(ledRed1, OUTPUT);  // Define o pino do LED vermelho como saída
  pinMode(ledGreen1, OUTPUT); // Define o pino do LED verde como saída
  
  // Configurações do sensor 2
  pinMode(trigPin2, OUTPUT); // Define o pino TRIG como saída
  pinMode(echoPin2, INPUT);  // Define o pino ECHO como entrada
  pinMode(ledRed2, OUTPUT);  // Define o pino do LED vermelho como saída
  pinMode(ledGreen2, OUTPUT); // Define o pino do LED verde como saída
  
  analogWrite(6, Contrast);
  lcd.begin(16, 2);
  delay(100); 
  lcd.setCursor(0, 0);
  lcd.print("|VAGA 1||VAGA 2|");
  lcd.setCursor(0, 1);
  lcd.print("|      ||      |");
 
 
}

void loop() {
  // Leitura do sensor 1
  long duration1;
  float distance1;

  
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1 = (duration1 * 0.0343) / 2;

  Serial.print("Sensor 1 - Distância: ");
  Serial.print(distance1);
  Serial.println(" cm");

  if (distance1 > 0 && distance1 <= 10) {
    digitalWrite(ledRed1, HIGH);   // Acende o LED vermelho
    digitalWrite(ledGreen1, LOW);  // Apaga o LED verde
  	lcd.setCursor(1, 1);
  	lcd.print("CHEIO");
  } else {
    digitalWrite(ledRed1, LOW);    // Apaga o LED vermelho
    digitalWrite(ledGreen1, HIGH); // Acende o LED verde
  	lcd.setCursor(1, 1);
  	lcd.print("LIVRE");
  }

  // Leitura do sensor 2
  long duration2;
  float distance2;

  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  duration2 = pulseIn(echoPin2, HIGH);
  distance2 = (duration2 * 0.0343) / 2;

  Serial.print("Sensor 2 - Distância: ");
  Serial.print(distance2);
  Serial.println(" cm");

  if (distance2 > 0 && distance2 <= 10) {
    digitalWrite(ledRed2, HIGH);   // Acende o LED vermelho
    digitalWrite(ledGreen2, LOW);  // Apaga o LED verde
    lcd.setCursor(9, 1);
  	lcd.print("CHEIO");
  } else {
    digitalWrite(ledRed2, LOW);    // Apaga o LED vermelho
    digitalWrite(ledGreen2, HIGH); // Acende o LED verde
	lcd.setCursor(9, 1);
  	lcd.print("LIVRE");
  }

  delay(500); // Aguarda meio segundo antes de medir novamente
}
