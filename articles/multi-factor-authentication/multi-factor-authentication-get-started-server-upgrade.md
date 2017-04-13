<properties 
    pageTitle="A PhoneFactor Agent Azure többtényezős hitelesítés kiszolgáló frissítése"
    description="A dokumentum veheti használatba az Azure MFA-kiszolgáló és a régebbi phonefactor ügynök frissítéséről ismerteti."
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

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>A PhoneFactor Agent Azure többtényezős hitelesítés kiszolgáló frissítése

A PhoneFactor ügynök v5.x vagy régebbi üzeneteket, hogy az Azure többtényezős hitelesítést kiszolgáló frissítése az szükséges PhoneFactor ügynök és kapcsolt összetevők el kell távolítani, mielőtt a többtényezős hitelesítés és kapcsolt összetevői telepíthető.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>A PhoneFactor Agent frissítésének Azure többtényezős hitelesítést kiszolgáló
<ol>
<li>Először készítsen biztonsági másolatot az PhoneFactor adatfájl. Az alapértelmezett telepítési helye C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Ha a felhasználó portál telepítve van:</li>
<ol>
<li>Keresse meg a telepítés mappát, és készítsen biztonsági másolatot a fájlt. Az alapértelmezett telepítési helye C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Egyéni téma a portálon felvett, készítsen biztonsági másolatot az egyéni mappa a C:\inetpub\wwwroot\PhoneFactor\App_Themes könyvtár alatt.</li>


<li>Távolítsa el a felhasználó portál a PhoneFactor ügynök (csak akkor használható, ha telepítve van a PhoneFactor Agent ugyanazon a kiszolgálón) vagy Windows-programok és szolgáltatások keresztül.</li></ol>




<li>Ha a Mobile alkalmazást webszolgáltatás telepítve van:
<ol>
<li>Nyissa meg a telepítés mappát, és készítsen biztonsági másolatot a fájlt. Az alapértelmezett telepítési helye C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Távolítsa el a mobil alkalmazás webszolgáltatás keresztül Windows programok és szolgáltatások.</li></ol>

<li>Ha a Web Service SDK telepítve van, távolítsa el azt a PhoneFactor Agent vagy Windows-programok és szolgáltatások keresztül.

<li>Távolítsa el a PhoneFactor Agent keresztül Windows programok és szolgáltatások.

<li>Telepítse a többtényezős hitelesítés kiszolgálót. Figyelje meg, hogy a telepítés elérési van felvett az előző PhoneFactor ügynök telepítési beállításjegyzékéből azt ugyanazon a helyen (például C:\Program Files\PhoneFactor) kell telepíteni. Új telepítések egy másik alapértelmezett elérési útját (például C:\Program Files\Multi varianciaanalízis hitelesítési kiszolgáló) telepítése lesz. Az előző PhoneFactor ügynök által írt adatfájl a telepítés során lesznek frissítve, így a felhasználók és a beállítások kell továbbra is megtalálhatók az új többtényezős hitelesítést Server telepítése után.

<li>Ha a rendszer kéri, a többtényezős hitelesítést kiszolgáló aktiválása, és győződjön meg róla, a megfelelő replikációs csoport hozzá van rendelve.

<li>Ha korábban már telepítette a Web Service SDK, telepítse az új webhely szolgáltatás SDK többtényezős hitelesítést kiszolgáló felhasználói felületen. Ne feledje, hogy az alapértelmezett virtuális könyvtár neve most "MultiFactorAuthWebServiceSdk" helyett "PhoneFactorWebServiceSdk". Ha az előző nevet szeretne, módosítania kell a virtuális könyvtár nevét a telepítés során. Ha engedélyezi az új alapértelmezett nevet szeretne a telepítést, akkor módosíthatja bármely alkalmazásokban, például a felhasználó portál és a Mobile alkalmazást webszolgáltatás, mutasson a megfelelő helyen, a Web Service SDK hivatkozó URL-CÍMÉT.

