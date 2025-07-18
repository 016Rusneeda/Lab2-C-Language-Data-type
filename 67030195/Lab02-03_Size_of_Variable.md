# ใบงานที่ 02.03 การเรียนรู้ชนิดข้อมูลในภาษา C บน ESP32 (Wokwi Simulation)

__วัตถุประสงค์__

1. ทำความเข้าใจและสามารถระบุชนิดข้อมูลพื้นฐานในภาษา C (เช่น int, float, char, bool, long, long long, unsigned int, byte, double)

2. เข้าใจแนวคิดเรื่องขอบเขตของข้อมูล (Data Range) สำหรับ ESP32 และผลกระทบจากการเกินขอบเขต (overflow/underflow)

3. เรียนรู้การใช้โอเปอเรเตอร์ sizeof() เพื่อตรวจสอบขนาดของชนิดข้อมูล

4. สามารถประกาศตัวแปรและกำหนดค่าเริ่มต้นให้กับตัวแปรได้อย่างถูกต้อง

### เกี่ยวกับ sizeof() โอเปอเรเตอร์

`sizeof()` เป็นโอเปอเรเตอร์ในภาษา C/C++ ที่ใช้สำหรับหาขนาดของชนิดข้อมูลหรือตัวแปรในหน่วยไบต์ (bytes) มันมีประโยชน์มากในการทำความเข้าใจว่าแต่ละชนิดข้อมูลใช้หน่วยความจำไปเท่าใด ซึ่งอาจแตกต่างกันไปในแต่ละแพลตฟอร์มหรือสถาปัตยกรรมของไมโครคอนโทรลเลอร์ เช่น ขนาดของ int บน Arduino Uno (8-bit) อาจไม่เท่ากับบน ESP32 (32-bit)

### รูปแบบการใช้งาน

sizeof(ชนิดข้อมูล) เช่น sizeof(int)

sizeof(ชื่อตัวแปร) เช่น 

``` c
int myVar; 
sizeof(myVar);
```

ผลลัพธ์ที่ได้จาก `sizeof()` จะเป็นจำนวนเต็มที่บอกถึงจำนวนไบต์ที่ข้อมูลนั้นใช้ในการจัดเก็บ

### 1. ทดลองกับ int (จำนวนเต็ม) และ sizeof():


__วัตถุประสงค์__ 

ทำความเข้าใจการเก็บค่าจำนวนเต็ม ขอบเขต และขนาดของ int บน ESP32

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`


``` cpp
Serial.println("--- ทดสอบชนิดข้อมูล int (ESP32) ---");
Serial.print("ขนาดของ int: ");
Serial.print(sizeof(int));
Serial.println(" bytes");

int myInteger = 100;
Serial.print("ค่าเริ่มต้นของ myInteger: ");
Serial.println(myInteger);

// ลองกำหนดค่าให้เกินขอบเขตสูงสุดของ int (ประมาณ 2,147,483,647 สำหรับ 32-bit int)
myInteger = 2147483647; // ค่าสูงสุด
Serial.print("myInteger = 2147483647 ผลลัพธ์: ");
Serial.println(myInteger);

myInteger = 2147483647 + 1; // ลองเกินค่าสูงสุด
Serial.print("myInteger = 2147483647 + 1 ผลลัพธ์: ");
Serial.println(myInteger); // สังเกตผลลัพธ์ที่อาจเกิด Overflow

// ลองกำหนดค่าให้ต่ำกว่าขอบเขตต่ำสุดของ int (ประมาณ -2,147,483,648 สำหรับ 32-bit int)
myInteger = -2147483648; // ค่าต่ำสุด
Serial.print("myInteger = -2147483648 ผลลัพธ์: ");
Serial.println(myInteger);

