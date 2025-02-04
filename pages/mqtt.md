# MQTT

> **MQTT** (Message Queuing Telemetry Transport) je lehký a jednoduchý protokol určený pro zasílání zpráv mezi zařízeními v síti s nízkou šířkou pásma a/nebo vysokou latencí. Využívá se zejména v oblasti **IoT (Internet of Things)**, kde umožňuje efektivní výměnu dat mezi senzory, aktuátory a dalšími uzly v síti.
- ---
- ## Proč použít MQTT?
	- **Nízká režie komunikace** – Ideální pro sítě s omezenou šířkou pásma (např. mobilní nebo satelitní sítě).
	- **Jednoduchá implementace** – Podpora na různých platformách od mikrokontrolérů (Arduino, ESP, STM32) až po desktopové servery.
	- **Spolehlivost** – Nabízí několik úrovní kvality služeb (QoS) pro zajištění doručení zpráv.
	- **Flexibilita** – Komunikace je založena na architektuře **publisher-subscriber**, takže lze snadno přidávat nebo odebírat zařízení bez nutnosti zásahu do celé infrastruktury.
- ---
- ## Základní princip
	- Architektura MQTT je založena na tzv. **Brokeru**, který zprostředkovává komunikaci mezi **publishery** a **subscribery**.
	- 1. **Broker** – Centrální uzel (server), který přijímá zprávy od publisherů a rozesílá je subscriberům, kteří o dané zprávy projevili zájem.
	- 2. **Publisher** – Zdroj dat, který posílá zprávy (např. senzor teploty).
	- 3. **Subscriber** – Klient, který se přihlásí k odběru zpráv na určitých kanálech (topics).
	- ![MQTT Diagram](https://www.twilio.com/content/dam/twilio-com/global/en/blog/legacy/2023/what-is-mqtt/MQTT_Diagram_gOmDdU4.png "MQTT Protocol")
- ### Topics (Témata)
	- Zprávy v MQTT jsou vždy přiřazeny k nějakému **topicu** (např. `skola/kybernetika/teplota`). Subscriber se může přihlásit k odběru konkrétního topicu (nebo skupiny topiců pomocí wildcard) a díky tomu dostává pouze ty zprávy, které ho zajímají.
- ### QoS (Quality of Service)
  MQTT definuje tři úrovně kvality služeb:
	- **QoS 0** – Zpráva je odeslána jen jednou, bez potvrzení doručení.
	- **QoS 1** – Zpráva je doručena alespoň jednou (vyžaduje potvrzení od příjemce).
	- **QoS 2** – Zpráva je doručena přesně jednou (nejvyšší spolehlivost).
- ---
- ## Typické použití
	- **Sběr dat ze senzorů** (např. teplota, vlhkost, CO2) a jejich zasílání do centrálního serveru (brokeru).
	- **Ovládání zařízení** (spínání relé, světel, motorů) vzdálenými příkazy.
	- **Monitorování a logování** – MQTT zprávy lze ukládat do databáze nebo posílat do vizualizačního nástroje (Grafana, Node-RED Dashboard).
- ---
- ## Autentizace a zabezpečení
  
  V produkčních prostředích nebo případech, kdy jsou data citlivá, je vhodné použít:
  1. **Uživatelské jméno a heslo**
	- Mnoho brokerů (např. Mosquitto) podporuje konfigurační soubor, kde nastavíte uživatele a hesla.
	- V klientovi pak zadáte `username` a `password` při připojení.
	- Zvyšuje to zabezpečení, protože k brokeru se nepřipojí neautorizovaný uživatel.
- 2. **Šifrovaná komunikace (TLS/SSL)**
	- Broker je nastaven na portu 8883 (typicky) a vyžaduje certifikát.
	- Na straně klienta musí být podpora SSL (u malých mikrokontrolérů to může být náročnější).
	- Zabrání odposlouchávání a umožňuje ověření brokeru i klientů.
- 3. **Access Control Lists (ACL)**
	- Pomocí ACL lze stanovit, který uživatel může publikovat či odebírat určité topicy.
	- Tím zajišťujete, že například senzor bude moci jen posílat data, ale nikoliv ovládat jiná zařízení.
- > **Poznámka**: Nastavení autentizace se obvykle provádí v konfiguračním souboru MQTT brokeru a liší se podle konkrétní platformy (např. Mosquitto, HiveMQ, EMQX).  
  
  ---
- ## Příkladový kód v MicroPythonu
  ```python
  import network
  import time
  from machine import Pin
  from umqtt.simple import MQTTClient
  
  wlan = network.WLAN(network.STA_IF)
  wlan.active(True)
  wlan.connect("ssid","pass")
  time.sleep(5)
  print(wlan.isconnected())
  
  sensor = Pin(16, Pin.IN)
  
  mqtt_server = 'broker.hivemq.com'
  client_id = 'sauron'
  topic_pub = b'middle_earth/sauron'
  topic_sub = b'middle_earth/sauron'
  topic_msg = b'Ach nach utunbagul'
  
  def callback(topic, msg):
    print("Message received: ", msg)
    print("Topic: ", topic)
  
  def mqtt_connect():
    client = MQTTClient(client_id, mqtt_server, keepalive=3600)
    client.connect()
    print('Connected to %s MQTT Broker'%(mqtt_server))
    return client
  
  def reconnect():
    print('Failed to connect to the MQTT Broker. Reconnecting...')
    time.sleep(5)
    machine.reset()
  
  try:
    client = mqtt_connect()
    client.set_callback(callback)
    client.subscribe(topic_sub)
  except OSError as e:
    reconnect()
  while True:
    client.check_msg()
    client.publish(topic_pub, topic_msg)
    time.sleep(3)
  ```
- ### Důležité stáhnut potřebný mqtt modul pomocí mip
  ```python
  import mip
  mip.install("umqtt.simple")
  mip.install("umqtt.robust")
  ```