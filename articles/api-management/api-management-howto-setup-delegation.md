<properties 
    pageTitle="Hogyan regisztrációs és a termék előfizetés felhasználói meghatalmazás" 
    description="Megtudhatja, hogy miként delegálása felhasználói regisztrációs és a termék előfizetés Azure API-kezelés a harmadik félre." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Hogyan regisztrációs és a termék előfizetés felhasználói meghatalmazás

Meghatalmazás lehetővé teszi a meglévő webhelye használatát a fejlesztői bejelentkezési – a/sign ellenőrzése és a termékek, nem pedig a a beépített funkció használata a developer Portal segítségével az előfizetés kezeléséhez. A webhely címét a felhasználói adatok saját, és hajtsa végre az alábbi lépéseket az érvényesítési egyéni módon szolgáltatás lehetővé teszi.

## <a name="delegate-signin-up"> </a>Delegálása developer bejelentkezési és a regisztrációs

A delegálni developer bejelentkezési és a regisztrációs speciális meghatalmazás végpont létrehozása a webhelyen, amely végpontként az API-kezelés developer Portal által kezdeményezett kérelmet bejegyzés pontos szüksége lesz a meglévő webhelyét.

A végleges munkafolyamat a következők:

1. A bejelentkezési vagy regisztráció hivatkozására a API-kezelés developer Portal segítségével a fejlesztői kattintással
2. Böngésző rendszer átirányítja a meghatalmazás végpont
3. Meghatalmazás végpont cserébe átirányítja, vagy hogyan jeleníti meg a felhasználót a bejelentkezési vagy előfizetési kéri felhasználói felület
4. A sikeres a felhasználónak van átirányítva a API kezelése developer portal lapon ezek a lépések


A kezdéshez nézzük első beállítási API kezelési és irányítja kéri a meghatalmazás végpont keresztül. Az API kezelése a publisher-portálon kattintson a **Biztonság** , és válassza a a **Meghatalmazás** fülre. "Meghatalmazott bejelentkezési és -előfizetési" engedélyezése jelölőnégyzetre.

![Meghatalmazás lap][api-management-delegation-signin-up]

* Döntse el, mit a speciális meghatalmazás végpont URL-címe lesz és **Meghatalmazás végpont URL-cím** mezőjében adjon meg. 

* Belül a **Meghatalmazás hitelesítési kulcs** mezőben adja meg a titkos meg annak érdekében, hogy a kérelem valóban érkező Azure API-kezelés az ellenőrzéshez megadott aláírás kiszámításához használt. Választhatja, hogy az API Managemnet véletlenszerűen meg a kulcs generálása **létrehozása** gombra.

Most a **Meghatalmazás végpont**létrehozásához szükséges. Számos műveletek elvégzéséhez van:

1. Kéri, a következő formátumban:

    > *{URL-cím a forrás lap} http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= & só = {karakterlánc} & aláírás = {karakterlánc}*

    A lekérdezés paraméterei a bejelentkezés / regisztráció eset:
    - **a művelet**: azonosítja meghatalmazás típusának kérhetik azt - ebben az esetben **– a bejelentkezési** csak lehet
    - **returnUrl**: a lapot, ahol a felhasználó kattint, a bejelentkezési és -előfizetési hivatkozást URL-címe
    - **só**: biztonsági kivonat számítások használható különleges só karakterlánc
    - **aláírás**: a saját összehasonlító használandó kiszámított biztonsági kivonat kiszámított kivonat

2. Ellenőrizze, hogy a kérelem érkezik Azure API-kezelés (nem kötelező, de biztonsági erősen ajánlott)

    * Egy karakterlánc **returnUrl** és **só** lekérdezésparaméter ([például kód alább]) alapján HMAC-SHA512 kivonat kiszámítása:
        > HMAC (**só** "\n" + **returnUrl**)
         
    * Összehasonlíthatja a fenti számított kivonat az **aláírás** lekérdezési paraméter értékét. A két tiltva felel meg, lépjen tovább a következő lépésben, egyébként utasítania.

2. Ellenőrizze, hogy egy kérelemre bejelentkezési/bejelentkezési felfelé Igen: a **művelet** lekérdezési paraméter értéke "**bejelentkezik**" lesz.

3. A felhasználó bemutató a felhasználói felület bejelentkezési vagy regisztráció

4. Ha a felhasználó aláírási felfelé megfelelő fiók létrehozása őket az API-kezelés van. [Felhasználó létrehozása] a API-kezelés REST API-val. Ha ezzel a módszerrel arról, hogy ugyanannak van a felhasználó tárolóban vagy egy Azonosítót, amely azt segítségével nyomon követheti a beállítva a felhasználói Azonosítójával.

5. Amikor a felhasználó sikerült hitelesíteni:

    * [egyetlen – bejelentkezés (SSO) jogkivonat kérése] az API-kezelés REST API keresztül

    * returnUrl lekérdezési paraméter hozzáfűzése az egyszeri bejelentkezés az API-hívás feletti kapott URL-cím:
        > pl. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * a felhasználó átirányítása a fenti gyártott URL-címe

