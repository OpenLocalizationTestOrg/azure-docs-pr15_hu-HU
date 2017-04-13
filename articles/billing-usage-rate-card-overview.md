<properties
   pageTitle="Szerezzen be a Microsoft Azure erőforrás-felhasználás háttérismeretek |} Microsoft Azure"
   description="A háttérismeretek az Azure erőforrás-felhasználás és a trendek megadására szolgáló Azure számlázás használatát és RateCard API-hoz, elvi áttekintése."
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

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Nyereség mélyebb be a Microsoft Azure erőforrás-felhasználás

Ügyfelei és partnerei csak az azt jelenti, hogy pontosan előrejelzésére és azok Azure költségek kezelésére.  Mozgás egy Capex a Opex modellre, az azt jelenti, hogy végezze el az összehasonlítás visszaterhelési analysis showback, valamint biztosítanak mód funkcióvesztést becslés és a számlázásra, különösen a nagy felhő telepítések a is szükséges.

Az Azure Erőforrás kihasználtsága és ráta kártya API-khoz tárgyalt Ez a cikk a cím ezeket az igényeinek, mivel az Azure erőforrások felhasználása az új összefüggésekre.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Az Azure Erőforrás kihasználtsága és RateCard API-khoz bemutatása

Az erőforrás-kihasználtság Azure és RateCard API-khoz egy erőforrás szolgáltatóként API-k által az Azure erőforrás-kezelő téve a család részeként szerepelni fog.  

### <a name="azure-resource-usage-api-preview"></a>Azure erőforrás-kihasználtság API (előzetes verzió)
Ügyfelei és partnerei segítségével az Azure az Erőforrás kihasználtsága API becsült Azure felhasználás adataikat. A funkciók:

