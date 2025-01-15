# Nastavení Visual Studio Code pro MicroPython
- > Podmiňující programy: Python (>3.10)
- ## Rozšíření
	- Rozklikněte si z levého bočního panelu seznam rozšíření a **vyhledejte přímo v seznamu** a nainstalujte si následující:
	  logseq.order-list-type:: number
	- K práci s MicroPythonem potřebujete rozšíření [MicroPico](https://marketplace.visualstudio.com/items?itemName=paulober.pico-w-go)
	  logseq.order-list-type:: number
	- K práci s Pythonem a jeho deriváty potřebujete set rozšíření [Python extension pack](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python-extension-pack)
	  logseq.order-list-type:: number
- ## Příprava projektu
	- Vytvořte si složku pro váš projekt. Např. `hello_world`
	  logseq.order-list-type:: number
	- Otevřete si tuto složku ve VSC
	  logseq.order-list-type:: number
	- Stiskněte klávesy `CTRL`+`L_SHIFT`+`P`. Objeví se příkazový řádek. Napište do něj `MicroPico: Configure Project` a potvrďte
	  logseq.order-list-type:: number
	- Vše připraveno, můžete zašít přidávat soubory
	  logseq.order-list-type:: number
- ## Časté dotazy
	- ### Jak si otevřu terminál desky?
		- `Terminal` > `New Terminal`. Pak vpravo níže `+` a `MicroPico VREPL`
	- ### Spuštění kódu?
		- Setkáte se zde se dvěma tlačítky "play". Jedno v pravém horním rohu, to spouští skripty na **PC** (tj. v Pythonu) To druhé, cca uprostřed nalevo **DOLE** označené nápisem `Run` spustí skript na desce.
	- ### Nahrání na desku
		- Aby kód zůstal na desce, je třeba jej na ni nahrát. Pro použití vlastních knihoven je to nezbytné. Klikněte do části s projektovými soubory a dejte Upload project to Pico`
		- Pokud chcete nahrát jen jeden soubor, klikněte na něj pravým a dejte `Upload file to Pico`
		- Také můžete použít tlačítko `Toggle Mpy FS` (Spodní lišta uprostřed). Objeví se vám další složka obsahující soubory přímo na desce, kam stačí knihovny zkopírovat