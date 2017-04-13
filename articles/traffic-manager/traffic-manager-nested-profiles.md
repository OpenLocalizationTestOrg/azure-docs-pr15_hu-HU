<properties
    pageTitle="Egymásba ágyazott forgalom Manager profilok |} Microsoft Azure"
    description="Ez a cikk ismerteti a beágyazott profilok funkció az Azure forgalom kezelő"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Beágyazott forgalom Manager profilok

Forgalom Manager körét kínálja, amely lehetővé teszi, hogy hogyan forgalom Manager úgy dönt, hogy melyik végpont kell kapniuk a forgalmat a minden végfelhasználói forgalom útválasztás módszereket. További tudnivalókért lásd: a [forgalom Manager forgalom útválasztás módszerek](traffic-manager-routing-methods.md).

Minden forgalom Manager profilja egységes forgalom útválasztás módszert. Vannak azonban olyan esetek bonyolultabb forgalom útválasztás, mint a továbbítás nyújtotta egyetlen forgalom Manager profil igénylő. Egynél több forgalom útválasztás módszer előnyei egyesítéséhez forgalom Manager profilok ágyazhatók egymásba. Beágyazott profilok nagyobb támogatási és összetettebb alkalmazás telepítések alapértelmezett forgalom Manager működési módjának felülbírálására teszi lehetővé.

Az alábbi példák bemutatják az egymásba ágyazott forgalom Manager profilok használata a különböző forgatókönyvekben.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Példa 1: Kombinálásával "Teljesítmény" és "Weighted" forgalom továbbítása

Feltételezve, hogy telepítették-e az alkalmazások az alábbi Azure régiókban: nyugati Amerikai Egyesült Államok, a nyugati Európa és a kelet-ázsiai. A régió, a felhasználó legközelebb forgalom terjesztése forgalom Manager "Teljesítmény" forgalom útválasztás módszer segítségével.

![Egyetlen forgalom Manager profil][1]

Most tegyük fel, hogy tesztelje a szolgáltatás frissítésére, mielőtt körben közbeni további kíván. Módszerrel a súlyozott"forgalom útválasztás irányítja a forgalmat a próba-példányhoz csekély szeretne. A próba-példányban mellett a meglévő éles üzemi beállítása a nyugati Európa.

Nem lehet kombinálni mindkét Weighted és "teljesítmény forgalom-köröztetése egyetlen profilhoz. A támogatási ebben az esetben, a nyugati Europe végpontokat és a "Weighted" forgalom útválasztás módszer forgalom Manager profil létrehozása. Ezután adja hozzá a "child" profil zárólap, a "szülő" profil. A szülő-profil továbbra is a teljesítmény forgalom útválasztás módszert használja, amelyben a más globális telepítés esetén, a végpontok.

Az alábbi ábra bemutatja az ebben a példában:

![Beágyazott forgalom Manager profilok][2]

Ebben a konfigurációban a szülő-profil keresztül irányított forgalom elosztja forgalom régiók a szokásos módon. A beágyazott profil belül nyugati Európa, elosztja a forgalmat a termelési és próba végpontok a hozzárendelt súlyok megfelelően.

