
<br><div id="header" align="center">
  <img src="https://media.giphy.com/media/M9gbBd9nbDrOTu1Mqx/giphy.gif" width="100"/>
</div><br>

<div id="badges" align="center">
  <a href="https://www.facebook.com/porawapat.mutarapat">
    <img src="https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white" alt="Facebook Badge"/>
  </a>
</div><br>

<div align="center">
  <h1>Hi there 👋</h1>
</div>

<div align="center">
  <img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbGgxaGs0bHNrYzZtbzJwaGh2b3M1ODlhMXl2ZGZ0OTg5aDB0Mmk3ZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/SWoSkN6DxTszqIKEqv/giphy.gif" width="500" height="375"/>
</div><br>

About Me:









/*************|
 * จอแสดงผล   |
 *************/

    #include <SPI.h>
    #include <Adafruit_GFX.h>
    #include <Adafruit_SSD1306.h>
    
    #define SCREEN_WIDTH 128 // OLED display width, in pixels
    #define SCREEN_HEIGHT 64 // OLED display height, in pixels
    
    // Declaration for SSD1306 display connected using software SPI (default case):
    #define OLED_MOSI   9
    #define OLED_CLK   10
    #define OLED_DC    11
    #define OLED_CS    12
    #define OLED_RESET 13
    Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
                             OLED_MOSI, OLED_CLK, OLED_DC, OLED_RESET, OLED_CS);

/*************|
 * QR-Code    |
 *************/

    #include <qrcode.h>
    QRCode qrcode;


    
void setup() {
    
    /*************|
     * ตั้งค่า Serial|
     *************/
     
        Serial.begin(115200);
    
    /***************|
     * เริ่มต้น Display|
     ***************/
     
        // SSD1306_SWITCHCAPVCC = generate display voltage from 3.3V internally
        if (!display.begin(SSD1306_SWITCHCAPVCC)) {
          Serial.println(F("SSD1306 allocation failed"));
          for (;;); // Don't proceed, loop forever
        }
    
    /***************|
     * สร้าง QR-code |
     **************/
     
        uint8_t qrcodeBytes[qrcode_getBufferSize(1)];
        qrcode_initText(&qrcode, qrcodeBytes, 1, ECC_LOW, "arduinona.com");
    
    
    /************************************|
     * Print QR-code ออก Serial monitor  |
     ************************************/
       
        for (uint8_t y = 0; y < qrcode.size; y++) {
          for (uint8_t x = 0; x < qrcode.size; x++) {
            if (qrcode_getModule(&qrcode, x, y)) {
              Serial.print("*");
            } else {
              Serial.print(" ");
            }
          }
          Serial.print("\n");
        }
    
    
    /************************************|
     * Print QR-code ออก Display         |
     ************************************/
      
        display.clearDisplay();
        /*
         * QR-code ต้องมีพื้นที่สีสว่างกว่าตัว block ของ code เลยต้องถม background 
         * ส่วนที่จะแสดง QR-code ให้เป็นสีขาว (หน้าจอจะออกเป็นสีตามเม็ดสีบน oled ซึ่งคือสีฟ้า)
         */
        display.fillRect(37,16,46,46, WHITE);
        for (uint8_t y = 0; y < qrcode.size; y++) {
          for (uint8_t x = 0; x < qrcode.size; x++) {
            if (qrcode_getModule(&qrcode, x, y)) {
              /*
               * วาด Rectangle ขนาด 2x2 ในแต่ละตำแหน่งของ qrcode บนหน้าจอ 
                โดยวางมุมซ้ายบนสุดของ QR-Code ไว้ที่พิกัด (39, 18)
                */
              display.fillRect(x*2 + 39, y*2 + 18, 2, 2, BLACK);
            }
          }
          Serial.print("\n");
        }
        display.setTextSize(1);             // Normal 1:1 pixel scale
        display.setTextColor(WHITE);        // Draw white text
        display.setCursor(10,0);             // Start at top-left corner
        display.println("www.ArduinoNa.com");
        display.display(); // Update screen with each newly-drawn rectangle

}

void loop() {

}


