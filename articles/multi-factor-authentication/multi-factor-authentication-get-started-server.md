<properties 
    pageTitle="Az Azure többtényezős hitelesítést Server – első lépések"
    description="Ez a leírja, hogy miként veheti használatba az Azure MFA Server Azure többtényezős hitelesítést weblapot."
    services="multi-factor-authentication"
    keywords="hitelesítés server azure tényező hitelesítési alkalmazás aktiválási többoldalas, hitelesítési kiszolgáló letöltése"
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

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Az Azure többtényezős hitelesítést Server – első lépések




<center>![Felhőalapú](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Most, hogy azt határozott, hogy használni a helyszíni többtényezős hitelesítés, pedig hozzuk működésbe a folyamatban lévő. Ezen az oldalon új példányát a kiszolgálón, és a helyszíni Active Directoryval való beállítása első azt foglalkozik. Ha már telepítve a PhoneFactor server és frissítése, akkor olvassa el a [frissítése az Azure többtényezős kiszolgálón](multi-factor-authentication-get-started-server-upgrade.md) keresi, vagy ha a keresett telepítéséről a webszolgáltatás lásd: [az Azure többtényezős hitelesítés kiszolgáló Mobile alkalmazást webes szolgáltatás üzembe helyezése](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Töltse le az Azure többtényezős hitelesítés kiszolgáló



Kétféleképpen különböző, hogy az Azure többtényezős hitelesítést kiszolgáló letöltheti. Mindkét végzett az Azure portálon keresztül. Az első el a többtényezős hitelesítés szolgáltató közvetlenül kezelése. A második pedig a szolgáltatás beállításai keresztül. A második lehetőség többtényezős hitelesítés szolgáltató vagy egy Azure MFA, az Azure Active Directory Premium vagy a vállalati mobilitás Suite licenc szükséges.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Az Azure többtényezős hitelesítés kiszolgáló letöltése az Azure portálról
--------------------------------------------------------------------------------

1. Jelentkezzen be rendszergazdaként az Azure portálra.
2. A bal oldalon válassza az Active Directory.
3. Az Active Directory lapon a képernyő tetején kattintson a **Többtényezős hitelesítés szolgáltatók**
4. A képernyő alján kattintson a **kezelés** elemre.
5. Ekkor megnyílik egy új lapon.  Kattintson a **letöltések.** 
 ![Letöltése](./media/multi-factor-authentication-sdk/download.png)
6. Fent **Készítése aktiválási hitelesítő adatait**, kattintson a **letöltése.** 
 ![Letöltése](./media/multi-factor-authentication-get-started-server/download4.png)
7. Mentse a letöltést.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Az Azure többtényezős hitelesítés kiszolgálón keresztül a szolgáltatás beállításai letöltése


1. Jelentkezzen be rendszergazdaként az Azure portálra.
2. A bal oldalon válassza az Active Directory.
3. Kattintson duplán az Azure Active Directory-példányt.
4. A képernyő tetején kattintson a **Konfigurálás**gombra
![letöltése](./media/multi-factor-authentication-sdk/download2.png)
5. Többtényezős hitelesítés csoportban válassza a **Manage szolgáltatás beállításai**
6. A szolgáltatások beállításai lapon a képernyő alján kattintson a **Nyissa meg a portálon**.
![Letöltés](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Ekkor megnyílik egy új lapon. Kattintson a **letöltések.**
8. Fent **Készítése aktiválási hitelesítő adatait**, kattintson a **letöltése.**
9. Mentse a letöltést.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Az Azure többtényezős hitelesítés kiszolgáló telepítése és beállítása
Most, hogy a kiszolgáló letöltött telepítése, és adja meg.  Ügyeljen arra, hogy a kiszolgáló a telepít megfelel-e az alábbi követelményeknek:



Azure többtényezős hitelesítés Kiszolgálóigény|Leírás|
:------------- | :------------- |
Hardveres|<li>200 MB szabad lemezterület</li><li>x32 vagy x64 alkalmas processzor</li><li>1 GB vagy több RAM</li>
Szoftver|<li>A Windows Server 2008 vagy újabb, ha a fogadó kiszolgáló operációs rendszer</li><li>Windows 7 vagy újabb, a fogadó egy ügyfél operációs rendszer esetén</li><li>Microsoft .NET 4.0-keretrendszer</li><li>IIS 7.0-s vagy újabb, ha a felhasználó portálon vagy a webhely szolgáltatás telepítése SDK</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure többtényezős hitelesítést tűzfal kiszolgálóigény
--------------------------------------------------------------------------------
MFA-kiszolgálókon tudjanak a következő kimenő 443-as port kommunikálni kell:

- https://pfd.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Ha a kimenő tűzfalak korlátozza a 443-as port, az alábbi IP-címtartományok kell nyithatók meg:

IP-alhálózat|Maszk|IP-tartomány
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Ha Azure többtényezős hitelesítést esemény megerősítő funkciók nem használ, és a felhasználók nem rendelkeznek a vállalati hálózaton eszközökről többtényezős hitelesítés mobilalkalmazásokkal hitelesítése az IP tartományok csökkenthető a következő:


IP-alhálózat|Maszk|IP-tartomány
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Telepítse és az Azure többtényezős hitelesítés beállítása
--------------------------------------------------------------------------------


1. Kattintson duplán a végrehajtható fájl. Ez a program a telepítés megkezdéséhez.
2. A telepítés Mappaválasztás képernyőn győződjön meg arról, hogy helyesen-e a mappát, és kattintson a Tovább gombra.
3. Miután elvégezni a telepítést, kattintson a Befejezés gombra.  Ez elindítja a Fiókkonfiguráló varázslót.
4. A Fiókkonfiguráló varázslót a üdvözlőképernyő, jelölje be a **Kihagyás gombra a hitelesítés konfigurálása varázsló használatával** , és kattintson a **Tovább**gombra.  Ezzel zárja be a varázslót, és indítsa el a kiszolgálót.
    ![Felhőalapú](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Vissza a kiszolgálóról azt letöltött lapon a **Készítése aktiválási hitelesítő adatok** gombra.  Ezt az információt másolja be az Azure MFA webkiszolgálóra a rendelkezésre álló mezőkbe, és kattintson az **Aktiválás**gombra.


A fenti lépések megjelenítése egy gyors telepítés a konfigurációs varázslóval.  A hitelesítési varázsló futtassa újra az Eszközök menü a kiszolgálón kiválasztásával.



##<a name="import-users-from-active-directory"></a>Az Active Directory felhasználók importálása

Most, hogy a kiszolgáló telepítve és beállítva gyorsan importálhatja felhasználók az Azure MFA kiszolgáló.

### <a name="to-import-users-from-active-directory"></a>Az Active Directory felhasználók importálása
--------------------------------------------------------------------------------


1. A bal oldali Azure MFA kiszolgálón jelölje ki a **felhasználókat**.
2. A képernyő alján jelölje be az **Active Directoryból importálása**.
3. Most már az egyes felhasználók részére keresése, vagy az Active directory keres, a szervezeti felhasználók őket.  Ebben az esetben azt határozza meg a szervezeti felhasználók.
4. Jelölje ki az összes felhasználó a jobb oldalon, és kattintson az **Importálás**gombra.  Előugró ablak jelzi, hogy sikeres volt-e meg kell kapnia.  Az importálási ablak bezárásához.

![Felhőalapú](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Küldje el a felhasználók e-mailben
Most, hogy a felhasználók importálta az Azure többtényezős hitelesítés kiszolgáló, azt javasoljuk, hogy a felhasználók egy e-mail elküldése, amely tájékoztatja, hogy azok van már tehát többtényezős hitelesítés.

Az Azure többtényezős hitelesítést kiszolgálóval módon különböző többtényezős hitelesítés használatával a felhasználók beállítása.  Például ha, hogy a felhasználói telefonszámok vagy tudott telefonszámok importálja a vállalati címtárban Azure többtényezős hitelesítést kiszolgálóról, az e-mailt közli, hogy konfigurálva Azure többtényezős hitelesítés, néhány útmutatást Azure többtényezős hitelesítés használatával és értesíti a felhasználót a telefonszám után, a hitelesítési kapnak a felhasználókat.  

Az e-mail tartalma, hogy be van állítva a felhasználó (pl. telefonhívás, SMS-mobilalkalmazás) hitelesítési módtól függően változhatnak.  Például ha a felhasználónak van szükség a PIN-kód során, az e-mailt a program tájékoztatja őket Mi a kezdeti PIN-kód van beállítva.  Felhasználók általában a PIN-kód módosítása során az első hitelesítés szükséges.

Ha felhasználói telefonszámok nincs beállítva vagy az Azure többtényezős hitelesítést kiszolgáló importálja, vagy felhasználók előre konfigurált a mobilalkalmazás hitelesítéshez, elküldheti őket egy e-mailt, amely lehetővé teszi őket, hogy Azure többtényezős hitelesítés használatára konfigurált, és meg fog irányítsa őket, a fiók regisztrációs az Azure többtényezős hitelesítést felhasználói portálon keresztül befejezéséhez.  Hivatkozás fog szerepelni, hogy a felhasználó kattint a felhasználó portál eléréséhez. Amikor a felhasználó a hivatkozásra kattint, a webböngészőben nyissa meg, és a cég Azure többtényezős hitelesítést felhasználói portálra kihasználásához.   


### <a name="configuring-email-and-email-templates"></a>A levelezés és a Levélsablonok konfigurálása

A bal oldalon az e-mailben ikonra kattintva beállíthatja a beállításokat az e-maileket küldhet.  Ez az adott adhatja meg az SMTP-adatokat a levelezési kiszolgáló, és lehetővé teszi, hogy egy keretszerződés széles e ellenőrzést az elküldés hozzáadásával felhasználók jelölőnégyzetet levelek küldése.

![E-mail beállítások](./media/multi-factor-authentication-get-started-server/email1.png)

Az e-mailben a tartalom lapon látni fogja az összes a különböző e-mail elérhető sablonok közül választhat.  Attól függően, hogy miként állította be a felhasználóknak, hogy többtényezős hitelesítést használ, válassza ki a sablon úgy, hogy a legjobb elképzeléseinek.

![E-mail sablonok](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Hogyan kezeli az Azure többtényezős hitelesítést kiszolgáló a felhasználói adatok

A többtényezős hitelesítés (MFA) kiszolgáló helyszíni használatakor egy felhasználó adatait a helyszíni kiszolgálók vannak tárolva. Nincs állandó felhasználói adatok a felhőben vannak tárolva. Amikor a felhasználó egy kétfaktoros hitelesítés hajt végre, a MFA kiszolgáló adatokat küld a Azure MFA felhőalapú szolgáltatást a hitelesítés végrehajtásához. Amikor elküldi ezeket hitelesítési kérések a felhőbeli szolgáltatástól, az alábbi mezők küldjük összehívások és a naplókat, hogy érhetők el a felhasználói hitelesítés és használati jelentések. Egyes mezői nem kötelező, így engedélyezhető vagy tiltható le a többtényezős hitelesítést kiszolgálón belül. A kommunikáció a MFA-kiszolgálóról a MFA felhőalapú szolgáltatás SSL/TLS használja a kimenő 443-as port keresztül. Ezeket a mezőket a következők:

- Egyedi azonosító – felhasználónév vagy a belső MFA kiszolgáló azonosítója
- Első és utolsó nevét – nem kötelező
- E-mail címet – nem kötelező.
- Telefonszám - során a hanghívás vagy SMS-hitelesítés használatával
- Eszköz jogkivonat - mobilalkalmazás hitelesítési során
- Hitelesítési mód
- Hitelesítés eredménye
- MFA-kiszolgáló neve
- MFA-kiszolgáló IP
- Ügyfél IP – Ha elérhető



A fenti mezők mellett a hitelesítés eredménye (sikeres/megtagadását) és a bármilyen elutasításokat okát egyben tárolt, a hitelesítési adatok és a hitelesítési és használati jelentések keresztül érhető el.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Speciális Azure többtényezős hitelesítés kiszolgáló beállításait
További információ a speciális beállítás és a konfigurációs információk használja az alábbi táblázatban.

A módszer|Leírás
:------------- | :------------- |
[Felhasználói portál](multi-factor-authentication-get-started-portal.md)|  Telepítés és konfigurálása a felhasználói portál, például telepítési és a felhasználó önkiszolgáló vonatkozó információk.
[Az Active Directory összevonási szolgáltatás](multi-factor-authentication-get-started-adfs.md)|Többtényezős hitelesítést Azure Active Directory összevonási szolgáltatások beállításával kapcsolatos információk.
[RADIUS-hitelesítés](multi-factor-authentication-get-started-server-radius.md)|  A telepítő és Azure MFA-kiszolgáló konfigurálása sugarú vonatkozó információk.
[IIS-hitelesítés](multi-factor-authentication-get-started-server-iis.md)|A telepítő és Azure MFA-kiszolgáló konfigurálása az IIS vonatkozó információk.
[Windows-hitelesítés](multi-factor-authentication-get-started-server-windows.md)|  A telepítő és a Windows-hitelesítéssel az Azure-MFA kiszolgáló konfigurálása vonatkozó információk.
[LDAP-hitelesítés](multi-factor-authentication-get-started-server-ldap.md)|A telepítő és Azure MFA-kiszolgáló konfigurálása LDAP hitelesítéssel vonatkozó információk.
[Távoli asztali átjáró és Azure többtényezős hitelesítést kiszolgáló RADIUS használata](multi-factor-authentication-get-started-server-rdg.md)|  A telepítő és Azure MFA-kiszolgáló konfigurálása a távoli asztali átjáró RADIUS használata vonatkozó információk.
[A Windows Server Active Directory szinkronizáló](multi-factor-authentication-get-started-server-dirint.md)|Információ a telepítő és konfigurálása az Active Directory és az Azure MFA kiszolgáló közötti szinkronizálást.
[A Azure többtényezős hitelesítés kiszolgáló mobil alkalmazás webes szolgáltatás üzembe helyezése](multi-factor-authentication-get-started-server-webservice.md)|A telepítő és a Azure MFA server webszolgáltatás beállítása vonatkozó információk.
