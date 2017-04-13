<properties
    pageTitle="Mi változott alkalmazás szolgáltatás API - alkalmazások |} Microsoft Azure"
    description="Megtudhatja, hogy az API-alkalmazások Azure alkalmazás szolgáltatás újdonságai."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Mi változott alkalmazás szolgáltatás API - alkalmazások

A csatlakozás az esetben a November Skype 2015 vállalati Azure alkalmazás szolgáltatás javított számos voltak [jelenti be](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). E fejlesztések közé tartoznak az alapul szolgáló változtatásokat jobban zárt beállítást a Mobiltelefonról és Web Apps, csökkentését fogalmat száma és telepítési és runtime teljesítmény javítása API-alkalmazás. Kezdési 2015 November 30., új API-alkalmazások létrehozása az Azure adatkezelési portál használatával, vagy a legújabb szerszámok tükrözni fogja a módosítások. Ez a cikk ismerteti a változtatások, valamint hogyan kihasználhatja a meglévő alkalmazások telepítsen újra.

## <a name="feature-changes"></a>Változások a szolgáltatások
Az API-alkalmazások – hitelesítés, CORS és az API-metaadatok – kulcs funkció át lett helyezve, közvetlenül az alkalmazás szolgáltatás. A módosítással a szolgáltatások érhetők el Web, a mobil és az API-alkalmazások között. Erre valójában mindhárom megosztása **Microsoft.Web/sites** erőforrás azonos típusú erőforrás-kezelő. Az API-alkalmazások átjáró már nem szükséges, vagy az API-alkalmazásokhoz kínált. Is így könnyebben használható Azure API-kezelés, mivel a csak az egyetlen API az adatkezelési átjáró lesz.

![API-alkalmazások – áttekintés](./media/app-service-api-whats-changed/api-apps-overview.png)

A fő tervezési elve az API-alkalmazások frissítéssel ahhoz, hogy ahhoz, hogy a API formátumát, a választott nyelven.  A API webalkalmazás vagy Mobile alkalmazásban már telepítve van, ha nem rendelkezik telepítsen újra az alkalmazást az új szolgáltatások előnyeit. Ha jelenleg a API App alkalmazások előzetes verziója, áttérési útmutató részletes alatt.

