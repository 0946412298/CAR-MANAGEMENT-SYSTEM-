# CAR-MANAGEMENT-SYSTEM-
#define Input  8 // Nguyên gửi giá trị
#define Sw_Up  3 // Hành trình ở trên mức 1
#define Sw_Down  2 // Hành trình ở dưới mức 1

#define Lift_Up 9 // Động cơ ở trên
#define Lift_Down  10 // Động cơ phía dưới

#define Sensor   7 // Cảm biến tiện cận, dây dên tín hiêu, xanh GND, hồng Vcc
int flag = 0; // Cơ ngắt khi sensor = 0

void setup() {
  Serial.begin(9600);

  pinMode (Sensor, INPUT);

  pinMode (Sw_Up, INPUT);
  pinMode (Sw_Down, INPUT);

  pinMode (Lift_Up, OUTPUT);
  pinMode (Lift_Down, OUTPUT);

  pinMode(Input, INPUT_PULLUP);

  analogWrite(Lift_Up, 0);
  analogWrite(Lift_Down, 0);
}

void loop() {
    if(Serial.available())
  {
//    switch(Serial.read())
//    {
//      case '0': digitalWrite(8,LOW);
//                break;
//      case '1': digitalWrite(8,HIGH);
//                break;
//      default: break;
//      }
while(Serial.read(
    }
  int Value_Sensor = digitalRead(Sensor);
  int Value_Sw_Up = digitalRead(Sw_Up);
  int Value_Sw_Down = digitalRead(Sw_Down);
  int Value_Input = digitalRead(Input);

  if ((Value_Sw_Down == 0) && (Value_Input == 0)) {
      analogWrite(Lift_Up, 40);
    Serial.println("mở cửa");
    analogWrite(Lift_Down, 0);
    //Value_Input = 1;
  }
  Serial.print("Value_Sw_Down:");
  Serial.println(Value_Sw_Down);
 
  if ((Value_Sw_Down == 1) && (Value_Input == 0)) {
    analogWrite(Lift_Up, 40);
  }

  if (Value_Sw_Up == 0)
  {
    analogWrite(Lift_Up, 0);
    analogWrite(Lift_Down, 0);
    Serial.println("Dừng chờ xe đi qua");
    if (Value_Sensor == 0) {
      //    Serial.print("Gia tri cam bien:");
      //    Serial.println(Value_Sensor);//Nếu sensor = 1 thì không phát hiện vật cản, nếu sensor = 0 thì phát hiện vật cản
      flag = 1;
      analogWrite(Lift_Up, 0);
      analogWrite(Lift_Down, 50);
    }
  }



  if ((Value_Sensor == 1) && (flag == 1)) {
    analogWrite(Lift_Up, 0);
    analogWrite(Lift_Down, 50);
    Serial.println("Đóng cửa");
    analogWrite(Lift_Down, 40);
    flag = 0;
  }

  if (Value_Sw_Down == 0) {
    analogWrite(Lift_Up, 0);
    analogWrite(Lift_Down, 0);
    Serial.println("Chờ xe tiếp theo");
  }
}
