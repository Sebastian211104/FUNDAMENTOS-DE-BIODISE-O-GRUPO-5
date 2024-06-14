// Definición de los pines
const int dirPin = 2;
const int stepPin = 3;
const int trigPin = 4;
const int echoPin = 5;
const int pirPin = 6;
const int switchPin = 7;

//const int ledPin = 13;

// Variables para la distancia y el estado del PIR
long duration;
int Distance;
bool pirState = LOW;


void setup() {
  // Configuración de los pines
  pinMode(dirPin, OUTPUT);
  pinMode(stepPin, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(pirPin, INPUT);
  pinMode(switchPin, INPUT);
 // pinMode(ledPin, OUTPUT);
  
  // Inicialización de la comunicación serial
  Serial.begin(9600);
}

void loop() {
  // Verificar el estado del interruptor general
  if (digitalRead(switchPin) == HIGH) {
    // Mover el motor hasta que el sensor de distancia detecte el objetivo
    moveMotorToDistance(5); // Ajusta la distancia objetivo según tu necesidad
    
    // Esperar hasta que el sensor PIR se desactive
    while (digitalRead(pirPin) == HIGH) {

      //digitalWrite(ledPin, HIGH);
      delay(100);
    }
    //digitalWrite(ledPin, LOW);
    // Regresar el motor a la posición inicial
    moveMotorToDistance(100);
  }
}

int getDistance();
// Función para mover el motor a una distancia específica
void moveMotorToDistance(int targetDistance) {
  // Obtener la distancia actual del sensor
  int currentDistance = getDistance();
  
  // Determinar la dirección del movimiento
  if (currentDistance > targetDistance) {
    digitalWrite(dirPin, HIGH); // Mover hacia adelante
  } else {
    digitalWrite(dirPin, LOW); // Mover hacia atrás
  }
  
  // Mover el motor hasta alcanzar la distancia objetivo
  while (currentDistance != targetDistance) {
    digitalWrite(stepPin, HIGH);
    delayMicroseconds(1000); // Ajusta según la velocidad deseada
    digitalWrite(stepPin, LOW);
    delayMicroseconds(1000);
    
    // Actualizar la distancia actual
    currentDistance = getDistance();
  }
}