### <a name="authentication"></a>Hitelesítés
A meglévő kulcsrakész API-alkalmazások, a Mobile Services-alkalmazások és a Web Apps alkalmazások hitelesítési szolgáltatások van már az egyesített, és az adatkezelési portálon egyetlen Azure alkalmazás szolgáltatás hitelesítési lap érhetők el. Az alkalmazás szolgáltatás hitelesítési szolgáltatások bemutatása című [kibontása alkalmazás szolgáltatás hitelesítési / engedélyezési](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Ha API esetben számos vonatkozó új funkciók:

- **Azure Active Directory közvetlenül használatának támogatása**, hogy a AAD jogkivonat, a munkamenet jogkivonat exchange client programkód írása nélkül: A ügyfél csak elhelyezheti a AAD tokenek engedélyezési fejlécen az bearer jogkivonat szabvány szerint. Ez azt is jelenti nincs alkalmazás szolgáltatásspecifikus SDK ügyfél vagy kiszolgáló oldalán van szükség. 
- **Szolgáltatás és a "Belső" hozzáférés**: Ha egy démonhoz vagy egy másik ügyfélen erre szolgáló nélkül felületet API-hoz való hozzáférés kérése egy AAD szolgáltatás egyszerű használatával jogkivonat, és adja át alkalmazás szolgáltatás a hitelesítést az alkalmazással.
- **Elhalasztva engedélyezési**: sok alkalmazások van az alkalmazás különböző részein eltérő hozzáférés-korlátozásait. Esetleg érdemes néhány API-khoz nyilvánosan elérhető lesz, míg mások bejelentkezést igényelnek. Az eredeti hitelesítési/engedélyezési szolgáltatás volt mindent, a bejelentkezést igénylő teljes webhellyel. Ez a beállítás továbbra is fennáll, de azt is megteheti lehetővé teszi az alkalmazás kódját jeleníti meg az access döntéseket után alkalmazás szolgáltatás a felhasználót hitelesítette.
 
Az új hitelesítési szolgáltatások kapcsolatos további tudnivalókért lásd: a [hitelesítési és az API-alkalmazások Azure alkalmazás szolgáltatás engedélyezési](app-service-api-authentication.md). Információ arról, hogy miként áttelepítendő az újba meglévő API-alkalmazásokat az előző API alkalmazások modellből című [áttelepítését meglévő API](#migrating-existing-api-apps) a jelen cikk.
 
### <a name="cors"></a>CORS
Egy vesszővel tagolt **MS_CrossDomainOrigins** app beállítása, hanem van most egy lap az Azure kezelőportálja CORS konfigurációs. Másik lehetőségként konfigurálható például Azure PowerShell, CLI vagy [Erőforrás Explorer](https://resources.azure.com/)szerszámok erőforrás-kezelő használatával. A **cors** tulajdonság a **Microsoft.Web/sites/config** erőforrás típusa a ** &lt;webhelynév&gt;/webes** erőforrás. Példa:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-metaadat
Az API-definíciós lap már elérhető webhelyen, a Mobile és az API-alkalmazások között. Az adatkezelési portálon relatív URL-cím vagy az abszolút URL-címe zárólap, hogy a API állomások Swagger 2.0-s ábrázolása is megadhat. Azt is megteheti hogy konfigurálhatók erőforrás-kezelő szerszámok. A **apiDefinition** tulajdonság a **Microsoft.Web/sites/config** erőforrás típusa a ** &lt;webhelynév&gt;/webes** erőforrás. Példa:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Ekkor a metaadat-alapú végpont kell nyilvánosan hozzáférhető sok későbbi ügyfélalkalmazások (például a Visual Studio REST API-ügyfél generációs és PowerApps "API hozzáadása" forgalom) felhasználja azt hitelesítés nélkül. Ez jelent, ha alkalmazás szolgáltatás hitelesítési használ, és szeretné elérhetővé tenni a API definíció magát az alkalmazásból, meg kell adja meg a hitelesítési elhalasztott beállítást úgy, hogy az útvonalat a Swagger metaadat-alapú nyilvános ismertetett.

## <a name="management-portal"></a>Adatkezelési portál
Kijelölés **Új > Web + Mobile > API-alkalmazás** a portálon hoz létre, amelyek az új funkciók a cikkben ismertetett tükrözik API-alkalmazásokat. **Tallózás > API-alkalmazások** csak megjelenítése új API alkalmazások lesz. Keresse meg az API-alkalmazásba, ha a lap megosztja, elrendezés és a Funkciók, mint a Mobile-alkalmazások és webes. Csak különbségek a következők: quickstart útmutató tartalmat, és rendezési beállítások megadása.

Meglévő API-alkalmazások (vagy logikai alkalmazásokból létrehozott piactér API-alkalmazások) az előző mintával funkciók is látható lesz a logika alkalmazások tervezőben és összes erőforrásában található erőforrás csoport megnyitásához.

## <a name="visual-studio"></a>Visual Studio

A legtöbb Web Apps alkalmazások szerszámok az új API-alkalmazások fog működni, mivel az azonos mögöttes **Microsoft.Web/sites** erőforrástípus információikat. Az Azure Visual Studio szerszámok, azonban frissített verzióra 2.8.1 vagy későbbinek kell lennie, mert azt közzététele, hogy egy szám adott API funkciók. Töltse le a SDK csomagjában talál az [Azure letöltési oldalon](https://azure.microsoft.com/downloads/).

Az alkalmazás szolgáltatás típusú racionalizálásából közzététele is az a egyesített **Közzététel > a Microsoft Azure alkalmazás szolgáltatás**:

![API-alkalmazások közzététele](./media/app-service-api-whats-changed/api-apps-publish.png)

SDK 2.8.1 kapcsolatos további információért olvassa el a [blog közzé](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)bejelentése.

Azt is megteheti manuálisan importálhatja a közzététel profil ahhoz, hogy a közzététel az adatkezelési portálról. Jó helyen jár, felhőalapú Explorer, a kód létrehozása és az API alkalmazás kijelölés/létrehozása esetén kell SDK 2.8.1 vagy újabb verziójában.

## <a name="migrating-existing-api-apps"></a>Meglévő API-alkalmazások áttelepítése
Az egyéni API-val telepíti, a korábbi verziót API-alkalmazásokat, hogy kérése, hogy áttelepítése az új modellre az API-alkalmazások 2015 December 31-én. Mind a régi és új modell Web App szolgáltatásban tárolt API-k alapján, mivel a meglévő kódot többsége felhasználhassa őket.

### <a name="hosting-and-redeployment"></a>Tárolása és újratelepítés
Az újbóli lépéseit ugyanazok, mint bármelyik meglévő webes API-val telepíti az alkalmazás szolgáltatás. A lépéseket:

1. Hozzon létre egy üres API-alkalmazást. Ezt megteheti a portálon az új > API-alkalmazást, a Visual Studióban közzététel vagy erőforrás-kezelő szerszámok. Erőforrás-kezelő szerszámok vagy sablonok használata, a **készüléke** értékének beállítása **api** -nak, hogy a QuickStarts csomagban, és a beállítások az adatkezelési portálon nullához API esetek lépések **Microsoft.Web/sites** erőforrás típusa
2. Csatlakozás, és a projekt üzembe az üres API-alkalmazásba, az alkalmazás szolgáltatás által támogatott telepítési mechanizmusok bármelyikét használja. Olvassa el a [Azure alkalmazás szolgáltatás üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md) további információt. 
  
### <a name="authentication"></a>Hitelesítés
Az alkalmazás szolgáltatás hitelesítési szolgáltatások támogatása találhatók meg az előző API-alkalmazások modell amelyekre ugyanolyan lehetőségeket. Munkamenet tokenek használ, és csak SDK, használja az alábbi ügyfél- és kiszolgálóoldali SDK:

- [Azure mobil ügyfélprogram SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) ügyfél:
- [Microsoft Azure mobilalkalmazás .NET hitelesítési kiterjesztés](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) kiszolgáló: 

Ha inkább az alkalmazás szolgáltatás alfa SDK, ezek most elavult:

- [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService) ügyfél:
- [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service) kiszolgáló:

Különösen az Azure Active Directory azonban nem alkalmazás szolgáltatásspecifikus szükség közvetlenül a AAD jogkivonat használja.

### <a name="internal-access"></a>Belső access
Az előző API-alkalmazások modell szerepel egy beépített belső hozzáférési szintet. Aláíráshoz kérelmek a SDK felhasználása ez szükséges. Ismertetett módon, és az új API-alkalmazások modellt AAD szolgáltatás rendszerbiztonsági használható egy alternatív a szolgáltatás hitelesítést anélkül, hogy egy App szolgáltatásspecifikus SDK csomagjában talál. Megtudhatja, hogy több az [API-Appok Azure alkalmazás szolgáltatás fő hitelesítési szolgáltatás](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Feltárás
Az előző API-alkalmazások modell API-khoz volt, a többi API-alkalmazás az azonos erőforráscsoport mögött az átjárón futásidőben felfedezése. Az különösen hasznos architektúrákban, amely microservice mintázatok valósítja. Amíg ez nem közvetlenül támogatott, több lehetőség állnak rendelkezésre:

1. Az Azure erőforrás-kezelő API feltárás használatával.
2. Azure API kezelése az alkalmazás szolgáltatás által üzemeltetett API-khoz elé helyezi el. Azure API-kezelés egy homlokzati szolgál, és stabil külső néz URL-címet is adja meg, még akkor is, ha belső topológiai változásairól.
3. A saját feltárás API-alkalmazás létrehozása, és egyéb API-alkalmazások regisztrálhatja a feltárás alkalmazással az indítási van.
4. Telepítési időben a végpontjaikat többi API-alkalmazás az összes olyan API-alkalmazások (és ügyfelek) az alkalmazás beállításainak adataival. Ez a sablon környezetekben, és mivel API-alkalmazások most ad életképes a vezérlő az URL-cím.

## <a name="using-api-apps-with-logic-apps"></a>Az API-alkalmazások használata a logika alkalmazással

Az új API-alkalmazások modell jól [Logika alkalmazások séma verziója 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md)működik.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért olvassa el a cikkek [API-alkalmazások dokumentáció szakaszban](https://azure.microsoft.com/documentation/services/app-service/api/). Az új minta megfelelően az API-alkalmazások frissített. Ezeken kívül keresse meg a további részleteket vagy áttérési útmutató a fórumokban:

- [Fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Egymást fedő túlcsordulás](http://stackoverflow.com/questions/tagged/azure-api-apps)
