<properties 
    pageTitle="A felhasználó portál az Azure többtényezős hitelesítést kiszolgáló üzembe helyezése"
    description="Ez a leírja, hogy miként veheti használatba Azure MFA és a felhasználó portál Azure többtényezős hitelesítést weblapot."
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

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>A felhasználó portál az Azure többtényezős hitelesítést kiszolgáló üzembe helyezése

A felhasználó portál lehetővé teszi, hogy a rendszergazda telepítse és állítsa be az Azure többtényezős hitelesítést felhasználói portálon. A felhasználó portál, amelynek az IIS-webhelyet, amely lehetővé teszi a felhasználóknak, hogy léptessék be az Azure többtényezős hitelesítés és a fiókok kezelése. Egy felhasználó előfordulhat, hogy azok telefonszámai alapján módosítása, a PIN-kód módosítása és Azure többtényezős hitelesítés átlépése során a következő bejelentkezési a.

Felhasználók fog jelentkezzen be a felhasználó portálra a normál felhasználónévvel és jelszóval és fog Azure többtényezős hitelesítés hívás befejezése, vagy biztonsági kérdésekre a hitelesítési befejezéséhez. Felhasználói regisztrációs engedélyezett, ha egy felhasználó beállítja a telefonszám és a PIN-kód az első alkalommal a felhasználó portálra jelentkeznek.

Felhasználói portál rendszergazdák előfordulhat, hogy állítsa be, és engedélyt új felhasználók hozzáadása és módosítása meglévő felhasználóknak.

