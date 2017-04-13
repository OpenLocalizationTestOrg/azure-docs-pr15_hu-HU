<properties
   pageTitle="Szolgáltatás háló alkalmazások megtervezése kapacitás |} Microsoft Azure"
   description="Megtudhatja, hogy miként szolgáltatás háló alkalmazáshoz szükséges számítási csomópontok számának meghatározása"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Kapacitás szolgáltatás háló alkalmazások megtervezése


A dokumentum útmutatást ad becsüljük meg az összeget az Azure Service háló alkalmazások futtatásához szükséges erőforrások (processzorok – RAM, lemezterülettel) módját. Az erőforrás igényeinek megfelelően módosíthatja az adott idő alatt közös. Néhány erőforrások általában miközben a szolgáltatás fejlesztését, próba, majd szükség van további források éles módba, és az alkalmazás a Népszerűségi megnő szükséges. Az alkalmazás tervezésekor gondolja végig a hosszú távú követelményeknek, és lehetővé a szolgáltatás nagy ügyfél igényeknek méretezni döntéseket.

 A szolgáltatás háló fürtre létrehozásakor, hogy milyen típusú virtuális gépeken futó (VMs) alkotó a fürt. Minden egyes virtuális processzor (magmintákat és sebességének), a hálózati sávszélesség, a RAM és a szabad tárterület formájában erőforrások korlátozott mennyiségű megtalálható. A szolgáltatás növekedésével adott idő alatt, amely nagyobb erőforrások ajánlja fel, illetve további VMs hozzáadása a fürthöz VMs frissíthet. Ehhez az utóbbi, meg kell tervezővel a szolgáltatás kezdetben, hogy kihasználhassa a fürt első dinamikusan felvett új VMs.

Egyes szolgáltatások kezelése kis az adatokat nem a VMs magukat. Ezért tervezés, az alábbi szolgáltatások kapacitás kell koncentráljon elsősorban teljesítmény, ami azt jelenti, hogy kijelöli a VMs megfelelő processzor (magmintákat és sebességének). Hálózati sávszélesség, milyen gyakran hálózati átvitelek fordul elő, és hogy mennyi adatátvitel együtt Ezenkívül figyelembe. Ha a szolgáltatás hajtsa végre, valamint szolgáltatás használatát nő, további VMs hozzáadhatja a hálózati kérelmek át az összes VMs fürt és betöltés egyenlege.

Nagy mennyiségű adatot a VMs a kezelő szolgáltatások kapacitás tervezés kell koncentráljon elsősorban méretét. Így gondosan vegye figyelembe a virtuális memória és a szabad tárterület kapacitása. A virtuális memória kezelése a Windows rendszer ellenőrzi RAM néznek ki az alkalmazás kód lemezterületet. A szolgáltatás háló futási idejű emellett intelligens lapozási csak meleg adatok megőrzési memória és a hideg-adatok áthelyezése a lemez. Alkalmazások így használhatja, mint a ténylegesen megtalálható a virtuális memória. Egyszerűen több RAM problémákat megnöveli teljesítmény elérése érdekében, mivel a virtuális több szabad tárterület RAM tárolhatja. A virtuális, akkor jelölje ki a lemezen elég nagy ahhoz, hogy tárolja az adatokat, amelyet a virtuális kell rendelkeznie. A virtuális hasonlóan, hogy a kívánt teljesítmény biztosítson elég RAM kell rendelkeznie. Ha a szolgáltatás adatok időbeli megnő, felveheti további VMs a fürthöz és az adatok partíciót összes VMs keresztül.

## <a name="determine-how-many-nodes-you-need"></a>Határozza meg, hány csomópontok szükséges

A szolgáltatás szétválasztás lehetővé teszi a webszolgáltatás adataira méretezése. Szétválasztás kapcsolatos további tudnivalókért olvassa el a [Szolgáltatás háló szétválasztás](service-fabric-concepts-partitioning.md)című témakört. Egyes partíciók meg kell elférjenek egy egyetlen virtuális, de több (kicsi) partíciót egy egyetlen virtuális fektetni is. Igen további kis partíciót kellene lépve néhány nagyobb partíciót problémákat-nál nagyobb rugalmasság érdekében. A csökkentés pedig, hogy problémákat partíciót rengeteg növeli a szolgáltatás háló terhelést feldolgozott műveletek partíciót keresztül nem tudja elvégezni. Nincs is több lehetséges hálózati forgalmának engedélyezésére Ha gyakran kell a szolgáltatáskód csatolt fájllal, amely a különböző partíciók élő eléréséhez. A szolgáltatás tervezésekor gondosan vegye figyelembe a pozitívumokat és a negatívumokat egy hatékony particionáló stratégia elérésének.

