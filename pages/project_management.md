# Řízení projektu
- ## Doporučená souborová struktura pro uPy
- ```
  my_micropython_project/
  │
  ├── lib/
  │   ├── some_library.py
  │   └── another_library.py
  ├── boot.py
  ├── main.py
  └── requirements.txt
  ```
- Adresářová struktura je velkým pomocníkem při hledání klíčových informací o projektu, ale i při jeho vývoji. Klíčové části:
	- `/lib`: Do této složky ukládejte veškeré použité knihovny/moduly. Pak už je stačí jen nahrát do desky a `include some_library`
	- `boot.py`: Toto je soubor pro bootovací skripty. Spustí se při zapnutí desky
	- `main.py`: Hlavní skript. Spustí se po bootovacím (pokud je přítomen, jinak ihned).
	- `requirements.txt`: Sem se píší potřebné moduly. Nepovinné.