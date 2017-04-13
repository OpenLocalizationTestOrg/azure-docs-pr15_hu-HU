<properties
   pageTitle="A szolgáltatás háló fürt kapacitás tervezési |} Microsoft Azure"
   description="Szolgáltatás háló fürt kapacitás tervezési szempontjait. Nodetypes, tartósság és megbízhatóságára rétegek"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Szolgáltatás háló fürt kapacitás megtervezésével kapcsolatos szempontok

Bármely éles üzemi környezetben, kapacitás tervezés nem fontos lépést. Íme néhány az elemeket, amelyeket figyelembe kell vennie a folyamat részeként.

- A fürt kell kezdődnie csomópont-típusok száma
- A Tulajdonságok mindegyikének csomópont típusa (méret, az elsődleges, internetes, VMs száma stb.)
- Megbízhatósága és tartóssági jellemzői a fürthöz

Tudassa velünk röviden tekintse át az összes ezeket az elemeket.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>A fürt kell kezdődnie csomópont-típusok száma

Első lépésként, ki kell deríteni hoz létre a fürt okok használandó, és milyen típusú alkalmazásokat szeretné a fürt történő telepítéséhez. Ha nem a fürt célját a világos, akkor valószínűleg még nem készen áll a tervezési folyamat kapacitás adja meg.

A fürt kell kezdődnie csomópont típus számának meghatározásához.  Minden csomópont típusa van hozzárendelve egy virtuális gép skála megadása. Csomópont típusonként majd kell méretezett vagy lefelé független, különböző csoportjaihoz nyitott portokat van, és beállíthatja, hogy a különböző kapacitás mértékek. Így a szám típusú csomópontot a döntési lényegében megtalálható az alábbi szempontokat:

- Az alkalmazás rendelkezik-e több szolgáltatásban, és végezze el őket közül kell lennie, nyilvános vagy internetes? Tipikus alkalmazások előtér-átjáró szolgáltatás, amely a bemeneti kap egy ügyfél, és egy vagy több háttéradatbázist szolgáltatás, amely közli az előtér-szolgáltatások tartalmazzák. Így ebben az esetben, amilyennek legalább két csomópont típus problémákat.

- Szükség van a szolgáltatások (alkotó az alkalmazás) például nagyobb RAM vagy újabb Processzor ciklus infrastruktúra különböző igényeinek? Tudassa velünk tegyük fel például, hogy az alkalmazás telepítéshez használni kívánt tartalmaz egy előtér és a háttéradatbázist szolgáltatás. Az előtér-szolgáltatás futtatását is lehetővé teszi a nyissa meg az internethez portokat kisebb VMs (virtuális méretű például D2).  A háttéradatbázist szolgáltatás azonban kiszámítása igényes és kell futtatni, amelyek nem az interneten (a D4 D6, D15 hasonló méretű virtuális) nagyobb VMs néző.

 Ebben a példában noha eldöntheti, hogy a szolgáltatások elhelyezése egy csomópont típusa, ajánlott, hogy elhelyeztem őket a fürtre két csomópont-típus.  Ez lehetővé teszi, hogy az egyes csomópontot, hogy a különböző tulajdonságait, például a internetkapcsolat, vagy a virtuális parancsára. Független, valamint megadhat VMs számát.  

- A jövőben nem tudja előre, mivel látogassa meg tudja az adatokat, és döntse el, a csomópont típusok induljon van szükség az alkalmazások számára. Bármikor hozzáadása vagy eltávolítása a csomópont-típusok később. A szolgáltatás háló fürtre rendelkeznie kell legalább egy csomópont típusát.

## <a name="the-properties-of-each-node-type"></a>Írja be a minden csomópont tulajdonságainak

A **csomópont típusa** egyenértékű Cloud Services szerepkörökhöz is láthatja. A virtuális méretű, VMs számát és tulajdonságaik csomópont típus megadása Minden definiált szolgáltatás háló fürt csomópont típusa egy külön virtuális gép skála megadása van állítva. Virtuális skála készletei olyan, az Azure kiszámítása az erőforrás üzembe helyezéséhez és kezeléséhez tárolt virtuális gépeken futó gyűjteménye is használhatja. Eltérők virtuális skála állítja be, mint definiálását, minden csomópont típusa majd kell méretezett vagy lefelé egymástól függetlenül, rendelkezik nyitott portokat különböző csoportjaihoz, és beállíthatja, hogy a különböző kapacitás mértékek.

A fürt beállíthatja, hogy egynél több csomópont típusa, de az elsődleges csomópont típusa (az első szakasz, amely csak a portálon) rendelkeznie kell legalább öt VMs gyártási munkaterhelésekből használt fürtre vonatkozóan (vagy legalább három VMs próba fürt esetében). Ha az erőforrás-kezelő sablonnal fürt szeretne létrehozni, egy **elsődleges** attribútum csoportban a csomópont típusú definíció fog talál. Az elsődleges csomópont típusa a csomópont típusa, ahol szolgáltatás háló rendszer szolgáltatások kerülnek.  

### <a name="primary-node-type"></a>Elsődleges csomópont típusa
A többféle csomópont fürt szüksége lesz szeretné elsődleges közül. Íme egy elsődleges csomópont típusa jellemzői:

