# Obsługa braku autoryzacji

Jeżeli użytkownik pomiędzy 1-wszym a 2-gim krokiem autoryzacji klienta (opisanej w sekcji ["Pobieranie Access Tokena"] (/authorization/retrieving_access_token.md)) odmówi autoryzacji to klient ma możliwość obsłużenia takiej sytuacji. Serwer OAuth przekierowuje wtedy użytkownika na adres podany w zmiennej REDIRECT_URI wraz z dołaczoymi do "query string" zmiennymi `error` i `error_description`.

Przykład:

```
http://www.example.com/?error=access_denied&error_description=The+user+denied+access+to+your+application
```