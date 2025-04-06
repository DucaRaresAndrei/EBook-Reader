**Duca Andrei-Rares - 331CA**

# OpenBook - E-Book Reader

## Diagrama bloc

## Descrierea functionalitatilor hardware

Proiectul OpenBook Reader se bazeaza pe un microcontroller ESP32-C6 care gestioneaza comunicatia si controlul mai multor componente si senzori. Sistemul este alimentat fie printr-un port USB-C, fie printr-o baterie Li-Po reincarcabila, gestionata de un modul de incarcare MCP73831. Tensiunea de 3.3V necesara majoritatii componentelor este furnizata de un regulator LDO.

### Module si componente principale:

- **ESP32-C6 (modul principal)**  
  Controleaza comunicatia cu toate modulele periferice prin protocoale SPI, I2C, UART si GPIO.

- **Modul de incarcare MCP73831 + baterie Li-Po**  
  Incarca acumulatorul si permite alimentarea portabila a dispozitivului.

- **LDO Voltage Regulator (XC6220A331MR-G)**  
  Stabilizeaza tensiunea la 3.3V pentru alimentarea componentelor sensibile.

- **E-Ink Display 1.5" (EPD)**  
  Afiseaza continutul static intr-un mod eficient energetic. Este controlat prin interfata SPI.

- **SD Card**  
  Utilizat pentru stocarea fisierelor, accesat prin SPI.

- **External NOR Flash 64MB (W25Q512JVEIQ)**  
  Memorie externa conectata prin SPI pentru stocare suplimentara.

- **RTC Module DS3231SN**  
  Asigura un ceas de timp real precis, comunicand prin I2C.

- **Sensor BME688**  
  Senzor ambiental (temperatura, umiditate, presiune, calitate aer) conectat prin I2C.

- **Butoane fizice (RESET, BOOT, CHANGE)**  
  Conectate direct la GPIO pentru interactiune manuala si control.

### Specificatii si interfete utilizate:

| Componenta             | Interfata       | Tensiune  | Detalii                                    |
|------------------------|-----------------|-----------|---------------------------------------------|
| E-Ink Display          | SPI             | 3.3V      | 1.5", 200x200px                             |
| SD Card                | SPI             | 3.3V      | Card microSD pentru stocare externa         |
| External NOR Flash     | SPI             | 3.3V      | Memorie Flash 64MB                          |
| RTC DS3231SN           | I2C             | 3.3V      | Ceas cu cristal integrat                    |
| Senzor BME688          | I2C             | 3.3V      | Temperatura, presiune, gaz, umiditate       |
| Butoane                | GPIO            | -         | RESET, BOOT, CHANGE                         |

### Aspecte legate de consumul de energie:

E-Ink-ul si RTC-ul sunt componente cu consum foarte redus, ceea ce permite un timp indelungat de utilizare pe baterie. Comutarea modulelor periferice se face prin pinii GPIO pentru a permite dezactivarea lor in modul deep sleep. Consumul estimat in idle este de aproximativ 0.5 mA, iar in activitate de pana la 150 mA (in functie de sarcina EPD si activitatea ESP32).


## Pini ESP32-C6

| Pin ESP32-C6 | Functie            | Componenta          | Justificare                                                                 |
|--------------|--------------------|----------------------|------------------------------------------------------------------------------|
| EN           | RESET              | Buton Reset          | Reset hardware general pentru microcontroller                               |
| IO0          | INT RTC            | RTC DS3231SN         | Semnal de wake-up de la RTC                                                 |
| IO1          | 32KHZ              | RTC DS3231SN         | Iesire frecventa 32kHz din RTC                                               |
| IO2          | MISO               | SPI (Flash, SD)      | Linie de date SPI pentru citire (shared)                                    |
| IO3          | EPD_BUSY           | E-Ink Display         | Citire stare de ocupare a display-ului                                      |
| IO4          | SS_SD              | SD Card              | Chip Select pentru cardul microSD                                           |
| IO5          | EPD_DC             | E-Ink Display         | Selectie Comanda/Data pentru e-paper                                        |
| IO6          | SCK                | SPI Bus              | Clock SPI comun pentru SD, Flash, EPD                                       |
| IO7          | MOSI               | SPI Bus              | Linie de date SPI pentru scriere (shared)                                   |
| IO8          | IO/BOOT            | Buton Boot           | Intrare in modul programare (Flash Mode)                                    |
| IO9          | -                  | -                    | Pin neutilizat                                                              |
| IO10         | -                  | -                    | Pin neutilizat                                                              |
| IO11         | FLASH_CS           | NOR Flash            | Chip Select dedicat pentru memoria externa                                  |
| IO12         | USB D-             | USB-C                | Linie USB pentru comunicare si alimentare                                   |
| IO13         | USB D+             | USB-C                | Linie USB pentru comunicare si alimentare                                   |
| IO15         | IO/CHANGE          | Buton CHANGE         | Buton pentru schimbarea modului de afisare sau functionalitate              |
| IO16         | TX (UART0)         | Debug Serial         | Transmitere date catre monitor serial                                       |
| IO17         | RX (UART0)         | Debug Serial         | Receptie date de la monitor serial                                          |
| IO18         | RTC_RST            | RTC DS3231SN         | Reset software pentru RTC                                                   |
| IO19         | I2C_PW             | BME688               | Control alimentare senzor prin tranzistor                                   |
| IO20         | EPD_3V3_C          | E-Ink Display         | Activare alimentare 3.3V pentru e-paper                                     |
| IO21         | SDA                | I2C (RTC, BME688)    | Linia de date I2C partajata                                                 |
| IO22         | SCL                | I2C (RTC, BME688)    | Linia de ceas I2C partajata                                                 |
| IO23         | EPD_RST            | E-Ink Display         | Reset hardware pentru display                                               |

