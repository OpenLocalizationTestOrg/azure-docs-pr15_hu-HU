<properties 
    pageTitle="Azure RemoteApp – teszteli a hálózati sávszélesség-használat a néhány gyakori alkalmazási területek |} Microsoft Azure"
    description="Megtudhatja, hogyan kell tudni használatát esetei, amelyek segíthetnek találnia, hogy a hálózati sávszélességre van szükség, Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp – teszteli a hálózati sávszélesség-használat a néhány gyakori alkalmazási területek

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A legjobb módszer a hálózathoz Azure RemoteApp hatása néhány használatát futtatása megállapítása azt vizsgálja, hogy tárgyalt, a [hálózati sávszélesség-használat becslése Azure RemoteApp](remoteapp-bandwidth.md). Meghatározott időtartamra e vizsgálatok futtatása, és minden esetben szükséges sávszélességet mérjük. Ha a képesség, mérje le a hálózati csomag veszteség és a hálózati vibrálás a hálózati forgalmat a adott környezetben létrehozott megértéséhez.

    
A sávszélesség-használat kiértékelésekor ne feledje, hogy használatát a vállalaton belül a különböző felhasználók között változik. Például szöveg olvasók és szövegírói általában mobilalkalmazásokban kisebb, mint a felhasználóknál, akik a videó sávszélesség A legjobb tanulmányozza a saját felhasználói igényeinek megfelelően, és hozzon létre a felhasználókat a vállalat legjobb megjelenítő kombinálja az alábbi helyzetekben a. Ne feledje, [tekintse át a tényezőkkel, amelyek hatása a sávszélesség-használat és a felhasználói felület](remoteapp-bandwidthexperience.md) -, amellyel azonosítani tudja a ideális vizsgálatok.

Első tesztek, olvassa el a mix válassza ki, és futtassa őket. Az alábbi táblázat segítségével teljesítmény követni.

>[AZURE.NOTE] Ha nem saját hálózat tesztelése, vagy nincs ezt az időt, olvassa el az [alapvető hálózati sávszélesség ad becslést/javaslatok](remoteapp-bandwidthguidelines.md). A fogyasztási eltérőek lehetnek, ezért ha a saját vizsgálatok *is* fut, tegye a következőt.


## <a name="the-usage-tests"></a>A használati vizsgálatok
E vizsgálatok futtatása az idő különböző mennyiségű, és tesztelje a funkciók és szolgáltatások, amely a hálózat sávszélessége felhasználni. Ne felejtse el, válassza a próba vegyesen, amely a legközelebb a egyéni vállalati felhasználóknak.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Ügyvezető/komplex PowerPoint - 900-1000 másodpercig futtatása

Egy felhasználó bemutat 45 – 50 nagy hűségű diák teljes képernyős módban a Microsoft Office PowerPoint használatával között. A diák képek, áttűnések (animációval) és színátmenetet, amelyek a vállalat tipikus hátterek kell tartalmaznia. A felhasználó legalább 20 másodperc kell töltött minden dia.
    
Ebben az esetben cellaveszteségi forgalom, hoz létre, ha a dia átmenetek a következő diára a bemutatóban.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Egyszerű PowerPoint - ~ 620 másodpercig futtatása

A felhasználó egyszerű PowerPoint-fájl körülbelül 30 diák teljes képernyős módban a Microsoft Office PowerPoint használatával eltéréseit. A diák további szöveg igényű mint a az ügyvezető/komplex PowerPoint alkalmazási példát, és egyszerűbb hátterek és háttérképek (fekete diagramok). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Az Internet Explorer - legfeljebb 250 másodpercig futtatása

A felhasználó a webes megnyitja az Internet Explorer használatával. A felhasználó megnyitja és görget, kombinálja a szöveg, képek természetes és néhány sematikus diagramok. A weblapok a helyi meghajtón távoli asztali munkamenetgazda (munkamenetgazda) kiszolgáló tárolja az. MHT-fájlt. A felhasználó görgetés, Page Up, Page Down, felfelé és lefelé billentyűk résznél használata változó intervallumok kulcs/hibatípusonként görgetősáv:
    
    - Lefelé – 250 billentyű lenyomásával nagyon 500 ms
    - Page Up – a 36 billentyű lenyomásával minden 1000 ms
    - Lefelé - 75 billentyű lenyomásával minden 100 ms
    - Page Down – 20 billentyű lenyomásával minden 500 ms
    - Fel - 120 billentyűleütést minden 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-dokumentum - egyszerű – ~ 610 másodpercig futtatása
A felhasználó beolvassa, és a PDF-dokumentum keres különböző módokon Adobe Acrobat Reader használatával. A dokumentum táblázatok, egyszerű grafikonok és több betűtípusokat kell állnia. A dokumentum 35-40 lapok hosszú. A felhasználó görget, két különböző kamatlába korábbi verziókkal és továbbítja, négy különböző nagyítási méretben (igazítás lap, igazítás a szélesség, 100 %-os, a másik pedig a kiválasztásának). A Nagyítás vagy kicsinyítés biztosíthatja, hogy a szöveg (betűtípus) jeleníti meg különböző méretű. Görgetés lefelé használata a Page Up, Page Down, felfelé és lefelé kulcsok, az egyes görgetés változó intervallumokkal van.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>A dokumentum PDF - vegyes - Futtatás ~ 320 másodpercig
A felhasználó beolvassa, és a PDF-dokumentum keres különböző módokon Adobe Acrobat Reader használatával. A dokumentum (például fényképek) kiváló minőségű képek, táblázatok, egyszerű grafikonok és több betűtípusokat áll. A felhasználó görget, két különböző kamatlába korábbi verziókkal és továbbítja, négy különböző nagyítási méretben (igazítás lap, igazítás a szélesség, 100 %-os, a másik pedig a kiválasztásának). A Nagyítás vagy kicsinyítés biztosítja, hogy a szöveg (betűtípus) jeleníti meg különböző méretű. Görgetés lefelé használata a Page Up, Page Down, felfelé és lefelé kulcsok, az egyes görgetés változó intervallumokkal van.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash-videó lejátszás - ~ 180 másodpercig futtatása
A felhasználó tekint meg Adobe Flash-címként kódolt videó az weblapon beágyazva. A helyi merevlemez-meghajtó munkamenetgazda kiszolgáló tárolja az weblapon. A videó lejátszása az Internet Explorerben egy beágyazott Windows Media Player beépülő modult.

