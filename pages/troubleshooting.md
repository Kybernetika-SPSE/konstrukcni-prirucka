# Časté problémy
	- ## Visual Studio Code
		- ### Šílené chování rozšíření MicroPico
			- Zkontrolujte, zda-li cesta k vaší projektové složce obsahuje diakritiku, nebo jiné speciální znaky (háčky, mezery apod.)
			  logseq.order-list-type:: number
				- **OK** `C:\Users\ja\Projekty\Pico\program.py`
				- **NEOK** `C:\Users\já\Projekty škola\Ročníkovka\program.py`
			- Zkontrolujte, zda-li vaše projektová složka není na sdíleném disku (SMB/ActiveDirectory/...)
			  logseq.order-list-type:: number
			- Zkontrolujte instalaci Python (v. 3.11+) a jeho přidání do proměnného prostředí
			  logseq.order-list-type:: number
				- Pokud instalaci máte, ale python nefunguje správně, vyhledejte na PC "Proměnné prostředí" -> "Upravit proměnné prostředí pro váš účet", rozklikněte si "Path".
					- Vidíte **dvě** řádky s Pythonem ve spodní části? (Python + scripts) (A potenciálně třetí na začátku - ta vás odkazuje na tu instalaci z MS Store) -> posuňte je úplně nahoru, nebo tu **jedinou** co je nahoře odstraňte (pokud neinstalujete python z MS Store)
					- Vidíte jen jednu, ale python jste instalovali a instalace neskončila chybou?
						- Neměnili jste přednasavenou cestu instalace? -> přidejte `%USERPROFILE%\AppData\Local\Programs\Python\Python312\` a `%USERPROFILE%\AppData\Local\Programs\Python\Python312\Scripts\`
			- Pokud nic jiného nezabralo, nainstalujte Python z MS Store
			  logseq.order-list-type:: number
	- ## Thonny IDE
	- ## Raspberry Pi Pico
		- ### Raspberry Pi Pico systém nedetekuje jako disk ani po držení tl. bootsel
		  logseq.order-list-type:: number
			- Zkontrolujte USB kabel - může být pouze napájecí
			- Zkontrolujte pin `3V3_EN` - přizemnění vypíná interní regulátor
			- Zkontrolujte, zda-li není deska ve zkratu
		- ### Raspberry Pi Pico nevidí mé knihovny
		  logseq.order-list-type:: number
			- Pravým tl. na volné místo v seznamu souborů ve VS Code ->`MicroPico: Upload Project to Pico`
		- ### Další potíže
		  logseq.order-list-type:: number
			- [[Nahrání firmware]]