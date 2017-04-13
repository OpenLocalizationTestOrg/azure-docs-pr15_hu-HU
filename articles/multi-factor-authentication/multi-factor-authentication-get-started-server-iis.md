<properties 
    pageTitle="IIS-hitelesítés és Azure többtényezős hitelesítés kiszolgáló"
    description="Ez a az Azure többtényezős hitelesítés oldalt, amely segítséget nyújt az IIS-hitelesítés és Azure többtényezős hitelesítést Server telepítése."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="iis-authentication"></a>IIS-hitelesítés

Az Azure többtényezős hitelesítést kiszolgáló hitelesítést az IIS szakasza engedélyezése és konfigurálása az IIS-integráció a Microsoft IIS webalkalmazások teszi lehetővé. Az Azure többtényezős hitelesítést kiszolgáló telepíti beépülő modul amely annak érdekében, hogy adja hozzá az Azure többtényezős hitelesítést az IIS-webkiszolgálón végzett kérések is szűrheti. A beépülő modul IIS a képernyő-alapú hitelesítés és a beépített Windows HTTP-hitelesítés támogatja. Megbízható IP-címei is beállítható adómentes belső IP-címek a kétfaktoros hitelesítés.


![IIS-hitelesítés](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Azure többtényezős hitelesítés kiszolgálóval űrlapokon alapuló IIS-hitelesítés használatával

Biztonságos használó űrlapokon alapuló hitelesítést az IIS webalkalmazást, telepítse az Azure többtényezős hitelesítést kiszolgáló IIS webkiszolgálón, és állítsa be a az alábbi eljárás egy a kiszolgálón.

1. Az Azure többtényezős hitelesítést kiszolgálón belül kattintson a hitelesítést az IIS ikonra a bal oldali menüben.
2. Kattintson a képernyő-alapú fülre.
3. Kattintson a Hozzáadás... gombra.
4. Felhasználónév észleli, hogy a jelszó és a tartomány változók automatikusan, adja meg a bejelentkezési URL-cím (például https://localhost/contoso/auth/login.aspx) a Auto-Configure Form-Based webhely párbeszédpanelen, és kattintson az OK gombra.
5. Jelölje be a többtényezős hitelesítés szükséges felhasználói egyezés jelölőnégyzetet, ha minden felhasználónak, hogy vagy a program importálja a kiszolgálóra, és amelyekre többtényezős hitelesítés. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve többtényezős hitelesítés alól lesz, a üresen hagyja bejelölve.
6. Ha a lap változók nem lehet automatikusan észlelni, kattintson a adja meg, kézzel... a webhely Auto-Configure Form-Based párbeszédpanel gombjára.
7. Add Form-Based webhely párbeszédpanelen a bejelentkezési lapon, a Küldés URL-címe mezőben adja meg az URL-címet, és írja be az alkalmazás nevét (nem kötelező). Az alkalmazás neve Azure többtényezős hitelesítés jelentések jelenik meg, és előfordulhat, hogy az SMS vagy mobilalkalmazás hitelesítési üzenetekben jeleníthető meg. További információt a súgófájl nézze meg a Küldés URL-CÍMÉT.
8. Jelölje ki a megfelelő kérelem formátumban. Ez értéke "BEJEGYZÉST, vagy el" a legtöbb webalkalmazások esetében.
9. Írja be a Username változó, a jelszó változó és a tartomány változó (Ha a bejelentkezési lapon megjelenik). Előfordulhat, hogy bejelentkezési lapjának egy webböngészőben, kattintson a jobb gombbal a lapon, és válassza a "Forrás megtekintése" keresése az oldalon belül a beviteli mezők azoknak a.
10. Jelölje be a Azure többtényezős hitelesítés szükséges felhasználói egyezés jelölőnégyzetet, ha minden felhasználónak, hogy vagy a program importálja a kiszolgálóra, és amelyekre többtényezős hitelesítés. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve többtényezős hitelesítés alól lesz, a üresen hagyja bejelölve. További információt a súgófájl nézze meg ezt a szolgáltatást.
11.  Kattintson a speciális... Tekintse át a Speciális beállítások, beleértve az azt jelenti, hogy jelöljön ki egy egyéni megtagadását lap fájlt időbeli cookie-k használata a gyorsítótár-sikeres hitelesítési a webhelyet, és válassza ki, hogy a Windows-tartomány, LDAP-címtár vagy RADIUS kiszolgáló ellen elsődleges hitelesítő gombra. Amikor elkészült kattintson az OK gombra kattintva térjen vissza a webhely Add Form-Based párbeszédpanel. További információt a súgófájl nézze meg a Speciális beállítások.
12. Kattintson az OK gombra.
13. Miután az URL-CÍMEK és lap változók észlelt, vagy a megadott, a webhely adatok jelennek meg a az űrlapokra épülő panelen.
14. Lásd: az IIS engedélyezése bővítmények közvetlenül az alábbi Azure többtényezős hitelesítést kiszolgáló szakasz az IIS hitelesítési konfigurálása befejezéséhez.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Integrált Windows-hitelesítés használatával Azure többtényezős hitelesítés kiszolgálóval

Biztonságos használó integrált Windows HTTP-hitelesítést az IIS webalkalmazást, telepítse az Azure többtényezős hitelesítést kiszolgáló IIS webkiszolgálón, és állítsa be a az alábbi eljárás egy a kiszolgálón.

1. Az Azure többtényezős hitelesítést kiszolgálón belül kattintson a hitelesítést az IIS ikonra a bal oldali menüben.
2. Kattintson a HTTP fülre. Kattintson a képernyő-alapú fülre.
3. Kattintson a Hozzáadás... gombra.
4. A következő URL-cím hozzáadása párbeszéd mezőben adja meg azt az URL-cím a webhely hol található a HTTP-hitelesítés (pl. http://localhost/owa) elvégzett alap URL-cím mezőjébe, és írja be az alkalmazás nevét (nem kötelező). Az alkalmazás neve Azure többtényezős hitelesítés jelentések jelenik meg, és jelennek meg az SMS vagy mobilalkalmazás hitelesítési üzenetekben.
5. Módosítsa a üresjárati időtúllépés és a Maximum munkamenet időpontok Ha az alapértelmezett nem elegendő.
6. Jelölje be a többtényezős hitelesítés szükséges felhasználói egyezés jelölőnégyzetet, ha minden felhasználónak, hogy vagy a program importálja a kiszolgálóra, és amelyekre többtényezős hitelesítés. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve többtényezős hitelesítés alól lesz, a üresen hagyja bejelölve. További információt a súgófájl nézze meg ezt a szolgáltatást.
7. Ha azt szeretné, jelölje be a cookie-k gyorsítótárazás jelölőnégyzetet.
8. Kattintson az OK gombra.
9. A [engedélyezése az IIS beépülő moduljainak Azure többtényezős hitelesítést Server](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) közvetlenül az alábbi teljes hitelesítést az IIS konfigurálása szakaszban olvashat.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Server Azure többtényezős hitelesítést az IIS bővítmények engedélyezése

Miután beállította a képernyő-alapú HTTP-hitelesítés URL-EK és a beállítások, ki kell választania a helyek, ahol a Azure többtényezős hitelesítést az IIS bővítmények kell betöltése és engedélyezve van az IIS vagy. Kövesse az alábbi lépéseket:

1. IIS 6 forgalmi, ha a ISAPI lapján, és jelölje ki az futtató a webalkalmazás csoportban (pl. alapértelmezett webhely) ahhoz, hogy a létrehozott beépülő modul Azure többtényezős hitelesítést ISAPI szűrő webhelyet.
2. Ha IIS 7-es vagy újabb fut, kattintson a natív modul fülre, és jelölje ki a kiszolgáló, webhely(ek) vagy ahhoz, hogy a kívánt szintjeit a beépülő modul IIS alkalmazásokat.
3. Jelölje be az IIS engedélyezése hitelesítési a képernyő tetején. Azure többtényezős hitelesítés most biztonságossá tétele a a kijelölt IIS alkalmazást. Győződjön meg arról, hogy a felhasználók be a kiszolgáló nincs importálva. Az alábbi IP-címei megbízható szakaszt nézzen whitelist belső IP-címek, így kétfaktoros hitelesítés nem szükséges az adott helyen a webhelyen történő bejelentkezéskor szeretné.


## <a name="trusted-ips"></a>Megbízható IP-címei

A megbízható IP-címei lehetővé teszi a felhasználóknak figyelmen kívül hagyása az Azure többtényezős hitelesítés adott IP-címek vagy alhálózat származó webhely kérelmekhez. Például érdemes adómentes felhasználóknak az Azure többtényezős hitelesítés az office a bejelentkezés közben. Meg szeretné adni az office-alhálózat megbízható IP-címei bejegyzésként. A megbízható IP-címei konfigurálása kövesse az alábbi lépéseket:

1. A hitelesítést az IIS csoportban kattintson a megbízható IP-címei fülre.
2. Kattintson a Hozzáadás... gombra.
3. A hozzáadása a megbízható IP-címei párbeszédpanel megjelenésekor válassza a egyetlen IP, IP-tartomány vagy alhálózat választógomb.
4. b IP-címét az IP-címek vagy alhálózathoz, amelyet whitelisted kell lennie. Ha alhálózat ad meg, jelölje ki a megfelelő maszk, és kattintson az OK gombra. A whitelist most hozzá lett adva.
