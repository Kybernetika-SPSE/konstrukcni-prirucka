# Sériové komunikační protokoly
- > Tady bude povídání
- ## I2C/IIC (Inter Integrated Circuit)
	- ### Základní info
		- > Jedná se o velmi jednoduchý kom. protokol pro nízkorychlostní komunikace (<1Mbps) na krátké vzdálenosti (<2m)
		- Počet potřebných pinů: **2** (SCK/SCL a SDA)
		- Typ komunikace: **Master-Slave** synchronní, s časovou a datovou linkou (nevyužívá diferenciální sig.)
		- Max. počet zařízení: **127** (teoreticky)
	- ### Import do vlastního skriptu
		- > Lze využít knihovnu I2C (pro HW linku) nebo SoftI2C (pro emulovanou/bit-bangovanou linku)
		- Snipplet pro HW I2C linku (MCU má na zvolených pinech možnost vyvolat I2C linku)
			- ```python
			  from machine import Pin, I2C
			  line = I2C(0, sda=Pin(0), scl=Pin(1))	# I2C linka 0 na Raspberry Pi Pico, SDA na GP0, SCL na GP1 
			  ```
		- Snipplet pro emulovanou I2C linku (MCU nemá možnost na vybraných pinech vyvolat HW I2C)
			- ```python
			  from machine import Pin, SoftI2C
			  line = SoftI2C(sda=Pin(0), scl=Pin(1))	# Emulovaná I2C linka na Raspberry Pi Pico, SDA na GP0, SCL na GP1 
			  ```
		- Otestování a mapování zařízení
			- ```python
			  print("Devices on IIC line: ", [hex(device) for device in line.scan()])
			  ```
	- ### Dodatečné informace
		- [I2C.pdf](../assets/I2C_1733823129406_0.pdf)
		- [I²C – Wikipedie](https://cs.wikipedia.org/wiki/I%C2%B2C)
		- [i2c_spec.pdf](../assets/i2c_spec_1733823278733_0.pdf ':include') - z [Wayback Machine](https://web.archive.org/web/20110811173228/http://www.nxp.com/acrobat_download2/literature/9398/39340011.pdf)
-
-