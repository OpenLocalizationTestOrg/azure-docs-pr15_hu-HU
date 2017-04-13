<properties
   pageTitle="Szolgáltatás háló fürt erőforrás-kezelő - alkalmazás csoportok |} Microsoft Azure"
   description="Az alkalmazás csoport funkciókat be a szolgáltatás háló fürt erőforrás-kezelő – áttekintés"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introduction-to-application-groups"></a>Bevezetés a csoportokba alkalmazás
Erőforrás-kezelő szolgáltatás háló fürt általában úgy a betöltés (metrikus keresztül jelöli) egyenletesen egész a fürt kezeli a fürt erőforrásait. Szolgáltatás háló is kezeli a fürt és a fürt csomópontjai minőségében a teljes kapacitásának hány az állomástól keresztül. A különböző típusú munkaterhelésekből, de néha hozása további követelmények a különböző háló szolgáltatásalkalmazás-példányok használó mintázatok sok nagyszerű működik. Néhány további követelmények jellemzően:

- Azt jelenti, hogy bizonyos csomópontok számának alkalmazás példány szolgáltatások kapacitásának lefoglalása
- Azt jelenti, hogy a teljes futtathatnak engedélyezett szolgáltatások alkalmazáson belül egy adott csoportját csomópontok számának korlátozása
- Az alkalmazás-példányt, magát kapacitás definiáló érdekében korlátozza a szám vagy a szolgáltatások található összes erőforrás felhasználás

Annak érdekében, hogy felel meg ezeknek a követelményeknek, ezeket hívjuk alkalmazás csoportok támogatása fejlesztett azt.

## <a name="managing-application-capacity"></a>Alkalmazás kapacitás kezelése
Az alkalmazások, továbbá a teljes metrikus betöltés arra, hogy az alkalmazás-példány egyéni csomóponton átnyúló csomópontok számának korlátozása alkalmazás kapacitás használható. Is használható az alkalmazás a fürt erőforrások lefoglalására.

Alkalmazás kapacitás beállítható, hogy az új alkalmazások létrehozásukkor; is frissíthető meglévő alkalmazásokhoz létrehozott alkalmazás kapacitás nélkülire.

### <a name="limiting-the-maximum-number-of-nodes"></a>A maximális csomópontok számának korlátozása
A legegyszerűbb használatieset-alkalmazás kapacitás esetén, az alkalmazás példányosítási kell lennie a csomópontok egy bizonyos maximális száma korlátozott. Ha nincs alkalmazás kapacitás van megadva, a szolgáltatás háló fürt erőforrás-kezelő fog hozható létre (terheléselosztás vagy töredezettségmentesítés), normál szabályoknak megfelelő kópiák általában azt jelenti, hogy szolgáltatásai között az összes rendelkezésre álló csomópontok a fürt húzza szét, amely, vagy ha töredezettségmentesítési néhány tetszőleges, de kisebb csomópontok számának be van kapcsolva.

Az alábbi képen látható egy alkalmazás példány potenciális helyzetének nélkül definiált csomópontok maximális számát, és majd ugyanazt a kérelmet a csomópontok maximális számát adja meg. Ne feledje, hogy nincs mely kópiák vagy példányai, amelyek szolgáltatások fog első elhelyezett együtt készített garanciáit.

![Alkalmazás-példány maximális csomópontok számának meghatározása][Image1]

A bal oldali példában az alkalmazás nincs beállítása alkalmazás beosztását, és három szolgáltatások van. CRM tett logikai döntés széthúzása összes másolatnál hat rendelkezésre álló csomópontok végig az ajánlott egyenleg a fürt elérése érdekében. Megfelelő esetben láthatja, hogy korlátozza a három csomópontot, és hol szolgáltatás háló CRM elérhető-e a replikák alkalmazás szolgáltatások legjobb egyenlege ugyanazt a kérelmet.

