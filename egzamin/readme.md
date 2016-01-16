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

## Agregacja (5 stanów z największą ilością kodów)

Agregacja:
```
 db.collection('postal').aggregate(
     [
       { $match : { "Country" : "US" } },
       { $group : { _id : "$an1", "count" : { $sum: 1 } } },
       { $sort : { "count" : -1 } },
       { $limit : 5 }
     ]
   )
```

Wynik:
```
[ { _id: 'California', count: 2796 },
  { _id: 'Texas', count: 2743 },
  { _id: 'New York', count: 2308 },
  { _id: 'Pennsylvania', count: 2253 },
  { _id: 'Illinois', count: 1646 } ]
```

#### Zestawienie danych

| Stan USA         | Liczba kodów       |
| ------------- | ------------- |
| California        | 2743 |
| Texas      | 2743      |
| New York | 2308      |
| Pennsylvania | 2253      |
| Illinois | 1646      |

## Agregacja (5 stanów z najmniejszą ilością kodów)

Agregacja:
```
db.collection('postal').aggregate(
     [
       { $match : { "Country" : "US" } },
       { $group : { _id : "$an1", "count" : { $sum: 1 } } },
       { $sort : { "count" : 1 } },
       { $limit : 6 }
     ]
   )
```

Wynik:
```
[ { _id: '', count: 19 },
  { _id: 'Rhode Island', count: 91 },
  { _id: 'Delaware', count: 99 },
  { _id: 'Hawaii', count: 143 },
  { _id: 'Wyoming', count: 204 },
  { _id: 'Nevada', count: 263 } ]
```

#### Zestawienie danych

| Stan USA         | Liczba kodów       |
| ------------- | ------------- |
| Rhode Island        | 91 |
| Delaware      | 99      |
| Hawaii | 143      |
| Wyoming | 204      |
| Nevada | 263      |

## Agregacja (województwa w Polsce)

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


## Agregacja (5 województw z największą ilością kodów)

Agregacja:
```
db.collection('postal').aggregate(
     [
       { $match : { "Country" : "PL" } },
       { $group : { _id : "$an1", "count" : { $sum: 1 } } },
       { $sort : { "count" : -1 } },
       { $limit : 5 }
     ]
   )
```

Wynik:
```
[ { _id: 'Mazowieckie', count: 4657 },
  { _id: 'Zachodniopomorskie', count: 1964 },
  { _id: 'Łódzkie', count: 1936 },
  { _id: 'Pomorskie', count: 1625 },
  { _id: 'Dolnośląskie', count: 1623 } ]
```

#### Zestawienie danych

| Województwo        | Liczba kodów       |
| ------------- | ------------- |
| Mazowieckie        | 4657 |
| Zachodniopomorskie      | 1964      |
| Łódzkie | 1936      |
| Pomorskie | 1625      |
| Dolnośląskie | 1623      |


## Agregacja (5 województw z najmniejszą ilością kodów)

Agregacja:
```
db.collection('postal').aggregate(
     [
       { $match : { "Country" : "PL" } },
       { $group : { _id : "$an1", "count" : { $sum: 1 } } },
       { $sort : { "count" : 1 } },
       { $limit : 6 }
     ]
   )
```

Wynik:
```
[ { _id: '', count: 1 },
  { _id: 'Lubuskie', count: 480 },
  { _id: 'Podkarpackie', count: 511 },
  { _id: 'Świętokrzyskie', count: 643 },
  { _id: 'Opolskie', count: 651 },
  { _id: 'Podlaskie', count: 823 } ]
```

#### Zestawienie danych

| Województwo        | Liczba kodów       |
| ------------- | ------------- |
| Lubuskie        | 480 |
| Podkarpackie      | 511      |
| Świętokrzyskie | 643      |
| Opolskie | 651      |
| Podlaskie | 823      |