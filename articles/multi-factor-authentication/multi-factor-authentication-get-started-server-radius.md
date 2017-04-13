<properties 
    pageTitle="Többtényezős hitelesítés-kiszolgáló RADIUS-hitelesítés és Azure"
    description="Ez a az Azure többtényezős hitelesítés oldalt, amely segítséget nyújt a RADIUS-hitelesítés és Azure többtényezős hitelesítést Server telepítése."
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
    ms.date="08/15/2016"
    ms.author="kgremban"/>



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>Többtényezős hitelesítés-kiszolgáló RADIUS-hitelesítés és Azure

A RADIUS-hitelesítés szakasz lehetővé teszi engedélyezése és beállítása az Azure többtényezős hitelesítést kiszolgáló RADIUS-hitelesítést. RADIUS egy olyan szabványos protokoll, fogadja el a hitelesítési kérések és feldolgozni azokat a kérelmeket. Az Azure többtényezős hitelesítést kiszolgáló működik-e a RADIUS-kiszolgáló, és a program beszúrja a RADIUS-ügyfél (például a virtuális Magánhálózati készülék) és a hitelesítési cél, amelyek lehetnek a Active Directory (AD), LDAP-címtár vagy egy másik RADIUS-kiszolgáló, annak érdekében, hogy az Azure többtényezős hitelesítés hozzáadása között. Többtényezős hitelesítés Azure függvény az Azure többtényezős hitelesítést kiszolgáló meg kell adnia, hogy az ügyfél-kiszolgálók és a hitelesítési cél is kommunikálhat. Az Azure többtényezős hitelesítést kiszolgáló fogadja el a RADIUS-ügyfél kérelmeket, ellenőrzi a hitelesítési célértékhez viszonyított hitelesítő adatait, összeadja az Azure többtényezős hitelesítés és válasza a RADIUS ügyfélnek. A teljes hitelesítés sikeres lesz, csak akkor, ha az elsődleges hitelesítés, mind az Azure többtényezős hitelesítés sikeres.

>[AZURE.NOTE]
>A MFA-kiszolgáló csak akkor támogatja, PAP (jelszavát hitelesítéshez protocol), és MSCHAPv2 (a Microsoft kérdés-Handshake hitelesítési Protocol) RADIUS protokollok során eljáró RADIUS kiszolgáló.  Más protokollok, például EAP (protokoll) is használható, amikor a MFA-kiszolgáló működik-e egy másik RADIUS-kiszolgálóhoz, amely támogatja az adott protokollt, például Microsoft NPS RADIUS-proxy.
></br>
>Amikor más protokollok ebben a konfigurációban, egyirányú SMS és elfogadható tokenek nem fog működni, mivel a MFA-kiszolgáló nem tudja kezdeményezése egy sikeres RADIUS kérdés-válasz, a protokoll használatával.


