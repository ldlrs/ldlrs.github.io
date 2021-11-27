# Código de programación
```Arduino
#include<Servo.h>

Servo servo1;
Servo servo2;

int boton=12;
int motor1=2;
int motor2=4;
int motored=13;
int molde1=3;
int molde2=5;
int echo1=6;
int trig1=9;
int echo2=10;
int trig2=11;
int estado, salida, estado2;
int tiempo1,distancia1;
int tiempo2,distancia2;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);

  pinMode(boton, INPUT_PULLUP);
  pinMode(echo1, INPUT);
  pinMode(echo2, INPUT);
  pinMode(trig1, OUTPUT);
  pinMode(trig2, OUTPUT);
  
  pinMode(motor1, OUTPUT);
  pinMode(motor2, OUTPUT);
  pinMode(motored, OUTPUT);
  pinMode(molde1, OUTPUT);
  pinMode(molde2, OUTPUT);
  

  servo1.attach(7);
  servo2.attach(8);
}

void loop() {
  
  // put your main code here, to run repeatedly:
  
//Se mandan los pulsos al sensor
  estado=digitalRead(boton);
  
  digitalWrite(trig2, LOW);
  delayMicroseconds(2);
  digitalWrite(trig2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig2, LOW);
  
  distancia2 = tiempo2 / 58;
  tiempo2 = pulseIn (echo2, HIGH);
  Serial.print("distancia2: ");
  Serial.println(distancia2);
  
  digitalWrite(trig1, LOW);
  delayMicroseconds(2);
  digitalWrite(trig1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig1, LOW);
  
  distancia1 = tiempo1 / 58;
  tiempo1 = pulseIn (echo1, HIGH);
  Serial.print("distancia1: ");
  Serial.println(distancia1);
  
  
  if((estado==1)&&(estado2==0))
  {
    salida=1-salida;
    
    delay(50);
  }
   estado2=estado;
  Serial.print("salida: ");
  Serial.println(salida);
   if(salida==0){
    digitalWrite(motor1,LOW);
    digitalWrite(motor2, LOW);
    digitalWrite(motored, LOW);
    digitalWrite(molde1, LOW);
    digitalWrite(molde2, LOW);
    servo1.write(0);
   
    servo2.write(0);
    
  }
  else{
    digitalWrite(motor1,HIGH);
    digitalWrite(motor2, LOW);
    digitalWrite(motored, LOW);
    digitalWrite(molde1, LOW);
    digitalWrite(molde2, LOW);
    servo1.write(0);
    
    servo2.write(0);
    

    while((distancia1<=6)&& (distancia2>=6)){
      digitalWrite(motor1, LOW);
      digitalWrite(motor2, HIGH);
      digitalWrite(motored, HIGH);
      digitalWrite(molde1, LOW);
      digitalWrite(molde2, LOW);
      salida=0;
      delay(5000);//se debe de accionar 5s después
      servo1.write(35);
      servo2.write(0);
      
      
      digitalWrite(trig2, LOW);
      delayMicroseconds(2);
      digitalWrite(trig2, HIGH);
      delayMicroseconds(10);
      digitalWrite(trig2, LOW);

      distancia2 = tiempo2 / 58;
      tiempo2 = pulseIn (echo2, HIGH);


      digitalWrite(trig1, LOW);
      delayMicroseconds(2);
      digitalWrite(trig1, HIGH);
      delayMicroseconds(10);
      digitalWrite(trig1, LOW);

      distancia1 = tiempo1 / 58;
      tiempo1 = pulseIn (echo1, HIGH);

      
    }
    while((distancia1<=6)&& (distancia2<=6))
    {
    for(int veces=1; veces<10; veces++)
    {
  	  digitalWrite(motor1, LOW);
      digitalWrite(motor2, LOW);
      digitalWrite(motored, LOW);
      digitalWrite(molde1, LOW);
      digitalWrite(molde2, LOW);
      salida=0;
      servo1.write(0);           
      servo2.write(35);
      delay(8000);
      
      digitalWrite(motor1, LOW);
      digitalWrite(motor2, LOW);
      digitalWrite(motored, LOW);
      digitalWrite(molde1, LOW);
      digitalWrite(molde2, HIGH);
      salida=0;
      servo1.write(0);
      servo2.write(0); 
      delay(5000);
      
      digitalWrite(motor1, LOW);
      digitalWrite(motor2, LOW);
      digitalWrite(motored, LOW);
      digitalWrite(molde1, HIGH);
      digitalWrite(molde2, LOW);
      salida=0;
      servo1.write(0);
      servo2.write(0);  
      Serial.print (" repeticiones: ");
      Serial.println(veces);
    }
      distancia2=10;
    }               
}
}   
```