//Añadimos la libreria del servo
#include <Servo.h>
//Crear una variable de tipo servo para el motor
Servo motor;

//Variable para controlar la posición del motor
int posicion = 0; 

void setup() {
  Serial.begin(9600);
  // Indicar en que pin analógico está conectado el motor
  motor.attach(9);  
}

void loop() {
  motor.write(0);   // Levar al motor a la posición 0 grados
  delay(2000);
  motor.write(90);  // Levar al motor a la posición 90 grados (agarre de comida)
  delay(1000);

  //Mover el motor desde 90 grados hasta 60 grados (Inclinación de la cuchara para que el usuario se acerque)
  for (posicion=90; posicion>=60; posicion--) { 
    motor.write(posicion); 
    delay(100);  //Mover un grado cada 100 milisegundos                     
  }
  //Delay para que el usuario se acerque y recepcione el alimento
  delay(4000);
  //Mover el motor desde 60 grados hasta 0 grados
  for (posicion=60; posicion>=0; posicion--) { 
    motor.write(posicion);              
    delay(50);  //Mover un grado cada 50 milisegundos                                            
  }
  //Delay mientras el servo vuelve a su estado inicial
  delay(4000);
 }
