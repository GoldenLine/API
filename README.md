# Dokumentacja GoldenLine.pl API

Goldenline API jest to RESTowy interfejs pozwalający na interakcję z serwisem poprzez samodokumentujący się zestaw , linkujących między sobą, **zasobów** i **metod** pozwalających na ich modyfikację.

Jako warstwę prezentacji wybraliśmy standard HAL, a autoryzację żądań do API powierzyliśmy mechanizmom OAuth2.

## Autoryzacja

* [Pobieranie Access Tokena] (/authorization/retrieving_access_token.md)
* [Odświeżanie Access Tokena] (/authorization/refreshing_access_token.md)
* [Obsługa braku autoryzacji klienta] (/authorization/handling_declined_authorization.md)

## Dostepne zasoby API

Poniżej prezentujemy listę zasobów dostępnych w GoldenLine API. API jest ciągle rozwijane, więc ta lista może się zmienić.

Prace nad zasobami, których adresy URI są ~~przekreślone~~, nie zostały jeszcze ukończone.

* `GET /api`
* `GET /api/users/{id}` - Dane użytkownika o identyfikatorze {id}
* `GET /api/me` - Dane obecnie autoryzowanego użytkownika
* `GET /api/users/{id}/cv` - Plik PDF z CV użytkownika o identyfikatorze {id}
* `GET /api/users/{id}/contacts` - Stronnicowana lista kontaktów użytkownika o identyfikatorze {id}
* `GET /api/firms` - Stronnicowana lista firm
* `GET /api/firms/{id}` - Dane firmy o identyfikatorze {id}
* `GET /api/firms/{id}/employees` - Stronnicowana lista pracowników firmy o identyfikatorze {id}
* `GET /api/job_ads` - Stronnicowana lista ofert pracy
* `GET /api/job_ads/{id}` - Dane oferty pracy o identyfikatorze {id}

## Narzędzia

Z myślą o developerach stworzyliśmy aplikację pozwalającą w przyjemny sposób eksplorować nasze API. Aplikacja znajduje się pod adresem [https://www.goldenline.pl/aplikacja/hal-browser] (https://www.goldenline.pl/aplikacja/hal-browser)

## Załączniki

* [Specyfikacja standardu HAL] (http://tools.ietf.org/html/draft-kelly-json-hal-05)
* [Specyfikacja standardu OAuth2] (http://tools.ietf.org/html/rfc6749)
