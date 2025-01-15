# AGS10 - TVOC Senzor pro MicroPython

AGS10 je pokročilý senzor pro měření celkových těkavých organických sloučenin (TVOC) pomocí I²C rozhraní. Tento senzor je ideální pro aplikace, kde je potřeba sledovat kvalitu ovzduší a detekovat přítomnost škodlivých těkavých organických látek. Knihovna `ags10.py` pro MicroPython umožňuje snadnou integraci s mikrokontroléry, jako jsou ESP32 nebo ESP8266.
- ## Připojení Senzoru
- ### Pinové Rozložení
  
  | Pin | Název | Popis           |
  |:---:|:-----:|:----------------|
  | 1   | SCL   | Serial clock    |
  | 2   | SDA   | Serial data     |
  | 3   | GND   | Zem              |
  | 4   | VCC   | Napájení         |
  
  > ## Upozornění!
  >
  > - Senzor používá I²C na nízké rychlosti. Frekvence by měla být MENŠÍ než 15kHz.
  >
  > - Datasheet doporučuje nepředvádět častá měření, zejména pokud není potřeba časté měření TVOC. Časté měření může rychle zhoršit výkon senzoru.
  >
  > - Mezi dvěma po sobě jdoucími měřeními by měla být pauza alespoň 1,5 sekundy.
- ## Příkladový Kód v MicroPythonu
  
  Následující příklad ukazuje, jak použít knihovnu `ags10.py` pro čtení hodnot TVOC a odporu ze senzoru AGS10.
  
  ```python
  from ags10 import AGS10
  from machine import I2C, Pin
  from time import sleep_ms
  
  # Inicializace I2C - přizpůsobte piny podle vašeho boardu
  # Příklad pro RP2040:
  i2c = machine.I2C(0, sda=machine.Pin(8), scl=machine.Pin(9))
  
  # Inicializace senzoru
  sensor = AGS10(i2c)
  
  # Povolení kontrolního součtu (CRC) pro měření
  sensor.check_crc = True
  
  # Měření TVOC v ppb (parts per billion)
  tvoc = sensor.total_volatile_organic_compounds_ppb
  print("TVOC: {} ppb".format(tvoc))
  
  # Čekání před dalším příkazem k senzoru
  sleep_ms(2000)
  
  # Měření odporu v kohmech
  resistance = sensor.resistance_kohm
  print("Odpor: {} kohm".format(resistance))
  
  # Rekalibrace nuly
  # Nastavení odporu v kohmech ve virtuální paměti.
  # Potřebujete alespoň 15 minut expozice na čerstvém vzduchu před kalibrací.
  sensor.zero_point_calibrate(resistance)
  sleep_ms(30)
  
  # Resetování nuly na tovární nastavení
  sensor.zero_point_factory_reset()
  sleep_ms(30)
  
  # Aktualizace I²C adresy
  sensor.update_address(126)
  ```
- ## Krok za Krokem
  
  1. **Připojení Senzoru:**
	- Připojte piny `SCL` a `SDA` senzoru AGS10 k odpovídajícím pinům na vašem mikrokontroléru (viz tabulka výše).
	- Připojte napájení (`VCC` a `GND`) podle specifikací senzoru.
	  
	  2. **Instalace Knihovny:**
	- Stáhněte knihovnu `ags10.py` z [GitHubu](https://github.com/gaveshalabs/AGS10_sensor) a nahrajte ji do vašeho zařízení s MicroPythonem.
	  
	  3. **Spuštění Kódu:**
	- Ujistěte se, že I²C piny v kódu odpovídají vašemu hardwaru.
	- Nahrajte a spusťte skript. Senzor začne měřit TVOC a odpor, přičemž vypisuje hodnoty do konzole.
- ## Poznámky
- **Častá Měření:** Vyhněte se příliš častým měřením TVOC, aby nedošlo k předčasnému opotřebení senzoru. Dodržujte doporučený interval alespoň 1,5 sekundy mezi měřeními.
- **Rekalibrace Nuly:** Pro přesné měření je důležité provést rekalibraci nuly v čistém ovzduší po dobu alespoň 15 minut. To zajistí přesné detekce změn v koncentraci TVOC.
- **I²C Adresa:** Výchozí I²C adresa senzoru je obvykle `0x5A`. Pokud máte více zařízení na stejné sběrnici, můžete adresu změnit pomocí metody `update_address()`.
- **Kontrolní Součet (CRC):** Povolení kontrolního součtu zajišťuje integritu dat při měření. Doporučuje se jej ponechat aktivní pro spolehlivé výsledky.
  
  Pro více informací navštivte [GitHub repository AGS10_sensor](https://github.com/gaveshalabs/AGS10_sensor) a [datasheet AGS10](http://www.aosong.com/userfiles/files/Datasheet%20AGS10.pdf).