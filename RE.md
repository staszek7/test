# AppLeka



## Instalacja

Używając polecenia [pip](https://pip.pypa.io/en/stable/) należy zainstalować potrzebne pakiety.

```bash
pip install -r requirements.txt
```

## Uruchomienie serwera

Serwer można uruchomić poleceniem:
```bash
python -m flask run
```
na [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

## Rejestracja

By zarejestrować nowe konto użytkownika  należy wykonać żądanie POST na [http://127.0.0.1:5000/register](http://127.0.0.1:5000/register).\
Body:
```json
{
    email : "user1@abcd.e",
    password : "haslo" 
}
```
Jeżeli podany email został już wcześniej użyty do rejestracji, zwrócony zostanie JSON:
```JSON
{
   error : "Email is already being used"
}
```
Jeżeli rejestracja przebiegła poprawnie, zwrócony zostanie JSON:
```javascript
{
   message : "User user1@abcd.e registered"
}
``` 
## Logowanie

W celu zapewnienia bezpieczeństwa użytkownika dostęp do prywatnych danych wymaga autoryzacji na podstawie tokena generowanego podczas logowania. Żeby uzyskać token należy wykonać żądanie POST na [http://127.0.0.1:5000/login](http://127.0.0.1:5000/login).\
Body:
```JSON
{
    email : "user1@abcd.e",
    password : "haslo"
}
```
Jeżeli dane logowania są poprawne wówczas zostanie wygenerowany token `USER_TOKEN`:
```
{
   token : "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoid29qdGVrIiwiZXhwIjoxNTkxMDYwNTUzfQ.yt522FVw-f0DKW4o-jEQaYRJiyWYrxnW9lfBmU-OPCQ"
}
```
Jeżeli użytkownik o podanym emailu nie został zrejestrowany, zwrócony zostanie JSON: 
```JSON
{
   error : "Wrong email"
}
```
z kodem 404.\
Jeżeli podany email jest zarejestrowany w bazie, natomiast podane hasło jest błędne, zwrócony zostanie JSON: 
```JSON
{
   error : "Wrong password"
}
```
z kodem 404.

## Możliwe operacje

W poniższych żądaniach GET i POST pole `Authorization` w nagłówku wiadomości ma mieć wartość:
```
Bearer USER_TOKEN
```


## Dodanie schowka

Żeby dodać nowy schowek na leki, wraz z jego opisem, należy wykonać żądanie POST na [http://127.0.0.1:5000/add/cupboard](http://127.0.0.1:5000/add/cupboard).\
Body:

```JSON
{
    name : "szafka",
    description : "Szafka w łazience"
}
```
Zwrócona zostanie wiadomość:
```JSON
"Cupboard added successfully"
```
z kodem 200.
## Dodanie leku

Żeby dodać nowy lek, wraz z jego opisem, należy wykonać żądanie POST na [http://127.0.0.1:5000/add/drug](http://127.0.0.1:5000/add/drug).\
Body:
```JSON
{
    name : "Apap, 500mg.",
    active_substances : "Paracetamol",
    quantity : "50 szt.",
    expiration_date : "24/04/2024",
    cupboard_id : "5ed57e0c4687e30a314b9cc4",
    description : "Opis leku"
}
```
Zwrócona zostanie wiadomość:
```JSON
"Drug added successfully"
```
z kodem 200.

## Pobranie informacji o wszystkich schowkach

Żeby pobrać informacje o wszystkich schowkach, należy wykonać żądanie GET na [http://127.0.0.1:5000/cupboards](http://127.0.0.1:5000/cupboards).\
Zostanie zwrócony JSON:
```JSON
[   
	{
		_id : {
        	$oid : "5ed57e0c4687e30a314b9cc4"
		},
      	name : "Zielona szafka",
      	description : "W kuchni",
      	user : "user1@abcd.e"
	},
   	{
		_id : {
        	$oid : "5ed57e5b4687e30a314b9cc5"
		},
    	name : "Apteczka",
      	description : "W łazience",
      	user : "user1@abcd.e"
	}
]
```


## Pobranie informacji o wszystkich lekach

Żeby pobrać informacje o wszystkich lekach, należy wykonać żądanie GET na [http://127.0.0.1:5000/drugs](http://127.0.0.1:5000/drugs).\
Zostanie zwrócony JSON:
```JSON
[   
	{
		_id : {
        	$oid : "5ed594930791087bc6da8406"
      	},
		name : "Apap, 500mg.",
      	active_substances : "Paracetamol",
      	quantity : "50 szt.",
      	expiration_date : "24/04/2024",
      	cupboard_id : "5ed57e0c4687e30a314b9cc4",
      	description : "Opis leku",
      	user : "user1@abcd.e"
   }
]
```

## Ustawienie lub nadpisanie wiadomości ICE

Żeby ustawić lub nadpisać wiadomość ICE, należy wykonać żądanie POST na [http://127.0.0.1:5000/setICEmessage](http://127.0.0.1:5000/setICEmessage).\
Body:
```JSON
{
    message : "test message2"
}
```
Zostanie zwrócony JSON zawierający `MESSAGE_HASH`:
```JSON
{
    message_hash : "d2d47f9ea4a41e725a4a026010b946e3415aafd72aa05a9c0b4849b742b2c2f9"
}
```
z kodem 200.\
Otrzymany `MESSAGE_HASH` pozwala na wygenerowanie kodu `QR`, który umożliwia osobom trzecim przeczytać aktualną wiadomość ICE.

## Pobranie aktualnej wiadomości ICE przez użytkownika

Żeby pobrać aktualną wiadomość ICE, należy wykonać żądanie GET na [http://127.0.0.1:5000/getICEmessage](http://127.0.0.1:5000/getICEmessage).\
Zostanie zwrócony JSON:
```JSON
{
    message : "test message2",
    message_hash : "d2d47f9ea4a41e725a4a026010b946e3415aafd72aa05a9c0b4849b742b2c2f9"
}
```
## Zmiana hasha wiadomości ICE

Żeby zmienić hash wiadomość ICE, należy wykonać żądanie POST na [http://127.0.0.1:5000/revokeAccessToken](http://127.0.0.1:5000/revokeAccessToken).\
Zostanie zwrócony JSON zawierający nowy `MESSAGE_HASH`:
```JSON 
{
    message_hash : "dhs63g448jsgd8310op3vbchd9801saft52j4tekdj74h3g45dgfwterkjirjw"
}
```
z kodem 200.\
Jeżeli użytkownik nie ustawił wcześniej żadnej wiadomości ICE, wówczas zwrócony zostanie JSON:
```JSON
{
   error : "ICE message not found"
}
```
z kodem 404.

##### W poniższych żądaniach GET autoryzacja w nagłówku nie jest wymagana.

## Pobieranie informacji o leku na podstawie kodu kreskowego GTIN

Żeby pobrać informacje o leku na podstawie kodu kreskowego GTIN, należy wykonać żądanie GET na [http://127.0.0.1:5000/scan/GTIN](http://127.0.0.1:5000/scan/).\
Zostanie zwrócony JSON:
```JSON
{
   status : "Produkt dopuszczony do obrotu.",
   product_name : "APAP",
   product_producer : "US PHARMACIA",
   form : "TABL. POWL.",
   dose : "0,5 G",
   package : "6 TABL.",
   product_international_name : "PARACETAMOL",
   substances : "",
   product_synonym : "PARACETAMOLUM O 1-D 0,5 G"
}
```
z kodem 200.\
Jeżeli podany kod GTIN nie jest dostępny w wykorzystywanej bazie danych, wówczas zwrcany jest JSON:
```JSON
{
   error : "Wrong GTIN"
}
```
z kodem 404.\
Jeżeli struktura bazy ulegnie zmianie, zostanie zwrócony JSON:
```JSON
{
   error : "Drugs database has changed its structure. Change drug_info.py"
}
```
z kodem 410. Należy wówczas odpowiednio edytować plik `drug_info.py`.\
Jeżeli połączenie z zewnętrznym serwisem się nie powiedzie, zostanie zwrócony JSON:
```JSON
{
   error : "Drugs database error"
}
```
z otrzymanym od serwisu kodem. Należy wówczas zmienić link do serwisu w pliku `get_info.py` oraz wymienić plik `drug_info.py`.

## Autouzupełnianie nazwy leku

Żeby uzyskać nazwy leków o wspólnym prefiksie `DRUG_PREFIX`, dostępnych w wykorzystywanej bazie, należy wykonać żądanie GET na [http://127.0.0.1:5000/search/DRUG_PREFIX](http://127.0.0.1:5000/search/).\
Zostanie zwrócony JSON:
```JSON
{
   Apap (tabletki) : 488,
   Apap (zawiesina i krople) : 489,
   Apap Direct : 490,
   Apap Extra : 491,
   Apap Junior : 492,
   Apap Noc : 493,
   Apap Przeziębienie FAST (Apap C Plus) : 494
}
```
z kodem 200.\
Jeżeli struktura bazy ulegnie zmianie, zostanie zwrócony JSON:
```JSON
{
   error : "Leaflet database has changed its structure. Change leaflet_info.py"
}
```
z kodem 410. Należy wówczas odpowiednio edytować plik `leaflet_info.py`.\
Jeżeli połączenie z zewnętrznym serwisem się nie powiedzie, zostanie zwrócony JSON:
```JSON
{
   error : "Leaflet database error"
}
```
z otrzymanym od serwisu kodem. Należy wówczas zmienić link do serwisu w pliku `get_info.py` oraz wymienić plik `leaflet_info.py`.


## Pobieranie ulotki

Żeby pobrać ulotkę leku o id `DRUG_ID`, uzyskanego na podstawie autouzupełnienia nazwy leku, należy wykonać żądanie GET na [http://127.0.0.1:5000/leaflet/DRUG_ID](http://127.0.0.1:5000/leaflet/).\
Zostanie zwrócony JSON:
```JSON
{
   category : [
      "Ośrodkowy układ nerwowy"
   ],
   substances : [
      "paracetamol"
   ],
   forms : [
      "tabletki"
   ],
   specializations : [
      "Medycyna rodzinna"
   ],
   treatments : [
      "przeciwgorączkowe",
      "przeciwbólowe"
   ],
   tags : [
      "przeziębienie",
      "paracetamol",
      "środek przeciwbólowy"
   ],
   recommendations : "Bóle różnego pochodzenia",
   dosage : "Doustnie. Dorośli: 1-2 tabletki 3-4 razy na dobę",
   possible_side_effects : "Mogą wystąpić reakcje nadwrażliwości (pokrzywka)",
   tell_your_doctor : "W okresie ciąży należy skonsultować się z lekarzem."
}
```
z kodem 200.\
Jeżeli struktura bazy ulegnie zmianie, zostanie zwrócony JSON:
```JSON
{
   error : "Leaflet database has changed its structure. Change leaflet_info.py"
}
```
z kodem 410. Należy wówczas odpowiednio edytować plik `leaflet_info.py`.\
Jeżeli połączenie z zewnętrznym serwisem się nie powiedzie, zostanie zwrócony JSON:
```JSON
{
   error : "Leaflet database error"
}
```
z otrzymanym od serwisu kodem. Należy wówczas zmienić link do serwisu w pliku `get_info.py` oraz wymienić plik `leaflet_info.py`.


## Pobranie aktualnej wiadomości ICE przez osobę trzecią

Żeby pobrać aktualną wiadomość ICE posiadając `MESSAGE_HASH`, należy wykonać żądanie GET na [http://127.0.0.1:5000/getICEmessage/MESSAGE_HASH](http://127.0.0.1:5000/getICEmessage/).\
Zostanie zwrócony JSON:
```JSON
{
    message : "Wiadomość ICE dla osoby trzeciej"
}
```
Jeżeli wiadomość o podanym hashu `MESSAGE_HASH` nie istnieje, wówczas zwracany jest JSON:
```JSON
{
   error : "ICE message not found"
}
```
z kodem 404.


## License
[MIT](https://choosealicense.com/licenses/mit/)