myInteger = -2147483648 - 1; // ลองต่ำกว่าค่าต่ำสุด
Serial.print("myInteger = -2147483648 - 1 ผลลัพธ์: ");
Serial.println(myInteger); // สังเกตผลลัพธ์ที่อาจเกิด Underflow
```

- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/744158cd-338a-4da6-8327-1d937949a450)

  
__คำถาม__ 

1.1  int บน ESP32 ใช้กี่ไบต์?

  ตอบ : ใช้ 4 ไบต์
  
1.2 จากผลลัพธ์ นักศึกษาสังเกตเห็นอะไรเมื่อค่าเกินขอบเขตของ int บน ESP32? 

  ตอบ : สังเกตเห็นว่า เมื่อค่าเพิ่มขึ้นเกินขีดจำกัดสูงสุด (2,147,483,647) ค่าจะ "วนกลับ" (wrap around) ไปเป็นค่าติดลบที่น้อยที่สุดของ int คือ -2,147,483,648 ซึ่งเป็นค่าต่ำสุดที่ int เก็บได้

และเมื่อค่าลดลงเกินขีดจำกัดต่ำสุด (-2,147,483,648) ค่าจะ "วนกลับ" (wrap around) ไปเป็นค่าบวกที่มากที่สุดของ int คือ 2,147,483,647 ซึ่งเป็นค่าสูงสุดที่ int เก็บได้ 


### 2. ทดลองกับ float และ double (ทศนิยม) และ sizeof()

__วัตถุประสงค์__ 

ทำความเข้าใจการเก็บค่าทศนิยม ความแม่นยำ และขนาดของ float และ double บน ESP32

__หมายเหตุ__

 บน ESP32, float คือ 4 ไบต์ (Single Precision) และ double คือ 8 ไบต์ (Double Precision) ซึ่งมีความแม่นยำสูงกว่า


- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`


``` c
Serial.println("\n--- ทดสอบชนิดข้อมูล float และ double (ESP32) ---");
Serial.print("ขนาดของ float: ");
Serial.print(sizeof(float));
Serial.println(" bytes");
Serial.print("ขนาดของ double: ");
Serial.print(sizeof(double));
Serial.println(" bytes");

float myFloat = 3.14159265; // ค่า Pi
Serial.print("ค่า myFloat (float): ");
Serial.println(myFloat, 8); // แสดงผลทศนิยม 8 ตำแหน่ง

double myDouble = 3.141592653589793; // ค่า Pi ที่แม่นยำกว่า
Serial.print("ค่า myDouble (double): ");
Serial.println(myDouble, 15); // แสดงผลทศนิยม 15 ตำแหน่ง
```

- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/8106e123-2d09-4773-a4d6-552aadfeed85)



__คำถาม__


- float และ double บน ESP32 ใช้กี่ไบต์ตามลำดับ?
  
  ตอบ : float ใช้ 4 ไบต์   double ใช้ 8 ไบต์

- นักศึกษาสังเกตเห็นความแตกต่างของความแม่นยำระหว่าง float และ double อย่างไร? สถานการณ์ใดที่คุณควรเลือกใช้ double แทน float?

  ตอบ : double ให้ความแม่นยำในการเก็บและแสดงผลค่าทศนิยมสูงกว่า float อย่างชัดเจน เนื่องจากใช้พื้นที่จัดเก็บถึง 8 ไบต์ เยอะกว่าสองเท่าของ float
        ควรเลือกใช้ double แทน float ในสถานการณ์ที่ ความแม่นยำของค่าทศนิยมมีความสำคัญอย่างยิ่งยวด และการปัดเศษเพียงเล็กน้อยอาจส่งผลกระทบอย่างมีนัยสำคัญต่อผลลัพธ์หรือการตัดสินใจ ตัวอย่างเช่น:

  1.การคำนวณทางวิทยาศาสตร์และวิศวกรรม: เช่น การจำลองฟิสิกส์, ดาราศาสตร์, การวิเคราะห์ข้อมูลทางวิทยาศาสตร์, การออกแบบทางวิศวกรรม ที่ต้องการความละเอียดสูงมาก เพื่อหลีกเลี่ยงข้อผิดพลาดสะสมจากการปัดเศษ
  
  2.การคำนวณทางการเงิน: เช่น การคำนวณดอกเบี้ย, การซื้อขายหุ้น, การจัดการบัญชี ที่แม้แต่ทศนิยมตำแหน่งสุดท้ายก็มีความสำคัญต่อมูลค่าทางการเงิน 

  3.กราฟิกคอมพิวเตอร์ขั้นสูง/CAD: สำหรับการคำนวณตำแหน่งวัตถุหรือเส้นทางที่ต้องการความแม่นยำสูงมาก เพื่อป้องกันข้อผิดพลาดในการแสดงผลที่อาจทำให้เกิดความคลาดเคลื่อนที่มองเห็นได้ หรือการออกแบบที่ต้องการความเที่ยงตรงสูง 