Ez a jelenség vezérlőket paraméter neve MaximumNodes. A paraméter megadása alkalmazás létrehozása során, vagy frissített alkalmazás példány volt már fut, amely, amelyben a eset szolgáltatás háló CRM megtartása lesz a replikák alkalmazás szolgáltatások csomópontot a definiált maximális száma.

A PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Alkalmazás mértékek, a betöltés és a kapacitás
Alkalmazás csoportok is lehetővé teszi a mértékek társított egy adott alkalmazás-példányt, valamint azokat a mértékek kapcsolatban az alkalmazás a kapacitás meghatározása. Így például adható meg, amely sok szolgáltatás van szüksége sikerült létrehozni

Minden mérőszám, 2 értékek vannak, amelyek az adott alkalmazás példányhoz kapacitásának írja le:

-   Alkalmazás teljes kapacitásának – az alkalmazás egy adott mérőszám teljes kapacitásának jelöli. Szolgáltatás háló CRM megpróbálja a szeretné korlátozni a megadott érték; ez az alkalmazás szolgáltatások metrikus terhelések összege Ezenkívül van az alkalmazás-szolgáltatások már használata más felfelé ezt a korlátot betöltése, ha háló fürt erőforrás-kezelő szolgáltatás nem engedélyezi az új szolgáltatásokat vagy a teljes betöltés válassza ezt a korlátot fölé okozó partíciót kibocsátása.
-   Maximális csomópont kapacitás – Itt adhatja meg a maximális replikáinak az alkalmazás-szolgáltatások teljes terhelés egyetlen csomóponton. Ha teljes betöltése a csomópontra a kapacitás fölé kerül, a szolgáltatás háló CRM megpróbálja kópiák áthelyezése más csomópontok, hogy a kapacitás kényszer betartják.

## <a name="reserving-capacity"></a>Kapacitás foglalása
Egy másik közös az alkalmazás-csoportok használata annak érdekében, hogy a fürt belül erőforrások vannak fenntartva az adott alkalmazás példány, még akkor is, ha az alkalmazás-példányt a benne lévő szolgáltatások még nincs, vagy még akkor is, ha ezek az erőforrások még nem fogyasztása. Most tekintsünk át egy, akinek működéséről.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>A lehető legkevesebb csomópontok és erőforrás-foglaláshoz megadása
Az alkalmazás-példány néhány további paramétereket meg kell adni a erőforrások lefoglalására: *MinimumNodes* és *NodeReservationCapacity*

- MinimumNodes – hasonlóan egy cél maximális számát megadó csomópontot, amely az alkalmazáson belüli szolgáltatások futtatását is lehetővé teszi, a is megadhatja, hogy az alkalmazás működnie kell a csomópontok a lehető legkevesebb. Ez a beállítás hatékony határozza meg, amely legalább az erőforrásokat kell foglalható a csomópontok számának belül az alkalmazás-példány létrehozása részeként a fürt kapacitás biztosítása.
- NodeReservationCapacity – a NodeReservationCapacity az alkalmazáson belül minden mérőszám definiálható. Az alkalmazás bármely csomóponton, amelyen a replikák bármelyike és benne a szolgáltatások példányai található fenntartva metrikus betöltés mennyiségét határozza meg.

Most tekintsünk át egy példa kapacitás Foglalás:

![Szolgáltatásalkalmazás-példányok fenntartott kapacitás meghatározása][Image2]

A bal oldali példában alkalmazások nem rendelkezik bármely alkalmazás kapacitás meghatározása. Háló fürt erőforrás-kezelő szolgáltatást fogja egyenleg az alkalmazás gyermek szolgáltatások kópiák, és a példányok együtt azokat más szolgáltatások (kívül az alkalmazás) ahhoz, hogy a fürt egyenlege.

