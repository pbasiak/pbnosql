# Zadanie 2 NOSQL

# Spis tresci
1. [Opis](#opis)
  * [Liczba rekordów](#liczba-rekordow)
  * [Rozmiar pliku](#rozmiar-pliku)
  * [Format pliku](#format-pliku)
2. [Informacje o sprzecie i oprogramowaniu](#informacje-o-sprzecie-i-oprogramowaniu)
3. [Praca systemu bez obciazen](#praca-systemu-bez-obciazen)
  * [Maszyna fizyczna](#maszyna-fizyczna)
  * [Maszyna wirtualna](#maszyna-wirtualna)
4. [Praca z zadaniem](#praca-z-zadaniem)
  * [Obciazenie podczas importu](#obciazenie-podczas-importu)

## Opis

Rekordy pochodza ze strony http://www.geonames.org/postal-codes/
Znajduja sie tam wszystkie kody pocztowe na calym swiecie.

### Liczba rekordow
1119268

### Rozmiar pliku
111 MB

### Format pliku
CSV

### Wnioski
Czas importu w porownaniu do liczby rekordow jest calkiem przyjemny.
Ogolnie jest przeswiadczenie, że procesor jest najważniesjszy poczas takich operacji.
W moim przypadku (nie tylko podczas importu do bazy danych) najgorszym czynnikiem byl dysk. Szybkosć dysku niestety nie pomagala. Praca na komputerze nie byla mozliwa podczas operacji importu czy konwersji pliku do csv. Widac bylo znaczne skoki pracy dysku. Wniosek jest taki, że do to takich operacji powinno sie zwrocic uwage przede wszystkim na dysk. Dysk powinien być szybki czyli najlepiej wybrać dysk SSD szczególnie podczas pracy na wirtualnej maszynie kiedy system musi dzielić sprzet pomiedzy maszyny.

## Informacje o sprzecie oraz oprogramowaniu

Typ  | Sprzęt | Maszyna wirtualna
------------- | ------------- | -------------
Rodzaj | Laptop ASUS N56VZ | =
Procesor | Intel Core i5 3210M @ 2.50GHz | =
Liczba rdzeni  | 2 | 2
Dysk | HDD 5200 (1TB) | 180GB
RAM | 2 x 4GB DDR3 | 5067MB
Karta graficzna | NVIDIA GeForce GT 650M 2GB | =
System | Windows 8.1 64-bit | Ubuntu 15.10 64-bit
Wirtualizacja | - | Oracle VM VirtualBox v4.3.32 r103443
Mongodb | - | 2.6.10
PostgreSQL | - | 9.4.5

## Praca systemu bez obciażeń

### Maszyna fizyczna
[przed]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/przed.jpg
![przed]

### Maszyna wirtualna
[przed-virtual]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/bezvirtual.png
![przed-virtual]

## Praca z zadaniem

### Obciażenie podczas importu

#### Maszyna fizyczna

##### MongoDB
[import-fizy]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/fiz-obciazenie.jpg
![import-fizy]
##### PosgreSQL
[p-import-fizy]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/posgreimport.png
![p-import-fizy]

#### Maszyna wirtualna

##### MongoDB
[import-wirt]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/wirt-import.jpg
![import-wirt]

##### PostgreSQL
[p-import-wirt]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/posgreimport-virtual.png
![p-import-wirt]

#### Czas importu

##### MongoDB
[import-time]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/time-import.jpg
![import-time]

##### PostgreSQL
8037ms

#### Liczba zaimportowanych rekordów

##### MongoDB
[import-count]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/count-import.jpg
![import-count]

##### PostgreSQL

#### Zapytania i prezentacja danych

##### MongoDB
###### Liczba rekordów
```mongodb
db.postal.count();
```

###### Liczba rekordów z dana wartoscia (np. Gdańsk)
```mongodb
db.postal.find( Name: "Gdańsk" ).count();
db.postal.find( Country: "PL" ).count();
```

##### PostgreSQL
###### Liczba rekordów
```Postgres
SELECT COUNT(*) FROM zipcodes;
```

###### Liczba rekordów z dana wartoscia
```Postgres
SELECT COUNT(postal) FROM zipcodes WHERE name = 'Gdańsk';
```
###### Wypisanie rekordów z dana wartoscia
```Postgres
SELECT name FROM zipcodes WHERE country = 'PL';
```

###### 5 państw z liczba kodow pocztowych wieksza niż 51000
```Postgres
SELECT count(*), country FROM zipcodes GROUP BY country HAVING COUNT(*) > 51000  LIMIT 5; 
```


##### Prezentacja danych

###### Miasta w Polsce oraz ich liczba kodów pocztowych
Miasto  | Liczba kodów pocztowych
------------- | -------------
Sczecin | 1190
Koszalin | 545
Gdańsk | 661
Olsztyn | 687
Bialystok | 693
Zielona Góra | 373
Poznań | 1312
Bydgoszcz | 720
Warszawa | 4166
Łódź | 1701
Lublin | 599
Kielce | 519
Wroclaw | 1307
Opole | 492
Katowice | 671
Kraków | 1131
Rzeszów | 166

###### Wybrane państwa oraz liczba ich kodów
Państwo | Liczba kodów pocztowych
------------- | -------------
Polska | 21980
USA | 43633
Niemcy | 16484

###### Województwo Pomorskie
Województwo | Liczba kodów pocztowych
------------- | -------------
Pomorskie | 1625

###### 5 państw posiadajacych najwiecej kodów pocztowych
Państwo | Skrót | Liczba
------------- | ------------- | -------------
Portugalia | PT | 204001
Indie | IN | 154809
Meksyk | MX | 144513
Japonia | JP | 94388
Francja | FR | 51131

