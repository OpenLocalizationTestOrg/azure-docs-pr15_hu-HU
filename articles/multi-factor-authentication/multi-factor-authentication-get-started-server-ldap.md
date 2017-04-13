<properties 
    pageTitle="LDAP-hitelesítés és Azure többtényezős hitelesítés kiszolgáló"
    description="Ez a az Azure többtényezős hitelesítés oldalt, amely segítséget nyújt az LDAP hitelesítési és Azure többtényezős hitelesítést Server telepítése."
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

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-hitelesítés és Azure többtényezős hitelesítés kiszolgáló


Az Azure többtényezős hitelesítést kiszolgáló alapértelmezés szerint be van állítva importálásához vagy szinkronizál felhasználókat az Active Directoryból. Azonban azt beállítható úgy, hogy más LDAP könyvtárak, például egy ADAM címtár vagy adott Active Directory tartományi vezérlő kötést létrehozni. LDAP keresztül könyvtárában csatlakozhat konfigurálásakor az Azure többtényezős hitelesítést kiszolgáló beállítható úgy, hogy a használni kívánt egy LDAP proxy hitelesítési végrehajtásához. Lehetővé teszi a felhasználni LDAP kötés RADIUS célként előtti hitelesítést az IIS-hitelesítés használata esetén a felhasználók, illetve az Azure többtényezős hitelesítést felhasználói portálon elsődleges hitelesítési.

Amikor egy LDAP proxy Azure többtényezős hitelesítést használ, az Azure többtényezős hitelesítést kiszolgáló beszúrt az LDAP-ügyfél (VPN-készülék, alkalmazás) és az LDAP-címtár-kiszolgáló közötti többtényezős hitelesítés lekérdezésbe. Többtényezős hitelesítés Azure függvény az Azure többtényezős hitelesítést kiszolgáló kell beállítania, hogy az ügyfél-kiszolgálók és az LDAP-címtár is kommunikálhatnak. Ebben a konfigurációban az Azure többtényezős hitelesítést kiszolgáló fogadja el az ügyfél-kiszolgálók és alkalmazások LDAP kérései, és továbbítja őket a cél LDAP-címtár kiszolgáló elsődleges hitelesítő adatok ellenőrzése lehetőségre. Az LDAP-címtár válasza azt mutatja, hogy ezek elsődleges hitelesítő adatok érvényesek, ha Azure többtényezős hitelesítés hajtja végre a második varianciaanalízis hitelesítést, és válasza a LDAP-ügyfél. A teljes hitelesítés csak akkor, ha a hitelesítést az LDAP-kiszolgálóhoz, mind a többtényezős hitelesítés sikeres fog sikerülni.





## <a name="ldap-authentication-configuration"></a>LDAP hitelesítési konfigurálása


LDAP-hitelesítés konfigurálása, telepítse az Azure többtényezős hitelesítést kiszolgáló a Windows server. Kövesse az alábbi lépéseket:

