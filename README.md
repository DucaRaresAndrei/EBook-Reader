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


## Bill Of Materials

| Componenta | Link de cumparare | Datasheet |
|-----------|--------------|-----------|
| ESP32-C6-WROOM-1-N8 | [Model](https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif+Systems/view-part/?ref=eda) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://www.mouser.com/catalog/specsheets/Espressif_ESP32_C6_WROOM_1%20_Datasheet_V0.1_PRELIMINARY_en.pdf?srsltid=AfmBOooHQKNitqODRaaPjoZInfWKTacDER1t5uRK6sKqT13TrzvVo_B7) |
| Si1308EDL-T1-GE3 | [Model](https://componentsearchengine.com/part-view/SI1308EDL-T1-GE3/Vishay) | [Datasheet](https://www.alldatasheet.com/view.jsp?Searchword=Si1308edl&gad_source=1&gbraid=0AAAAADcdDU-px713ONYSnQ2O-gcwqYcFq&gclid=Cj0KCQjwqcO_BhDaARIsACz62vN_Nz3MJOc6J_03gnVBm7aSqC8v9wyP0VD-iRKP-gFrYgdhLi99I14aAlVJEALw_wcB) |
| KP-1608SURCK | [Model](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/view-part/?ref=search&t=LED%200603) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://media.elv.com/file/107153_led_surck1608_data.pdf) |
| R0402 | [Model](https://componentsearchengine.com/part-view/R0402%201%25%20100%20K%20(RC0402FR-07100KL)/YAGEO) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://www.resistor.com/assets/pdf/0402tstd.pdf) |
| DS3231SN# | [Model](https://www.snapeda.com/parts/DS3231SN%23/Analog+Devices/view-part/?ref=eda) | [Datasheet](https://www.alldatasheet.com/view.jsp?Searchword=Ds3231sn%20datasheet&gad_source=1&gbraid=0AAAAADcdDU-Gy9URfMxGmqiPg7ci5L3wR&gclid=Cj0KCQjwqcO_BhDaARIsACz62vMkK3ETSnW2w7mo0Fa-wgWJGn89AxWCyIND6k5X8MmoPl6hv6VWwT8aAiS-EALw_wcB) |
| PGB1010603MR | [Model](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [Datasheet](https://www.alldatasheet.com/view.jsp?Searchword=Pgb1010603mr&gad_source=1&gbraid=0AAAAADcdDU8aYfZtfJfdZ9I5j6RwZ_cbA&gclid=Cj0KCQjwqcO_BhDaARIsACz62vOPBOBe0eOh5gDUFkkKl4JBcbmoFZYtJ8BOnbaWqr_BuUCcVWvbutAaAmGkEALw_wcB) |
| USBLC6-2SC6Y | [Model](https://www.snapeda.com/parts/USBLC6-2SC6Y/STMicroelectronics/view-part/?ref=eda) | [Datasheet](https://www.digikey.com/en/htmldatasheets/production/1375342/0/0/1/usblc6-2sc6y) |
| USB4110-GF-A | [Model](https://componentsearchengine.com/part-view/USB4110-GF-A/GCT%20(GLOBAL%20CONNECTOR%20TECHNOLOGY)) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://gct.co/files/drawings/usb4110.pdf) |
| CPH3225A | [Model](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) | [Datasheet](https://octopart.com/datasheet/cph3225a-seiko-25340571) |
| Adafruit | [Model](https://eu.mouser.com/ProductDetail/Adafruit/4208?qs=PzGy0jfpSMtbScLbr0L5dw%3D%3D) | [Datasheet](https://www.arrow.com/en/manufacturers/adafruit-industries/datasheets) |
| SD0805S020S1R0 | [Model](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [Datasheet](https://www.alldatasheet.com/view.jsp?Searchword=SD0805S&sField=2) |
| W25Q512JVEIQ | [Model](https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif+Systems/view-part/?ref=eda) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://www.mouser.com/datasheet/2/949/W25Q512JV_SPI_RevB_06252019_KMS-2487502.pdf?srsltid=AfmBOoquExqDVgxEELF9CzuOGxHos0CD1nQDROHD6Eebdm2foNzqozqU) |
| EVQPUJ02K | [Model](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k) | [Datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2201121800_PANASONIC-EVQPUJ02K_C2936858.pdf) |
| XC6220A331MR-G | [Model](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) | [Datasheet](https://www.alldatasheet.com/view.jsp?Searchword=Xc6220&gad_source=1&gbraid=0AAAAADcdDU8aYfZtfJfdZ9I5j6RwZ_cbA&gclid=Cj0KCQjwqcO_BhDaARIsACz62vPS06NB6tLgniZzfaVpKNu1m811BNk6AEPfg4DbP6f5S8QWA_pW_UQaAv-0EALw_wcB) |
| Bobina | [Model](https://store.comet.srl.ro/Catalogue/Product/43497/) | [Datasheet](https://www.scribd.com/document/814581278/Datasheet-Bobina) |
| MAX17048G+T10 | [Model](https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/?ref=eda) | [Datasheet](https://www.alldatasheet.com/view.jsp?Searchword=Max17048&gad_source=1&gbraid=0AAAAADcdDU8aYfZtfJfdZ9I5j6RwZ_cbA&gclid=Cj0KCQjwqcO_BhDaARIsACz62vNa9xrVfzjCjADRwXD0RBbo4Nret3ywwteDGLJKZui8ZL8KdVlTE7caAvQxEALw_wcB) |
| SMD Solder | [Model](https://grabcad.com/library/solder-jumpers-1) | [Datasheet]() |
| BUTTON | [Model](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k) | [Datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2201121800_PANASONIC-EVQPUJ02K_C2936858.pdf) |
| BME680 | [Model](https://www.snapeda.com/parts/BME680/Bosch/view-part/?welcome=home) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf) |
| PFMF | [Model](https://www.mouser.co.uk/ProductDetail/EPCOS-TDK/B72520T0350K062?qs=dEfas%2FXlABIszF52uu7vrg%3D%3D) | [Datasheet](https://ro.mouser.com/c/ds/circuit-protection/thermistors/resettable-fuses-pptc/?m=Schurter&series=PFMF) |
| DMG2305UX-7 | [Model](https://componentsearchengine.com/part-view/DMG2305UX-7/Diodes%20Incorporated) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://www.mouser.com/datasheet/2/115/DMG2305UX-266242.pdf?srsltid=AfmBOop22k34YTJJra1xubiU6LPiN4M4JlcWbRoSNdxSGFak8uWgXPpK) |
| BD5229G-TR | [Model](https://componentsearchengine.com/part-view/BD5229G-TR/ROHM%20Semiconductor) | [Datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2201131330_ROHM-Semicon-BD5229G-TR_C962636.pdf) |
| MCP73831T-5ACI/OT | [Model](https://www.mouser.co.uk/ProductDetail/Microchip-Technology/MCP73831T-5ACI-OT?qs=hH%252BOa0VZEiAcgAcEkuamXg%3D%3D) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://ww1.microchip.com/downloads/en/DeviceDoc/MCP73831-Family-Data-Sheet-DS20001984H.pdf) |
| CAPACITOR | [Model](https://componentsearchengine.com/part-view/R0402%201%25%20100%20K%20(RC0402FR-07100KL)/YAGEO) | [Datasheet](//efaidnbmnnnibpcajpcglclefindmkaj/https://www.resistor.com/assets/pdf/0402tstd.pdf) |


## Etapele implementarii proiectului:

- Am realizat schematicul conform exemplului din cerinta si folosind biblioteca specificata. Am verificat cu ERC si nu am intampinat nicio eroare. Warningurile avute au fost acceptate deoarece nu reprezinta o problema pentru structura proiectului.

- Am proiectat PCb-ul, unde am respectat in clar dimensiunile date. Am pozitionat cu grija piesele, incercand sa le pozitionez ca in fisierul exemplu. Am facut rutarea, planurile de masa si am rulat cu fisierul corect de DRC. Am rezolvat erorile de air wire pe cat de mult posibil, ramanand doar una, ce am fost nevoit sa o accept.

- Am modelat piesele 3D si le-am integrat in placa. Am intampinat probleme in a gasi anumite modele 3D, ba chiar am fost nevoit sa creez singur test pad-urile.

- Am creat manual bateria si displayul, dupa masuratorile si specificatiile cerute si le-am atasat, alaturi de modelul 3d al PCB-ului, la carcasa eBook-Reader-ului.
