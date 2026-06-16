# Obstacle-Avoidance-Robot-using-Arduino
This project implements an autonomous obstacle avoidance robot using an Arduino, an ultrasonic sensor, and DC motors. The robot continuously measures the distance to obstacles and changes its direction when an obstacle is detected.

-:CODE:-

/*----------------------------------------------------
  Obstacle Avoidance Robot
  Controller : Arduino UNO
  Sensor     : HC-SR04 Ultrasonic Sensor
  Driver     : L298N Motor Driver
----------------------------------------------------*/

#define TRIG_PIN 9
#define ECHO_PIN 10

#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5

#define SAFE_DISTANCE 20   // cm

/* Function Prototypes */
void Motor_Forward(void);
void Motor_Backward(void);
void Motor_Left(void);
void Motor_Right(void);
void Motor_Stop(void);
unsigned int Get_Distance(void);

void setup()
{
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);

    pinMode(IN1, OUTPUT);
    pinMode(IN2, OUTPUT);
    pinMode(IN3, OUTPUT);
    pinMode(IN4, OUTPUT);

    Serial.begin(9600);
}

void loop()
{
    unsigned int distance;

    distance = Get_Distance();

    Serial.print("Distance = ");
    Serial.print(distance);
    Serial.println(" cm");

    if(distance > SAFE_DISTANCE)
    {
        Motor_Forward();
    }
    else
    {
        Motor_Stop();
        delay(300);

        Motor_Backward();
        delay(500);

        Motor_Stop();
        delay(200);

        Motor_Right();
        delay(700);

        Motor_Stop();
        delay(200);
    }

    delay(50);
}

/* Ultrasonic Distance Measurement */
unsigned int Get_Distance(void)
{
    long duration;
    unsigned int distance;

    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);

    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);

    digitalWrite(TRIG_PIN, LOW);

    duration = pulseIn(ECHO_PIN, HIGH);

    distance = (duration * 0.0343) / 2;

    return distance;
}

/* Move Forward */
void Motor_Forward(void)
{
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, LOW);

    digitalWrite(IN3, HIGH);
    digitalWrite(IN4, LOW);
}

/* Move Backward */
void Motor_Backward(void)
{
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, HIGH);

    digitalWrite(IN3, LOW);
    digitalWrite(IN4, HIGH);
}

/* Turn Left */
void Motor_Left(void)
{
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, HIGH);

    digitalWrite(IN3, HIGH);
    digitalWrite(IN4, LOW);
}

/* Turn Right */
void Motor_Right(void)
{
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, LOW);

    digitalWrite(IN3, LOW);
    digitalWrite(IN4, HIGH);
}

/* Stop Motors */
void Motor_Stop(void)
{
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);

    digitalWrite(IN3, LOW);
    digitalWrite(IN4, LOW);
}