- Az elsődleges csomópont Type VMs minimális méretének a kiválasztott tartóssági réteg határozza meg. A tartóssági réteg alapértelmezés szerint bronz. Görgessen lefelé a tartóssági réteg van, és az értékek is tovább tarthat tájékoztatást.  

- A lehető legkevesebb VMs elsődleges csomópont típusának a kiválasztott megbízhatóság réteg határozza meg. A megbízhatóság réteg alapértelmezés szerint ezüst. Görgessen lefelé a megbízhatósági szint van, és az értékek is tovább tarthat tájékoztatást.

- A szolgáltatás háló rendszer szolgáltatások (például a kezelő szolgáltatás vagy kép tár szolgáltatás) kerülnek a elsődleges csomópont adja, és így megbízhatóságának és a fürt tartósságát határozza meg a megbízhatósági szint és tartóssági réteg értéke, jelölje ki a elsődleges csomópontot.

![Képernyőkép: a két csomópont típus tartalmazó fürt ][SystemServices]


### <a name="non-primary-node-type"></a>Az elsődleges nem csomópont típusa
Több csomópont-típus fürt van egy elsődleges csomópont típusa és a többi nem elsődleges. Íme egy nem elsődleges csomópont típusa jellemzői:

- A csomópont típushoz VMs minimális méretét a kiválasztott tartóssági réteg határozza meg. A tartóssági réteg alapértelmezés szerint bronz. Görgessen lefelé a tartóssági réteg van, és az értékek is tovább tarthat tájékoztatást.  

- A csomópont típushoz VMs minimális számú egyike lehet. Az alkalmazás/szolgáltatásokat szeretné, hogy futtassa a csomópont-típus replikái száma alapján száma azonban kell választania. Csomópont típusú VMs számát növelhető a fürt telepítése után.


## <a name="the-durability-characteristics-of-the-cluster"></a>A fürt tartóssági jellemzői

A tartóssági réteg jelezheti a rendszer a jogosultságok, amely a VMs, hogy a mögöttes Azure infrastruktúra használják. Írja be a elsődleges csomópontot Ez a jogosultság lehetővé teszi, hogy szünet virtuális szintű infrastruktúra kérésének (például egy virtuális indítsa újra a rendszert, virtuális reimage vagy egy virtuális áttelepítés), amely hatással lehet a rendszer services és az állapot-nyilvántartó services kvórum követelményei szolgáltatás háló. Ez a jogosultság nem elsődleges csomópont fájltípusok lehetővé teszi a szolgáltatás háló szünet virtuális szintű infrastruktúra kérelmet például virtuális indítsa újra a rendszert, a virtuális reimage, a virtuális áttelepítési fut, akkor az állapot-nyilvántartó szolgáltatások kvórum követelményei lehetnek stb.

Ez a jogosultság van megadva az alábbi értékeket:

- Arany - infrastruktúrát feladatok is lehet fel van függesztve UD / 2 órán időtartamára

- Ezüst - a infrastruktúra feladatok is felfüggesztett, egy időtartam 30 percig per UD (Ez jelenleg nem engedélyezett való használatra. Egyszer engedélyezve ez lesz elérhető összes normál VMs egyetlen core és annál).

- Bronz - nincs jogosultságokkal. Ez az alapértelmezett érték.

## <a name="the-reliability-characteristics-of-the-cluster"></a>A fürt megbízhatóság jellemzői

A rendszer szolgáltatások, amely a használni kívánt a fürt elsődleges csomópont típusú kópiák számának megadása a megbízhatósági szint szolgál. A további a szám kópiák, a megbízhatóbb a rendszer szolgáltatások vannak a fürt.  

A megbízhatóság réteg is igénybe vehet az alábbi értékeket.

- Platina - rendszer szolgáltatásokat futtatják a cél kópiakészlet 9 száma

- Arany - rendszer szolgáltatásokat futtatják a cél kópiakészlet 7 száma

- Ezüst - a rendszer szolgáltatások futtatása a cél kópiakészlet 5 száma

- Bronz - rendszer szolgáltatásokat futtatják a cél kópiakészlet 3 száma

>[AZURE.NOTE] A megbízhatóság réteg, kiválaszthatja, hogy a lehető legkevesebb csomópontok rendelkeznie kell a elsődleges csomópont típusa határozza meg. A megbízhatósági szint nincs hatással van a fürt maximális méretét. 20 csomópont fürt is van, így a bronz megbízhatóság rendszerű.

 Megadhatja, hogy a másikra az egy réteg fürt megbízhatóságát frissítéséhez. Ezzel elindítja a fürt frissítések szükséges módosít a rendszer szolgáltatások listában válassza a szám. Várja meg a frissítés folyamatban befejeződése után a változtatások más a fürthöz, például vehet fel a csomópontok stb.  A frissítés szolgáltatás háló Explorer vagy futtassa a [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) végrehajtásának figyelheti

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Miután végzett a kapacitás tervezése és beállítása a fürtre, olvassa el a következőket:
- [Szolgáltatás háló fürt biztonsági](service-fabric-cluster-security.md)
- [Szolgáltatás háló állapot modell – bevezetés](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
