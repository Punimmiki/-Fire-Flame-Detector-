# Physical Computing เครื่องตรวจจับเปลวไฟ แจ้งเตือนผ่าน Line Notification
####   Project นี้เป็นส่วนหนึ่งในรายวิชา PHYSICAL COMPUTING 06016409 ภาคเรียนที่ 1 ปีการศึกษา 2567 ของคณะเทคโนโลยีสารสนเทศ สาขาเทคโนโลยีสารสนเทศ สถาบันเทคโนโลยีพระจอมเกล้าเจ้าคุณทหารลาดกระบัง

# บทคัดย่อ
  โปรเจคแจ้งเตือนไฟไหม้ผ่าน Line Notify ด้วย NodeMCU ESP8266 และเซ็นเซอร์ตรวจจับควันและเปลวไฟนี้ มีวัตถุประสงค์เพื่อสร้างระบบแจ้งเตือนความปลอดภัยภายในบ้านที่ใช้งานง่ายและประหยัดพลังงาน โครงการนี้ออกแบบมาเพื่อให้นักเรียนและผู้ที่เริ่มต้นสามารถเรียนรู้พื้นฐานการเขียนโปรแกรมและการเชื่อมต่ออุปกรณ์ IoT ได้ โดย NodeMCU จะตรวจจับเปลวไฟหรือควันผ่านเซ็นเซอร์และแจ้งเตือนไปยัง Line ทันทีเมื่อพบอันตราย เพิ่มความสะดวกสบายและความปลอดภัยภายในบ้าน โดยจะมีอุปกรณ์ที่ใช้งานใน Project ดังกล่าวทั้งหมดดังนี้

# อุปกรณ์ที่ใช้งาน
* Nodemcu V.2
>![image](https://inex.co.th/home/wp-content/uploads/2020/07/node-mcuv2-001.jpg)

* Sensor Infrared IR Flame Detector
>![image](https://o.lnwfile.com/_/o/_raw/8w/un/73.jpg)

# VDO สาธิตการใช้งานโปรเจค
[Video](https://www.youtube.com/watch?v=8GmCTsqaVTw)


# POSTER
![Poster เครื่องตรวจจับเปลวไฟ](https://github.com/user-attachments/assets/21fd0d9a-5527-4cb2-b8b2-a79cddf74e95)
[Poster](https://github.com/Punimmiki/-/blob/main/Poster/Poster%20%E0%B9%80%E0%B8%84%E0%B8%A3%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%87%E0%B8%95%E0%B8%A3%E0%B8%A7%E0%B8%88%E0%B8%88%E0%B8%B1%E0%B8%9A%E0%B9%80%E0%B8%9B%E0%B8%A5%E0%B8%A7%E0%B9%84%E0%B8%9F%20pdf.pdf)



# Library ที่ใช้งาน
```cpp
#include <TridentTD_LineNotify.h>  // สำหรับการแจ้งเตือนผ่าน Line Notify
#include <ESP8266WiFi.h>           // สำหรับการเชื่อมต่อ WiFi ของ ESP8266
```

# การตั้งค่าตัวแปรและ Pin
```cpp
#define SSID        "ชื่อ WiFi ของคุณ"      // ชื่อเครือข่าย WiFi
#define PASSWORD    "รหัสผ่าน WiFi"         // รหัสผ่าน WiFi
#define LINE_TOKEN  "โทเค็นของ Line Notify"  // Token สำหรับการแจ้งเตือนผ่าน Line

const int SensorPin = D2;  // กำหนดพินเชื่อมต่อกับเซ็นเซอร์
```

# ฟังก์ชันที่สำคัญ
#### void setup()
* เริ่มต้นการทำงาน เช่น เชื่อมต่อ WiFi และตั้งค่าเซ็นเซอร์พร้อมส่งการแจ้งเตือนเมื่อเชื่อมต่อสำเร็จ.
```cpp
void setup() {
    Serial.begin(115200); // เริ่มต้น Serial Monitor เพื่อดูข้อมูลจาก Arduino
    pinMode(SensorPin, INPUT); // กำหนดพินเซ็นเซอร์เป็น INPUT เพื่ออ่านค่าจากเซ็นเซอร์
    WiFi.begin(SSID, PASSWORD); // เริ่มต้นการเชื่อมต่อ WiFi ด้วย SSID และ PASSWORD ที่กำหนด

    while (WiFi.status() != WL_CONNECTED) { // รอให้เชื่อมต่อ WiFi จนกว่าจะเชื่อมต่อสำเร็จ
        delay(1000); // รอ 1 วินาทีในแต่ละรอบ
    }
    LINE.setToken(LINE_TOKEN); // ตั้งค่า Line Token สำหรับการแจ้งเตือน
    LINE.notify("เชื่อมต่อสำเร็จ"); // ส่งข้อความแจ้งเตือนเมื่อเชื่อมต่อ WiFi สำเร็จ
}
```
#### void loop()
* ตรวจสอบสถานะเซ็นเซอร์และส่งการแจ้งเตือนผ่าน Line Notify เมื่อมีการตรวจจับเปลวไฟ.
```cpp
void loop() {
    if (digitalRead(SensorPin) == HIGH) { // ตรวจสอบสถานะเซ็นเซอร์ หากเซ็นเซอร์ตรวจจับเปลวไฟ
        LINE.notify("ไฟไหม้บ้านแล้ว"); // ส่งการแจ้งเตือนเมื่อมีการตรวจจับ
        delay(5000); // รอ 5 วินาทีก่อนตรวจสอบสถานะเซ็นเซอร์อีกครั้ง
    }
}
```
# สมาชิกผู้จัดทำ
| ชื่อ - นามสกุล                | รหัสนักศึกษา |
|-----------------------------|-------------|
| นางสาวนุ่มนวล มาไข่        | 64070172    |
| นายอภิวิชญ์ ร่มโพธิ์ใหญ่     | 64070251    |
| นายพรรษา ร่องยืด           | 64070071    |
