// Starting of Program
int m1a = 9;
int m1b = 10;
int m2a = 11;
int m2b = 12;
int trigPin = 8; // Ultrasonic sensor trigger pin
int echoPin = 9; // Ultrasonic sensor echo pin
int buzzerPin = 7; // Buzzer pin
char val;

void setup() 
{  
  pinMode(m1a, OUTPUT);  // Digital pin 10 set as output Pin
  pinMode(m1b, OUTPUT);  // Digital pin 11 set as output Pin
  pinMode(m2a, OUTPUT);  // Digital pin 12 set as output Pin
  pinMode(m2b, OUTPUT);  // Digital pin 13 set as output Pin
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
}

void loop()
{
  // Ultrasonic sensor code
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) / 29.1; // Convert distance to centimeters

  // Buzzer code
  if (distance < 20) // Adjust the distance threshold as needed
  {
    digitalWrite(buzzerPin, HIGH); // Turn on the buzzer
  }
  else
  {
    digitalWrite(buzzerPin, LOW); // Turn off the buzzer
  }

  // Serial communication code
  while (Serial.available() > 0)
  {
    val = Serial.read();
    Serial.println(val);
  }

  // Motor control code
  if (val == 'F') // Forward
  {
    digitalWrite(m1a, HIGH);
    digitalWrite(m1b, LOW);
    digitalWrite(m2a, HIGH);
    digitalWrite(m2b, LOW);
  }
  else if (val == 'B') // Backward
  {
    digitalWrite(m1a, LOW);
    digitalWrite(m1b, HIGH);
    digitalWrite(m2a, LOW);
    digitalWrite(m2b, HIGH);
  }
  // ... (remaining motor control code)

  // Stop the motors if an obstacle is detected
  if (distance < 20) // Adjust the distance threshold as needed
  {
    digitalWrite(m1a, LOW);
    digitalWrite(m1b, LOW);
    digitalWrite(m2a, LOW);
    digitalWrite(m2b, LOW);
  }
}
