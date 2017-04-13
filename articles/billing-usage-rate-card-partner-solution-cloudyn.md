<properties
   pageTitle="Microsoft Azure használatát és RateCard API-k engedélyezése Cloudyn ITFM nyújt a vevők |} Microsoft Azure"
   description="Microsoft Azure számlázási partner Cloudyn, a szolgáltatások az Azure számlázási API-k integrálása a termék egyedi perspektíva ismertetése  Az Azure és Cloudyn ügyfelek, amely érdekli a használatával/próbálja Cloudyn az Azure-szolgáltatások különösen hasznos."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Microsoft Azure használatát és RateCard API-k engedélyezése Cloudyn ITFM nyújt a vevők

Az új Microsoft Azure Erőforrás kihasználtsága és RateCard API-hoz a személyes kép választott Cloudyn, a Microsoft fejlesztési partner és a felhő szolgáltatásairól, a kezdő szolgáltatóként.  A használati API előfizetéshez becsült Azure felhasználás adatok hozzáférést biztosít. A RateCard API információk teljes árak az összes Azure szolgáltatás, a nem vállalati szerződés EA - ügyfeleknek. Integrált együtt, ezek API-khoz alapot részletekkel informatikai pénzügyi Management (ITFM) eszközök, például a Cloudyn által biztosított bevitt.

## <a name="introduction"></a>– Bevezetés

A használatát API adatai alapján a RateCard API-adatainak úgynevezett "szorzás" ([Mennyiség] használatát ár [$unit] részletes használatát és a költség =) hozza létre a legtöbb részletes, pontosak és megbízható számlázási adatait elérhető Azure ma.

![ITFM – áttekintés][1]

