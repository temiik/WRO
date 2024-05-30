#include <Arduino.h>

// Настройка пинов
const int TRIG_PIN_LEFT = 2;
const int ECHO_PIN_LEFT = 3;
const int TRIG_PIN_RIGHT = 4;
const int ECHO_PIN_RIGHT = 5;
const int MOTOR_LEFT_FORWARD = 6;
const int MOTOR_LEFT_BACKWARD = 7;
const int MOTOR_RIGHT_FORWARD = 8;
const int MOTOR_RIGHT_BACKWARD = 9;

// Пороговое значение для срабатывания
const int THRESHOLD_DISTANCE = 100;

void setup() {
  // Настройка пинов моторов как выходы
  pinMode(MOTOR_LEFT_FORWARD, OUTPUT);
  pinMode(MOTOR_LEFT_BACKWARD, OUTPUT);
  pinMode(MOTOR_RIGHT_FORWARD, OUTPUT);
  pinMode(MOTOR_RIGHT_BACKWARD, OUTPUT);

  // Настройка пинов ультразвуковых датчиков как входы и выходы
  pinMode(TRIG_PIN_LEFT, OUTPUT);
  pinMode(ECHO_PIN_LEFT, INPUT);
  pinMode(TRIG_PIN_RIGHT, OUTPUT);
  pinMode(ECHO_PIN_RIGHT, INPUT);

  // Инициализация серийного соединения
  Serial.begin(9600);

  // Начальная езда прямо
  moveForward();
}

void loop() {
  // Получение расстояний с ультразвуковых датчиков
  unsigned int distanceLeft = getDistance(TRIG_PIN_LEFT, ECHO_PIN_LEFT);
  unsigned int distanceRight = getDistance(TRIG_PIN_RIGHT, ECHO_PIN_RIGHT);

  // Вывод данных в сериальный монитор
  Serial.print("Left: ");
  Serial.print(distanceLeft);
  Serial.print(" cm, Right: ");
  Serial.print(distanceRight);
  Serial.println(" cm");

  // Проверка дистанции и управление движением
  if (distanceRight > THRESHOLD_DISTANCE) {
    // Поворот направо
    turnRight();
  } else if (distanceLeft > THRESHOLD_DISTANCE) {
    // Поворот налево
    turnLeft();
  } else {
    // Езда прямо
    moveForward();
  }

  delay(100);
}

unsigned int getDistance(int trigPin, int echoPin) {
  // Генерация импульса
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Чтение времени отклика
  unsigned long duration = pulseIn(echoPin, HIGH);

  // Вычисление расстояния в сантиметрах
  unsigned int distance = duration * 0.034 / 2;

  return distance;
}

void moveForward() {
  digitalWrite(MOTOR_LEFT_FORWARD, HIGH);
  digitalWrite(MOTOR_LEFT_BACKWARD, LOW);
  digitalWrite(MOTOR_RIGHT_FORWARD, HIGH);
  digitalWrite(MOTOR_RIGHT_BACKWARD, LOW);
}

void turnLeft() {
  digitalWrite(MOTOR_LEFT_FORWARD, LOW);
  digitalWrite(MOTOR_LEFT_BACKWARD, HIGH);
  digitalWrite(MOTOR_RIGHT_FORWARD, HIGH);
  digitalWrite(MOTOR_RIGHT_BACKWARD, LOW);
  delay(500); // Время поворота, может быть скорректировано
  moveForward();
}

void turnRight() {
  digitalWrite(MOTOR_LEFT_FORWARD, HIGH);
  digitalWrite(MOTOR_LEFT_BACKWARD, LOW);
  digitalWrite(MOTOR_RIGHT_FORWARD, LOW);
  digitalWrite(MOTOR_RIGHT_BACKWARD, HIGH);
  delay(500); // Время поворота, может быть скорректировано
  moveForward();
}
