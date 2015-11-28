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

[przed]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/przed.jpg
![przed]

## Praca z zadaniem

### Obciażenie podczas importu

#### Machina fizyczna

[import-fizy]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/fiz-obciazenie.jpg
![import-fizy]

#### Machina wirtualna

[import-wirt]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/wirt-import.jpg
![import-wirt]

#### Czas importu

[import-time]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/time-import.jpg
![import-time]

#### Liczba zaimportowanych rekordów

[import-count]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/img/count-import.jpg
![import-count]


## Geojson

[geo]: https://raw.githubusercontent.com/pbasiak/pbnosql/master/zad2/mapa.geojson
![geo]
