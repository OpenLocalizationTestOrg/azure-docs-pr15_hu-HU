<properties
    pageTitle="Alkalmazás regisztrációs portál súgótémaköröket |} Microsoft Azure"
    description="A Microsoft alkalmazás regisztrációs portál számos szolgáltatásainak leírása."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>Alkalmazás regisztrációs hivatkozás
A dokumentum tartalmaz a környezet és a Microsoft alkalmazás regisztrációs Portal [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)található különböző szolgáltatások leírása.

## <a name="my-applications"></a>Saját alkalmazások
A lista tartalmazza az összes, az alkalmazás regisztrált az Azure Active Directory 2.0-s verzió végpontot való használatra.  Ezeket az alkalmazásokat az azt jelenti, hogy jelentkezzen be Microsoft-fiók és Azure Active Directory munkahelyi vagy iskolai fiókot a két személyes fiókokkal rendelkező felhasználók van.  Többet szeretne tudni az Azure Active Directory 2.0-s verzió végpontot, hogy a saját [2.0-s verzió – áttekintés](active-directory-appmodel-v2-overview.md)című témakörben találhat.  Ezeket az alkalmazásokat is használható a Microsoft fiók hitelesítési végpont integrálása `https://login.live.com`.

## <a name="live-sdk-applications"></a>Élő SDK alkalmazások
Ez a lista tartalmazza az összes, az alkalmazás regisztrált kizárólag a Microsoft-fiókkal való használatra.  Nincsenek engedélyezve Azure Active Directory bármilyen való használatra.  Itt volt korábban regisztrálva MSA developer Portal segítségével az alkalmazásokat fogja meg `https://account.live.com/developers/applications`.  Az összes függvény, amely a korábban elvégzett `https://account.live.com/developers/applications` most végezheti el az e-új portálon `https://apps.dev.microsoft.com`.  Ha a Microsoft-fiók alkalmazásokkal kapcsolatos bármilyen további kérdései vannak, forduljon us.

## <a name="application-secrets"></a>Alkalmazás titkos kulcsok
Alkalmazás titkos kulcsok, amelyek lehetővé teszik az alkalmazás megbízható [ügyfél-hitelesítés](http://tools.ietf.org/html/rfc6749#section-2.3) az Azure Active Directory hitelesítő.  A OAuth és OpenID csatlakozni, az alkalmazás titkos kulcsok gyakran nevezik egy `client_secret`.  A 2.0-s verzió protokoll, bármely alkalmazásban, amely a biztonsági jogkivonat egy webes címmel rendelkező helyen napokon (használata egy `https` séma) azonosítja magát, a biztonsági jogkivonat visszaváltás Azure AD-alkalmazás titkos kulcsot kell használnia.  Ezenkívül bármilyen natív ügyfele, hogy recieves jogkivonatok a eszköz fog tiltott ügyfél-hitelesítés, végezze el az alkalmazás titkos segítségével nem biztonságos környezetben titkos kulcsok tárolására kívánó felhasználókat.

Minden alkalmazás két érvényes alkalmazás titkos kulcsok, adott bármikor is tartalmazhat, az idő.  Két titkos kulcsok jelenti, ha a ablilty végig az alkalmazás teljes környezetben periodikus kulcs átváltási végrehajtásához.  Miután áttelepítette a teljes egészében egy új titkos az alkalmazás, a régi titkos törlése, és egy új kiépítése.

Jelenleg csak kétféle alkalmazás titkos kulcsok vannak engedélyezve, az alkalmazás regisztrációs portálon.  Válassza az **Új jelszó létrehozása** készíthet, és egy megosztott titkos tárolni a megfelelő adattár, amelynek az alkalmazás használatával.  Válassza az **Új kulcs generálása párt** hoz létre új nyilvános és titkos kulcs két letöltött, és az ügyfél-hitelesítéshez Azure ad.

## <a name="profile"></a>Profil
A bejelentkezési lapon, az alkalmazás testreszabása használható az alkalmazás regisztrációs portált a profil című szakaszát.  Most megváltoztathatja a bejelentkezési lap alkalmazás embléma, a szolgáltatás URL-CÍMÉT, és az adatvédelmi nyilatkozat feltételeit.  Az embléma kell lennie egy áttetsző 48 x 48 és 50 x 50 képpont beállítása GIF, PNG vagy JPEG fájlban 15 KB vagy kisebb.  Próbálja ki az értékek módosítása, és az eredményül kapott bejelentkezési oldal megtekintése!

## <a name="live-sdk-support"></a>Élő SDK támogatás
Ha engedélyezi a "Élő SDK támogatás", minden alkalmazás titkos kulcsok hoz létre mindkét az Azure Active Directory be kell építenie és a Microsoft-Account adatokat tárolja.  Ez lehetővé teszi a Microsoft-Account szolgáltatás (login.live.com) közvetlenül integrálása az alkalmazást.  Ha szeretne közvetlenül (nem pedig használja az Azure Active Directory 2.0-s verzió végpontot) használata a Microsoft Account alkalmazás összeállítása, gondoskodnia kell arról, hogy engedélyezve van-e a Live SDK támogatása.

Live SDK támogatási letiltása biztosítja, hogy az alkalmazás titkos csak bejegyzi az Azure Active Directory-adatokat tárolja.  Az Azure Active Directory-adatokat tároló ezzel a paranccsal beépíti a vállalati szintű szabályok, amelyek lehetővé teszik, hogy bizonyos szabványoknak, például a FISMA megfelelőségi felel meg.  Ha engedélyezi a Live SDK támogatását, az alkalmazás nem érhetnek el ezeket a szabványokat részét megfelelés.

Ha csak valamikor használni az Azure Active Directory 2.0-s verzió végpontot, letilthatja a Live SDK támogatási biztonságosan.

