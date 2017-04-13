<properties
    pageTitle="Azure alkalmazás szolgáltatás és hatása a meglévő Azure-szolgáltatások"
    description="Ebből a cikkből megtudhatja, milyen hatással van az új alkalmazás Azure szolgáltatás és szolgáltatásai meglévő szolgáltatások Azure-ban."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure alkalmazás szolgáltatás és a meglévő Azure-szolgáltatások

Ez a cikk a meglévő Azure szolgáltatások módosultak ismertet a közös számos Azure szolgáltatás vihet át [Azure alkalmazás szolgáltatás](https://azure.microsoft.com/services/app-service/), egy új integrált ajánlja fel a módosítás részeként.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>– Áttekintés

[Azure alkalmazás](https://azure.microsoft.com/services/app-service/) szolgáltatása egy új és egyedi felhőszolgáltatásában, amely lehetővé teszi, hogy a fejlesztők hozhat létre web és mobilalkalmazásai bármely platform és bármilyen eszközről. Alkalmazás-szolgáltatás az integrált megoldáskészítő célja, hogy egyszerűbb ismételt coding funkciók integrálása a vállalati és a szoftver rendszerek és üzleti folyamatok automatizálása igényeinek megfelelően biztonsági, megbízhatóságának és méretezhetőség: az értekezlet közben nem.

Alkalmazás szolgáltatás az egyetlen, a következő meglévő Azure szolgáltatások - [webhelyek](https://azure.microsoft.com/services/websites/), a [Mobil szolgáltatások](https://azure.microsoft.com/services/mobile-services/)és a [Biztalk szolgáltatások](https://azure.microsoft.com/services/biztalk-services/) megoldássá egyetlen kombinált szolgáltatás hatékony új funkciók hozzáadása közben.  Alkalmazás szolgáltatás lehetővé teszi a következő alkalmazás típusú szolgáltató:

-   Web Apps alkalmazások
-   Mobilalkalmazások
-   API-alkalmazások
-   Logika alkalmazások

Az alábbi táblázat ismerteti, hogyan meglévő Azure szolgáltatások térkép alkalmazás szolgáltatás és az alkalmazás típusok benne.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Meglévő Azure szolgáltatás</th>
<th align="left", style="width:10%">Azure alkalmazás szolgáltatás</th>
<th align="left", style="width:80%">Mi változott</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure webhelyek</td>
<td align="left">Web Apps alkalmazások</td>
<td align="left"><li>A Azure webhelyeket alkalmazás szolgáltatás nem feltétlenül korlátozott Web Apps alkalmazások webhelyek nevének módosítása.
<p><li>A webhely meglévő előfordulását állnak a Web Apps alkalmazást szolgáltatásban.</p>
<p><li>A meglévő webhely az <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure-portálon</a>, ahol megtalálja a meglévő webhelyeken a <em>Web Apps alkalmazások</em>keresztül is elérheti.</p>
<p><li><em>Webes szolgáltatója megtervezése</em> ettől kezdve <em>Alkalmazás szolgáltatás megtervezése</em>. Az <em>Alkalmazás szolgáltatás megtervezése</em> üzemeltetheti bármilyen alkalmazás típusú alkalmazás szolgáltatást, például a webes, Mobiltelefonról, logika vagy API-alkalmazások.</p>
<p><li>Azure alkalmazás Web Apps alkalmazások az általános elérhetőségét.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">További tudnivalók a Web Apps alkalmazások</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobil szolgáltatások</td>
<td align="left">Mobilalkalmazások</td>
<td align="left"><p><li>Mobil szolgáltatások továbbra is elérhető verziót különálló szolgáltatásként, és továbbra is a teljes mértékben támogatott.</p>
<p><li>Mobile-alkalmazások egy olyan alkalmazás típus, az alkalmazás-szolgáltatása, amelynek egyesíti az összes Mobile Services és az egyéb funkcióit.</p>
<p><li>Nagyon egyszerűen <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">Mobile-alkalmazások Mobile szolgáltatások</a>áttelepítése.</p>
<p><li>Alkalmazás szolgáltatás részét képező az Mobile-alkalmazások beszerzése integráció a helyszíni és a szoftver rendszerek, például túl Mobile Services, az új funkciók helyek, WebJobs, jobban méretezési beállításokat, és az egyéb átmeneti.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">További tudnivalók a Mobile-alkalmazások</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API-alkalmazások</td>
<td align="left">
<p><li>API-alkalmazások olyan új alkalmazás alkalmazás szolgáltatás, amellyel egyszerűen összeállítása és felhasználása API-hoz a felhőben.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">További tudnivalók az API-alkalmazások</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logika alkalmazások</td>
<td align="left">
<p><li>Logika alkalmazások olyan új alkalmazást az App szolgáltatás, amellyel egyszerűen az üzleti folyamatok automatizálása.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">További tudnivalók a logika alkalmazások</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk szolgáltatások</td>
<td align="left">BizTalk API-alkalmazások</td>
<td align="left">
<li><p>BizTalk szolgáltatások továbbra is elérhető verziót különálló szolgáltatásként, és továbbra is a teljes mértékben támogatott.</p>
<li><p>BizTalk szolgáltatások összes funkcióját közös alkalmazás szolgáltatás alkalmazás szolgáltatás vállalati alkalmazás integrálása és az alkalmazás a fenti B2B-integrációs forgatókönyvek végrehajtásához API-alkalmazások engedélyezése felhasználóként</p>
<li><p>Logika alkalmazással automatizálása most az üzleti folyamatok egy vizuális tervezési hatékonyságát használatáról a munkafolyamatok létrehozása</p></td>
</tr>
</tbody>
</table>

További információért látogasson el [alkalmazás szolgáltatás dokumentációja](https://azure.microsoft.com/documentation/services/app-service/).