<li>Ha a felhasználó portál korábban már telepítve volt a PhoneFactor ügynök kiszolgáló, telepítse az új többtényezős hitelesítést felhasználói portál többtényezős hitelesítést kiszolgáló felhasználói felületen. Ne feledje, hogy az alapértelmezett virtuális könyvtár neve most "MultiFactorAuth" helyett "PhoneFactor". Ha az előző nevet szeretne, módosítania kell a virtuális könyvtár nevét a telepítés során. Ellenkező esetben ha engedélyezi az új alapértelmezett nevet szeretne a telepítést, kell a többtényezős hitelesítést kiszolgáló felhasználói portál gombjára, és frissítse a felhasználó portál URL-CÍMÉT, kattintson a beállítások lapon.

<li>Ha a felhasználó portál és a Mobile alkalmazást webszolgáltatás korábban már telepítve volt az PhoneFactor ügynök egy másik kiszolgálón:
<ol>
<li>Nyissa meg a telepítési helye (például C:\Program Files\PhoneFactor), és másolja a vágólapra a megfelelő installer(s) más kiszolgálóra. Vannak olyan 32 bites és 64 bites telepítője az felhasználói portál és a Mobile alkalmazást webszolgáltatásból. Hívott MultiFactorAuthenticationUserPortalSetupXX.msi és MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi rendre.</li>
<li>A felhasználó portál telepítése az érintett webkiszolgálóra, nyisson meg egy parancssorablakot rendszergazdaként, és futtassa a MultiFactorAuthenticationUserPortalSetupXX.msi. Ne feledje, hogy az alapértelmezett virtuális könyvtár neve most "MultiFactorAuth" helyett "PhoneFactor". Ha az előző nevet szeretne, módosítania kell a virtuális könyvtár nevét a telepítés során. Ellenkező esetben ha engedélyezi az új alapértelmezett nevet szeretne a telepítést, kell a többtényezős hitelesítést kiszolgáló felhasználói portál gombjára, és frissítse a felhasználó portál URL-CÍMÉT, kattintson a beállítások lapon. Meglévő felhasználóknak kell tájékoztatni az új URL-címet.</li>
<li>Nyissa meg a felhasználó portálra telepítése a helye (például C:\inetpub\wwwroot\MultiFactorAuth), és szerkesztheti a fájlt. Másolja az értékeket az appSettings és applicationSettings szakaszok az eredeti fájlból web.config mentett a frissítés előtt be a új fájlt. Ha az új alapértelmezett virtuális könyvtár nevet a Web Service SDK telepítésekor lett naprakész, módosíthatja az URL-cím a applicationSettings szakaszban mutasson a megfelelő helyre. Ha bármely más Alapértelmezések módosultak a korábbi fájlt, ezekhez a változtatásokhoz alkalmazása a új fájlt.</li>
<li>A mobil alkalmazás webes szolgáltatás telepítése az érintett webkiszolgálóra, nyisson meg egy parancssorablakot rendszergazdaként, és futtassa a MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Ne feledje, hogy az alapértelmezett virtuális könyvtár neve most "MultiFactorAuthMobileAppWebService" helyett "PhoneFactorPhoneAppWebService". Ha az előző nevet szeretne, módosítania kell a virtuális könyvtár nevét a telepítés során. Érdemes válasszon rövidebb megkönnyítheti a felhasználóknak mobilkészülékükön írja be a nevet. Ellenkező esetben ha engedélyezi az új alapértelmezett nevet szeretne a telepítést, kell a többtényezős hitelesítést kiszolgáló mobilalkalmazás gombjára, és frissítse a Mobile alkalmazást webes szolgáltatás URL-címe.</li>
<li>Nyissa meg a Mobile alkalmazást webszolgáltatás telepítse helye (például C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService), és szerkesztheti a fájlt. Másolja az értékeket az appSettings és applicationSettings szakaszok az eredeti fájlból web.config mentett a frissítés előtt be a új fájlt. Ha az új alapértelmezett virtuális könyvtár nevet a Web Service SDK telepítésekor lett naprakész, módosíthatja az URL-cím a applicationSettings szakaszban mutasson a megfelelő helyre. Ha bármely más Alapértelmezések módosultak a korábbi fájlt, ezekhez a változtatásokhoz alkalmazása a új fájlt.</li></ol>