A **bejelentkezési lapja – a** művelet mellett is elvégezheti számlakezelési az előző lépések és az alábbi műveletek egyikével.

-   **A ChangePassword**
-   **ChangeProfile**
-   **CloseAccount**

A következő lekérdezés paraméterek fiók műveletekhez hatolhat.

-   **a művelet**: (ChangePassword, ChangeProfile vagy CloseAccount) átadási kérés típusának azonosítása
-   **felhasználóazonosító**: a fiók kezelése felhasználóazonosító
-   **só**: biztonsági kivonat számítások használható különleges só karakterlánc
-   **aláírás**: a saját összehasonlító használandó számított biztonsági kivonat kiszámított kivonat

## <a name="delegate-product-subscription"> </a>Termék előfizetés delegálása

Termék előfizetés delegálása működik, hasonlóan a felhasználónak bejelentkezés/felfelé delegálása. A végleges munkafolyamat lesz az alábbi képlettel történik:

1. Fejlesztői az API-kezelés developer portal kijelöli a terméket, és az előfizetés gombra kattint
2. Böngésző rendszer átirányítja a meghatalmazás végpont
3. Meghatalmazás végpont szükséges termék előfizetés műveleteket hajtja végre – ez felfelé, és járhat átirányítása egy másik lapjára, és a számlázási adatait kérő kapcsolatos további kérdésekre, vagy egyszerűen tárolja az információkat, és nem minden olyan felhasználói beavatkozás kérése


Ha engedélyezni szeretné a funkciókat, **Meghatalmazás** lapján kattintson a **meghatalmazott termék előfizetés**.

Ezután biztosítja a meghatalmazás végpont hajtja végre a következő műveleteket:


1. Kéri, a következő formátumban:

    > *{művelet} http://www.yourwebsite.com/apimdelegation?operation= & productId {termék előfizetéséhez} = & felhasználóazonosító = {felhasználó kérés} & só = {karakterlánc} & aláírás = {karakterlánc}*

    A lekérdezés paraméterei a termék előfizetés eset:
    - **a művelet**: milyen típusú Ez a meghatalmazás kérelem azonosítja. Termék előfizetés kérelmek a érvényes beállításokat a következők:
        - "Előfizetés": a felhasználót, hogy egy adott termék az előfizetés kérelmének megadott azonosító (lásd alább)
        - "Előfizetés lemondása": kérelmének termékének felhasználó lemondása
        - "Megújítása": egy requst (pl., előfordulhat, hogy lejár) előfizetés megújítása
    - **productId**: a felhasználó előfizetéséhez kért termék azonosítója
    - **felhasználóazonosító**: a felhasználót, akinek kérésének azonosítója
    - **só**: biztonsági kivonat számítások használható különleges só karakterlánc
    - **aláírás**: a saját összehasonlító használandó kiszámított biztonsági kivonat kiszámított kivonat


2. Ellenőrizze, hogy a kérelem érkezik Azure API-kezelés (nem kötelező, de biztonsági erősen ajánlott)

    * HMAC szerepel egy **termékazonosító**, **felhasználóazonosító** és **só** lekérdezésparaméter alapján karakterlánc SHA512 egy kiszámítása:
        > HMAC (**só** + "\n" + **productId** + "\n" + **felhasználóazonosító**)
         
    * Összehasonlíthatja a fenti számított kivonat az **aláírás** lekérdezési paraméter értékét. A két tiltva felel meg, lépjen tovább a következő lépésben, egyébként utasítania.
    
3. Bármely termék előfizetés feldolgozás függően a **művelet** – például a számlázás, további kérdésekre stb kért művelet végrehajtása

4. A sikeres előfizetés a felhasználót, hogy a terméket az oldalon, az előfizetés a felhasználót, hogy az API-kezelés termék hívja fel [A REST API -t termékkulcs előfizetés].

## <a name="delegate-example-code"></a> Példa kódot. ##

Ezek mintakódok bemutatják, hogyan állapotba a *Meghatalmazás érvényesítési kulcs*, amelynek értéke a meghatalmazás képernyőn a publisher portál hozzon létre egy HMAC érvényesítésére az aláírást, majd használt igazolja az átadott returnUrl érvényességét. A termékazonosító, és enyhe módosítással felhasználóazonosító működik ugyanazt kódot.

**C# kód returnUrl kivonat készítése**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS kód returnUrl kivonat készítése**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Következő lépések

Meghatalmazás a további tudnivalókért lásd: az alábbi videóban.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[egy egyetlen – bejelentkezés (SSO) token kérése]: http://go.microsoft.com/fwlink/?LinkId=507409
[felhasználó létrehozása]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[hívja fel a REST API termék előfizetéshez]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[Példa kód pontjait az alábbiakban]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 