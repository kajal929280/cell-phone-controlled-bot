# cell-phone-controlled-bot
 Arduino code for a simple Bluetooth-controlled robot that you can control using an Android phone via a Bluetooth module (HC-05 or HC-06). The bot will have four motors controlled via an L298N motor driver and will move based on commands received over Bluetooth.
// Arduino Code for Cell Phone Controlled Robot using Bluetooth
// Components: Arduino UNO, HC-05 Bluetooth Module, L298N Motor Driver

#define ENA 9   // Enable A - Motor A speed control
#define IN1 8   // Motor A Input 1
#define IN2 7   // Motor A Input 2

#define ENB 10  // Enable B - Motor B speed control
#define IN3 6   // Motor B Input 3
#define IN4 5   // Motor B Input 4

char command; // Variable to store Bluetooth command

void setup() {
    pinMode(ENA, OUTPUT);
    pinMode(IN1, OUTPUT);
    pinMode(IN2, OUTPUT);
    
    pinMode(ENB, OUTPUT);
    pinMode(IN3, OUTPUT);
    pinMode(IN4, OUTPUT);

    Serial.begin(9600);  // Initialize serial communication for HC-05
}

void loop() {
    if (Serial.available()) {
        command = Serial.read();
        Serial.println(command);  // Echo command for debugging
        moveRobot(command);
    }
}

void moveRobot(char cmd) {
    switch(cmd) {
        case 'F': // Forward
            digitalWrite(IN1, HIGH);
            digitalWrite(IN2, LOW);
            digitalWrite(IN3, HIGH);
            digitalWrite(IN4, LOW);
            analogWrite(ENA, 150); // Speed control
            analogWrite(ENB, 150);
            break;

        case 'B': // Backward
            digitalWrite(IN1, LOW);
            digitalWrite(IN2, HIGH);
            digitalWrite(IN3, LOW);
            digitalWrite(IN4, HIGH);
            analogWrite(ENA, 150);
            analogWrite(ENB, 150);
            break;

        case 'L': // Left turn
            digitalWrite(IN1, LOW);
            digitalWrite(IN2, HIGH);
            digitalWrite(IN3, HIGH);
            digitalWrite(IN4, LOW);
            analogWrite(ENA, 100);
            analogWrite(ENB, 150);
            break;

        case 'R': // Right turn
            digitalWrite(IN1, HIGH);
            digitalWrite(IN2, LOW);
            digitalWrite(IN3, LOW);
            digitalWrite(IN4, HIGH);
            analogWrite(ENA, 150);
            analogWrite(ENB, 100);
            break;

        case 'S': // Stop
            digitalWrite(IN1, LOW);
            digitalWrite(IN2, LOW);
            digitalWrite(IN3, LOW);
            digitalWrite(IN4, LOW);
            break;

        default:
            // Stop robot if unknown command is received
            digitalWrite(IN1, LOW);
            digitalWrite(IN2, LOW);
            digitalWrite(IN3, LOW);
            digitalWrite(IN4, LOW);
            break;
    }
}
