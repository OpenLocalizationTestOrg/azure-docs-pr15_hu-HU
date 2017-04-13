<properties
    pageTitle="Az Azure-portálon Azure keresési index létrehozása |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Az Azure-portálon index létrehozása."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Az Azure-portálon Azure keresési index létrehozása
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [TÖBBI](search-create-index-rest-api.md)

Ez a cikk azt ismerteti, hogy az Azure-portálon Azure keresési [index](search-what-is-an-index.md) létrehozásának folyamatát.

Ez az útmutató következő és az index létrehozása előtt rendelkeznie kell már [létrehozott egy Azure keresési szolgáltatás](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>Lehet. Nyissa meg az Azure keresés lap
1. Kattintson a "Összes erőforrás" az [Azure portál](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) bal oldalán a menü
2. Jelölje ki az Azure keresési szolgáltatás

## <a name="ii-add-and-name-your-index"></a>II. Adja hozzá, és nevezze el az index
1. Kattintson a "Hozzáadása index" gombra
2. Adjon nevet az Azure keresési index. Ebből az útmutatóból szállodák keresett index létre azt, mivel "Szállodák" indexben van nevű.
  * Az index neve kell olyan betűvel kezdődő és csak a kisbetűket, számjegyű vagy szaggatott vonalat tartalmaz ("-").
  * A szolgáltatás neve hasonlít az index neve választhat is része lesz végpont URL-CÍMÉT hol küld a HTTP-kérések az Azure keresési API
3. Kattintson a "Mezők" tétel egy új lap megnyitása

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Hozzon létre, és adja meg a tárgymutatóhoz mezőket
1. Jelölje ki a "Mezők" szöveg, az index definíciójának beírása űrlapok egy új lap nyílik meg.
2. Mezők hozzáadása a tárgymutatóhoz a űrlapon.

  * Egy *kulcsot* Edm.String típusú akkor kötelező minden Azure keresési indexhez. A kulcs mezőjét az mező neve "azonosító" alapértelmezés szerint jön létre. Módosítottuk az azt a "hotelId" az indexben.
  * A tárgymutató-séma bizonyos tulajdonságainak is csak egyszer kell beállítani, és a jövőben nem frissíthetők. Emiatt lenne szükség, például mezőtípusok újraindexelés séma frissítéseiről sem jelenleg lehetséges kezdeti konfigurálása után.
  * Gondosan választotta, azt az egyes mezők alapján, hogy megítélése szerint hogyan az alkalmazásokban használandó értékű. A tárgymutatóhoz tervezésekor, minden mezőben a [megfelelő tulajdonságokat](https://msdn.microsoft.com/library/azure/dn798941.aspx)kell rendelni, ne feledje keresés felhasználói felület és a vállalati igényeinek megfelelően. Melyik mezőkre vonatkozó alábbi tulajdonságok szabályozására keresése szolgáltatások (szűrés, faceting, rendezés, teljes szöveges keresési, stb.). Ha például valószínű, hogy szállodák keresése a személyek lesz a "" leírásmezőre, a kulcsszó találatokat érdekli, hogy teljes szöveges keresés engedélyezése a ezt a mezőt az "Kereshető" tulajdonságot.
    * Is beállíthatja, hogy a [nyelv analyzer](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) minden egyes mezőhöz, kattintson a lap tetején a "Analyzer" fülre. Alatt láthatja, hogy azt kiválasztása után egy francia analyzer egy mező az indexben szánt francia nyelvű szöveget.

3. Kattintson az **OK gombra** kattintva erősítse meg a mező definíciók a "Mezők" lap
4. A "Hozzáadása index" lap mentéséhez és az imént megadott index létrehozása: kattintson az **OK gombra** .

Az alábbi képernyőképek megtekintheti, hogyan azt nevű és a mezők megadott "Szállodák" indexben.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Következő
Azure keresési index létrehozása után fogja töltse fel [a tartalmat az index](search-what-is-data-import.md) készen áll arra, hogy máris az adatok keresése.
