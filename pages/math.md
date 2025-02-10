 # Základní matematické operace v Pythonu

Python podporuje základní matematické operace přímo v jazyce. 
Sčítání: a+b
Odečítání: a-b
Násobení: a*b
Dělení: a/b (vrátí jako desetinné číslo)
Celočísselné dělení: a//b (vrátí celé číslo, vždy zaokrouhluje dolů, funguje jako, když napíše první celé číslo menší než výsledek)
Zbytek po dělení: a%b (obě čísla a, b nemusí být celá, funguje tak, že dopočítá, o kolik je dělenec větší než nejbližší menší celočíselný násobek dělitele)
Mocniny: a**b (dá se tímto způsobem také dělat odmosnina, bez knihovny math)
x=6/7
print (x)

0.8571428571428571 

y=(-1)//100
print (y)

-1

z=12%7
print (z)

5

a=-12%7
print (a)

2

b=1.1%3.5
print (b)

1.1

c= 5**3
print (c)

125

d= 256**(1/4)
print (d)

4,0

 # Knihovna math

Pro složitější matematické operace využíváme knihovnu math. Musí se importovat. 
import math
Knihovna obsahuje matematické konstanty.
print(math.pi)
print(math.e)
Vrátí: 
3.141592653589793
2.718281828459045

Převodníky jednotek, ze stupňů na radiany a obráceně.
print(math.degrees(math.pi/2))
print(math.radians(90))
Vrátí:
90.0
1.5707963267948966

Zaobrouhloání nahoru pomocí ceil a dolů pomocí floor.

print (math.ceil(6.3))
print (math.floor(9.8))
Vrátí:
7
9

Goniometrické funkce !!pozor!! jako vstup jsou hodnoty v radiánech

Logaritmus ze zadaného základu, bez něj vrátí přirozený logaritmus.

Absolutní hodnota čísla math.abs(x), fabs (x): vrací absolutní hodnotu z čísla

Největší společný dělitel gcd

Odmocnina sqrd: vrátí odmocnimu z daného čísla

Zajímavé 
modf: rozdělí číslo na celou část a desetinnou
trunc: zaokrouhlí k 0
fmod: zbytek po dělení



 # Knihovny SymPy
import sympy as sp
Úprava matematických výrazů.
Vrací upravené hodnoty. Pomocí pprint vrací hodnoty se standardnější matematickou formou, například pro odmocninu dá znak odmocniny místo vypsání sqrt.

Umožňuje také práci s neznámými. Musíme hodnotu nastavit jako proměnnou. 
x = sp.symbols('x')
Můžeme jich nastavit více.
x,y,z = sp.symbols('x,y,z')

Po označení, že jde o neznámé, s nimi umí sám python dělat některé záklední operace. (exponenty se píší jako **, x na druhou = x**2)
sp.factor vypíše rozložený výraz na součin
sp.expand obráceně roznásobí závorky
solve - mnoho funkcí, pro nás je nejužitečnější hledání kořenů kvadratické rovnice
dokáže vyjádřit obecně. pokud je více neznámých, umí vypočítat jako rovnici s parametrem.
Pokud nejsou reálná řešení, najde i řešení jako komplaxní čísla.

Pomocí solve je možné řešit i rovnice s polynomy vyšších stupňů. Získáme všechna možná řešení, včetně komplexních čísel.

Solve se dá také použít pro vyhledávání nulových hodnot goniometrických funkcí. 
Tato verze nevrátí všechna řešení ale jen ta do 2π. Solveset najde i ta ostatní a zapíče matematicky.

Solve a solveset umí napsat řešení také u jednoduchých nerovnic, ale u složitějších vrací špatná řešení.

 ## Derivace
využívá příkaz diff(f, x). Takto zadaná zderivuje funkci f podle x.
Pomocí diff dokáže zderivovat i složitější výrazy.

 ## Integrace
Vzor použití integrate(f, x).

Příkaz latex - přepíše zadaný výraz do formátu jak by byl zapsán v LaTeXu. 

Knihovna umí další funkce jako výpočet singularit, výpočty lokálních nimim a maxim, 
test jestli je funkce v zadanám internalu klesající/rostoucí - musí se řešit samostatně na dva příkazy,
derivace vyšších řádů a limit. 
Více o těchto funkcích najdeta na https://www.root.cz/clanky/analyza-funkci-s-vyuzitim-knihovny-sympy/

 # Knihovny NumPy
Slouží pro práci s poli/maticemi. Můžeme mít matice s libovolně rozměry.

Pole polí (Prvky pole jsou tvořeny dalšími poli.)
Vytvořená pole mají neměnou velikost a datový typ.

 ## Vytvoření matice
import numpy
array = numpy.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
array (vypíše ji)

Můžeme dělat jednoduché úpravy tak, že k matici přičteme 1 a ke všem hodnotám se přičte jedna. 
Chová se jako, kdyby šlo o jednotlivé proměné a upravu udělá se všemi z nich.

import numpy
array = numpy.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
array = (array -1)/2+4
array 

Vrátí matici 
array([[4. , 4.5, 5. ],
       [5.5, 6. , 6.5],
       [7. , 7.5, 8. ]])

Můžeme také dělat pravdivostní tabulky, tím že zadáme podmínku.

 ## Volání podle indexu
Lze psát do jedné hranaté závorky.
Lze používat ":". Pokud napíšeme jen ":" , tak vrátí celý požadovaný sloupec/řádek.
Užití "1:" vrátí z požadovaného sloupce/řádku všechny hodnoty s indexem větší než 1. Pokud dáme čísla z obou stran 1:3, tak vrátí všechny od 1 a menší než 3.

array = numpy.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
array[0:3, 1:]

Vrátí:
array([[2, 3],
       [5, 6],
       [8, 9]])

Můžeme dát jako index podmínku a vrátí prvky, které ji splňují.

Lze také používat operátory *= apod, které všechny prvky v poli vynásobí.

 ## Matice
Matice zadáváme rozměry a hodnotami do nich, nebo jdou tvořit jako pole, což jsme popsali dříve.
numpy.full((4, 4), 12.34)

Jednorozměrné matice můžeme tvořit pomocí arange, což funguje jako range, které používáme nejčastěji při for cyklech.
array.reshape((3, 4)) příkaz pro změnu tvaru matice, pokud zůstane stejný počet prvků. Z matice 1x12 udělá matici 3x4. (Pokud v nové matici uděláme změnu, tak se změní i v původním.)

Násobení matic: array1 @ array2


 # Vizualizace matematiky pomocí knihovny matplotlib
plt.plot(souřadnice x, souřadnice y, co zobrazí)
zobratí graf s osami x a y

import matplotlib.pyplot as plt
import numpy as np

xpoints = np.array([1, 8])
ypoints = np.array([3, 10])

plt.plot(xpoints, ypoints)
plt.show()

Vrátí úsečku od [1;3] do [8;10]



x = np.array([5,7,8,7,2,17,2,9,4,11,12,9,6])
y = np.array([99,86,87,88,111,86,103,87,94,78,77,85,86])

plt.scatter(x, y)
plt.show()
Zobrazí body v grafu


 # Knihovna statistics

Obsahuje vzorce pro statistiku, umí počítat průměr, harmonický průměr, medián, modus, odchylku, rozptyl, atd.


 # Odkazy:
https://naucse.python.cz/lessons/intro/numpy/
https://www.root.cz/clanky/sympy-knihovna-pro-symbolicke-vypocty-zapsane-primo-v-pythonu/