Ebben az esetben a multimédiás tartalmazó multimédiás tartalom weblapok megtekintő felhasználók amelynek azonos. A legtöbb adatot kell téglalap VOBR keresztül.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Távoli írja be a Word - ~ 1800 másodpercig futtatása
Egy felhasználó gépel a dokumentum egy RDP-munkamenet fölé. Billentyű lenyomásával elküldi az ügyféloldali keresztül az RDP-munkamenet egy dokumentumot a Microsoft Word fut a távoli munkamenetet. A gépelési mérték egy karakterrel minden 250 ms (7050 karakterek száma). 

Ez az egyik Tudásbázis dolgozó leggyakoribb esetet tárgyal. Ebben az esetben azt vizsgálja, hogy a felhasználó beírja a modern munka processzor növeléséhez. Ebben az esetben érzékeny sávszélesség-használat kis változását is.

## <a name="tracking-the-test-results"></a>A vizsgálati eredmények nyomon követése

Az alábbi táblázat segítségével kiértékelésének eredménye az jelenik meg a környezetben. Az adatok az alábbiakra van, csak az ábra – nagyban eltér, tekintse meg lehet. 

Az egyszerűség kedvéért feltételezzük, hogy minden esetben kiértékelésének 1920 x 1080 képpont képernyőfelbontását azonos használatával, és a 120 ms + megjelölés 1 %-os szolgáltatás TCP átvitelek 200 ms és a hálózati alatt (késleltetés) válaszidejű hálózaton.

Tudnivalók a táblázatban:
- A hálózat sávszélessége, ahol felhasználói termelékenység nem jelentősen hatással van, de nem zárja ki alkalmi video- vagy problémák **átlag tapasztalható** tartalmazza. A rendszer nem sikerült helyreállítani gyorsan kihasználva a dinamikus logikájának. A hálózati sávszélesség ad becslést kísérlet a felhasználói élmény minősége biztosítása érdekében.
 - A hálózat sávszélessége, ahol felhasználók megfigyelheti lennének jelentős problémák, és azok igényekhez szabott termelékenység hatással van a mérhető időszakhoz tartozó **Noticeable problémák (oldaltörés pont)** tartalmazza. Ezen a ponton a RDP algoritmusok tud vannak lépést, és nem garantálja az a felhasználói élmény minősége miatt nem elegendő a sávszélesség.
 - **Ajánlott** a hálózat sávszélessége ajánlott a jó vagy kiváló élmény tartalmazza. Általában egy lépésben nagyobb, mint a **átlag tapasztalható** oszlop értéke ajánlatos.
 - **Jegyzetek** közé tartozik, észrevételek és a megjegyzéseket.
 
| Teszt                  | Átlagos élmény | Észrevehető problémák (oldaltörés pont) | Ajánlott hálózati sávszélesség | Jegyzetek                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Ügyvezető/komplex PPT | A 10 MB-os/s             | 1 MB/s                           | > 10 MB-os/s, 100 MB/s előnyben részesített    | 1 MB/s sok animációk elvesznek                                   |
| Egyszerű PPT            | 5 MB/s              | 256 KB/s                         | A 10 MB-os/s                        | 256 KB/s a diák észrevehető késleltetéssel betöltése                   |
| Az Internet Explorer     | A 10 MB-os/s             | 1 MB/s                           | > 10 MB-os/s, 100 MB/s előnyben részesített    | 1 MB/s webes videók elmosódottan vagy és akadozik, gyors görgetés kapcsolatos problémák |
| Egyszerű PDF-fájl            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | 256 KB/s tart a betöltés közben                       |
| Vegyes PDF-fájl             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | 256 KB/s az oldal egy jelentős időt vesz betöltése    |
| Flash-videó lejátszása  | A 10 MB-os/s             | 1 MB/s                           | > 10 MB-os/s, 100 MB/s előnyben részesített    | 1 MB/s a videó szemcsésen és néhány keretek megszakadnak           |
| Word-távoli beírása    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | 256 KB/s felhasználói tapasztalhatja billentyű lenyomásával között eltelt idő             |

A hálózat sávszélessége felhasználónként értékeléséhez, hozzon létre a fenti esetek vegyesen, és a megfelelő szükséges hálózati sávszélesség aránya. Válassza ki a legnagyobb számtól a esetek szükséges. Felhasználó gyakorlatilag soha nem használ egyedül a rendszer, mivel fontolja meg az egyes felhasználóknál, akik a hálózaton egyidejűleg dolgozhat foglalása.
     
## <a name="learn-more"></a>tudj meg többet
- [Azure RemoteApp hálózati sávszélesség-használat becslése](remoteapp-bandwidth.md)

- [Azure RemoteApp – hogyan hálózat sávszélessége és minőségét tapasztalja munka együtt?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp hálózati sávszélesség - tippet olvashat a (Ha nem lehet tesztelni saját)](remoteapp-bandwidthguidelines.md)