Ezek API-k használata más kulcs információt nyújt az ügyfelek használatát és költségek, lehetővé teszi olyan egyszerű, programozott módon elemzése a felhasználói fiókok és különböző ITFM feladatok végrehajtása felelősséget ügyfelei Cloudyn.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Cloudyn integrálása a RateCard és a használat API-hoz
A RateCard API több bemeneti paramétereket – például régió adatai, a pénznem és a területi beállítások – igényel, de a legfontosabb egy OfferDurableID, amely meghatározza, hogy az Azure kínáló az ügyfél típusú van (kirovó, régebbi 6-os vagy 12 hónapos lekötési tervek, MSDN ajánlatok, MPN ajánlatok, promóciós ajánlatok és használatával mások). A OfferDurableID a [Azure használatát és a számlázási portálon](https://account.windowsazure.com/Subscriptions), a "kínálnak Azonosítót" az adott előfizetés csoportban találhatók.

Az [Azure-Cloudyn](https://www.cloudyn.com/microsoft-azure/) szolgáltatások regisztrálásakor ügyfelek OfferDurableID kódjukat, amely lehetővé teszi az RateCard API-k vonatkozó árak adataikat csoportosítani Cloudyn adhat hozzá.  Ajánlatok különböző típusú információkat is található, egyet a [Microsoft Azure kínálnak részleteket tartalmazó](https://azure.microsoft.com/support/legal/offer-details/) lapot.

![Cloudyn ITFM motor – áttekintés][2]

Cloudyn használ, a használatát és a RateCard API-mellett az Azure teljesítmény API-t, további rétegek képi megjelenítés analytics, riasztási, kezelési költséget és a értekezletekre javaslatok, létrehozása az Azure ügyfelek egy megbízható vállalati felhő ITFM eszköz biztosítása.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM használjon esetekben engedélyezte a használatát és RateCard API-integráció
Közös Cloudyn ITFM használjon esetekben engedélyezte a használatát, és RateCard API-khoz tartalmazza:

+ **Költségelemzés** - lehetővé teszi, hogy a felhő költségek törött le bármilyen natív azonosító dimenzió (szolgáltató, szolgáltatás, fiókja és terület stb.). Az Azure használatát és a RateCard API-hoz, mert a legtöbb részletes használatát bontásban egy egyszerű feladat Ez legyen, és egy fiókot, majd csoportosított és Cloudyn szerint szűrt és a felhasználónak, ábra vagy táblázatos formában megjelenő adatait.

![Költség Analysis kördiagram][3]

+ **Költség terhelés 360** - lehetővé teszi, hogy Pénzügy és a tényleges költség-lebontási, illesztőprogramok és a felhő telepítés trendek Kihúzás informatikai menedzserek. További lehetővé teszi a menedzserek egyszerűen részlegek, részlegek, régiók és az egyéb költségeket felhőben való eddiginél mélyebb kezeléséről, és a vállalati chargebacks és showbacks megkönnyítése a telepítési kiadások társítani. Az Azure használatát és RateCard API-khoz bemenetként átadja kiegészíti az API-k módszerek és üzleti logikai funkcióinak nem címkézett vagy untaggable erőforrások hozzárendelése az megadásával Cloudyn a költség terhelés motor szolgálnak.

![A költség terhelés 360 diagram][4]

+ **Cost-Effective átméretezése** - jobbra méretezéshez javaslatokat tesz a virtuális gépeken futó kap elegendő tevékenységet, így csökken a vevő kiadások túl nagy méretű vagy túlságosan kiépített gépeken. Igen, vizsgálata virtuális gép Processzor és RAM mértékek (keresztül teljesítmény alkalmazásprogramozási felület), órában futási idejű (keresztül használatát API-val) és költség (keresztül RateCard API-val) alapján. Cloudyn majd jobbra méretezéshez javaslatok alapján kap elegendő tevékenységet Processzor vagy RAM erőforrások (teljesítményét) tartalmaz, valamint az ár delta (RateCard) szorzás becsült megtakarítási között a VMs a tényleges idő-kihasználtság (használat) kap elegendő tevékenységet a gép szerint számítja ki.

![Tényleges költség átméretezése][5]

+ **Felhőalapú portolása javaslatok** - pénzügyi tanácsokat felhő portolása. A felhasználó aktuális költségek felhő erőforrások fő felhő szállítók telepítendő megvizsgálja, és összehasonlítja azt a Azure-egyenértékű telepítési költségét. Ezután felajánl részletes, per-erőforrás, pénzügyi-alapú portolása Azure javaslatokat. A szükséges (mértékek és a felhasználói beállítások párbeszédpanel teljesítmény alapján) Azure egyenértékű példányban értékelésekor után Cloudyn a RateCard API-t használja a költség a Azure egyenértékű helyezési ki szeretné számítani.

+ **Teljesítményjelentéseinek** - engedélyezte a Azure a teljesítmény API, ezeket a jelentéseket optimalizálása a javaslatok Processzor- és RAM kihasználása funkciók tömbjét adja meg. Az alábbi képen a példány kihasználtsági jelentés példát, példány breakdown bemutató átlagos Processzor kihasználtsági szerint.

![Teljesítményjelentéseinek][6]

+ **Kategória manager** - Cloudyn unorganized felhő erőforrások sorrendben magasabbra, hogy egy hatékony funkciója. Felhasználók biztosít, amelyek megfelelnek a üzletvitelbe mellett hozhat létre saját egyedi kategóriák (címkék) hatékony mérési és kimutatások készítése céljából. További, a felhasználók könnyen szabályozására és kategorizálása a várttól eltérően működik címkézés (azaz a írta és az egyéb eltérések) és automatikus észlelése nem címkézett anyagok pontos költség alapon.

![Kategória Manager][7]

## <a name="video"></a>A videó

Íme egy rövid videót, amely mutatja az Azure ügyfele használatát az Azure és Azure számlázási API Cloudyn háttérismeretek az Azure felhasználási adataikat eléréséhez.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Következő lépések

+ Nyissa meg [az Azure Cloudyn](https://www.cloudyn.com/microsoft-azure/) kattintva megtekintheti, hogy miként szerezhet be költség áttetszőség testre szabott jelentéskészítéssel ingyenes próbaverziót, és a Microsoft Azure elemzésének cloud telepítési.
+ Lásd: [szerezhet be a Microsoft Azure erőforrás-felhasználás háttérismeretek](billing-usage-rate-card-overview.md) az Azure Erőforrás kihasználtsága és RateCard API-khoz áttekintése.
+ Nézze meg az [Azure számlázási REST API -val hivatkozás](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mindkét API-hoz, az Azure-kezelő által biztosított API-khoz halmazára részét képező további információt.
+ Ha a kívánt kördiagramjaira jobbra a példakódot be, olvassa el a Microsoft Azure számlázási API mintakódok a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>tudj meg többet
+ Ha többet szeretne megtudni a Microsoft Azure nagyvállalati szerződés (EA) ajánlatok, látogasson el [licencelési Azure vállalati] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Ha többet szeretne tudni az Azure erőforrás-kezelő [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md) témakörben.
+ További információt az a felhő megértéséhez elveszti töltött, olvassa el a Gartner cikkére [Market útmutató informatikai pénzügyi Management (ITFM) eszközök](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)segítségével szükséges eszközök csomagot.

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