- **Azure szerepköralapú hozzáférés-vezérlés** - ügyfelek és partnerek a hozzáférési házirendek beállíthatók úgy, az [Azure portál](https://portal.azure.com) vagy [Azure PowerShell-parancsmagok](powershell-install-configure.md) megadhatja, hogy mely felhasználók vagy alkalmazások hozzáférhet az előfizetés használati adatainak keresztül. A hívók hitelesítési szabványos Azure Active Directory tokenek kell használnia. A hívó is hozzá kell adnia a olvasó, tulajdonosának vagy a közös munka szerepkör érheti el a használati adatok adott Azure előfizetéshez.

- **Óránkénti vagy napi összesítések** - hívók megadhatja, hogy azok szeretné-e az Azure használati adatainak óradíj időszakok vagy napi időszakok. Az alapértelmezett érték napi.

- **Megadott példány metaadatokat (erőforrás-címkéket tartalmazza)** – példány szintű részleteket, például a teljesen minősített erőforrás uri (/subscriptions/ {előfizetés azonosítója} / …), az erőforrás együtt csoport adatait, és az erőforrás címkék biztosítja a válaszban. Ezzel deterministically segít a felhasználóknak és programozás útján kiosztani használatát a címkéket, amelyet, a használati-esetek tetszik idegen töltés alatt.

- A válasz, a hívók mi lett felhasznált átláthatóbbá adjon is átadott **megadott erőforrás metaadat** - erőforrás részleteket, például állapotjelzője nevét, állapotjelzője kategória, állapotjelzője alkategória, egység és régió. Erőforrás-metaadatok terminológia igazítása az Azure portálon keresztül is dolgozunk Azure használatát CSV, a számlázási CSV és egyéb nyilvános élményt ügyfelek adatok összehangolására keresztül szolgáltatások engedélyezése EA.

- **Az ajánlat diagramtípusokat felhasználását** – használati adatok lesznek elérhetők az összes ajánlat típusok kirovó, az MSDN webhelyen, pénzbeli lekötési, pénzbeli vizsgálat és EA beleértve többek között.

### <a name="azure-resource-ratecard-api-preview"></a>Azure erőforrás RateCard API (előzetes verzió)
Ügyfelei és partnerei segítségével az Azure erőforrás RateCard API a lista beszerzése az elérhető Azure erőforrások együtt minden egyes árinformációkat becsült. A funkciók:

- **Azure szerepköralapú hozzáférés-vezérlés** - ügyfelek és partnerek a hozzáférési házirendek beállíthatók úgy, az [Azure portál](https://portal.azure.com) vagy [Azure PowerShell-parancsmagok](powershell-install-configure.md) megadhatja, hogy mely felhasználók vagy alkalmazások hozzáférhet az RateCard adatok keresztül. A hívók hitelesítési szabványos Azure Active Directory tokenek kell használnia. A hívó is hozzá kell adnia a olvasó, tulajdonosának vagy a közös munka szerepkör érheti el a használati adatok adott Azure előfizetéshez.

- Azure ajánlat szintű ráta adatait, és az előfizetés szintű **kirovó, az MSDN, a pénzügyi lekötési, és a pénzbeli hitelképesség támogatása kínál (nem támogatott EA)** – Ez API találja.  Ez az API kíván a hívó féllel kell eltelnie az erőforrások részleteinek és mértékek ajánlat adatait.  EA ajánlatok testreszabott díjak száma a regisztrációs, ahogy azt nem lehet adja meg a EA díjak adott időben.

## <a name="scenarios"></a>Felhasználási területei

Íme néhány végzett a használatát, ha a RateCard API-khoz is módosíthat az jelenik meg:

- A **hónap során töltött azure** - ügyfelek használhatja a használatát, és RateCard API-khoz együtt jobb háttérismeretek az a felhő megszerezni használatát és a költség becslést óránkénti és a napi időszakok elemzésével a hónapban televíziózással töltenek.

- **Állíthat be értesítéseket** – ügyfelei és partnerei állíthatja be a felhőben felhasználási erőforrás- vagy pénzbeli alapú értesítéseket a becsült felhasználás és a használat és a RateCard API segítségével ingyenesen becslés első.

- **Predict számla** – ügyfelei és partnerei is használható lesz a becsült felhasználás és a felhő töltött és gépi tanulási algoritmusok előre, Mik azok a számla lenne számlázási ciklustól végén alkalmazása.

- **Költségelemzés előtti felhasználás** – ügyfelek lehet is a használata a RateCard API előre, hogy mennyi a számla lenne, lépjen a munkaterhelésekből Azure-szerint nyújtó voltak kívánt számok használatát. Ha a vevők más felhőket vagy magánjellegű felhőket meglévő munkaterhelésekből, azok is képezhető a használatát, az Azure egy a becsült Azure jobb becslése megszerezni díjak televíziózással töltenek. Ez a bővített funkciójú nézetének mi szerezhető be a [Azure árak Számológép](https://azure.microsoft.com/pricing/calculator/)keresztül biztosít, (például a) a számlázási partnerek tartalmaznak-e a ajánlat kimutatása és összehasonlítási és kontraszt túl kirovó, például pénzbeli lekötési és pénzbeli hitelképesség különböző ajánlat típusok között. Az API-khoz is biztosít a az azt jelenti, hogy a költség terület módosításait becslés engedélyezése a lehetőségelemzés a világ különböző DCS nyekhttp://www.Office.com/redir/xt103979639 erőforrások teljes költségét közvetlen hatással vannak is telepítési döntések szükséges típusát.

- **Lehetőségelemzés** -

    - Ügyfelei és partnerei megállapítható, hogy egy másik tartományban lévő, vagy egy másik konfigurálásról az Azure erőforrás futtatásához a munkaterhelésekből költséghatékonyabb lenne. Azure erőforrás költségek eltérhetnek az Azure régió fut, amelyben alapján, és így ügyfelei és partnerei költség optimalizálásokat eléréséhez.

    - Ügyfelei és partnerei is megadhatja, ha egy másik Azure ajánlat típusú jobb díjának kínál az Azure-erőforrás.

## <a name="partner-solutions"></a>A partnermegoldások

[A Microsoft Azure használati és RateCard API -k engedélyezése Cloudyn ITFM adja meg az ügyfeleknek való](billing-usage-rate-card-partner-solution-cloudyn.md) ismerteti a [Cloudyn](https://www.cloudyn.com/microsoft-azure/)Azure számlázási API-partnerek által kínált integrációs funkciói.  Ebben a cikkben részletes körét a szolgáltatások, például egy rövid videót, amely bemutatja, hogyan az Azure ügyfele használható Cloudyn és az Azure számlázási API-nyereség mélyebb az Azure felhasználási adataikat.

[Felhőalapú Cruiser és a Microsoft Azure számlázási API integráció](billing-usage-rate-card-partner-solution-cloudcruiser.md) a [felhő Cruiser Express Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) működése közvetlenül a WAP portálról segítségével, így zökkenőmentesen kezelése a Microsoft Azure magánjellegű vagy szolgáltatott nyilvános felhő működési és pénzügyi tulajdonságát egyetlen kezelőfelületen ügyfelek ismerteti.   

## <a name="next-steps"></a>Következő lépések
+ Nézze meg a [Azure számlázási REST API -val hivatkozás](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mindkét API-hoz, az Azure-kezelő által biztosított API-khoz halmazára részét képező rendszeren.
+ Ha a kívánt kördiagramjaira jobbra a példakódot be, olvassa el a Microsoft Azure számlázási API mintakódok a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>tudj meg többet
+ Ha többet szeretne tudni az Azure erőforrás-kezelő [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md) témakörben.
+ További információt az a felhő megértéséhez elveszti töltött, olvassa el a Gartner cikkére [Market útmutató informatikai pénzügyi Management (ITFM) eszközök](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)segítségével szükséges eszközök csomagot.