### 3. ทดลองกับ char (อักขระ) และ sizeof()

__วัตถุประสงค์__

- ทำความเข้าใจการเก็บอักขระ ขนาด และการแปลงเป็นค่า ASCII

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`

``` c
Serial.println("\n--- ทดสอบชนิดข้อมูล char ---");
Serial.print("ขนาดของ char: ");
Serial.print(sizeof(char));
Serial.println(" bytes");

char myChar = 'Z';
Serial.print("myChar = 'Z' ผลลัพธ์: ");
Serial.println(myChar);
Serial.print("myChar ในรูป ASCII: ");
Serial.println((int)myChar); // แปลง char เป็น int เพื่อดูค่า ASCII

myChar = 122; // 122 คือค่า ASCII ของ 'z'
Serial.print("myChar = 122 ผลลัพธ์: ");
Serial.println(myChar);
```

- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/e5205dc7-3ee6-42d3-bcbc-279be9a263d9)

__คำถาม__

- char ใช้กี่ไบต์? ค่าตัวเลข (ASCII value) มีความสัมพันธ์กับอักขระอย่างไร?

  ตอบ : char ใช้ 1 ไบต์ , ค่าตัวเลข (ASCII value) มีความสัมพันธ์โดยตรงกับอักขระ โดยแต่ละอักขระจะถูกกำหนดให้มีค่าตัวเลขเฉพาะตัว หรือที่เรียกว่ารหัส ASCII 

### 4. ทดลองกับ bool (ตรรกะ) และ sizeof():

__วัตถุประสงค์__

- ทำความเข้าใจการเก็บค่าความจริง (true หรือ false) และขนาด

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`

``` C++
Serial.println("\n--- ทดสอบชนิดข้อมูล bool ---");
Serial.print("ขนาดของ bool: ");
Serial.print(sizeof(bool));
Serial.println(" bytes");

bool myBool = true;
Serial.print("myBool = true ผลลัพธ์: ");
Serial.println(myBool); // true จะถูกแสดงเป็น 1

myBool = false;
Serial.print("myBool = false ผลลัพธ์: ");
Serial.println(myBool); // false จะถูกแสดงเป็น 0
```

- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/f476d3ad-acfb-44af-90a2-71e002739c35)


__คำถาม__

- bool ใช้กี่ไบต์? true และ false ถูกแสดงผลเป็นค่าใดบน Serial Monitor?

  ตอบ : bool ใช้ 1 ไบต์ , true ถูกแสดงผลเป็นค่า 1 และ false ถูกแสดงผลเป็นค่า 0

### 5. ทดลองกับ long, long long, unsigned int, unsigned long, unsigned long long (จำนวนเต็มขนาดใหญ่/ไม่มีเครื่องหมาย) และ sizeof():

__วัตถุประสงค์__ ทำความเข้าใจการใช้ชนิดข้อมูลสำหรับจำนวนเต็มที่มีขนาดใหญ่ขึ้นและแบบไม่มีเครื่องหมายบน ESP32 รวมถึงขนาด

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`

``` C++
Serial.println("\n--- ทดสอบชนิดข้อมูล long, long long, unsigned (ESP32) ---");
Serial.print("ขนาดของ long: ");
Serial.print(sizeof(long));
Serial.println(" bytes");
Serial.print("ขนาดของ long long: ");
Serial.print(sizeof(long long));
Serial.println(" bytes");
Serial.print("ขนาดของ unsigned int: ");
Serial.print(sizeof(unsigned int));
Serial.println(" bytes");
Serial.print("ขนาดของ unsigned long: ");
Serial.print(sizeof(unsigned long));
Serial.println(" bytes");
Serial.print("ขนาดของ unsigned long long: ");
Serial.print(sizeof(unsigned long long));
Serial.println(" bytes");

long myLong = 4000000000L; // long บน ESP32 คือ 32-bit (เท่า int)
Serial.print("myLong (4,000,000,000): ");
Serial.println(myLong); // จะเกิด overflow เพราะเกิน 2.1 พันล้าน

long long myLongLong = 9000000000000000000LL; // long long คือ 64-bit
Serial.print("myLongLong (9,000,000,000,000,000,000): ");
Serial.println(myLongLong);