A példában a jobb oldalon, tegyük fel, hogy az alkalmazás létrejött a egy értéke 2, 3 és 50 és 100, teljes alkalmazás kapacitása csomóponton 20, max kapacitásának foglalás célokkal definiált metrikus alkalmazás beállítása MaximumNodes MinimumNodes szolgáltatás lefoglalja a kék alkalmazáshoz két csomóponton kapacitás, és nem teszi lehetővé a fürt minőségben felhasználása más kópiájába. Ez a fenntartott alkalmazás kapacitás felhasznált és a fennmaradó fürt kapacitással ellen megszámlált veszi figyelembe.

Az alkalmazás a Foglalás létrehozásakor a fürt erőforrás-kezelő lefoglalása-e a MinimumNodes teljesítményű * NodeReservationCapacity a fürt, de nem lefoglalása adott csomóponton kapacitás mindaddig, amíg az alkalmazásszolgáltatások replikálását hozza létre és helyét. Ebben a csoportban adhatja rugalmasság érdekében óta csomópontok van kiválasztva, az új másolatnál csak ha létrehozásuk. Egy adott csomóponton kapacitás foglalt legalább egy replika rajta felépítése után.

## <a name="obtaining-the-application-load-information"></a>Az alkalmazás betöltés információk beszerzése
Az egyes alkalmazások, hogy alkalmazás kapacitás definiálva van, szerezhet be az összesítő terhelés kópiák szolgáltatásai által jelzett vonatkozó információkat. Szolgáltatás háló PowerShell és a felügyelt API-val lekérdezések biztosít, erre a célra.

Ha például betöltés tudja visszaszerezni az alábbi PowerShell-parancsmag használatával:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

A lekérdezés eredményét az alkalmazást, például a csomópontok minimális és maximális csomópontok megadott alkalmazás kapacitás alapvető információkat fogja tartalmazni. Is információt az alkalmazás éppen használó csomópontok számának lesz. Ily módon minden betöltés mérőszám lesz információk kapcsolatban:
- : Metrikus név a mérőszám.
-   Foglalás kapacitás: Ehhez az alkalmazáshoz a fürt van fenntartva fürt kapacitás.
-   Alkalmazás terhelés: Ez az alkalmazás gyermek kópiák teljes terhelés.
-   Alkalmazás kapacitás: Alkalmazás betöltés értékének megengedett legnagyobb.

## <a name="removing-application-capacity"></a>Alkalmazás kapacitás eltávolítása
Miután az alkalmazás kapacitás paraméterek be vannak állítva egy alkalmazást, ezeket lehet eltávolítani frissítés alkalmazása API-hoz vagy a PowerShell-parancsmagok használata. Példa:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Ez a parancs az összes alkalmazás kapacitás paraméter eltávolítja az alkalmazást, és háló fürt erőforrás-kezelő szolgáltatás indul el az alkalmazás kezelésének mint bármely más alkalmazás nincs telepítve a következő paraméterek definiált. A parancs hatása azonnali, és fürt erőforrás-kezelő Ez az alkalmazás az összes alkalmazás kapacitás paramétert is törli. Megadja őket, akkor a megfelelő paramétereket hívható frissítés alkalmazása API-k szükséges.

## <a name="restrictions-on-application-capacity"></a>Alkalmazás kapacitás korlátozásai
Nincsenek több korlátozásai alkalmazás kapacitás paraméterek, hogy be kell tartani. Abban az esetben, ha az adatérvényesítési hibák a létrehozási vagy a frissítés az alkalmazás elutasításra kerül hibát.
Az összes egész paraméterek nem negatív számnak kell lennie.
Ezenkívül az egyes paraméterek korlátozások a következők:

