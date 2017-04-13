<properties 
    pageTitle="Első lépések a MFA kiszolgáló Mobile alkalmazást webszolgáltatás"
    description="Az Azure többtényezős hitelesítést alkalmazás kínálja fel további sávon hitelesítést.  Lehetővé teszi a MFA-kiszolgálót, használja a felhasználóknak a leküldéses értesítéseket."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>Első lépések a MFA kiszolgáló Mobile alkalmazást webszolgáltatás

Az Azure többtényezős hitelesítést alkalmazás kínálja fel további sávon hitelesítést. Helyezze a egy automatizált telefonhívás vagy SMS-felhasználónak bejelentkezés során, hanem Azure többtényezős hitelesítés verembe küldi értesítés az Azure többtényezős hitelesítést alkalmazásba a felhasználó okostelefonon vagy táblagépen. A felhasználói egyszerűen koppintás a "Hitelesítés" (vagy megad egy PIN kódot, és koppintás a "Hitelesítés") való bejelentkezéshez az alkalmazásban.

Annak érdekében, hogy az Azure többtényezős hitelesítést alkalmazást használja, az alábbiak szükségesek, hogy az alkalmazás sikeresen kommunikálni Mobile alkalmazást webszolgáltatás:

- Tekintse át a hardver és a szoftverkövetelményekkel a hardver- és szoftverkövetelményei
- Használatával az v6.0 vagy újabb Azure többtényezős hitelesítést kiszolgáló
- Mobil alkalmazás webszolgáltatás kell telepíteni egy internetes webkiszolgálóra Microsoft® Internet Information Services (IIS) IIS 7.x vagy újabb verziójában.  További információt az IIS [IIS.NET](http://www.iis.net/)látható.
- Győződjön meg róla, ASP.NET v4.0.30319 van regisztrált és engedélyezett beállítása
- ASP.NET és 6 metabázis kompatibilitás az IIS-szükséges szerepkör szolgáltatások tartalmazzák
- Mobil alkalmazás webszolgáltatás egy nyilvános URL-CÍMEN keresztül érhető el kell lennie.
- Az SSL-tanúsítvány is védeni Mobile alkalmazást webszolgáltatásból.
- Azure többtényezős hitelesítést Web Service SDK telepíteni kell az IIS 7.x vagy újabb verziójában a kiszolgálón, amely az Azure többtényezős hitelesítést kiszolgáló
- Az SSL-tanúsítvány védeni kell az Azure többtényezős hitelesítést Web Service SDK.
- Mobil alkalmazás webszolgáltatás tud csatlakozni az Azure többtényezős hitelesítést Web Service SDK SSL kell lennie.
- Mobil alkalmazás webszolgáltatás kell tudni hitelesítést végezni az Azure többtényezős hitelesítés Web Service SDK "PhoneFactor rendszergazdák" nevű biztonsági csoport tagja szolgáltatásfiók hitelesítő adataival. Szolgáltatásfiók és a csoport található az Active Directory, ha a tartományhoz tartozó kiszolgálón fut az Azure többtényezős hitelesítést kiszolgáló. Szolgáltatásfiók és a csoport létezik helyileg a kiszolgálón Azure többtényezős hitelesítés, ha nem csatlakozik egy tartományt.


A felhasználó portál kívül az Azure többtényezős hitelesítést Server kiszolgálón telepítéséhez az alábbi három lépést:

1. A webszolgáltatás SDK telepítése
2. A mobilalkalmazásban webszolgáltatás telepítése
3. Többtényezős hitelesítés Server Azure mobilalkalmazás konfigurálása
4. Az Azure többtényezős hitelesítést alkalmazás aktiválásához a végfelhasználók számára

## <a name="install-the-web-service-sdk"></a>A webszolgáltatás SDK telepítése

Ha Azure többtényezős hitelesítést Web Service SDK még nem telepítette az Azure többtényezős hitelesítést kiszolgálón, nyissa meg a kiszolgáló, és nyissa meg az Azure többtényezős hitelesítést kiszolgáló. Kattintson a Web Service SDK ikonra, kattintson a szolgáltatás telepítése SDK... gombot, és kövesse az utasításokat mutatják be. A Web Service SDK az SSL-tanúsítvány is védeni szeretné. Önaláírt tanúsítvány rendben erre a célra, de van, hogy megbízzon tanúsítványt az SSL-kapcsolat kezdeményezésekor a felhasználó portál webkiszolgálón helyi számítógépfiók "Megbízható legfelső szintű hitelesítésszolgáltatók" tárolóba importálandó.

<center>![A telepítő](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>A mobilalkalmazásban webszolgáltatás telepítése
A mobilalkalmazásban webszolgáltatás a telepítés előtt kell szem előtt az alábbiakat:

- Ha az Azure többtényezős hitelesítést felhasználói portál már telepítve van az internetes kiszolgálón, a felhasználónév, a jelszó és az URL-CÍMÉT a Web Service SDK is másolja a felhasználó portál fájlt.
- Célszerű nyisson meg egy webböngészőt a internetes webkiszolgálón, és keresse meg azt a Web Service SDK a fájlt a beírt URL-CÍMÉT. Ha a böngésző érheti el a webszolgáltatás sikeresen, akkor kell hitelesítő adatokat kérhet. Írja be a felhasználónevét és jelszavát, pontosan úgy, ahogyan a fájlban megjelenik a fájlt a megadott. Győződjön meg arról, hogy nincs hiba vagy tanúsítvány figyelmeztetés jelenik meg.
- Ha egy fordított proxykiszolgáló vagy a tűzfalat a Mobile alkalmazást webszolgáltatás webkiszolgáló elé székhelye és hajt végre, mert az SSL, szerkesztheti a Mobile alkalmazást webszolgáltatás fájlt, és adja hozzá a következő kulcsot a <appSettings> , hogy a Mobile alkalmazást webszolgáltatás használható http helyett https szakaszban. Azonban SSL továbbra is szükség a Mobile alkalmazásból a tűzfal/fordított proxykiszolgáló. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>A mobilalkalmazásban webszolgáltatás telepítése

<ol>
<li>Nyissa meg a Azure többtényezős hitelesítést kiszolgáló a Windows Intézőt, és nyissa meg azt a mappát, ahová az Azure többtényezős hitelesítést kiszolgáló telepítve van (például C:\Program Files\Azure többtényezős hitelesítés). Válassza az Azure többtényezős AuthenticationPhoneAppWebServiceSetup telepítőfájlt tetszés szerint a kiszolgáló, amely a Mobile alkalmazást webszolgáltatás telepítendő 32 bites vagy 64 bites verzióját. Másolja a telepítőfájlt a internetes kiszolgálóra.</li>

<li>Az internetes webkiszolgálón telepítőfájl rendszergazdai engedélyekkel kell futtatni. A művelet legkönnyebben nyisson meg egy parancssorablakot rendszergazdaként, és keresse meg azt a helyet, ahová a telepítőfájlt másolta.</li>  

<li>Futtassa a többtényezős AuthenticationMobileAppWebServiceSetup telepítés fájlt, módosítja a webhelyet, ha azt szeretné, és módosítsa a virtuális könyvtár egy rövid nevet, például "PA". Egy rövid virtuális mappanév akkor javasolt, mert a felhasználónak kell megadnia a Mobile alkalmazást webes szolgáltatás URL-címe a mobileszközön be az aktiválás során.</li>

<li>A Befejezés az Azure többtényezős AuthenticationMobileAppWebServiceSetup telepítése után keresse meg a C:\inetpub\wwwroot\PA (vagy a virtuális könyvtár neve alapján megfelelő könyvtárra), és szerkesztheti a fájlt.</li>  

<li>Keresse meg a WEB_SERVICE_SDK_AUTHENTICATION_USERNAME WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD billentyűkombinációt, és állítsa az értékeket a felhasználónevét és jelszavát a szolgáltatásfiók, amelynek tagja a PhoneFactor rendszergazdák biztonsági csoportot (lásd a fenti vonatkozó követelményei című). Lehet, hogy ugyanazt a fiókot, ha, amely korábban már telepítve van az Azure többtényezős hitelesítést felhasználói portál identitással használatban. Ne felejtse el a felhasználónév és jelszó megadása legyen a sor végén az idézőjelek között (érték = "" / >). Minősített felhasználónév (pl.: TARTOMANY\felhasznalonev vagy machine\username) használata ajánlott.</li>  

<li>Keresse meg a pfMobile alkalmazás webes Service_pfwssdk_PfWsSdk beállítást, és módosítsa a Web Service SDK az Azure többtényezős hitelesítés (pl. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) kiszolgálón futó URL-címében a "http://localhost:4898/PfWsSdk.asmx" értéket. SSL-e a kapcsolat használja, mivel hivatkoznia kell a Web Service SDK kiszolgáló nevét, és nem IP-cím, mivel az SSL-tanúsítvány fog állítottak ki a kiszolgáló nevét, és a használt URL-cím meg kell egyeznie a tanúsítvány neve. Ha a kiszolgálónév nem úgy, hogy IP-címet az internetes kiszolgálóról, a szöveg beszúrása a hosts fájlt az Azure többtényezős hitelesítést-kiszolgáló nevének hozzárendelése IP-címének a kiszolgálón. Miután végzett módosítások, mentse a fájlt.</li>  

<li>Ha a webhelyet, hogy a Mobile alkalmazást webszolgáltatás telepítette a (pl. alapértelmezett webhely) már nem binded nyilvánosan aláírt tanúsítványt, a tanúsítvány telepíteni a kiszolgálón Ha nincs már telepítve van, nyissa meg az IIS-kezelő, és a tanúsítvány kötni a webhely.</li>  

<li>Nyisson meg egy webböngészőt minden olyan számítógépről, és nyissa meg azt az URL-címet, ahová a Mobile alkalmazást webszolgáltatás telepítve van (például https://www.publicwebsite.com/PA). Győződjön meg arról, hogy nincs hiba vagy tanúsítvány figyelmeztetés jelenik meg.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Többtényezős hitelesítés Server Azure mobilalkalmazás konfigurálása
Most, hogy a mobilalkalmazásban webszolgáltatás telepítve van, akkor konfigurálnia kell a Azure többtényezős hitelesítést kiszolgáló dolgozhat a portálon.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Többtényezős hitelesítés Server Azure mobilalkalmazás beállításainak

1. Azure többtényezős hitelesítést kiszolgáló kattintson a felhasználó portál ikonra. Ha a felhasználók számára engedélyezett vezérlő a hitelesítési módszereket, a beállítások lapon jelölje be a módszerrel engedélyezése a felhasználóknak a ellenőrzése Mobile alkalmazásban. Ezzel a funkcióval nélkül a végfelhasználók számára kötelező a lépjen kapcsolatba a belső segítő a Mobile alkalmazáshoz aktiválás befejezéséhez.
2. Jelölje be a felhasználóknak aktiválása mobilalkalmazás mezőbe.
3. A felhasználó regisztrációs engedélyezése jelölőnégyzetet.
4. Kattintson a Mobile-alkalmazás ikonjára.
5. Írja be az URL-címet a virtuális könyvtár az Azure többtényezős AuthenticationMobileAppWebServiceSetup telepítésekor létrehozott használatban. Egy fióknevet rögzítheti a beviteli mezőbe. A vállalat nevét a mobilalkalmazás jeleníti meg. Ha üresen hagyja a mezőt, a többtényezős hitelesítés szolgáltató által létrehozott az adatkezelési portálon Azure neve jelennek meg.



<center>![A telepítő](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
