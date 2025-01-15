# Nahrání firmware s MicroPythonem
- ## Desky založené na čipech `RP2040` a `RP2350`
	- > Raspberry Pi Pico & Pico W, Pico 2 & Pico 2 W, ...
	- ### Nahrání
		- Najděte si svoji desku na [MicroPython - Python for microcontrollers (port rp2)](https://micropython.org/download/?port=rp2)
		  logseq.order-list-type:: number
		- Vyberte si verzi (ideálně z "Releases", ty jsou již hotové a stabilní, ale pokud chcete otestovat nové funkce, klidně koukněte i do "Preview builds") a stáhněte si `.uf2` soubor
		  logseq.order-list-type:: number
		- Podržte na své desce tlačítko `BOOT` nebo `BOOTSEL` a připojte ji k PC. Bude se chovat jako "flashdisk"
		  logseq.order-list-type:: number
		- Zkopírujte na ni stažený soubor
		  logseq.order-list-type:: number
	- ### Troubleshooting
		- #### Povedlo se vám desku zablokovat a přehrání firmware nepomáhá? V tom případě bude potřeba atomová bomba
			- Stáhněte si [raspberrypi-pico/flash_nuke.uf2 at main · dwelch67/raspberrypi-pico](https://github.com/dwelch67/raspberrypi-pico/blob/main/flash_nuke.uf2)
			  logseq.order-list-type:: number
			- Místo FW nahrajte tento soubor. **OKAMŽITĚ DOJKE KE SMAZÁNÍ VEŠKÉRÉHO OBSAHU FLASH PAMĚTI DESKY**
			  logseq.order-list-type:: number
			- Nahrajte klasický FW
			  logseq.order-list-type:: number
		- #### Pico se nepřipojuje k počítači
			- Zkontrolujte, zda-li nemáte pouze napájecí USB kabel.
			- Zkontrolujte piny `3V3` a `3V3_EN` Na obou by jste měli naměřit napětí.
		- #### Nejde mi WiFi
			- Zkontrolujte, zda-li máte správný firmware (pro desku s WiFi)
	- ### Časté dotazy
		- #### Mám Pico 2. Jaký micropython mám vybrat?
			- To záleží na vás. RP2350 má dvě ARM a dvě RISC-V jádra a volba je na vašich osobních preferencích.
- ## Desky založené na čipech ESP32
	- >TBD