unsigned int myUnsignedInt = 4000000000U; // unsigned int คือ 32-bit
Serial.print("myUnsignedInt (4,000,000,000): ");
Serial.println(myUnsignedInt);

unsigned long myUnsignedLong = 4000000000UL; // unsigned long คือ 32-bit
Serial.print("myUnsignedLong (4,000,000,000): ");
Serial.println(myUnsignedLong);

unsigned long long myUnsignedLongLong = 18000000000000000000ULL; // unsigned long long คือ 64-bit
Serial.print("myUnsignedLongLong (1.8e19): ");
Serial.println(myUnsignedLongLong);
```

- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/ad8e7f29-9c30-41e5-9b5b-2273fc96decf)


__คำถาม__

- ชนิดข้อมูลจำนวนเต็มแต่ละชนิด (long, long long, unsigned int, unsigned long, unsigned long long) ใช้กี่ไบต์บน ESP32?

  ตอบ : long ใช้ 4 ไบต์, long long ใช้ 8 ไบต์, unsigned int ใช้ 4 ไบต์, unsigned long ใช้ 4 ไบต์, unsigned long long ใช้ 8 ไบต์

- บน ESP32, long มีขอบเขตเท่ากับ int หรือไม่? ชนิดข้อมูลใดที่คุณจะใช้หากต้องการเก็บค่าจำนวนเต็มบวกที่ใหญ่ที่สุด?

  ตอบ : long มีขอบเขตเท่ากับ int นั่นคือ 32 บิต หรือ 4 ไบต์ ,
        unsigned long long จะเป็นชนิดข้อมูลที่จะใช้หากต้องการเก็บค่าจำนวนเต็มบวกที่ใหญ่ที่สุด

### 6. ทดลองกับ byte (ข้อมูล 8 บิต) และ sizeof():

__วัตถุประสงค์__

ทำความเข้าใจการเก็บค่าจำนวนเต็มขนาดเล็ก (0-255) และขนาด

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`


``` C++
Serial.println("\n--- ทดสอบชนิดข้อมูล byte ---");
Serial.print("ขนาดของ byte: ");
Serial.print(sizeof(byte));
Serial.println(" bytes");

byte myByte = 250;
Serial.print("myByte = 250 ผลลัพธ์: ");
Serial.println(myByte);

myByte = 256; // ค่าเกินขอบเขต (0-255)
Serial.print("myByte = 256 ผลลัพธ์: ");
Serial.println(myByte); // สังเกตผลลัพธ์
```
- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/2bebc609-8829-43fb-99fc-e7a57dd9b40c)


__คำถาม__

- byte ใช้กี่ไบต์? เมื่อ myByte ถูกกำหนดให้เป็น 256 ผลลัพธ์ที่ได้คืออะไร และเพราะเหตุใด?
  
  ตอบ : byte ใช้ 1 ไบต์ , เมื่อ myByte ถูกกำหนดให้เป็น 256 ผลลัพธ์ที่ได้คือ 0 เพราะ byte มีค่าตั้งแต่ 0-255 ซึ่งเมื่อกำหนดให้เป็น 256 ที่เกินค่าสูงสุดมาแล้ว จะเกิดการ Overflow และจะวนกลับมายังค่าที่น้อยที่สุดนั้นคือ 0

## ส่วนที่ 2: การคำนวณและแปลงชนิดข้อมูลเบื้องต้น

### 7. การคำนวณด้วยชนิดข้อมูลต่างกัน (Type Promotion)

__วัตถุประสงค์__

- ทำความเข้าใจว่าการคำนวณระหว่างชนิดข้อมูลต่างกันมีผลลัพธ์อย่างไร

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`

``` C++
Serial.println("\n--- ทดสอบการคำนวณระหว่างชนิดข้อมูล ---");
int numA = 10;
float numB = 3.0;
double numC = 3.0;

float resultFloat = numA / numB; // int หาร float ผลลัพธ์จะเป็น float
Serial.print("10 / 3.0 (ผลลัพธ์ float): ");
Serial.println(resultFloat, 2);

double resultDouble = numA / numC; // int หาร double ผลลัพธ์จะเป็น double
Serial.print("10 / 3.0 (ผลลัพธ์ double): ");
Serial.println(resultDouble, 10);

