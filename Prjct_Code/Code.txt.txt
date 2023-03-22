int measurePin = 1; //Connect dust sensor to Arduino A1 pin
int ledPower = 2;   //Connect 3 led driver pins of dust sensor to Arduino D2
int relayPin = 8;   //Relay Pin
int sensorValue;

int samplingTime = 280;
int deltaTime = 40;
int sleepTime = 9680;

float voMeasured = 0;
float calcVoltage = 0;
float dustDensity = 0;

void setup(){
  Serial.begin(9600);
  pinMode(ledPower,OUTPUT);
  pinMode(relayPin, OUTPUT);      //Setting the Relay pin as an Output Pin
}

void loop(){
  digitalWrite(ledPower,LOW); // power on the LED
  delayMicroseconds(samplingTime);

  voMeasured = analogRead(measurePin); // read the dust value

  delayMicroseconds(deltaTime);
  digitalWrite(ledPower,HIGH); // turn the LED off
  delayMicroseconds(sleepTime);

  calcVoltage = voMeasured * (5.0 / 1024.0);

  dustDensity = 170 * calcVoltage - 0.1;

  Serial.print("Dust Sensor Reading = ");
  Serial.println(dustDensity); // unit: ug/m3

  // Controlling relay based on the Dust Sensor Value.
  if(dustDensity > 250) {
    digitalWrite(relayPin, LOW);
    delay(20000);
  }else{
    digitalWrite(relayPin, HIGH);
  }

  //Gas Sensor
  sensorValue = analogRead(0);
  Serial.print("AirQuality of  =");
  Serial.print(sensorValue, DEC);               // prints the value read
  Serial.println(" PPM");

  delay(500);
}