1. Az Azure többtényezős hitelesítést kiszolgálón belül kattintson a bal oldali menü LDAP hitelesítési ikonra.
2. LDAP-hitelesítés engedélyezése jelölőnégyzetet.![LDAP-hitelesítés](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Az ügyfelek lapon a TCP-port és az SSL-port esetén módosítsa az Azure többtényezős hitelesítést LDAP-szolgáltatás nem szabványos portokat kötendő figyelni a LDAP kéréseit az ügyfelek beállított lesz.
4. Ha az ügyfél LDAPS használatával az Azure többtényezős hitelesítést kiszolgáló, SSL-tanúsítvány telepítenie kell a kiszolgálón futó a kiszolgálón. Kattintson a Tallózás gombra. az SSL-tanúsítvány mező mellett gombot, és válassza ki a telepített tanúsítványt, a biztonságos kapcsolat használt.
5. Kattintson a Hozzáadás gombra. gombra.
6. LDAP-ügyfél hozzáadása párbeszédpanelen adja meg a készülék, a kiszolgáló vagy az alkalmazást, a program hitelesíti IP-címét a kiszolgálóra, és az alkalmazás nevét (nem kötelező). Az alkalmazás neve Azure többtényezős hitelesítés jelentések jelenik meg, és jelennek meg az SMS vagy mobilalkalmazás hitelesítési üzenetekben.
7. Azure többtényezős hitelesítés szükséges felhasználói egyezés Ha jelölőnégyzetet minden felhasználónak, hogy vagy a program importálja a kiszolgálóra, és mutli varianciaanalízis hitelesítési fizetnie. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve mutli varianciaanalízis hitelesítési alól lesz, a üresen hagyja bejelölve. További információt a súgófájl nézze meg ezt a szolgáltatást.
8. Előfordulhat, hogy ismételje 5 és további LDAP-ügyfelek hozzáadása 7.
9. LDAP hitelesítési fogadására az Azure többtényezős hitelesítés van beállítva, ha meg kell proxy ezeket az LDAP-címtár hitelesítési. A cél lap csak egy jelenít meg egyetlen, ezért lehetőség, amellyel az LDAP cél szürke. Adja meg az LDAP-címtár-kapcsolatot, kattintson a címtár-integrációs ikonra.
10. A beállítások lapon jelölje be a használata bizonyos LDAP konfiguráció választógombot.
11. Kattintson a Szerkesztés... gombra.
12. LDAP konfigurációs szerkesztése párbeszédpanelen a mezőket az LDAP-címtár csatlakozni a szükséges információk adataival. Leírások a mezők az alábbi táblázatban szereplő. Megjegyzés: Ez az információ is szerepel a Azure többtényezős hitelesítést kiszolgáló súgófájlt.![Címtár-integráció](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Tesztelje a LDAP-kapcsolat tesztelése gombra kattintva.
14. Ha az LDAP kapcsolat tesztelésének sikerességéről tájékoztat, kattintson az OK gombra.
15. Kattintson a szűrők fülre. A kiszolgáló egy előre konfigurált tárolók, a biztonsági csoportokat és a felhasználók az Active Directory betöltéséhez. Ha egy másik LDAP-címtár kötés, valószínűleg szüksége lesz a szűrők jelenik meg a szerkesztendő. Kattintson a Súgó hivatkozásra a szűrők további információt.
16. Kattintson a attribútumok fülre. A kiszolgáló egy előre konfigurált Active Directoryból attribútumok megfeleltetése.
17. Ha egy másik LDAP-címtár vagy az előre beállított attribútum megfeleltetésének módosításához kötés, kattintson a Szerkesztés... gombra.
18. Attribútumok szerkesztése párbeszédpanelen módosítsa a címtár-megfeleltetések LDAP attribútum. Attribútum nevek és hangulatjelet tartalmazó, vagy kiválasztott gombra kattintva a... minden mező melletti gombra.
19. Kattintson a Súgó hivatkozásra attribútumok további információt.
20. Kattintson az OK gombra.
21. Kattintson a vállalat beállítások gombra, és válassza ki a felhasználónév kiküszöbölése fülre.
22., ha a tartományhoz tartozó kiszolgálótól csatlakoztatása az Active Directory, láthatja, hagyja az a Windows biztonsági azonosítók megfelelő kijelölt felhasználónevét választógomb. Ellenkező esetben válassza a megfelelő felhasználónevét választógomb használata LDAP egyedi azonosító attribútum. Ha be van jelölve, az Azure többtényezős hitelesítést kiszolgáló megpróbálja minden felhasználónév úgy, hogy az LDAP-címtár az egyedi azonosítót kaphat. LDAP-kereséshez fog történni a felhasználónév a címtár-integráció a meghatározott attribútumok-Attribútumok lap >. Amikor a felhasználó hitelesíti, felhasználónév fog megfeleltetni az egyedi azonosító az LDAP-címtár, és egyedi azonosítója lesz az Azure többtényezős hitelesítés adatfájlba a felhasználónak megfelelő. Nagybetűk összehasonlításokhoz, így, valamint hosszú és rövid felhasználónév formátum ezt befejezte a Azure többtényezős hitelesítés beállítása. A kiszolgáló a beállított portokon LDAP hozzáférési kérelmek a beállított ügyfelektől érkező most figyel, és ezeket az LDAP-címtár hitelesítéshez kérések proxy beállítása.


## <a name="ldap-client-configuration"></a>LDAP-ügyfél konfigurálása

Ha szeretné beállítani az LDAP-ügyfél, kövesse az útmutatásokat:

- Állítsa be az készülék, a kiszolgáló vagy a program, mintha az LDAP-címtár LDAP az Azure többtényezős hitelesítést kiszolgálón keresztül hitelesítést végezni. Ugyanazokat a beállításokat, csatlakoztassa közvetlenül a LDAP-címtár, kivéve a kiszolgáló nevét, vagy az IP-címet, amely lesz az Azure többtényezős hitelesítést kiszolgáló, amely a szokásos módon használható kell használni.
- Állítsa be az LDAP időkorlátját 30-60 másodpercre, úgy, hogy az LDAP-címtár a felhasználó hitelesítő adatok érvényesítése, végezze el a második varianciaanalízis hitelesítés, a válasz érkezik és majd megválaszolása az LDAP hozzáférési engedély kérése.
- Ha LDAPS használ, a készülék vagy a kiszolgáló a LDAP lekérdezések létrehozása kell bíznia az SSL-tanúsítvány telepíteni a kiszolgálón Azure többtényezős hitelesítést.