![RADIUS-hitelesítés](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>RADIUS hitelesítési konfigurálása

RADIUS hitelesítés konfigurálása, telepítse az Azure többtényezős hitelesítést kiszolgáló a Windows server. Ha Active Directory környezetben, a kiszolgáló kell lennie a tartományhoz a hálózaton belül. Az alábbi eljárással az Azure többtényezős hitelesítés kiszolgáló konfigurálása:

1. Az Azure többtényezős hitelesítést kiszolgálón belül kattintson a bal oldali menü RADIUS-hitelesítés ikonra.
2. Jelölje be a hitelesítés RADIUS engedélyezése jelölőnégyzetet.
3. Az ügyfelek lapon a hitelesítési port és a könyvelési port esetén módosítsa az Azure többtényezős hitelesítést RADIUS szolgáltatás nem szabványos portokat kötendő RADIUS kérések beállított lesz az ügyfelektől meghallgatásához.
4. Kattintson a Hozzáadás gombra. gombra.
5. RADIUS-ügyfél hozzáadása párbeszédpanelen adja meg a készülék/kiszolgáló hitelesíteni fogja IP-címét az Azure többtényezős hitelesítést kiszolgáló, az alkalmazás nevét (nem kötelező), és egy közös titkos kulcs. A megosztott titkos kell ugyanazok a Azure többtényezős hitelesítést Server és a készülék/kiszolgálón. Az alkalmazás neve Azure többtényezős hitelesítés jelentések jelenik meg, és jelennek meg az SMS vagy mobilalkalmazás hitelesítési üzenetekben.
6. Jelölje be a többtényezős hitelesítés szükséges felhasználói egyezés jelölőnégyzetet, ha minden felhasználónak, hogy vagy a program importálja a kiszolgálóra, és amelyekre többtényezős hitelesítés. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve többtényezős hitelesítés alól lesz, a üresen hagyja bejelölve. További információt a súgófájl nézze meg ezt a szolgáltatást.
7. Jelölje be a engedélyezése alaplekérdezések elfogadható jogkivonat jelölőnégyzetet, ha a felhasználók fogja használni a Azure többtényezős hitelesítés mobilalkalmazás hitelesítés és az sávon a telefon hívás, SMS vagy leküldéses értesítést a visszalépési hitelesítés elfogadható jelszavai használni kívánt.
8. Kattintson az OK gombra.
9. Előfordulhat, hogy ismételje 4-8 további RADIUS-ügyfelek hozzáadása.
10. Kattintson a cél fülre.
11. Ha az Azure többtényezős hitelesítést kiszolgáló telepítve van a tartományhoz tartozó kiszolgálón az Active Directory-környezetben, jelölje ki a Windows-tartományt.
12. Ha a felhasználók kell-e hitelesíteni LDAP-címtár ellen, jelölje be a LDAP kötés. Amikor LDAP kötés, kattintson a címtár-integrációs ikonra, és kattintson a beállítások lap LDAP alapbeállítások szerkesztése, hogy a kiszolgáló a címtárában kell kötni. LDAP tartozó utasításokat az LDAP Proxy konfigurációs útmutató található.
13. Ha a történjen a felhasználók hitelesítése egy másik RADIUS kiszolgálón, jelölje be a RADIUS kiszolgálókat.
14. A kiszolgáló, hogy a kiszolgáló fog proxy a RADIUS kérelmek a Hozzáadás gombra kattintva konfigurálása... gombra.
15. A RADIUS-kiszolgáló hozzáadása párbeszédpanelen adja meg a RADIUS-kiszolgáló és a megosztott titkos IP-címét. A megosztott titkos kell ugyanazok a Azure többtényezős hitelesítést Server és a RADIUS kiszolgálón. Ha másik portokat a RADIUS-kiszolgáló által használt hitelesítési port és számlázási portja módosítása.
16. Kattintson az OK gombra.
17. Hozzá kell adnia az Azure többtényezős hitelesítést kiszolgáló RADIUS Server RADIUS ügyfélszámítógépeken, hogy azt dolgozza fel a hozzáférési kérelmek a Azure többtényezős hitelesítést kiszolgálóról küldött. Többtényezős hitelesítés Server Azure konfigurált azonos megosztott titkos kulcs kell használnia.
18. Előfordulhat, hogy ismételje meg a további RADIUS-kiszolgálók hozzáadása és konfigurálása a sorrendben, amelyben a kiszolgáló kell hívnia őket a fel és le gombbal ezt a lépést. Ez akkor egészíti ki a Azure többtényezős hitelesítés beállítása. A kiszolgáló most figyel az konfigurált portokon RADIUS hozzáférési kérelmek a beállított ügyfelek számára.   


## <a name="radius-client-configuration"></a>RADIUS ügyfél konfigurálása

A RADIUS-ügyfél beállításához kövesse az útmutatásokat:

- Állítsa be a készülék/kiszolgálót RADIUS hitelesíthet RADIUS-kiszolgálói jár a Azure többtényezős hitelesítést kiszolgáló IP-címére.
- A fenti konfigurált azonos megosztott titkos használja.
- Állítsa be a RADIUS időtúllépés 30-60 másodpercre, úgy, hogy a felhasználó hitelesítő adatok érvényesítése, végezze el a többtényezős hitelesítés, a válasz érkezik és majd megválaszolása a RADIUS hozzáférési engedély kérése.