Tegyük fel, az alkalmazás rendelkezik egyetlen állapot-nyilvántartó szolgáltatás, amely az egy évben DB_Size GB növekedjen várt áruházból legyenek. Nagyságát adja hozzá a további alkalmazások (és partíciót) tapasztal a NÖV túl az adott év szerint.  A replikáció tényező (RF), amely meghatározza a szolgáltatásbeli kópiák száma az összes DB_Size hatással van. A teljes DB_Size összes másolatnál végig a replikáció tényező DB_Size szorozva.  Node_Size a szabad terület/RAM egyes a szolgáltatásbeli használni kívánt csomópontok jelöli. A legjobb teljesítmény elérése érdekében a DB_Size kell elférjenek a memóriában a fürt, és egy Node_Size, amely a, a virtuális memória körül kell választani. Történő felosztása egy Node_Size, amely nagyobb, mint a RAM beosztását, a lapozási a szolgáltatás háló futtatókörnyezet által biztosított vannak támaszkodva. Ezért a teljesítmény nem feltétlenül optimális, ha a teljes adatok tekinthető meleg (ezt követően az adatokat az lapozható be/ki). Jó helyen jár csak egy részét, az adatok esetén meleg számos szolgáltatás, célszerű költséghatékonyabb.

A maximális teljesítmény szükséges csomópontok számának kiszámítása a következőképpen:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>NÖV-fiók

Érdemes a várt nő, a DB_Size jelenítik meg a a mellett a szolgáltatás DB_Size alapján csomópontok számát számítja ki. A szolgáltatás, hogy a program nem túlságosan kiépítési csomópontok számának növekedésével a ezután nagyobb csomópontok számának. De partíciók száma alapján kell, hogy van szükség, ha a szolgáltatás futtatja a maximális NÖV csomópontok számának.

Célszerű, hogy néhány további gépek bármikor érhető el, így kezelhető váratlan kiugrásainak megfelelő vagy a hiba (Ha például néhány VMs nyissa).  A felesleges kapacitás kell meghatározni a várt kiugrásainak megfelelő használatával, miközben kiindulási pont-e foglalni néhány további VMs (5-10 százalék további).

Az előző azt feltételezi, hogy egyetlen állapot-nyilvántartó szolgáltatás. Ha egynél több állapot-nyilvántartó szolgáltatást, akkor a egyenletté az egyéb szolgáltatásokhoz tartozó DB_Size hozzáadása. Másik lehetőségként külön-külön az egyes állapot-nyilvántartó szolgáltatásokhoz csomópontok számának is számítja ki.  A szolgáltatás lehetséges, hogy kópiák vagy partíciók, amely nem kiegyensúlyozott. Ne feledje, hogy partíciót is több adatot, mint másoknak. Szétválasztás kapcsolatos további tudnivalókért olvassa el a [szétválasztás gyakorlati tanácsok a cikk](service-fabric-concepts-partitioning.md)című témakört. Az előző egyenlet azonban partíciót és replika agnostic, mert a szolgáltatás háló biztosítja, hogy a replikák helyezkednek el a csomópontok között optimalizált módon.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>A számolótábla használata kiszámítása

Most vegyük néhány valós számmá helyezi el a képletet. Egy [Példa számolótábla](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) megtudhatja, hogy miként megtervezése az alkalmazás, amely tartalmazza a adatobjektumok háromféle kapacitása. Minden objektumhoz hogy közelítő értékét számító méretének és megakadályozási, ha azt szeretné, hogy hány objektumok. Azt is be minden objektumtípus szeretnénk hány kópiák. A számolótábla tárolni a fürt memória mennyiségét számítja ki.

Írja be azt a virtuális méretét és a havi költség. Virtuális mérete alapján, a számolótábla közli, hogy a lehető legkevesebb partíciót a csomóponton fizikailag igazítása az adatok felosztása kell használnia. Előfordulhat, hogy az alkalmazás adott számítási igazodik partíciók nagyobb számot megfelel, és a hálózati forgalmának engedélyezésére van szüksége. A számolótábla jeleníti meg, amelyek a felhasználói profil objektumok tartománya partíciók száma egy és hat növekedett.

Most alapján ezt az információt, a számolótábla mutatja, hogy fizikailag sikerült-e be a kívánt partíciót és kópiák az adatokat a 26-csomópont fürthöz. Azonban a fürt szeretné kell sűrűn jelentek meg, ezért érdemes néhány további csomópontok csomópontok hibáit és frissítések igazodik. A számolótábla is látható, hogy több, mint 57 csomópontok összevonása nincs további érték, mert azt szeretné, hogy üres csomópontot. Ismét érdemes Ugrás feletti 57 csomópontok mégis csomópontok hibáit és frissítések igazodik. Módosíthatja az animáció működését a számolótáblát, az alkalmazás igényeinek megfelelően.   

![A számolótábla-kiszámítása][Image1]



## <a name="next-steps"></a>Következő lépések

Nézze meg a [szolgáltatás háló szétválasztás szolgáltatások] [ 10] további információt a szétválasztás a szolgáltatásban.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