A szülő-profilt a "Teljesítmény" forgalom útválasztás módszert használja, ha minden végpontra hely e kiosztani. A hely van hozzárendelve, a végpontok konfigurálásakor. Válassza ki a legközelebb áll a központi telepítés Azure területhez tartozik. Az Azure régiók értékei helyét a Internet késés tábla által támogatott. További tudnivalókért lásd: [forgalom Manager "Teljesítmény" forgalom útválasztás módszer](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Például 2: Egymásba ágyazott profilokban figyelése végpont

Adatforgalom Manager aktívan figyeli minden szolgáltatás végpontjának állapotának. Ha a végpontok megsérült, a forgalom Manager alternatív végpontok megőrzése a szolgáltatást az elérhető felhasználók irányítja. Végpont figyelemmel kísérésére és feladatátvételi megoldásához minden forgalom útválasztás módszer vonatkozik. További tudnivalókért lásd: a [Forgalom Manager végpont figyelés](traffic-manager-monitoring.md). Figyelés beágyazott profilok másképp működik végpontjának. Beágyazott profilokat a szülő-profil nem végezni a gyermek a állapot-ellenőrzései közvetlenül. Ehelyett a gyermek profil végpontok állapotának használatos általános állapotát, a gyermek profil számítja ki. A rendszerállapot adatait propagálja a beágyazott profil hierarchia be. A szülő-profil ez összesíti, annak megállapításához, hogy e közvetlen forgalmat a gyermek profil állapotjelző. Lásd: a [Gyakori kérdések](#faq) szakaszban, a jelen cikk részletes ismertetése a rendszerállapot figyelése a beágyazott profilok.

Térjen vissza az előző példában, tegyük fel, hogy az éles üzemi nyugati Európában sikertelen lesz. Alapértelmezés szerint a "child" profil irányítja a próba-példányhoz minden forgalmat. A próba-példányban is sikertelen, ha a szülő-profil határozza meg, hogy a gyermek profil nem kell kapnia forgalmat, mivel az összes alárendelt végponton megsérült. A szülő-profil szétosztja azon részeire lépkedhet forgalmat.

![Beágyazott profil feladatátvevő (alapértelmezés)][3]

Előfordulhat, hogy ennél a módszernél elégedett. Vagy lehet, hogy minden forgalom nyugati Európa most fog a próba-példányhoz helyett egy korlátozott Alkészlet forgalom az érintett. A próba telepítési állapotának függetlenül szeretne átadni más régiók Ha a nyugati Európában éles üzemi nem sikerül. Ahhoz, hogy a feladatátvételi, mint a szülő-profil zárólap a gyermek profil konfigurálásakor megadhatja a "MinChildEndpoints" paramétert. A paraméter határozza meg, hogy a lehető legkevesebb elérhető végpontjait a gyermek-profilhoz. Az alapértelmezett értéke "1". Ebben az esetben a MinChildEndpoints értéke 2 beállítani. Ez a küszöb alatt a a szülő-profil nem érhető el, a teljes gyermek profil tekinti, és a irányítja a forgalmat a többi végpont.

Az alábbi ábra ebben a konfigurációban:

![Egymásba ágyazott "MinChildEndpoints" a profil feladatátvevő = 2][4]

>[AZURE.NOTE]
>A "Priority" forgalom útválasztás módszer elosztja egyetlen végpont minden forgalmat. Így van kis célja egy MinChildEndpoints beállítás '1' kívül a gyermek profilok.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Például: 3: Elsőbbségi feladatátvevő területeire "Teljesítmény" forgalom továbbítása

Az alapértelmezett működés "Teljesítmény" forgalom útválasztás mód elkerülése érdekében a következő legközelebbi végpont túlságosan betöltése és okozó hibák kaszkádolt sorozata lett tervezve. Zárólap sikertelensége esetén minden forgalom végpontra való átirányításához volna van egyenletesen elosztva más végpontjait összes régiók.

![Az alapértelmezett feladatátvevő útválasztás "Teljesítmény" forgalom][5]

Azonban tegyük fel, hogy inkább a nyugati Europe forgalom feladatátvételi nyugati Amerikai Egyesült Államok, és csak irányítsa át más régiók alapú forgalmat, ha mindkét végpontokhoz nem érhetők el. A gyermek profil használata a "Priority" forgalom útválasztás módszer megoldást hozhat létre.

![A kedvezményes feladatátvételi útválasztás "Teljesítmény" forgalom][6]

A nyugati Europe végpont a nyugati USA-beli végpont-nál magasabb prioritása, mivel minden forgalom a rendszer elküldi a nyugati Europe végpont Ha mindkét végpont online állapotban. Ha a nyugati Europe sikertelen, a rendszer a irányítja nyugati us forgalmat. A beágyazott profillal forgalom átirányításához kelet-ázsiai csak akkor, ha a nyugati Európa és a nyugati USA-beli sikertelen lesz.

A minta terület megismételheti. Cserélje a szülő-profilban lévő összes három végponton három gyermek profilt, minden egyes rangsorolt feladatátvevő sorozat nyújtó.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Példa 4: Szabályozása "Teljesítmény" forgalom útválasztás között ugyanabban a régióban több végpontok

Tegyük fel, hogy a "teljesítmény" forgalom útválasztás módszert az egy adott régióban egynél több végpont tartalmazó profil. Alapértelmezés szerint a régióba irányított forgalom van egyenletesen elosztott át az összes rendelkezésre álló végpontok az adott régióban.

![Továbbítás a régió forgalom eloszlás (alapértelmezés) "Teljesítmény" forgalom][7]

Helyett hozzáadása több végpontok nyugati Európában, ezeket a végpontok külön gyermek profilban idézőjelek közé. A gyermek profil nyugati Európában csak végpontjának bekerül a szülő. A gyermek profil beállítást szabályozhatja a nyugati Europe forgalom eloszlást, mivel az adott területen belül útválasztás prioritású vagy a súlyozott forgalmat.

![Egyéni terület a forgalom eloszláshoz útválasztás "Teljesítmény" forgalom][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Példa 5: Végpont ellenőrzési beállítások

Tegyük fel, hogy a forgalmat Manager használatával zökkenőmentesen áttelepítése régebbi érkező forgalmat a helyszíni webhely felhőalapú verziójának Azure-ban tárolt. A régi webhelyhez használni kívánt a Kezdőlap lap URI Lync-webhely állapota. Az új, felhőalapú verzió, az egyéni ellenőrzési lap végrehajtási, de (elérési út "/ monitor.aspx"), amely további ellenőrzést tartalmazó beállítás.

![Adatforgalom Manager végpont figyelése (alapértelmezés)][9]

Az adatforgalom-kezelő profil felügyeleti beállításai egyetlen profil belül minden végpontok vonatkozik. Beágyazott profilokkal használatával különböző gyermek profil webhelyenként különböző-ellenőrzési beállítások megadása.

![Adatforgalom Manager végpont végpont beállításokkal figyelése][10]

## <a name="faq"></a>GYAKORI KÉRDÉSEK

### <a name="how-do-i-configure-nested-profiles"></a>Hogyan állítható be a beágyazott profilok?

Beágyazott forgalom Manager profilok az Azure erőforrás-kezelő, mind a klasszikus Azure REST API-k, Azure PowerShell-parancsmagok és platformok Azure CLI parancsok használatával is beállítható. Az új Azure portálon keresztül azokat is használhatók. A klasszikus portálon azok nem támogatottak.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Hány rétegek beágyazási hatással van az adatforgalom-kezelő támogatási?

Legfeljebb 10 szintek mély profilok ágyazhatók egymásba. Hurkok"nem engedélyezett.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Is lehet keverje másfajta végponttípust beágyazott gyermek profilok ugyanarra a forgalom Manager profilra?

igen. Köre nincs korlátozva, a végpontok belül profil különböző típusú egyesített hogyan.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>A számlázási modell hogyan nem vonatkozik a beágyazott profilok?

Nem nem negatív árak beágyazott profilok használatával hatását.

Adatforgalom Manager számlázási két összetevője: állapot-ellenőrzései végpont és a DNS-lekérdezések milliónyi

- Állapot-ellenőrzései végpont: amikor egy szülő-profilhoz zárólap konfigurált gyermek profil ingyenesek. A szokásos módon felügyeletet igényel, a végpontok a gyermek profil számlázható.
- A DNS-lekérdezések: minden lekérdezés számít csak egyszer. A gyermek profilból zárólap visszaadó szülő profil lekérdezése ellen csak a szülő-profil számít.

Teljes részletekért olvassa el a [forgalom Manager árak lapon](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Van-e a teljesítményt, beágyazott profilok?

nem. Nincs beágyazott profilok használatakor felmerülő teljesítményt nem.

A forgalom Manager névkiszolgálók belső bejárása a profil hierarchia, minden egyes DNS-lekérdezés feldolgozása közben. A DNS-lekérdezések szülő profilhoz kaphatnak a DNS-válasz, a végpontok gyermek profilból. Egy CNAME rekord használják, hogy egyetlen profil vagy beágyazott profilok. Nincs szükség az egyes profilok CNAME rekord létrehozása a hierarchia.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hogyan számítja ki a forgalom kezelő egy szülő-profilban beágyazott végpontot állapota?

A szülő-profil nem hajtja végre a gyermek a állapot-ellenőrzései közvetlenül. Ehelyett a gyermek profil végpontok állapotának használt általános állapotát, a gyermek profil számítja ki. A beágyazott profil hierarchia határozza meg az egymásba ágyazott végpontot állapotának be ezt az információt propagálja. A szülő-profil e összesített állapot használja határozza meg, hogy a forgalmat a gyermek számára is irányítja.

Az alábbi táblázat ismerteti a működését a forgalom Manager állapot ellenőrzi, hogy egy beágyazott végpontot.

|Child profil Monitor állapota|Szülő-végpontot Monitor állapota|Jegyzetek|
|---|---|---|
|Letiltott. A gyermek profil le van tiltva.|Leállítva|A szülő-végpontot állam leállt-e kapcsolva. A letiltott állapotba kerül for jelezve, hogy le van tiltva a szülő profilban végpont van fenntartva.|
|Csökkent. Legalább egy Gyermekszint profil végpont csökkentett teljesítményű állapotban van.| Online: a gyermek profilban Online végpontok egy legalább MinChildEndpoints értékét.<BR>CheckingEndpoint: az Online plusz CheckingEndpoint végpontjait a gyermek profil egy legalább MinChildEndpoints értékét.<BR>Csökkent: más módon.|Az állapot CheckingEndpoint zárólap forgalom rendszer irányítja. Ha MinChildEndpoints túl nagy van állítva, a végpontok mindig teljesítménye csökkent.|
|Online. Legalább egy Gyermekszint profil végpont: az Online állapot. Nincs végpont csökkentett teljesítményű állapotban van.|Lásd a fenti.||
|CheckingEndpoints. Legalább egy Gyermekszint profil végpont "CheckingEndpoint". Nincsenek végpontok "Elérhető" vagy "csökkent|Ugyanaz, mint feljebb.||
|Inaktív. Az összes alárendelt profil végpontok Letiltva vagy a leállítva, vagy ha a profil még nincsenek végpontok.|Leállítva||


## <a name="next-steps"></a>Következő lépések

További tudnivalók a [forgalom Manager működése](traffic-manager-how-traffic-manager-works.md)

Megtudhatja, hogyan hozhat [létre a forgalom Manager profilok](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