<center>![A telepítő](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>A felhasználó portál Azure többtényezős hitelesítést kiszolgálójával azonos kiszolgálón üzembe helyezése

A következő előtti követelmények szükség a felhasználók portál telepíti az Azure többtényezős hitelesítést kiszolgálójával azonos kiszolgálón:

- IIS kell asp.net és az alap kompatibilitási IIS 6-metaadatok (IIS 7-es vagy újabb) is telepíteni
- Bejelentkezett felhasználó a számítógép és a tartomány rendszergazdai jogosultsággal kell rendelkeznie, ha van ilyen.  Ennek oka az, a fiók szükséges engedélyeket az Active Directory-biztonsági csoportokat hozhat létre.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>A felhasználó portál terjesztése az Azure többtényezős hitelesítést kiszolgáló

1. Belül az Azure többtényezős hitelesítést kiszolgáló: felhasználói portál ikonra a bal oldali menüben, felhasználói Portal telepítése gombra.
1. Kattintson a Tovább gombra.
1. Kattintson a Tovább gombra.
1. Ha a számítógép csatlakozik egy tartományt, és a felhasználó-portál és az Azure többtényezős hitelesítés szolgáltatás közötti kommunikáció biztonságossá tétele az Active Directory adatokat nem teljes, az Active Directory lépés jelennek meg. Kattintson a Tovább gombra kattintva automatikus kiegészítése ebben a konfigurációban.
1. Kattintson a Tovább gombra.
1. Kattintson a Tovább gombra.
1. Kattintson a Bezárás gombra.
1. Nyisson meg egy webböngészőt minden olyan számítógépről, és nyissa meg azt az URL-címet, ahová telepítve van a felhasználó Portal (pl. https://www.publicwebsite.com/MultiFactorAuth). Győződjön meg arról, hogy nincs hiba vagy tanúsítvány figyelmeztetés jelenik meg.

<center>![A telepítő](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Az Azure többtényezős hitelesítés kiszolgáló felhasználói portált a üzemeltető kiszolgálón üzembe helyezése

Annak érdekében, hogy az Azure többtényezős hitelesítést alkalmazást használja, az alábbiak szükségesek, hogy az alkalmazás sikeresen kommunikálni felhasználói portálon:

Tekintse át a hardver és szoftverkövetelmények a hardver- és szoftverkövetelményei:

- Használatával az v6.0 vagy újabb Azure többtényezős hitelesítést kiszolgáló.
- Felhasználói portál kell telepíteni egy internetes webkiszolgálóra Microsoft® Internet Information Services (IIS) 6.x, az IIS 7.x vagy újabb verziójában.
- IIS használatakor 6.x, győződjön meg róla, ASP.NET v2.0.50727 van regisztrált és engedélyezett értékűre.
- Szerepkör-szolgáltatások szükséges, az IIS használatakor 7.x vagy újabb ASP.NET és az IIS 6 metabázis kompatibilitási.
- Az SSL-tanúsítvány titkosítani kell felhasználói portálon.
- Azure többtényezős hitelesítést Web Service SDK telepíteni kell az IIS 6.x, az IIS 7.x vagy újabb verziójában, amely az Azure többtényezős hitelesítést kiszolgáló telepítve van a kiszolgálón.
- Az SSL-tanúsítvány védeni kell az Azure többtényezős hitelesítést Web Service SDK.
- Felhasználói portál tud csatlakozni az Azure többtényezős hitelesítést Web Service SDK SSL kell lennie.
- Felhasználói portál kell tudni hitelesítést végezni az Azure többtényezős hitelesítés Web Service SDK "PhoneFactor rendszergazdák" nevű biztonsági csoport tagja szolgáltatásfiók hitelesítő adataival. Szolgáltatásfiók és a csoport található az Active Directory, ha a tartományhoz tartozó kiszolgálón fut az Azure többtényezős hitelesítést kiszolgáló. Szolgáltatásfiók és a csoport létezik helyileg a kiszolgálón Azure többtényezős hitelesítés, ha nem csatlakozik egy tartományt.

A felhasználó portál kívül az Azure többtényezős hitelesítést Server kiszolgálón telepítéséhez az alábbi három lépést:

1. A webszolgáltatás SDK telepítése
2. A felhasználó portál telepítése
3. Felhasználói beállításait Portal Server Azure többtényezős hitelesítés


### <a name="install-the-web-service-sdk"></a>A webszolgáltatás SDK telepítése

Ha Azure többtényezős hitelesítést Web Service SDK még nem telepítette az Azure többtényezős hitelesítést kiszolgálón, nyissa meg a kiszolgáló, és nyissa meg az Azure többtényezős hitelesítést kiszolgáló. Kattintson a Web Service SDK ikonra, kattintson a szolgáltatás telepítése SDK... gombot, és kövesse az utasításokat mutatják be. A Web Service SDK az SSL-tanúsítvány is védeni szeretné. Önaláírt tanúsítvány rendben erre a célra, de van, hogy megbízzon tanúsítványt az SSL-kapcsolat kezdeményezésekor a felhasználó portál webkiszolgálón helyi számítógépfiók "Megbízható legfelső szintű hitelesítésszolgáltatók" tárolóba importálandó.

<center>![A telepítő](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>A felhasználó portál telepítése

A felhasználó portál külön kiszolgálón a telepítés előtt kell szem előtt az alábbiakat:

- Célszerű nyisson meg egy webböngészőt a internetes webkiszolgálón, és keresse meg azt a Web Service SDK a fájlt a beírt URL-CÍMÉT. Ha a böngésző érheti el a webszolgáltatás sikeresen, akkor kell hitelesítő adatokat kérhet. Írja be a felhasználónevét és jelszavát, pontosan úgy, ahogyan a fájlban megjelenik a fájlt a megadott. Győződjön meg arról, hogy nincs hiba vagy tanúsítvány figyelmeztetés jelenik meg.
- Ha egy fordított proxykiszolgáló vagy a tűzfalat a felhasználó portál webkiszolgáló elé székhelye és hajt végre, mert az SSL, szerkesztheti a felhasználói portál fájlt, és adja hozzá a következő kulcsot a <appSettings> , hogy a felhasználó portál használható http helyett https szakaszban. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>A felhasználó portál telepítése

1. Nyissa meg a Windows Intézőt az Azure többtényezős hitelesítést Server kiszolgálón, és keresse meg azt a mappát, ahová az Azure többtényezős hitelesítést kiszolgáló telepítve van (például C:\Program Files\Multi varianciaanalízis hitelesítési Server). Válassza a MultiFactorAuthenticationUserPortalSetup telepítőfájlt tetszés szerint a kiszolgáló, amely a felhasználó portál telepítendő 32 bites vagy 64 bites verzióját. Másolja a telepítőfájlt a internetes kiszolgálóra.
2. Az internetes webkiszolgálón telepítőfájl rendszergazdai engedélyekkel kell futtatni. A művelet legkönnyebben nyisson meg egy parancssorablakot rendszergazdaként, és keresse meg azt a helyet, ahová a telepítőfájlt másolta.
3. Futtassa a MultiFactorAuthenticationUserPortalSetup64 telepítés fájlt, módosítása a webhely és a virtuális könyvtár nevét, ha szükségesnek látja.
4. A felhasználó portál a telepítés befejezése, után keresse meg a C:\inetpub\wwwroot\MultiFactorAuth (vagy a virtuális könyvtár neve alapján megfelelő könyvtárra), és szerkesztheti a fájlt.
5. Keresse meg a USE_WEB_SERVICE_SDK billentyűt, és módosítsa az értékét HAMIS értéket Igaz értékre. Keresse meg a WEB_SERVICE_SDK_AUTHENTICATION_USERNAME WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD billentyűkombinációt, és állítsa az értékeket a felhasználónevét és jelszavát a szolgáltatásfiók, amelynek tagja a PhoneFactor rendszergazdák biztonsági csoportot (lásd a fenti vonatkozó követelményei című). Ne felejtse el a felhasználónév és jelszó megadása legyen a sor végén az idézőjelek között (érték = "" / >). Ajánlott használatára minősített felhasználónév (pl.: TARTOMANY\felhasznalonev vagy machine\username)
6. Keresse meg a pfup_pfwssdk_PfWsSdk beállítást, és módosítsa a Web Service SDK az Azure többtényezős hitelesítés (pl. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) kiszolgálón futó URL-címében a "http://localhost:4898/PfWsSdk.asmx" értéket. SSL-e a kapcsolat használja, mivel hivatkoznia kell a Web Service SDK kiszolgáló nevét, és nem IP-cím, mivel az SSL-tanúsítvány fog állítottak ki a kiszolgáló nevét, és a használt URL-cím meg kell egyeznie a tanúsítvány neve. Ha a kiszolgálónév nem úgy, hogy IP-címet az internetes kiszolgálóról, a szöveg beszúrása a hosts fájlt az Azure többtényezős hitelesítést-kiszolgáló nevének hozzárendelése IP-címének a kiszolgálón. Miután végzett módosítások, mentse a fájlt.
7. Ha a webhelyet, hogy a felhasználó portál telepítette a (pl. alapértelmezett webhely) már nem binded nyilvánosan aláírt tanúsítványt, a tanúsítvány telepíteni a kiszolgálón Ha nincs már telepítve van, nyissa meg az IIS-kezelő, és a tanúsítvány kötni a webhely.
8. Nyisson meg egy webböngészőt minden olyan számítógépről, és nyissa meg azt az URL-címet, ahová telepítve van a felhasználó Portal (pl. https://www.publicwebsite.com/MultiFactorAuth). Győződjön meg arról, hogy nincs hiba vagy tanúsítvány figyelmeztetés jelenik meg.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Felhasználói beállításait portal Server Azure többtényezős hitelesítés
Most, hogy a portálon telepítve van, akkor konfigurálnia kell a Azure többtényezős hitelesítést kiszolgáló dolgozhat a portálon.

Azure többtényezős hitelesítés kiszolgáló a felhasználó portál több lehetőséget nyújt.  Az alábbi táblázat ezeket a beállításokat, és a használatuk magyarázatát biztosít.

Felhasználói portál beállításai|Leírás|
:------------- | :------------- |
Felhasználói portál URL-címe| Írja be az URL-cím, ahol a portálon is teszi lehetővé.
Elsődleges hitelesítés| Milyen típusú hitelesítéssel használni a portálra bejelentkezéskor megadását teszi lehetővé.  A Windows, Radius vagy LDAP-hitelesítést.
Lehetővé teszi a felhasználóknak, hogy jelentkezzen be|Lehetővé teszi a felhasználóknak a felhasználónév és jelszó beírása a bejelentkezési lapján a felhasználó portál.  Ha nincs bejelölve, a mezőkben szürke lesz.
Felhasználói regisztrációs engedélyezése|Többtényezős hitelesítés a telepítő kéri a további információt, például a telefonszám megjelenő vételével igénylése vetni.  Kérdezze meg a biztonsági másolat telefon lehetővé teszi a felhasználóknak adjon meg egy másodlagos telefonszámot.  Külső Rákérdezés elfogadható jogkivonat lehetővé teszi a felhasználóknak a 3 fél elfogadható jogkivonat adja meg.
Engedélyezheti a felhasználóknak kezdeményezzen One-Time figyelmen kívül hagyása| Így a felhasználók egy egyszeri figyelmen kívül hagyása elindítására.  Ha egy felhasználó beállítása a felfelé lép, hogy a következő befolyásolja a felhasználó bejelentkezik a.  Rákérdezés figyelmen kívül hagyása másodperc ad a felhasználónak egy mezőt, módosíthatja az alapértelmezett 300 másodperc.  Ellenkező esetben az egyszeri figyelmen kívül hagyása lehetőség csak a 300 másodperc jó.
Engedélyezheti a felhasználóknak mód kiválasztása| Lehetővé teszi a felhasználóknak az elsődleges kapcsolatfelvételi mód megadása.  Ez lehet a telefonhívás, a szöveges üzenet, a mobilalkalmazásban vagy a elfogadható jogkivonat.
Engedélyezheti a felhasználóknak nyelvének kiválasztása|  Lehetővé teszi a felhasználóknak, amellyel a telefonhívás, a szöveges üzenet, a mobilalkalmazásban vagy a elfogadható jogkivonat nyelvének módosítása.
Felhasználóimnak, hogy aktiválja a mobilalkalmazásban| Lehetővé teszi a felhasználóknak a kommunikálni a kiszolgálóval használt mobilalkalmazás aktiválási folyamat befejezéséhez az aktiválási kód létrehozása.  Is beállíthatja, hogy azok is aktiválhatja ezt a eszközök száma.  Között 1 és 10.
Visszalépés biztonsági kérdések használata|Lehetővé teszi, hogy biztonsági kérdések többtényezős hitelesítés nem sikerül.  Megadhatja, hogy sikeresen megválaszolandó biztonsági kérdések számát.
Felhasználók külső elfogadható jogkivonat társítása| Lehetővé teszi a felhasználóknak egy harmadik fél elfogadható token adja meg.
Visszalépés elfogadható jogkivonat használata|Lehetővé teszi, hogy egy elfogadható jogkivonat használatára vonatkozó abban az esetben, ha nem sikeres többtényezős hitelesítés.  A munkamenet-időtúllépés perc is megadhatja.
A naplózás engedélyezése|Lehetővé teszi, hogy a felhasználó portálon naplózás.  A naplófájlok helye: C:\Program Files\Multi varianciaanalízis hitelesítési Server\Logs.

Ezek a beállítások a legtöbb jelennek meg a felhasználót, ha engedélyezve van, és a felhasználó jelek őket a felhasználó portálra.

![Felhasználói portál beállításai](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>A felhasználói beállításait portal Server Azure többtényezős hitelesítés




1. Azure többtényezős hitelesítést kiszolgáló kattintson a felhasználó portál ikonra. A beállítások lapon adja meg az URL-címet a felhasználó portál URL-címe mezőben lévő értéket a felhasználó-portálra. Ez az URL-címe lesz illeszteni a e-mailben küldött felhasználók importáláskor be az Azure többtényezős hitelesítést webkiszolgálóra Ha engedélyezve van-e-mailek funkcióit.
2. Válassza ki az, hogy a felhasználó portálon használni kívánt beállításokat. Ha például engedélyezett a hitelesítési módszereket szabályozhatja a felhasználók számára, győződjön meg róla, hogy engedélyezése a felhasználóknak mód kiválasztása be van jelölve, és a módszerek közül választhat.
3. Kattintson a Súgó hivatkozásra a súgó jelenik meg a beállítások ismertetése, a jobb felső sarokban.

<center>![A telepítő](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>A rendszergazdák lap
Ezen a lapon adhatja meg rendszergazdai jogosultságokkal rendelkező felhasználó hozzáadása egyszerűen.  A rendszergazda hozzáadásakor finomabb kapnak engedélyeket.  Ezzel a módszerrel biztos lehet, hogy csak a rendszergazda számára a szükséges engedélyeket.  Egyszerűen kattintson a Hozzáadás gombra, és válassza ki és a felhasználó és engedélyeiket, és kattintson a Hozzáadás gombra.

![Felhasználói portál rendszergazdák](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Biztonsági kérdések
Ezen a lapon adja meg, hogy a felhasználók kell megadnia a válaszokat, ha a használati biztonsági kérdések alaplekérdezések beállítás ki van jelölve a biztonsági kérdések teszi lehetővé.  Azure többtényezős Authenticaton kiszolgáló alapértelmezett kérdéseket használható megtalálható.  Is sorrendjének módosítása vagy hozzáadása a saját kérdéseit.  Fel saját kérdéseit, megadhatja a nyelvi meg szeretné jeleníteni, valamint e kérdést.

![Felhasználói portál biztonsági kérdések](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Átadott munkamenetek

## <a name="saml"></a>SAML
Lehetővé teszi a felhasználó portál fogadja el a egy identitásszolgáltató használata a SAML követelések beállítása.  Adja meg az időkorlát-munkamenetet, adja meg a hitelesítési tanúsítvány és a napló átirányítási URL-cím meg.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Megbízható IP-címei
Ezen a lapon adja meg, hogy ha egy felhasználónak bejelentkezés a az alábbi IP-címek egyikét, majd többtényezős hitelesítés elmarad adható IP-címtartományok vagy egyetlen IP-címek teszi lehetővé.

![Felhasználói portálon meghatalmazott IP-címei](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Önkiszolgáló felhasználói regisztrációs
Ha azt szeretné, hogy a felhasználóknak, hogy jelentkezzen be, és igénylése, jelölje be a bejelentkezési a felhasználóknak, és lehetővé teszi a felhasználó regisztrációs beállítások. Ne feledje, hogy a megadott beállításoktól hatással van a felhasználónak bejelentkezés módja.

Például ha egy felhasználó jelentkezik be a felhasználó-portálra, és a Log In gombra kattint, azok majd vesznek az Azure többtényezős hitelesítést felhasználói beállítási lapra.  Attól függően, hogy hogyan Azure többtényezős hitelesítés van beállítva a felhasználó lehet jelölje ki a hitelesítési módszert.  

Válassza a hangposta hívása hitelesítési mód vagy lett előre konfigurált ezzel a módszerrel, a lap kéri a felhasználónak meg kell adnia az elsődleges telefonszám és a bővítmény, ha van ilyen.  Azok is engedélyezhető az adja meg a biztonsági másolat telefonszámot.  

![Felhasználói portálon meghatalmazott IP-címei](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Ha a felhasználónak van szükség a PIN-kód során, a lapot is kérni fogja őket a adja meg a PIN-kódot.  Miután beírta a telefonszámmal vagy telefonszámokkal és a PIN-kód (ha van), a felhasználó a hívás, most hitelesítés gombra kattint.  Azure többtényezős hitelesítés elvégez egy telefonhívás hitelesítés, a felhasználó elsődleges telefonszámra.  A felhasználó a telefonhívás fogadása, és adja meg a PIN-kód (ha van) és nyomja le a # helyezheti a önálló regisztrációs folyamat a következő lépéssel.   

Ha a felhasználó kijelöli az SMS szöveg hitelesítési módszer, vagy már előre konfigurált ezzel a módszerrel, a lap kéri a felhasználó saját mobiltelefonszámát.  Ha a felhasználónak van szükség a PIN-kód során, a lapot is kérni fogja őket a adja meg a PIN-kódot.  Miután beírta a telefonszám és a PIN-kód (ha van), a felhasználó most gombra kattint a szöveg, a hitelesítés gombra.  Azure többtényezős hitelesítés elvégez egy SMS-hitelesítés a felhasználó mobiltelefonra.  A felhasználónak kell kapniuk az SMS, amely tartalmaz egy – idő-PIN kód (OTP) és válasz az üzenetre, hogy OTP és a PIN-kód, ha van ilyen) áthelyezése a következő lépés a önálló regisztrációs folyamat.

![Az SMS felhasználói portál](./media/multi-factor-authentication-get-started-portal/text.png)   

Ha a felhasználó kijelöli a mobilalkalmazásban hitelesítési módszer, vagy már előre konfigurált ezzel a módszerrel, a lap kéri a felhasználó az eszköze az Azure többtényezős hitelesítés alkalmazás telepítése és aktiválási-kód létrehozása.  Az Azure többtényezős hitelesítés alkalmazás telepítése után a felhasználó kattint az aktiválási kód létrehozása gombra.    

>[AZURE.NOTE]A felhasználó használatához az Azure többtényezős hitelesítés alkalmazást, engedélyeznie kell az eszköze leküldéses értesítések.

Az oldal akkor aktiváló kódot és URL-címet a vonalkód képpel együtt jeleníti meg.  Ha a felhasználónak van szükség a PIN-kód során, a lapot is kérni fogja őket a adja meg a PIN-kódot.  A felhasználó belép az aktiválás kódot és az URL-CÍMÉT az Azure többtényezős hitelesítés alkalmazásba, vagy használja a vonalkód képolvasóval szeretne képet beolvasni a vonalkód képet, és az aktiválás gombra kattint.    

Az aktiválás befejeződése után a felhasználó a hitelesítő most gombra kattint.  Azure többtényezős hitelesítés végez a felhasználó mobilalkalmazás-hitelesítést.  A felhasználó kell adja meg a PIN-kód (ha van), és nyomja le a hitelesítés gomb a mobilalkalmazásban helyezheti a következő lépés a önálló regisztrációs folyamat.  


Ha a rendszergazdák az Azure többtényezős hitelesítés kiszolgáló biztonsági kérdések és válaszok összegyűjtéséhez van beállítva, akkor a felhasználó majd venni, biztonsági kérdések lapjára.  A felhasználó a kell jelölje ki a négy biztonsági kérdések, és adja meg a kijelölt kérdésekre adott válaszok.    

![Felhasználói portál biztonsági kérdések](./media/multi-factor-authentication-get-started-portal/secq.png)  

A felhasználó saját tagság elkészült, és a felhasználó be van jelentkezve a felhasználó portálon.  Felhasználók jelentkezhetnek ismét a felhasználó portál bármikor későbbi módosíthatja a telefonszámot, PIN kódokat biztosítanak, hitelesítési módszereket és biztonsági kérdések, ha az azok a rendszergazdák által megengedett.
