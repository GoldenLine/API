# Pobieranie Access Tokena

W OAuth2 “flow” autoryzacji został uproszczony do minimum i wymaga od klienta (aplikacji) zaledwie 4 kroków.

1. Klient wysyła żądanie do serwera OAuth - w celu późniejszego pobrania *kodu autoryzacyjnego* - na adres `http://goldenline.pl/oauth/v2/auth?client_id=APPLICATION_ID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE`, gdzie zostanie wyświetlony użytkownikowi formularz autoryzacji klienta (aplikacji).

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
            <td>Adres URL na który serwer OAuth przekieruje użytkownika po udanej autoryzacji klienta. REDIRECT_URI nie jest dowolny. Developer aplikacji musi z góry podać listę URL na które serwer OAuth może dokonywać przekierowań</td>
        </tr>
        <tr>
            <th>SCOPE</th>
            <td>Zestaw uprawnień, o które prosi klient (aplikacja), rozdzielonych spacjami.
            Na tą chwilę posiadamy 3 możliwe uprawnienia:
            * email - dostęp do adresu email użytkownika
            * full_profile - dostęp do “dodatkowych” danych użytkownika takich jak edukacja, doświadczenie zawodowe, etc
            * contacts - dostęp do kontaktów użytkownika</td>
        </tr>
    </table>

    Przykład:

    ```
    http://goldenline.pl/oauth/v2/auth?client_id=1_5fw8bn7dk084gkwkskc0o80wsw0wskcck08wc4gow080cwc0gw&redirect_uri=http%3A%2F%2Fwww.example.com&response_type=code&scope=email%20full_profile%20contacts
    ```

2. Po potwierdzeniu autoryzacji, przez użytkownika, serwer OAuth przekierowuje użytkownika na adres REDIRECT_URI wraz z kodem autoryzacyjnym, w “query stringu”, pod kluczem `code`.

    Przykład:

    ```
    http://example.com/?code=YWVm2E2MzY3MjZjODIyM2YzMjdMzM3YjBhNzg1YjBhMTVjMTE0NjgxMzBhOTM2YTJmN2VmOTIyNmUwMTI2Nw
    ```

    Kod autoryzacyjny jest ważny 30 sekund.

    Istnieje możliwość, że użytkownik odmówi autoryzacji klienta. Obsługę takiego przypadku opisaliśmy [tutaj] (/authorization/handling_declined_authorization.md).

3. Od teraz teraz klient jest w stanie uzyskać tzw `Access Token`, których później posłuży do autoryzowania ządań do API. Aby to zrobic klient musi wykonać żądanie do serwera (tym razem po stronie backendu!) na adres `http://goldenline.pl/oauth/v2/token?grant_type=authorization_code&code=AUTH_CODE&redirect_uri=REDIRECT_URI&client_id=CLIENT_ID&client_secret=CLIENT_SECRET`.

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
            <th>AUTH_CODE</th>
            <td>Kod autoryzacyjny uzyskany w kroku 2</td>
        </tr>
        <tr>
            <th>CLIENT_SECRET</th>
            <td>Sekret aplikacji</td>
        </tr>
    </table>

    Przykład:
    
    ```
    http://goldenline.pl/oauth/v2/token?grant_type=authorization_code&code=YWVmN2E2MzY3MjZjODIyM2YzMjdiMzM3YjBhNzg1YjBhMTVjMTE0NjgxMzBhOTM2YTJmN2VmOTIyNmUwMTI2Nw&redirect_uri=REDIRECT_URI&client_id=1_5fw8bn7dk084gkwkskc0o80wsw0wskcck08wc4gow080cwc0gw&client_secret=58ceu78y1joc0owk0wock40kgos0k48040skk4ksoc8g0840ww
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

    `Access Token` jest ważny przez 60 minut. Istnieje możliwość odświeżenia wygasłego `Access Tokena` wykorzystując `Refresh Token`, który został przesłany razem razem z nim. Przeczytać o tym można [w osobnej sekcji tej dokumentacji] (/autorization/refreshing_access_token.md).

4. Posiadając `Access Token`, klient może już zacząć komunikować się z API. Aby wykonać uwierzytelnione zapytanie do API należy wywołać adres zasobu API wraz z dodatkowym nagłówkiem `Authorization: Bearer ACCESS_TOKEN`.

    Przykład:

    ```
    GET /api/users/474258
    Host: www.goldenline.pl
    Accept: applicaton/hal+json
    Authorization: Bearer M2Y5YTcxYjAxYzY4MzlkZDcwY2YwMDIwZTRhY2UyZDBmNjkzYjgyMDhkYzI4YzMxOTcyMjBkODcwNzQ1YmRiMw
    ```
