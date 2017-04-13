<properties
    pageTitle="A rendelkezésre álló összekötők és az API-alkalmazások listájában |} Microsoft Azure alkalmazás szolgáltatás"
    description="További információ: az összekötők és Azure App szolgáltatásban API-alkalmazások"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Összekötők és az API-alkalmazások használata a logika alkalmazásokhoz listája
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik. Logika alkalmazások általános elérhetőségű kiadás verzióhoz tartozó listája [Új összekötők](../connectors/apis-list.md).

Tudnivalók a rendelkezésre álló összekötők és a API-alkalmazások használata a összefüggés-alkalmazásokból a Microsoft által létrehozott.

Árak adatait, és mi megtalálható minden szolgáltatási réteg a listáját, [Azure alkalmazás szolgáltatás árak](https://azure.microsoft.com/pricing/details/app-service/)talál.

> [AZURE.NOTE] Első lépésként logika alkalmazások, mielőtt feliratkozna az Azure-fiók [Próbálja logika alkalmazás](https://tryappservice.azure.com/?appservice=logic)megnyitásához. Létrehozhat egy rövid életű starter logika alkalmazás azonnal App szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="core-connectors"></a>Alapvető összekötők
Az alábbi táblázat felsorolja az összes rendelkezésre álló összekötők és API-alkalmazások hozta létre a Microsoft Core összekötők előfizetésként érhető el:

név | Leírás
--- | ---
[A Bing fordító](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Szöveg fordítása más nyelvre történő a Bing segítségével.
[HTTP](app-service-logic-connector-http.md) | A HTTP-figyelő megnyílik egy végpontot, amely végpontként HTTP-kiszolgálón, és figyeli a bejövő HTTP vagy HTTPS felkérést. A HTTP-művelet használatához nincs szükség az API-alkalmazásokban és támogatja natív módon összefüggés-alkalmazásokból.
[Tartalékidő](app-service-logic-connector-slack.md) | Csatlakozás tartalékidő és tartalékidő csatornák üzenetet tesz közzé.


## <a name="enterprise-integration-connectors"></a>Nagyvállalati integrációs összekötők
Az alábbi táblázat felsorolja az összes rendelkezésre álló összekötők és vállalati integrációs összekötők érhető el a Microsoft által létrehozott API-alkalmazások:

név  | Leírás
------------- | -------------
[BizTalk szabályok](app-service-logic-use-biztalk-rules.md) | BizTalk szabályok segítségével határozza meg, és szabályozhatja az üzleti logikai funkcióinak szervezeten belül. Üzleti házirendek frissíthető ehhez újrafordítására sincs szükség vagy a kapcsolódó alkalmazásokhoz újbóli nélkül.
[XPath-készülék BizTalk](app-service-logic-xpath-extract.md) | Keres, és adatokat olvas XML-tartalmat a kiválasztott XPath alapján.
[DB2-összekötő](app-service-logic-connector-db2.md) | Csatlakozik az IBM DB2 adatbázis-helyszíni és az Azure virtuális gépen a Windows operációs rendszert futtató. Informix Structured Query Language parancsok is leképezhető webes API-val és a OData API műveletek. <br/><br/>Nincs indítók. Műveletek tartalmazza – táblázat kijelölése, Beszúrás, frissítés, törlés és egyéni utasítás<br/><br/>Ez az összekötő a kiszolgálóhoz való csatlakozáshoz egy Informix TCP/IP hálózaton keresztül DRDA Microsoft Client is tartalmaz.
[Fájl](app-service-logic-connector-file.md) | Ez az összekötő használata esetén csatlakozhat a helyszíni fájlrendszer vagy a hálózati és a teljes más fájl-és mappaműveletek, köztük feltöltése, törlése, fájlok nevére a partnerlistában, és más.
[Informix](app-service-logic-connector-informix.md) | Csatlakozás IBM Informix-adatbázishoz, a helyszíni és az Azure virtuális gépen a Windows operációs rendszert futtató. Informix Structured Query Language parancsok is leképezhető webes API-val és a OData API műveletek.<br/><br/>Nincs indítók. Műveletek hozzáadása a táblázat kijelölése, Beszúrás, frissítés, törlés és egyéni utasítást.<br/><br/>Amikor a helyszíni, VPN vagy a készült Azure ExpressRoute is használható. Ez az összekötő is tartalmaz egy Microsoft Client DRDA kiszolgálóhoz való csatlakozáshoz egy Informix TCP/IP hálózaton keresztül.
[A Microsoft SQL Server](app-service-logic-connector-sql.md) | Csatlakozik a helyszíni SQL Server- vagy Azure SQL-adatbázisból. Hozzon létre, frissítés, beszerzése és SQL-adatbázis táblákra bejegyzések törlése.
MQ | Csatlakozás IBM WebSphere MQ kiszolgáló verziója 8 rendszerben a helyszíni és az Azure virtuális gépen a Windows operációs rendszert futtató. Amikor a helyszíni, VPN vagy a készült Azure ExpressRoute is használható. Az összekötő is tartalmaz a Microsoft Client MQ.<br/><br/>Nincs indítók. A műveletek nem.<br/><br/>**Megjegyzés:** Jelenleg nem használhatók a összefüggés-alkalmazások használatát.

## <a name="connectors-as-triggers"></a>Eseményindítók, összekötők
Több összekötők indítók logika alkalmazások szükséges. Ezek a indítók két típusa van:

1. Szavazás indítók: A következő indítók lekérdezik a szolgáltatást, jelölje be az új adatokat szeretne meghatározott gyakorisággal. Új adatok érhető el, ha a logikai alkalmazás új példányát fut, az adatok bemeneteként. Egyező adatok megakadályozásához felhasználás alatt álló többször, az eseményindító előfordulhat, hogy adattisztítási olvasása és a logika alkalmazás átadott adatokat. Az ilyen összekötők példák a fájlt, az SQL és Azure tároló.
2. Leküldéses indítók: Ezek indítók meghallgatása végpont adatait, vagy egy eseményre. Ezt követően egy logikai alkalmazás új példányát indítja el. Az ilyen összekötők példák a HTTP-figyelő és a Twitteren.

## <a name="connectors-as-actions"></a>Az összekötők műveletek
Összekötők műveletek a logika alkalmazásokban is használható. Műveletek hasznosak az logika Appon belül is, akkor használja a végrehajtás adatok kereséséhez. Előfordulhat például, lépések elvégzésével keresheti meg adatokat egy SQL-adatbázis egy ügyfél további információt a megfelelő sorrendben feldolgozása közben. Vagy meg kell írni, módosítása vagy törlését a cél. Ezt megteheti a műveletek által az összekötők használatával. Műveletek feleltesse műveletek az API-alkalmazások (a Swagger metaadatok által meghatározott).

## <a name="create-your-own-connectors-and-api-apps"></a>A saját összekötők és az API-alkalmazások létrehozása
[Összekötők és az API-alkalmazások hivatkozás](http://aka.ms/appservicesconnectorreference)  
[Azure alkalmazás szolgáltatáshoz tartozó API-alkalmazás indítók](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logika alkalmazás hivatkozás](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>További tudnivalók a összekötők és az API-alkalmazások
[Mik azok az összekötők és BizTalk API-alkalmazások](app-service-logic-what-are-biztalk-api-apps.md)  
[A hibrid kapcsolatkezelő Azure alkalmazás szolgáltatás használata](app-service-logic-hybrid-connection-manager.md)  
[Kezelése és a beépített API-alkalmazások és összekötők figyelése](app-service-logic-monitor-your-connectors.md)
