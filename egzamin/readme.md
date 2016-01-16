# Aggregation Pipeline - Postal Codes from all Countries
Agregacje - Kody pocztowe z całego świata

## Informacje o zbiorze danych

| Nazwa         | Wartość       |
| ------------- | ------------- |
| Źródło        | [PostalCodes TXT](http://www.geonames.org/postal-codes/) |
| Format      | CSV      |
| Ilość rekordów | 1119268      |

### Struktura rekordu

```
country code      : iso country code, 2 characters
postal code       : varchar(20)
place name        : varchar(180)
admin name1       : 1. order subdivision (state) varchar(100)
admin code1       : 1. order subdivision (state) varchar(20)
admin name2       : 2. order subdivision (county/province) varchar(100)
admin code2       : 2. order subdivision (county/province) varchar(20)
admin name3       : 3. order subdivision (community) varchar(100)
admin code3       : 3. order subdivision (community) varchar(20)
latitude          : estimated latitude (wgs84)
longitude         : estimated longitude (wgs84)
accuracy          : accuracy of lat/lng from 1=estimated to 6=centroid
```


## Nodejs - przykłady
Przykładowe skrypty do sprawdzenia poprawności działania


#### Hello http
Połączenie nodejs z przeglądarką oraz wyświetlenie napisu
```javascript
var http = require('http');

var server = http.createServer(function(req, res) {
  res.writeHead(200);
  res.end('Hello Http');
});
server.listen(8080);
```

#### Mongodb
Testowe połączenie z Mongodb
```javascript
// Importowanie sterowników
var mongodb = require('mongodb');

var MongoClient = mongodb.MongoClient;

// Adres połączenia
var url = 'mongodb://localhost:27017/allc';

// Metoda connect łączy się z mongodb
MongoClient.connect(url, function (err, db) {
  if (err) {
    console.log('Błąd połączenia z bazą. Błąd:', err);
  } else {
    // Sukces!
    console.log('Połączono z ', url);

    // Zamknięcie połączenia
    db.close();
  }
});
```

#### Agregacje i inne skrypty
Przykładowy skrypt, który możemy modyfikować
```javascript
var mongodb = require('mongodb');
var MongoClient = mongodb.MongoClient;
var assert = require('assert');
var url = 'mongodb://localhost:27017/allc';

var aggregateCountries = function(db, callback) {
   db.collection('postal').aggregate(
     [
       // TUTAJ WSTAWIAMY TREŚĆ AGREGACJI
     ]
   ).toArray(function(err, result) {
     assert.equal(err, null);
     console.log(result);
     callback(result);
   });
};

MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  aggregateCountries(db, function() {
      db.close();
  });
});
```

## Python - połączenie oraz agregacje

```python
// Importujemy bibliotekę
from pymongo import MongoClient

// Instancja dla bazy
client = MongoClient()
db = client.allc

// Agregacja za pomocą kursora
cursor = db.restaurants.aggregate(
    [
        // TUTAJ WSTAWIAMY TREŚĆ AGREGACJI
    ]
)

// Drukujemy wynik za pomocą pętli
for document in cursor:
    print(document)
```

## Agregacje (Państwa)

Agregacja:
```
db.collection('postal').aggregate(
     [
       { $match: { "Country": "US" } },
       { $group: {"_id":"$Country" , "suma":{$sum:1}} }
     ]
   )
```

Wynik:
```
[ { _id: 'US', suma: 276 } ]
```

#### Zestawienie danych

| Państwo         | Liczba kodów       |
| ------------- | ------------- |
| USA        | 43633 |
| Polska      | 21980      |
| Ilość rekordów | 16484      |


## Agregacje (stany w USA)

Agregacja:
```
db.collection('postal').aggregate(
     [
       { $match: { "Country": "US", "an1": "Alaska" } },
       { $group: {"_id":"$Country" , "suma":{$sum:1}} }
     ]
   )
```

Dodajemy kolejny atrybut $match z nazwą szukanego stanu

Wynik:
```
[ { _id: 'Alaska', suma: 276 } ]
```

#### Zestawienie danych

| Stan USA         | Liczba kodów       |
| ------------- | ------------- |
| Alaska        | 276 |
| Florida      | 1578      |
| Kentucky | 1160      |
| New York | 2308      |
| Ohio | 1488      |

## Agregacje (województwa w Polsce)

Agregacja
```
db.collection('postal').aggregate(
     [
       { $match: { "Country": "PL", "an1": "Pomorskie" } },
       { $group: {"_id":"$an1" ,"suma":{$sum:1}} }
     ]
   )
```

Wynik
```
[ { _id: 'Pomorskie', suma: 1625 } ]
```

#### Zestawienie danych

| Województwo        | Liczba kodów       |
| ------------- | ------------- |
| Pomorskie        | 276 |
| Świętokrzyskie      | 643      |
| Małopolskie | 1549      |
| Mazowieckie | 4657      |
| Podlaskie | 823      |