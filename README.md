# SOIL-MOISTURE-AUTOMATIC-IRRIGATION-SYSTEM
Final year college project

** code **

`#include <SoftwareSerial.h>`

`#include <LiquidCrystal_I2C.h>`



`SoftwareSerial mySerial(3, 2);`

`LiquidCrystal_I2C lcd(0x27 ,16 , 2);`



`int soilMoistureValue = 0;`

`int percentage=0;`

`String Data_SMS;`

`String DATA_SMS;`

`int PUMP_STATUS;`

`char Received_SMS;`

`String Latest;`



`short SOIL_OK=-1,PUMP_OK=-1;`



`void setup()`

`{ `



`lcd.begin();`

`lcd.backlight();`

`lcd.setCursor(2, 0);`

`lcd.print("Started");`

`delay(800);`

`pinMode(4,OUTPUT);`

`Serial.begin(9600);`

`mySerial.begin(9600);`

`delay(1000);`

`ReceiveMode();`



`delay(500);`



`mySerial.println("AT"); //Once the handshake test is successful, it will back to OK`

`updateSerial();`

`mySerial.println("AT+CMGF=1"); // Configuring TEXT mode`

`updateSerial();`

`mySerial.println("AT+CMGS=\"+918693087527\"");//change ZZ with country code and xxxxxxxxxxx with phone number to sms`

`updateSerial();`

`mySerial.print("System Started"); //text content`

`updateSerial();`

`mySerial.write(26);`

`lcd.clear();`

`}`

`void loop()`

`{`



`String RSMS;`


`while(mySerial.available()>0){`

`Received_SMS=mySerial.read();`

`Serial.print(Received_SMS);`

`RSMS.concat(Received_SMS);`



`SOIL_OK=RSMS.indexOf("SOIL");`

`PUMP_OK=RSMS.indexOf("PUMP");`

`}`



`soilMoistureValue = analogRead(A1);`

`Serial.println(percentage);`

`percentage = map(soilMoistureValue, 450, 1023, 100, 0);`

`lcd.clear();`

`lcd.setCursor(0,0);`

`lcd.print("Percentage:");`

`lcd.setCursor (12, 0);`

`lcd.print(percentage);`

`delay(500);`



`if(SOIL_OK!=-1){`

`Latest = "LIVE = "+String(percentage)+"%";`



`Send_Data();`

`ReceiveMode();`

`SOIL_OK=-1;`

`PUMP_OK=-1;`

`}`



`if(PUMP_OK!=-1){`



`if(PUMP_STATUS == 1) {`

`Latest = " LIVE = PUMP TURNED ON ";`



`Send_Data();`

`ReceiveMode();`

`SOIL_OK=-1;`

`PUMP_OK=-1;`



`}`



`else {`

`Latest = " LIVE = PUMP TURNED OFF ";`



`Send_Data();`

`ReceiveMode();`

`SOIL_OK=-1;`

`PUMP_OK=-1;`

`}`

`}`

`if(percentage < 10)`

`{`

`lcd.setCursor(0,1);`

`lcd.print("PUMP TURNED ON");`

`delay (3000);`

`Pump_ON();`

`PUMP_STATUS = 1;`

`delay(500);`

`digitalWrite(4 ,LOW);`


`lcd.clear();`

`}`

`if(percentage >80)`

`{`

`lcd.setCursor(0,1);`

`lcd.print("PUMP TURNED OFF");`

`delay (500);`

`PUMP_STATUS = 0 ;`

`delay(100);`

`digitalWrite(4 ,HIGH);`

`}`

`}`

`void updateSerial()`

`{`

`delay(500);`

`while (Serial.available())`

`{`

`mySerial.write(Serial.read());//Forward what Serial received to Software Serial Port`

`}`

`while(mySerial.available())`

`{`

`Serial.write(mySerial.read());//Forward what Software Serial received to Serial Port`

`}`

`}`



`void ReceiveMode(){`



`mySerial.println("AT");`

`updateSerial();`

`mySerial.println("AT+CMGF=1");`

`updateSerial();`

`mySerial.println("AT+CNMI=2,2,0,0,0");`

`updateSerial();`

`}`



`void Pump_ON()`

`{`

`mySerial.println("AT"); //Once the handshake test is successful, it will back to OK`

`updateSerial();`

`mySerial.println("AT+CMGF=1"); // Configuring TEXT mode`

`updateSerial();`

`mySerial.println("AT+CMGS=\"+918693087527\"");//change ZZ with country code and xxxxxxxxxxx with phone number to sms`

`updateSerial();`

`delay(500);`

`Data_SMS = "Soil Mositure Percentage = "+String(percentage)+"%" + "\n Pump turned ON ";`

`mySerial.print(Data_SMS); //text content`

`updateSerial();`

`mySerial.write(26);`

`}`



`void Send_Data()`

`{`

`mySerial.println("AT"); //Once the handshake test is successful, it will back to OK`

`updateSerial();`

`mySerial.println("AT+CMGF=1"); // Configuring TEXT mode`

`updateSerial();`

`mySerial.println("AT+CMGS=\"+918693087527\"");//change ZZ with country code and xxxxxxxxxxx with phone number to sms`

`updateSerial();`

`delay(500);`

`mySerial.print(Latest); //text content`

`updateSerial();`

`mySerial.write(26);`

`}`


