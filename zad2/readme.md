# Zadanie 2 NOSQL

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

Zapytanie
db.postal.count();

##### PostgreSQL
COUNT(SELECT * FROM zipcodes);

#### Zapytania i prezentacja danych
(soon)


## Geojson

[GeoJSON]: https://github.com/pbasiak/pbnosql/blob/master/zad2/mapa.geojson
![GeoJSON]
