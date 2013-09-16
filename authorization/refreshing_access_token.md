# Odświerzanie Access Tokena

Jak pisaliśmy w sekcji ["Pobieranie Access Tokena"] (/authorization/retrieving_access_token.md) `Access Token` jest ważny tylko 60 min. Istnieje jednak możliwość jego odświeżenia bez ponownego przepuszczania użytkownika przez pełen proces autoryzacji. Służy do tego `Refresh Token` przesłany razem z `Access Tokenem`.

Aby odświeżyć `Access Token` klient (aplikacja) musi wykonać do serwera Oauth żadanie `http://golden2.localhost/app_dev.php/oauth/v2/token?grant_type=refresh_token&refresh_token=REFRESH_TOKEN&redirect_uri=REDIRECT_URI&client_id=CLIENT_ID&client_secret=CLIENT_SECRET`

<table>
        <tr>
            <th>Zmienna</th>
            <th>Opis</th>
        </tr>
        <tr>
            <th>APPLICATION_ID</th>
            <td>Identyfikator aplikacji</td>
        </tr>
        <tr>
            <th>REDIRECT_URI</th>
            <td></td>
        </tr>
        <tr>
            <th>REFRESH_TOKEN</th>
            <td>`Refresh Token` uzyskany w 3-cim kroku w sekcji ["Pobieranie Access Tokena"] (/authorization/retrieving_access_token.md)</td>
        </tr>
        <tr>
            <th>CLIENT_SECRET</th>
            <td>Sekret aplikacji</td>
        </tr>
</table>

Przykład:

```
http://golden2.localhost/app_dev.php/oauth/v2/token?grant_type=refresh_token&refresh_token=NjQ1MzViYmM2YDSFG5455rwOTMxNzY5OWEyOGRiMDhlNzJhZGFkNDFmYjBkNmYzMmI0YjJjMTFhNGI3MTlkZA&redirect_uri=http://www.example.com&client_id=1_5fw8bn7dk084ffgE4kc0o80wsw0wskcck08wc4gow080cwc0gw&client_secret=58ceu78y1joc0owk0wdgr8040skk4ksoc8g0840ww
```

W odpowiedzi serwer OAuth wyślę tablicę w formacie JSONa z którego w prosty sposób klient może wyłuskać nowy `Access Token` znajdujący sie pod kluczen `access_token`.

```json
    {
    "access_token": "M2Y5YTcxYjAxYzY4MzlkZDcwY2YwMDIwZTRhY2UyZDBmNjkzYjgyMDhkYzI4YzMxOTcyMjBkODcwNzQ1YmRiMw",
    "expires_in": 3600,
    "token_type": "bearer",
    "scope": "email full_profile contacts",
    "refresh_token": "NjM3ZmQ5NTdmODg1OWZiZGJiMzQ0NzYwMWM2MjM2MWM4YzY4MDY1YmUwYmQ0YjY0NzhhYzQ0ODcwZTJlNzYxZA"
    }
```

<table>
        <tr>
            <th>Zmienna</th>
            <th>Opis</th>
        </tr>
        <tr>
            <th>access_token</th>
            <td></td>
        </tr>
        <tr>
            <th>expires_in</th>
            <td>Czas jaki musi upłynąć nim `Access Token` straci ważnośc w sekundach</td>
        </tr>
        <tr>
            <th>token_type</th>
            <td>Typ tokenu</td>
        </tr>
        <tr>
            <th>scope</th>
            <td>Zestaw uprawnień bliżej omówiony w kroku 1-wszym</td>
        </tr>
        <tr>
            <th>refresh_token</th>
            <td>Token, dzięki któremu klient może pobrać nowy, aktywny `Access Token`, gdy oryginalny straci ważność. Więcej informacji można znależć w sekcji ["Odświerzanie Access Tokena"] (/authorization/refreshing_access_token.md)</td>
        </tr>
</table>