-   MinimumNodes soha nem MaximumNodes nagyobbnak kell lennie.
-   Ha a betöltés metrikus kapacitásának meg vannak adva, majd azok kell követendő szabály
  - Csomópont foglalás kapacitásának nem lehet nagyobb, mint a maximális csomópont kapacitásának. Például nem korlátozza a kapacitás mérőszám "Processzor" 2 egység a csomóponton, és minden csomóponton 3 egység próbáljon meg.
  - Ha MaximumNodes van megadva, majd MaximumNodes és maximális csomópont kapacitás szorzatát nem lehet nagyobb, mint a teljes alkalmazás kapacitása. Például ha a maximális csomópont kapacitás "Processzor" 8, és állítson be a maximális csomópontok 10 betöltés mérőszám, akkor teljes alkalmazás kapacitás kell nagyobb, mint a betöltés mérőszám 80.

A korlátozás akkor lép életbe, mind (a ügyféloldali) létrehozását és alatt (a kiszolgálóoldali) az alkalmazásfrissítést. Létrehozásakor, Íme egy példa a követelmények óta MaximumNodes törlése megsértése < MinimumNodes és a parancs sikertelen lesz az ügyfélprogramban a kérelem még szolgáltatás háló fürthöz elküldése előtt:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Példa érvénytelen frissítés a következőképpen történik. Ha egy meglévő alkalmazás készítése és frissítése csomópontok maximális valamilyen érték, a frissítés továbbítja:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Ezt követően azt megpróbálhatja minimális csomópontok frissítése:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Az ügyfél nincs elegendő az alkalmazással kapcsolatban, így lehetővé teszi a frissítést a szolgáltatás háló fürt átadandó. Azonban a fürt szolgáltatás ellenőrzi az új paraméter együtt a meglévő paramétereket és a frissítési művelet sikertelen lesz, mert foe minimális érték csomópontok értéke nagyobb, mint a maximális csomópontok értékét. Ebben az esetben alkalmazás kapacitás paraméterek változatlan marad.

Ezek a korlátozások kerülnek, az erőforrás felülete engedélyezni szeretné, hogy adja meg a legjobb helyzetének replikáinak alkalmazások szolgáltatások sorrendben helyen.

## <a name="how-not-to-use-application-capacity"></a>Hogyan nem használható az alkalmazás kapacitásának

-   Ne használja az alkalmazás kapacitás csomópontok alkalmazása meghatározott részhalmazát szeretné korlátozni: Bár a szolgáltatás háló biztosítja, hogy az egyes alkalmazások, amely tartalmazza a megadott alkalmazás kapacitás felhasználók nem döntse el, a program megjelenítésre csomópontok csomópontok maximális betartják. Ez lehet elérni elhelyezési kényszerek szolgáltatások használata.
-   Az alkalmazás kapacitásának ne használja annak érdekében, hogy az ugyanarra alkalmazásból két szolgáltatás mindig kerülnek egymás mellett. Ez a szolgáltatások affinitás viszonya használatával érhető el, és lehet, hogy csak a szolgáltatások, amelyek a ténylegesen kerüljön közös korlátozni affinitás.

## <a name="next-steps"></a>Következő lépések
- Az elérhető lehetőségekről további információt a témakör nézze át az erőforrás-kezelő fürt konfigurációk Services elérhető [szolgáltatások beállításával kapcsolatos további tudnivalók](service-fabric-cluster-resource-manager-configure-services.md)
- Megtudhatja, hogyan fürt az erőforrás-kezelő kezeli, és a fürt betöltés egyenlege, olvassa el a cikk a [terheléselosztás betöltése](service-fabric-cluster-resource-manager-balancing.md)
- Indítása az elején és [egy ismertetés, hogy a szolgáltatás háló fürt erőforrás-kezelő](service-fabric-cluster-resource-manager-introduction.md)
- Mértékek működése általában kapcsolatos további tudnivalókért olvassa el a [szolgáltatás háló betöltés](service-fabric-cluster-resource-manager-metrics.md) mérési módja miatt
- Az erőforrás-kezelő fürt túl sok a fürt leíró lehetőséget. Megtudhatja, hogy több róluk nézze meg a [szolgáltatás háló fürt ismertető](service-fabric-cluster-resource-manager-cluster-description.md) cikkben


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
