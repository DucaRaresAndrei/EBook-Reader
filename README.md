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

### Aspecte legate de consumul de energie

E-Ink-ul si RTC-ul sunt componente cu consum foarte redus, ceea ce permite un timp indelungat de utilizare pe baterie. Comutarea modulelor periferice se face prin pinii GPIO pentru a permite dezactivarea lor in modul deep sleep. Consumul estimat in idle este de aproximativ 0.5 mA, iar in activitate de pana la 150 mA (in functie de sarcina EPD si activitatea ESP32).