int resultInt = numA / (int)numB; // int หาร int ผลลัพธ์จะเป็น int (ทศนิยมถูกตัดทิ้ง)
Serial.print("10 / (int)3.0 (ผลลัพธ์ int): ");
Serial.println(resultInt);
```

- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/fe2b8c5f-51dc-4e2b-bba2-3f31c6e9a705)


__คำถาม__ 

ทำไม 10 / 3.0 (เมื่อตัวหารเป็น float หรือ double) ถึงได้ผลลัพธ์เป็นทศนิยม แต่เมื่อตัวหารถูกแปลงเป็น int แล้วผลลัพธ์เป็นจำนวนเต็ม?

ตอบ : เมื่อ 10 / 3.0 (เมื่อตัวหารเป็น float หรือ double) ได้ผลลัพธ์เป็นทศนิยม เพราะ 10 เป็น int ถูกหารด้วยตัวหารที่เป็น float (3.0f) หรือ double (3.0) ระบบจะทำการแปลงตัวถูกหารให้เป็นชนิดข้อมูลเดียวกันกับตัวหาร (implicit type conversion) ก่อนที่จะทำการหาร เนื่องจาก float และ double เป็นชนิดข้อมูลที่ใช้เก็บเลขทศนิยม การดำเนินการหารจึงเป็นการหารแบบทศนิยม ผลลัพธ์ที่ได้จึงเป็นค่าทศนิยมที่แม่นยำตามชนิดข้อมูลนั้นๆ เมื่อตัวหารถูกแปลงเป็น int ผลลัพธ์เป็นจำนวนเต็ม เพราะ มื่อตัวหาร 3.0 ถูกแปลงให้เป็น int (โดยใช้ (int)3.0) ค่า 3.0 จะถูก ตัดทศนิยมทิ้ง เหลือเพียง 3 ที่เป็นจำนวนเต็ม ผลลัพธ์เลยเป็นจำนวนเต็ม

### 8. การแปลงชนิดข้อมูล (Explicit Type Casting):

__วัตถุประสงค์__ 

-ฝึกการแปลงชนิดข้อมูลด้วยตัวเอง

- โค้ดที่ต้องเพิ่มใน `setup()` โดยลบโค้ดเดิมออก ยกเว้นบรรทัด `Serial.begin(115200);`

``` C++
Serial.println("\n--- ทดสอบการแปลงชนิดข้อมูล (Type Casting) ---");
float temperatureCelsius = 25.789;
Serial.print("อุณหภูมิ Celsius (float): ");
Serial.println(temperatureCelsius);

int temperatureInteger = (int)temperatureCelsius; // แปลง float เป็น int
Serial.print("อุณหภูมิ Celsius (แปลงเป็น int): ");
Serial.println(temperatureInteger);

// ตัวอย่างการใช้ char เป็นตัวเลข
char letter = 'P';
int asciiValue = (int)letter;
Serial.print("อักขระ 'P' มีค่า ASCII: ");
Serial.println(asciiValue);

// ตัวอย่างการแปลง int เป็น float
int count = 7;
int total = 20;
float average = (float)count / total; // ต้องแปลงอย่างน้อยหนึ่งตัวให้เป็น float ก่อนหาร
Serial.print("ค่าเฉลี่ย (7/20): ");
Serial.println(average);
```
- ดำเนินการจำลองตามวิธีที่ได้ทดลองมาแล้ว
- ผลลัพธ์ที่ได้
  ![image](https://github.com/user-attachments/assets/9bb1cfd4-b9eb-46e9-a76c-aaaedba00566)


__คำถาม__ 

นักศึกษาจะใช้การทำ Type Casting ในสถานการณ์ใดบ้างในการเขียนโปรแกรม?


    ตอบ : 1. การแปลงชนิดข้อมูลในการคำนวณ 
           2. การทำงานกับฟังก์ชัน/เมธอดที่ต้องการชนิดข้อมูลเฉพาะ 
           3. การลดความแม่นยำหรือขนาดข้อมูล (Downcasting) 
           4. การจัดการกับพอยน์เตอร์ (Pointers) และหน่วยความจำ
           5. การทำงานกับ Generic Programming หรือ Object-Oriented Programming (OOP)

