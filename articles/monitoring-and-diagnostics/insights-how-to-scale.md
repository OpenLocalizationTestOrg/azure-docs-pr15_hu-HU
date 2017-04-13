<properties
    pageTitle="Kézzel és automatikusan átméretezheti a példányok száma |} Microsoft Azure"
    description="További információ a szolgáltatások Azure méretezni."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Kézzel és automatikusan átméretezheti a példányok száma

Az [Azure-portálon](https://portal.azure.com/)manuálisan beállíthatja, hogy a szolgáltatás példány számát, vagy beállíthatja, hogy paramétereket pedig automatikusan skála igény szerinti alapján. Ez általában nevezik *skála,* vagy *a méretezés*.

Mielőtt példányok száma alapján méretezés, figyelembe, hogy méretezés által érintett **árak réteg** példányok száma mellett. Különböző árak rétegek beállíthatja, hogy más számozást magmintákat és a memória, és így rendelkeznek ugyanannyi példányok (amely *skála felfelé* vagy *lefelé skála*)-jobb teljesítményt. Ez a cikk bemutatja, *a méretezés* és *ki*kifejezetten.

A portál méretezheti, és használhatja a [REST API -t](https://msdn.microsoft.com/library/azure/dn931953.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) méretarányra módosíthatja, automatikusan vagy manuálisan.

> [AZURE.NOTE] Ez a cikk ismerteti, hogyan hozhat létre egy Automatikus méretezéssel beállítása a portált a [http://portal.azure.com](http://portal.azure.com). A portál létrehozott Automatikus méretezéssel beállításait nem lehet szerkeszteni, akkor a klasszikus portal ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Manuális méretezése

1. Az [Azure-portálon](https://portal.azure.com/)kattintson a **Tallózás gombra**, majd nyissa meg azt az erőforrás kívánja méretezni, például egy **alkalmazás szolgáltatáscsomagja**.

2. A **Méretezés** csempét a **Műveletek** a program tájékoztatja, méretezés állapotának: **ki** mikor meg vannak méretezés manuálisan, **a** mikor van szerint egy vagy több teljesítménymutatók méretezés.
    ![Méretezés csempe](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Kattintás a hivatkozásra kattintva megnyílik a **méret** lap. A méretezés a lap tetején látható Automatikus méretezéssel műveletek előzményeit a szolgáltatást.  
    ![Méret lap](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Csak az Automatikus méretezéssel által végrehajtott műveletek megjelenik a diagramon. Ha saját kezűleg módosítja a példányok száma, a módosítás nem jelennek a diagramon.

4. Manuálisan beállíthatja, hogy a csúszka **példányok** száma.
5. Kattintson a **Mentés** parancsra, és meg fog kell méretezni, hogy adott példányainak száma közel rögtön.

## <a name="scaling-based-on-a-pre-set-metric"></a>Egy előre beállított mérőszám méretezés alapján

Ha azt szeretné, hogy automatikusan beállíthatja példányok száma alapján egy mérőszám, válassza ki a kívánt **skála** legördülő. Ha például **alkalmazás szolgáltatáscsomagja** méretezheti **Processzor**százalékkal.

1. Amikor kijelöl egy mérőszám akkor kap egy csúszkát, és/vagy, szövegdobozok között átméretezheti kívánt példányokat számának megadására szolgáló:

    ![Processzor százalék méret lap](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Automatikus méretezéssel soha nem veszi alatt vagy fölött a határokat beállított, függetlenül attól, hogy a terhelést a szolgáltatásban.

2. Második válassza ki a mérőszám cél tartományát. Például ha **Processzor százalék**lehetőséget választotta, beállíthatja, hogy cél az átlagos processzor összes előfordulása a szolgáltatásban. Skálával ki fogja fordulhat elő, ha a az átlagos Processzor meghaladja a maximális definiálása, hasonlóan a skálával történik, amikor az átlag Processzor a minimális alá esik.

3. Kattintson a **Mentés** parancsra. Ellenőrzi, hogy az Automatikus méretezéssel néhány percenként győződjön meg arról, hogy Ön a példány tartomány és a cél a mérőszám. Ha a szolgáltatás további forgalmat kap, anélkül, hogy semmit módon kaphat további előfordulásait.

## <a name="scale-based-on-other-metrics"></a>A méretezés más mértékek alapján

A beépített, amely jelennek meg a **méretarányát** tartalmazó legördülő listára, és is rendelkeznek összetett skála ki, és szabályok méretezése eltérő alapján mértékek méretezheti.

### <a name="adding-or-changing-a-rule"></a>Megadása vagy módosítása a szabályok

1. Válassza a **Kimutatás és a teljesítmény szabályok** **skála** legördülő: ![teljesítmény szabályok](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Ha korábban Automatikus méretezéssel, a látni fogja a pontos szabályok volt nézet.

3. Ha át kívánja méretezni alapján egy másik metrikus kattintson a **Szabály hozzáadása** sor. Választhatja a meglévő módosítása a mérőszám a mérőszám kívánja méretezni hogy volt a sorok közül.
![Szabály hozzáadása](./media/insights-how-to-scale/Insights_AddRule.png)

4. Most Önnek módosítania kell a válassza ki, mely metrikus méretarányát szeretne. Ha egy mérőszám, létezik néhány dolog, amit érdemes kiválasztása:
    * Az *erőforrás* származik, a mérőszám. A szokásos ez lesz ugyanaz, mint az erőforrás van méretezését. Azonban által tároló várólista mélységét méretezni, az erőforrás szükség szerint átméretezheti, amelyet a várakozási sorban található.
    * A magára a *metrikus nevét* .
    * Az *idő összesítése* mérőszám. Ez a hogyan az adatok egyesítése az *időtartam*fölé.

5. Miután kiválasztotta a metrikus válassza ki a mérőszám, és az operátor küszöbértékét. Ha például akkor mondhatja **80 %** **-nál nagyobb** .

6. Válassza ki, hogy ki szeretne venni. Létezik néhány más típusú műveletek:
    * Növelése és csökkentése – ezzel adja hozzá vagy példányok definiálása az **érték** szám eltávolítása
    * Növelje vagy csökkentse a százalék – Ez egy %-kal megváltoztatja a példányok száma. Például **Az értékmező** 25 sikerült helyezi, és ha jelenleg 8 példányok, 2 módon adhatók hozzá.
    * Növelni vagy csökkenteni szeretné – Ez az elrendezéstípus az példányok száma definiálása **érték** .

7. Végezetül látványos lefelé – Ez a szabály mennyi ideig várjon Ha ismét az előző skála művelet után adhatja meg.

8. A szabály beállítása után kattintson az **OK gombra**.

9. Miután minden kívánt van beállítva, feltétlenül találati a **Mentés** parancsot.

### <a name="scaling-with-multiple-steps"></a>Méretezés több lépésben

A fenti példák meglehetősen egyszerű. Azonban több olyan szigorú méretezés felfelé (vagy lefelé) a felvenni kívánt, ha még akkor is felveheti a azonos mérőszám több skála szabályok. Két skála szabályok például Processzor százalékos adhatók meg:

1. Ha Processzor százalék 60 %-nál 1 példány által ki méretezése
2. Ha Processzor százalék 85 %-nál által 3 példányok ki méretezése

![Több skála szabályok](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

A további szabályt Ha a betöltés meghaladja a 85 %-át egy skála művelet előtt fog kap egy helyett a két további előfordulásait.

## <a name="scale-based-on-a-schedule"></a>A méretezés alapuló ütemezés


Alapértelmezés szerint skála szabály létrehozásakor, mindig érvényesek lesznek. A profil fejlécére kattint, láthatja:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Azonban érdemes több olyan szigorú méretezés alatt a nap, vagy a hét, mint a a hétvége van. A szolgáltatás teljesen ki a munkaidőt is leállíthatja.

1. Ehhez a profil van **Ismétlődés** helyett **mindig,** jelölje ki, és válassza a a profilt az alkalmazni kívánt időpontot.

2. Ha például egy profilt, amely a héten, a **napon** érvényes legördülő törölje a jelet **és **Vasárnapból**** .

3. Ahhoz, hogy egy profilt a nappali megtalálásában, állítsa a **Kezdő időpont** indítsa el a kívánt idejét.
    ![Alapértelmezett ismétlődés](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Kattintson az **OK gombra**.

5. Ezután szüksége lesz a profillal, amely máskor alkalmazni szeretné hozzáadni. Kattintson a **Profil hozzáadása** sorára.
    ![Nem dolgozom](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Az új, másodszor, a profil neve, például hívhatja azt **nem dolgozom**.

7. **Ismétlődés** parancsra, majd válassza a példány száma az új cellatartomány ez idő alatt.

8. Amikor az alapértelmezett profil válassza a **napok** érdemes a profil szeretne alkalmazni, és a **Kezdő időpont** a nap során.

>[AZURE.NOTE] Automatikus méretezéssel fogja használni a nyári megtakarítási szabályokat bármelyik **Időzóna** lehetőséget választja. Azonban idejének a világidőtől való eltérését jelennek meg az alap időzóna eltolás, nem a nyári megtakarítási UTC eltolás nyári megtakarítási idő alatt.

9. Kattintson az **OK gombra**.

10. Most szüksége lesz bármilyen szeretne alkalmazni a második profilhoz közben szabályok felvétele. Kattintson a **Szabály hozzáadása**gombra, és majd sikerült létrehozása során az alapértelmezett profil van ugyanezt a szabályt.
    ![Munka kikapcsolva szabály hozzáadása](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Ügyeljen arra, hogy ki a skála két szabály létrehozása és a – a profil példány számának során skála csak a nagyobb (vagy csökkentése).

12. Végül kattintson a **Mentés**gombra.

## <a name="next-steps"></a>Következő lépések

* [Monitor szolgáltatás mértékek](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
* A [Figyelés és diagnosztika engedélyezése](insights-how-to-use-diagnostics.md) részletes magas-gyakoriság mértékek gyűjt a szolgáltatás.
* Mértékek vagy [fogadás értesítések](insights-receive-alert-notifications.md) működési események fordulhat elő, amikor cross küszöbértéket.
* [Alkalmazás teljesítményének figyelése](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, ha meg szeretné érteni, hogy pontosan hogyan a kód végrehajtásához a felhőben.
* [Események megtekintése és naplók](insights-debugging-with-events.md) mindent megtudhatja, hogy történt a szolgáltatásban.
* [Monitor elérhetőségét, és a weblap növeléséhez](../application-insights/app-insights-monitor-web-app-availability.md) alkalmazás Hírcsatornájában láthatja, hogy ha a lapon, a nem működik.
