# Aosong DHT11 a DHT22
- > **DHT11** a **DHT22** jsou populární digitální senzory pro měření teploty a vlhkosti vzduchu. Jsou často používány v různých projektech založených na mikrořadičích, jako jsou Arduino nebo ESP8266/ESP32 s MicroPythonem.
- ### DHT11
	- **Teplotní rozsah:** 0 až 50 °C
	- **Přesnost teploty:** ±2 °C
	- **Vlhkostní rozsah:** 20 až 90 % RH (relativní vlhkost)
	- **Přesnost vlhkosti:** ±5 % RH
	- **Frekvence měření:** jedna měření za sekundu (1 Hz)
	- **Napájení:** 3.5V až 5.5V
	- **Cena:** Nižší cena, vhodný pro základní aplikace
- ### DHT22 (AM2302)
	- **Teplotní rozsah:** -40 až 80 °C
	- **Přesnost teploty:** ±0.5 °C
	- **Vlhkostní rozsah:** 0 až 100 % RH
	- **Přesnost vlhkosti:** ±2-5 % RH
	- **Frekvence měření:** jedna měření za 2 sekundy (0.5 Hz)
	- **Napájení:** 3.3V až 6V
	- **Cena:** Vyšší než DHT11, ale nabízí širší rozsahy a vyšší přesnost
- ### Rozdíly mezi DHT11 a DHT22:
	- **Rozsah měření:** DHT22 pokrývá širší rozsah teplot a vlhkosti.
	- **Přesnost:** DHT22 poskytuje vyšší přesnost měření.
	- **Frekvence měření:** DHT11 může měřit rychleji, což je výhodné pro aplikace vyžadující časté aktualizace.
	- **Cena:** DHT11 je levnější, což jej činí vhodným pro základní projekty.
- ### Ukázkový kód v MicroPythonu s využitím vestavěné knihovny `dht`
	- Následující příklad ukazuje, jak připojit a číst data ze senzoru DHT22 pomocí MicroPythonu. Předpokládáme, že používáte desku kompatibilní s MicroPythonem, například ESP8266 nebo ESP32.
	- #### Připojení senzoru:
		- **VCC** připojte na 3.3V nebo 5V (záleží na senzoru a desce)
		- **GND** připojte na zem
		- **Data** připojte na digitální GPIO pin (např. GPIO4)
	- #### Kód:
	  
	  ```python
	  import machine
	  import dht
	  import time
	  
	  # Inicializace pinu, ke kterému je připojen datový pin senzoru
	  # Například GPIO4
	  sensor_pin = machine.Pin(4)
	  
	  # Vytvoření instance senzoru DHT22
	  sensor = dht.DHT22(sensor_pin)
	  
	  while True:
	    try:
	        # Provádí měření
	        sensor.measure()
	        
	        # Čtení teploty a vlhkosti
	        temperature = sensor.temperature()
	        humidity = sensor.humidity()
	        
	        # Zobrazení naměřených hodnot
	        print('Teplota: {}°C, Vlhkost: {}%'.format(temperature, humidity))
	        
	    except OSError as e:
	        print('Chyba při čtení senzoru:', e)
	    
	    # Počká 2 sekundy před dalším měřením
	    time.sleep(2)
	  ```
	- #### Vysvětlení kódu:
	  
	  1. **Import knihoven:**
		- `machine` pro práci s hardwarem, jako jsou piny.
		- `dht` pro práci se senzory DHT11/DHT22.
		- `time` pro zavedení zpoždění mezi měřeními.
	- 2. **Inicializace pinu:**
		- Vytvoříme objekt `Pin` na GPIO4 (můžete změnit podle vašeho připojení).
	- 3. **Vytvoření instance senzoru:**
		- `dht.DHT22(sensor_pin)` inicializuje senzor DHT22 připojený na vybraný pin.
		- Pokud používáte DHT11, změňte na `dht.DHT11(sensor_pin)`.
	- 4. **Hlavní smyčka:**
		- V nekonečné smyčce opakovaně provádíme měření.
		- `sensor.measure()` spustí měření teploty a vlhkosti.
		- `sensor.temperature()` a `sensor.humidity()` načtou naměřené hodnoty.
		- Výstup se zobrazí v sériovém monitoru.
		- Po každém měření program čeká 2 sekundy, což je vhodné pro DHT22 (maximální frekvence měření je 0.5 Hz).
	- 5. **Ošetření chyb:**
		- Používáme `try-except` blok pro zachycení případných chyb při čtení senzoru, což může pomoci při ladění a spolehlivosti programu.
- #### Poznámky:
- **Volba senzoru:** Pokud používáte DHT11 místo DHT22, ujistěte se, že inicializujete správný typ senzoru:
  ```python
  sensor = dht.DHT11(sensor_pin)
  ```
- **Stabilita připojení:** Senzory DHT mohou být citlivé na dlouhé dráty a špatné připojení. Doporučuje se mít pevné a krátké propojení mezi senzorem a mikrořadičem.
- **Pull-up rezistor:** Některé moduly DHT vyžadují externí pull-up rezistor (např. 10kΩ) na datovém pinu. Pokud máte problémy s čtením dat, zkuste přidat rezistor.
  
  > Více informací lze nalézt např. zde: [Temperature and Humidity - MicroPython docs](https://docs.micropython.org/en/latest/esp8266/tutorial/dht.html)