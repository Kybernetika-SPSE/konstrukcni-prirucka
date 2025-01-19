# SaTea - malý meteorologický satelit
- Projekt SaTea je cílen primárně na začínající bastlíře a zaměřuje se na jednoduchost stavby a pochopení základů prototypování.
- ## I/O a nosný plošný spoj
	- Nosná PCB vyvádí na každé straně ("křídle") dvě I2C a UART linky a jednu SPI. Také je zde přítomen jeden analogový vstup
	- ### Orientace na PCB (pohled ze strany součástek, sol. panely dolů)
		- Celá deska je navržena tak, aby bylo snadné se na ní orientovat. Je rozdělena na 3 části - levé a pravé křídlo a spodní (napájecí) část
		- Na každém křídle je přítomen vstup napájení ze solárních panelů a dva nastavitelné **napájecí výstupy** za pomoci zkratspojek `JPx`. Ty jsou rovnou propojeny pro připojení výstupu na pin `3V3`. Při přebroušení cesty plošky 1 a 2 (krajní a středové) na `JPx` lze výstup odpojit a při proletování plošek 3 a 2 připojíte výstup na pin `VIN`, tedy přímé napájení z baterie a solárních panelů
		- **Vstupní a výstupní piny** jsou číslovány zleva doprava a z vrchu dolů. Piny na **levém** křídle jsou tedy označeny **A** a na **pravém** **B**. Na obou jsou číslovány od shora od 0 do 7, tj. horní je A0 (vlevo) / B0 (vpravo) a spodní A7/B7. Pro použití tohoto označení je potřeba použít příslušnou knihovnu [space_probe.py](https://github.com/Kybernetika-SPSE/KVA-SaTea/blob/main/SW/lib/space_probe.py)
- ## Knihovna pro řízení
	- Satelit využívá pro jednoduchost jiné číslování vývodů (jak již bylo řečeno výše) a má tak vlastní knihovnu s přiřazením příslušných pinů. Můžete si ji stáhnout z repozitáře projektu: [space_probe.py](https://github.com/Kybernetika-SPSE/KVA-SaTea/blob/main/SW/lib/space_probe.py)
	- **Instalace**: Přidejte ji do svého projektu (do složky `lib`) a nahrajte projekt na desku.
	- Jak na to: **TBD**
- ## Testovací kódy
	- `boot.py`
	- ```python
	  from space_probe import StatusLight, Battery, Pins, InternalSensors
	  from machine import Pin, I2C
	  from ags10 import AGS10
	  from bme280_float import BME280
	  from veml7700 import VEML7700
	  from MPU6050 import MPU6050
	  import time
	  import network
	  
	  wifis = [["ssid1","pass1"],["ssid2","pass2"]]
	  
	  led = StatusLight()
	  battery = Battery(360) # 360mAh battery
	  sensors = InternalSensors()
	  
	  wifi = network.WLAN(network.STA_IF)
	  
	  print("Battery voltage: ", battery.voltage())
	  print("Battery percentage: ", battery.percentage())
	  print("Battery capacity: ", battery.capacity())
	  
	  print("System temperature: ", sensors.sys_temperature())
	  print("System voltage: ", sensors.sys_voltage())
	  
	  left_line = I2C(1, scl=Pin(Pins.PA6), sda=Pin(Pins.PA5))
	  right_line = I2C(0, scl=Pin(Pins.PB1), sda=Pin(Pins.PB0))
	  
	  print("Devices on left line: ", [hex(device) for device in left_line.scan()])
	  print("Devices on right line: ", [hex(device) for device in right_line.scan()])
	  
	  wifi.active(True)
	  scan = wifi.scan()
	  networks = [""] * len(scan)
	  for i in range(len(scan)):
	      networks[i] = scan[i][0].decode("utf-8")
	  print("Available networks: ", scan)
	  for net in wifis:
	      if net[0] in networks:
	          wifi_ssid, wifi_password = net
	          print("Connecting to " + wifi_ssid + ".", end="")
	          wifi.connect(wifi_ssid, wifi_password)
	          time_start = time.time()
	          while not wifi.isconnected():
	              print(".", end="")
	              time.sleep(1)
	              if time.time() - time_start > 30:
	                  print("timeout.")
	                  break
	  
	          break
	  else:
	      print("No known network found.")
	      wifi_ssid, wifi_password = None, None
	  
	  if wifi.isconnected():
	      print("connected.")
	      led.on()
	      print("IP address: ", wifi.ifconfig()[0])
	      time.sleep(0.1)
	      led.off()
	  else:
	      print("Connection failed.")
	      led.on()
	      time.sleep(1)
	      led.off()
	  ```
	- `main.py`
	- ```python
	  import machine
	  import network
	  import time
	  from umqtt.simple import MQTTClient
	  
	  # MQTT configuration
	  MQTT_BROKER = "broker.hivemq.com"
	  MQTT_PORT = 1883
	  MQTT_TOPIC = "satea/sensors"
	  CLIENT_ID = "satea_0001"
	  
	  # I2C addresses for the sensors
	  AGS10_ADDR = 0x1A
	  BME280_ADDR = 0x76
	  MPU6050_ADDR = 0x68
	  VEML7700_ADDR = 0x10
	  
	  # Initialize I2C lines
	  # i2c_left = machine.I2C(0, scl=machine.Pin(22), sda=machine.Pin(21))
	  # i2c_right = machine.I2C(1, scl=machine.Pin(19), sda=machine.Pin(18))
	  
	  # Function to check if a device is present on the I2C bus
	  def is_device_present(i2c, addr):
	      try:
	          i2c.readfrom(addr, 1)
	          return True
	      except OSError:
	          return False
	  
	  # Check for sensors on the left I2C line
	  sensors_left = {
	      "AGS10": is_device_present(left_line, AGS10_ADDR),
	      "BME280": is_device_present(left_line, BME280_ADDR),
	      "MPU6050": is_device_present(left_line, MPU6050_ADDR),
	      "VEML7700": is_device_present(left_line, VEML7700_ADDR)
	  }
	  
	  # Check for sensors on the right I2C line
	  sensors_right = {
	      "AGS10": is_device_present(right_line, AGS10_ADDR),
	      "BME280": is_device_present(right_line, BME280_ADDR),
	      "MPU6050": is_device_present(right_line, MPU6050_ADDR),
	      "VEML7700": is_device_present(right_line, VEML7700_ADDR)
	  }
	  
	  
	  # Connect to MQTT broker
	  client = MQTTClient(CLIENT_ID, MQTT_BROKER, port=MQTT_PORT)
	  client.connect()
	  
	  # Publish sensor presence data
	  client.publish(MQTT_TOPIC, str({"left": sensors_left, "right": sensors_right}))
	  
	  while True:
	      # Try to read data from AGS10 on left line
	      if sensors_left["AGS10"]:
	          ags = AGS10(i2c=left_line)
	          ags.check_crc = True
	          tvoc = ags.total_volatile_organic_compounds_ppb
	          co2 = ags.carbon_dioxide_ppm
	          print("Total VOC: ", tvoc, "ppb")
	          print("CO2: ", co2, "ppm")
	          client.publish(MQTT_TOPIC, str({"sensor": "AGS10", "tvoc": tvoc, "co2": co2}))
	  
	      # Try to read data from BME280 on left line
	      if sensors_left["BME280"]:
	          bme = BME280(i2c=left_line)
	          temp, press, hum = bme.values
	          print("BME280 temp/press/hum ", bme.values)
	          client.publish(MQTT_TOPIC, str({"sensor": "BME280", "temp": temp, "press": press, "hum": hum}))
	  
	      # Try to read data from VEML7700 on right line
	      if sensors_right["VEML7700"]:
	          veml = VEML7700(i2c=right_line)
	          lux = veml.read_lux()
	          print("Ambient light: ", lux, "lux")
	          client.publish(MQTT_TOPIC, str({"sensor": "VEML7700", "lux": lux}))
	  
	      # Try to read data from MPU6050 on right line
	      if sensors_right["MPU6050"]:
	          mpu = MPU6050(i2c=right_line)
	          gyro_data = mpu.read_gyro_data()
	          print("MPU6050 values: ", gyro_data)
	          client.publish(MQTT_TOPIC, str({"sensor": "MPU6050", "gyro": gyro_data}))
	  
	      # Battery parameters
	      client.publish(MQTT_TOPIC, str({"sensor": "Battery", "voltage": battery.voltage(), "capa_remain": battery.capacity(), "percentage": battery.percentage()}))
	  
	      # System temperature
	      client.publish(MQTT_TOPIC, str({"sensor": "System", "temp": sensors.sys_temperature()}))
	  
	      time.sleep(10)
	  
	  # Disconnect from MQTT broker
	  client.disconnect()
	  ```
- > Více informací naleznete v repozitáři projektu [Kybernetika-SPSE/KVA-SaTea: Model malé vesmírné sondy pro výuku](https://github.com/Kybernetika-SPSE/KVA-SaTea)
- > Užitečné stránky: [Řízení projektu](project_management), [[Nahrání firmware]], [[Nastavení IDE]] , [[bme280]] [[veml7700]], [[ags10]], [[mpu6050]]