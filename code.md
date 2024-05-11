# trabalho-escola
/ Rega Automática 

   Guia de conexão:
   LCD RS: pino 12
   LCD Enable: pino 11
   LCD D4: pino 5
   LCD D5: pino 4
   LCD D6: pino 3
   LCD D7: pino 2
   LCD R/W: GND
   LCD VSS: GND
   LCD VCC: VCC (5V)
   Potenciômetro de 10K terminal 1: GND
   Potenciômetro de 10K terminal 2: V0 do LCD (Contraste)
   Potenciômetro de 10K terminal 3: VCC (5V)
   Sensor de umidade do solo A0: Pino A0
   Módulo Relé (Válvula): Pino 10

// define os pinos de conexão entre o Arduino e o Display LCD
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);


const int pinoSensor = A0;
const int pinoValvula = 10;
const int limiarSeco = 74;
const int tempoRega = 50; // Tempo de rega em segundos
int umidadeSolo = 0;

void setup() {

  pinMode(pinoValvula, OUTPUT);

  digitalWrite(pinoValvula, HIGH);

  lcd.begin(16, 2);
 
  lcd.print(" Rega do Manual ");

  Serial.begin(9600);


}

void loop() {

  for(int i=0; i < 5; i++) {

    lcd.setCursor(0, 1);

    lcd.print("Umidade: ");

    umidadeSolo = analogRead(pinoSensor);

    umidadeSolo = map(umidadeSolo, 1023, 0, 0, 100);
    lcd.print(umidadeSolo);
    lcd.print(" %    ");

    delay(1000);
  }

  if(umidadeSolo < limiarSeco) {

    lcd.setCursor(0, 1);
    // Exibe a mensagem no Display LCD:
    lcd.print("    Regando     ");
    // Liga a válvula
    digitalWrite(pinoValvula, LOW);
    // Espera o tempo estipulado
    delay(tempoRega*1000);
    digitalWrite(pinoValvula, HIGH);
  }
  else {

    lcd.setCursor(0, 1)
    lcd.print("Solo Encharcado ");
    delay(3000);
  }
}
