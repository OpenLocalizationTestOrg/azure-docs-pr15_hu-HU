<properties 
    pageTitle="Útmutató a nettó # adagolás hálózatok nyelv |} Microsoft Azure" 
    description="A nettó # adagolás szintaxisának hálózatok nyelv, hogyan hozhat létre egy egyéni adagolás hálózati modell nettó használata a Microsoft Azure Machine Learning példák együtt#" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jeannt" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Az Azure gépi tanulási nettó # adagolás hálózati nyelv útmutató

## <a name="overview"></a>– Áttekintés
Nettó # adagolás hálózati architektúrákban adagolás hálózati modulok definiálhat a Microsoft Azure gépi tanulási használt Microsoft által kifejlesztett nyelv. Ebben a cikkben megismerheti:  

-   A adagolás hálózatokhoz kapcsolódó alapfogalmak
-   Adagolás hálózati követelmények és az elsődleges összetevők megadása
-   Szintaxisának és a nettó # specifikációja nyelv kulcsszavak
-   Példák a nettó használatával létrehozott egyéni adagolás hálózatok# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Alapvető hálózati adagolás
A csomópontok között vannak rendezve, a ***Rétegek menügombra***, és súlyozott ***kapcsolatok*** (vagy ***élek***) ***csomópontot*** adagolás hálózati szerkezetet tartalmazza. A kapcsolatok irányt jelző elemeket is, és minden kapcsolat ***forrás*** csomópontot, a ***cél*** csomópontot.  

Minden ***trainable réteg*** (a rejtett vagy egy kimenet réteg) tartalmaz egy vagy több ***kapcsolat kötegeket***. A kapcsolat az első lépésekhez, ahol a forrás réteg és a kapcsolatokat a forrás réteg leírását. Adott a köteg minden kapcsolat ugyanazon a ***forrás réteg*** és ugyanazon a ***cél réteg***megosztása. A nettó # egy kapcsolatot az első lépésekhez tekinteni az első lépésekhez cél réteghez tartozó.  
 
Nettó # támogatja a különböző típusú kapcsolat kötegeket, amely lehetővé teszi a módon bemenetben testreszabása rejtett rétegek megfeleltetve, és a kimeneti értékeket megfeleltetve.   

Az alapértelmezett vagy szabványos az első lépésekhez egy **teljes az első lépésekhez**, amelyben minden egyes csomópont a forrás rétegben a cél réteg minden csomóponthoz kapcsolódik.  

Ezenkívül nettó # támogatja a következő négy típusú speciális csatlakozási kötegeket:  

-   **Szűrt kötegeket**. A felhasználó definiálásának lehetséges módjai egy predikátumok helye a forrás réteg csomópontot, és a cél réteg csomópontot. Csomópontok össze, amikor a predikátumok értéke igaz.
-   **Convolutional kötegeket**. A felhasználó a forrás réteg határozhatja meg a kis környékeken csomópontok. Minden egyes csomópontot a cél rétegben a forrás rétegben csomópontok egy hálózatok csatlakozik.
-   **Kötegeket egyesítését** és **Válasz normalizálás kötegeket**. Az alábbiakban convolutional kötegeket hasonló annak, hogy a felhasználó a forrás rétegben csomópontok kis környékeken határozza meg. A különbség a súlyok, az alábbi kötegeket a szegélyek, amelyek nem trainable. Előre definiált függvényt érvényes ehelyett a forrásértékeket csomópont határozza meg a célként megadott csomópontot.  

Nettó # adagolás hálózat struktúra használatával lehetővé teszi például mély adagolás hálózatok vagy tetszőleges dimenziókat, amelyekről javítható az adatok, például kép, hang vagy videó tanulási convolutions összetett szerkezetek meghatározása.  

## <a name="supported-customizations"></a>Támogatott testreszabások
Azure gépi tanulási létrehozott adagolás hálózati modellek architektúráját öröklődést testre szabható nettó # használatával. képes vagy:  

-   Hozza létre a rejtett rétegeket, és szabályozhatja a rétegekre található csomópontok számának.
-   Adja meg, hogyan rétegek egymással csatlakoztatni.
-   Speciális csatlakozási struktúrákat, például convolutions és kötegeket megosztása vastagságának megadása
-   Adja meg a másik aktiválási függvények.  

A specifikációja nyelvi szintaxist, című cikkben olvashat [Struktúra specifikációja](#Structure-specifications).  
 
[A példák néhány gyakori gépi tanulási feladatok szimplex: összetett, hogy az adagolás hálózatok definiáló példák azt.](#Examples-of-Net#-usage)  

## <a name="general-requirements"></a>Általános követelmények
-   Pontosan egy kimeneti réteg, legalább egy, a bemeneti réteg és nulla vagy több rejtett rétegek kell. 
-   Minden réteg alakít adott számú tetszőleges méretek téglalap alakú tömbje adatbázistáblákhoz rendezett csomópontok tartalmaz. 
-   Beviteli rétegek nem társított képzett paramétereket van, és képviseli, ahol a példány adatait a hálózat belép. 
-   Trainable rétegek (a rejtett és a kimeneti rétegek) képzett paraméterek neve súlyok és tömegmérés vannak társítva. 
-   A forrás- és céltáblák csomópontok külön sorokba kell lennie. 
-   Kapcsolatok aciklikus; kell lennie. Ez azt jelenti nem lehet egy lánc kapcsolatok vezet az eredeti forrás csomópontot.
-   A kimenet réteget nem lehet egy olyan kapcsolatot az első lépésekhez forrás réteg.  

## <a name="structure-specifications"></a>Szerkezeti alkalmazásra vonatkozó technikai adatok
A hálózati adagolás szerkezetének megadása három szakaszból áll: a **állandó deklaráció**, a **réteg nyilatkozat**, a **kapcsolat nyilatkozat**. Egy választható **deklaráció megosztása** szakaszban is van. A szakaszok sorrendben követik egymást adható meg.  

## <a name="constant-declaration"></a>Állandó deklaráció 
Egy konstans deklaráció nem kötelező. Teszi lehetővé az adagolás hálózati definícióban máshol használt értékek megadását. A deklarálási utasítás áll egy egyenlőségjelet, majd egy érték kifejezés azonosítót.   

Az alábbi kimutatás például **x**állandó határozza meg:  


    Const X = 28;  

Két vagy több állandók egyidejű definiálásához idézőjelek közé kell tenni azonosító neve és értékek kapcsos zárójelek, és őket választják el egymástól pontosvesszővel használatával. Példa:  

    Const { X = 28; Y = 4; }  

Mindegyik hozzárendelés kifejezésre jobb oldalán lévő lehet egész szám, valós szám, egy logikai érték (IGAZ vagy hamis) vagy egy matematikai kifejezést. Példa:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Réteg deklaráció
A réteg nyilatkozat szükség. Azt határozza meg, a méret és a réteget, attribútumok és együtt a kapcsolat kötegeket forrását. A deklarálási utasítás (beviteli, rejtett vagy kimeneti) a réteg nevével kezdődik követi a méretét, a réteg (egy sor bármelyik eleme pozitív egész számok). Példa:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   A termék méretek közül a réteg csomópontok számának. Ebben a példában nincsenek két dimenzióban [5,20], ami azt jelenti, hogy a réteg 100 csomópontok szerepelnek.
-   A réteg is be kell jelenteni, tetszőleges sorrendben, egy kivétellel: Ha egynél több beviteli réteg van megadva, a sorrendben, amelyben deklarálva meg kell egyeznie a bemeneti adatok szolgáltatások sorrendjének.  


Adja meg, hogy az egy réteg található csomópontok számának automatikusan kell meghatározni, használja az **automatikus** kulcsszó. Az **automatikus** kulcsszó különféle effektusok, attól függően, hogy a réteg foglalja magában:  

-   Az egy réteg beviteli deklaráció csomópontok számának a bemeneti adatok szolgáltatások számát.
-   A Rejtett réteg nyilatkozatot csomópontok számának a **rejtett csomópontok számának**a paraméter által megadott számot. 
-   A kimenet réteg nyilatkozat, a csomópontok számának két szintű osztályozásához a regressziós és multiclass osztályozási kimeneti csomópontok számának egyenlő 1 2.   

Például a következő hálózat definíciót lehetővé teszi, hogy minden réteg automatikusan meghatározott méretének:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


A réteg deklarálása trainable réteg (a rejtett és a kimeneti rétegek) tetszés szerint is elhelyezhet a kimenet függvény (más néven egy aktiválási függvény), amely alapértelmezés szerint **sigmoid** besorolás modellek és a **lineáris** regressziós modellekhez. (Még akkor is, ha az alapértelmezett használja, akkor is kifejezetten az aktiválás függvénnyel áttekinthetővé tetszés.)

A következő kimeneti függvények használhatók:  

-   sigmoid
-   lineáris
-   softmax
-   rlinear
-   négyzetes
-   gyök
-   srlinear
-   ABS
-   TANH 
-   brlinear  

Ha például a következő nyilatkozat a **softmax** függvényt használja:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Kapcsolat deklaráció
Közvetlenül azután, hogy a trainable réteg definiálásával, a rétegeket beállított közötti kapcsolatok deklarálása. A kapcsolat az első lépésekhez nyilatkozat kezdődik, a kulcsszó **a**, majd a nevét az első lépésekhez forrás réteg és hozhat létre kapcsolatot az első lépésekhez típusú.   

Kapcsolat kötegeket öt típusú jelenleg támogatott:  

-   **Teljes** kötegeket, a kulcsszó **összes** jelölt
-   **Szűrt** kötegeket, a kulcsszó **Hol**predikátumalakzat kifejezés követi jelölt
-   **Convolutional** kötegeket, a kulcsszó **convolve**, a konvolúció attribútumok követ jelölt
-   **Pooling** kötegeket, a kulcsszavak **maximális** vagy a **készlet jelenti** jelölt
-   **Válasz normalizálás** kötegeket, a kulcsszó- **Válasz normát** jelölt      

## <a name="full-bundles"></a>Teljes kötegeket  

Egy teljes kapcsolat az első lépésekhez a forrás réteg a cél réteg minden csomóponthoz minden csomóponton kapcsolat tartalmazza. Ez az alapértelmezett hálózati kapcsolat típusát.  

## <a name="filtered-bundles"></a>Szűrt kötegeket
A szűrt kapcsolat az első lépésekhez megadása egy predikátumalakzat, szintaktikai szempontból, kifejezett jelentős, például a C# lambda kifejezést tartalmaz. Ez a példa két szűrt kötegeket határozza meg:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   A _ByRow_előtagjának **s** indexet jelölő be a bemeneti réteg _képpont_, csomópontok téglalap alakú tömbje paraméter pedig **d** indexet jelölő be a Rejtett réteg _ByRow_csomópontok tömbje paraméter. **S** és a **d** hosszúságú két egész számok rekordhoz. Ebben az értelemben az egészérték részt veszi figyelembe az összes pár fölé **s** tartományok _0 < = [0] s < 10_ és _0 < = az s-[1] < 20_, és a **d** Ranges (tartományok) keresztül egész számok, az összes pár _0 < = [0] d < 10_ és _0 < = d[1] < 12_. 
-   A jobb oldalon a predikátumalakzat kifejezés értéke egy feltétel. Ebben a példában mindegyik értékét **s** és a **d** úgy, hogy a feltétel igaz, fennáll a forrás réteg csomópont él a cél réteg csomópontra. A filter kifejezésére így jelzi, hogy az első lépésekhez tartalmaz-e egy kapcsolatot az **s** a csomópontra, **d** , az összes olyan esetekben, amikor [0] s egyenlő [0] d által meghatározott által meghatározott csomópontot.  

Tetszés szerint megadhatja a szűrt az első lépésekhez súlyok csoportja. A **súlyok** attribútum érték, amely megfelel az első lépésekhez által meghatározott kapcsolatok száma hosszúságú lebegőpontos értékek a rekordhoz kell lennie. Alapértelmezés szerint súlyok véletlenszerűen jönnek létre.  

Súly értékeket a cél csomópont index szerint vannak csoportosítva. Ez azt jelenti, hogy az első cél csomópont K forrás csomópontok csatlakozik, ha az első _K_ a **súlyok** sor bármelyik eleme elemei az első cél csomópontra, tárgymutató-sorrendben súlyok. A többi címzett csomópontot ugyanúgy vonatkoznak.  

Ajánlatos súlyok közvetlenül állandó értékeket adja meg. Például ha korábban megtanulta súlyok, megadhatja őket a következő szintaxissal állandóként:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional kötegeket
Az oktatóanyag adatok szerkezete homogén, amikor convolutional kapcsolatok gyakran használják megtudhatja, az adatok magas szintű funkciókat. Például kép-, hang- vagy videofájlok adatokat, térbeli vagy időbeli dimensionality lehet meglehetősen egységes.  

Convolutional kötegeket is ütközik a dimenziókat keresztül téglalap alakú **mag** alkalmazhat. Minden egyes kernel lényegében egy sor olyan **kernel alkalmazások**néven alkalmazott helyi környékeken, súlyok határozza meg. Minden kernel alkalmazás felel meg a forrás réteg hivatkozik, amely a **központi csomópont**csomópontra. A súlyok egy kernel meg vannak osztva sok kapcsolatok között. Convolutional köteget, az egyes kernel téglalap alakú és minden kernel alkalmazások azonos méretű.  

Convolutional kötegeket támogatja a következő attribútumok:

**InputShape** a dimensionality a forrás réteg convolutional Ez az első lépésekhez alkalmazásában határozza meg. Az érték egy sor bármelyik eleme pozitív számokból álló kell lennie. Az egész számok szorzatát meg kell egyeznie a forrás rétegben csomópontok számának, de ellenkező esetben nem szükséges az adatforrás réteghez tartozó deklarálva dimensionality megfelelően. Ez a rekord hossza convolutional az első lépésekhez **Aritás** érték lesz. (Általában Aritás hivatkozik argumentumokat vagy függvény tehet operandus száma)  

Az alakzatra, és a mag helyének megadásához használja az attribútumok **KernelShape**, **lépésköz**, **belső margók**, **LowerPad**és **UpperPad**:   

-   **KernelShape**: (kötelező) Defines a dimensionality convolutional az első lépésekhez az egyes kernel. Az érték, amely megegyezik az első lépésekhez az aritása hosszúságú pozitív számokból álló rekordhoz kell lennie. Minden összetevő e összetevője nem nagyobb, mint a megfelelő összetevője **InputShape**kell lennie. 
-   **Lépésköz**: (nem kötelező) definiálja csúsztatható lépés méretű konvolúció (egy lépésben mérete az egyes dimenzió), amely a központi csomópontok közötti távolságot. Az érték, amely a köteget az aritása hosszúságú pozitív számokból álló rekordhoz kell lennie. Minden összetevő e összetevője nem nagyobb, mint a megfelelő összetevője **KernelShape**kell lennie. Az alapértelmezett értéke az egyik egyenlő minden összetevő egy sor bármelyik eleme. 
-   **Megosztás**: (nem kötelező) definiálja a konvolúció minden dimenziója megosztás vonalvastagság. Az érték lehet egyetlen logikai értéket vagy logikai értékek, amelyek a köteget az aritása hosszúságú egy sor bármelyik eleme. Egyetlen logikai értéket bővített egy sor bármelyik eleme, minden összetevő egyenlő a megadott értékkel rendelkező hossza megfelelő lesz. Az alapértelmezett értéke egy sor bármelyik eleme minden igaz értéket tartalmazza. 
-   **MapCount**: (nem kötelező) Defines convolutional az első lépésekhez a megfeleltetések szolgáltatás számát. Az érték az egyetlen pozitív egész vagy egy sor bármelyik eleme pozitív egész számok az a köteget aritása hosszúságú lehet. Egy egyetlen egész szám a megadott érték megegyezik az első összetevők, a megfelelő hosszának egy sor bármelyik eleme lehet terjeszteni, és a hátralévő összetevők egyik egyenlő. Az alapértelmezett értéket az egyik. Térképek szolgáltatás száma a sor bármelyik eleme összetevői szorzatát. Ez a teljes szám faktoring végig az összetevők határozza meg, hogyan szolgáltatás térkép értékeket csoportosítva vannak a cél csomópontok. 
-   **Súlyok**: az első lépésekhez kezdeti súlyok (nem kötelező) definiálja. Az érték egy sor bármelyik eleme lebegő hosszúságú mag hányszor legyen pont értékek súlyok száma egy kernel, a jelen cikk meghatározott kell lennie. Az alapértelmezett súlyok véletlenszerűen jönnek létre.  

Kétféle tulajdonságaik vannak, amelyek belső margóinak, a tulajdonságok alatt egymást kölcsönösen kizáró van:

-   **Belső margók**: (nem kötelező) Meghatározza hogy kell-e a a bemeneti konvertálása egy **alapértelmezett belső margóinak séma**használatával. Az érték lehet egyetlen logikai értéket, vagy egy sor bármelyik eleme logikai értékek, amelyek a köteget az aritása hosszúságú lehet. Egyetlen logikai értéket bővített egy sor bármelyik eleme, minden összetevő egyenlő a megadott értékkel rendelkező hossza megfelelő lesz. Dimenzió értéke igaz, ha a forrás logikailag konvertálása az adott dimenzióban a nulla értékű cellákra további kernel választására, úgy, hogy az adott dimenzióban az első és utolsó mag központi csomópontjai az első és utolsó csomópontok, hogy a forrás rétegben dimenzióban. Tehát minden irányban "üres" csomópontok számának automatikusan határozza meg, hogy pontosan elférjen _(InputShape [d] - 1) / [d] lépésköz + 1_ a tömöttnek forrás rétegbe mag. Dimenzió értéke HAMIS, ha a mag határozzák meg, hogy mindkét oldalon, hagyott csomópontok száma megegyezik a (legfeljebb 1 különbség). Az attribútum alapértelmezett értéke hamis egyenlő minden összetevő a rekordhoz.
-   **UpperPad** és **LowerPad**: (nem kötelező) Provide jobban kézben tarthatók kitöltés használata mennyiségét. **Fontos:** Következő attribútumok definiálható csak, ha a fenti **belső margók** tulajdonság ***nincs*** megadva. Az értékek, amelyek a köteget az aritása hosszúságú egész értékű kockára kell lennie. Ha ezek az attribútumok meg vannak adva, "üres" csomópontok hozzáadódnak az egyes dimenzió, a bemeneti réteg alsó és felső végén. Az alsó és felső végű minden irányban hozzáadott csomópontok számának rendre **LowerPad**[i] és [i] **UpperPad**alapján történik. Annak érdekében, hogy mag csak "valós" csomópontok és a "üres" csomópontok nem felel meg, az alábbi feltételek kell teljesülnie:
    -   Minden **LowerPad** összetevője kell feltétlenül kisebb, mint KernelShape [d] / 2. 
    -   Minden egyes **UpperPad** összetevője nem lehet nagyobb, mint KernelShape [d] / 2. 
    -   A következő attribútumok alapértelmezett értéke egy sor bármelyik eleme, amelyben minden összetevő egyenlő 0. 

A **Kitöltés** beállítás = true lehetővé teszi, hogy minél nagyobb tartalomelrendezésre ahhoz, hogy "" a dióbél belül "valós" bemenet van szükség szerint. Ez a művelet a matematikai egy kicsit kiszámításához a kimeneti méretét. Általánosságban elmondható, a kimeneti méretének _D_ számítja ki, hogy _D = (I - K) / S + 1_, ahol _lehet_ a bemeneti méretét, _K_ kernel mérete, _S_ a lépésköz, és _/_ van (a nulla felé kerekít kerekítés) hányados egész szám. Ha UpperPad = [1, 1], a bemeneti _lehet_ mérete hatékony 29, és így _D = (29-5) / 2 + 1 = 13_. Jó helyen jár, ha **belső margók** = igaz, lényegében _lehet_ kap bumped, _K – 1_; így _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Értékek megadásával **UpperPad** és **LowerPad** sokkal további beállítási lehetőségekre a belső margóinak, ha csak **belső margók** = igaz, mint kap.

Többet szeretne tudni a convolutional hálózatok és az alkalmazásaikat az alábbi cikkekben talál:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.microsoft.com/Pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.Mit.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Kötegeket egyesítése
**Az első lépésekhez egyesítése** érvényes mértani convolutional kapcsolódási hasonló, de a előre definiált függvények csomópontot forrásértékeinek használja a cél csomópont érték származhat. Ennélfogva egyesítési kötegeket van nincs trainable állapota (súlyok vagy tömegmérés). Egyesítési kötegeket támogatja a convolutional tulajdonságait, **súlyok**, **megosztás**és **MapCount**kivételével.  

Általában a szomszédos egyesítési egységek összesített mag nem áll átfedésben. A lépésköz [d] megegyezik KernelShape [d] minden irányban, a kapott réteg esetén a hagyományos helyi egyesítési réteget, amelyek gyakran alkalmaznak convolutional adagolás hálózatokban. Minden cél csomópont kiszámítja, a maximum vagy a közép, a forrás rétegben a kernel tevékenységének.  

A következő példa bemutatja az egyesítési az első lépésekhez: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Az aritása az első lépésekhez: 3 (a kockára **InputShape** **KernelShape**és **lépésköz**hosszát). 
-   Az adatforrás rétegben csomópontok számának van _5 *24* 24 = 2880_. 
-   Ez a hagyományos helyi egyesítési réteg, mert **KernelShape** és a **lépésköz** egyenlő. 
-   A cél réteg csomópontok számának van _5 *12* 12 = 1440_.  
    
Egyesítési rétegek olvashat az alábbi cikkekben talál:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Szakasz 3.4.)
-   [http://cs.nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Válasz normalizálás kötegeket
**Válasz normalizálás** egy helyi normalizálás színsémát, először megjelent Geoffrey Hinton, és mások [ImageNet Classiﬁcation-mély Convolutional adagolás hálózatokkal](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf)könyvben. Válasz normalizálás az adagolás háló általánosítás támogatási szolgál. Ha egy idegsejt aktiválási nagyon magas szinten van alkalmaznak, helyi válasz normalizálás réteg nem jelenik meg a környező idegsejtek csoportjának viselkedését aktiválási szintjét. Ez történik, három (***α***, ***β***, és a paraméterekkel ***k***) és convolutional struktúra (vagy használatával hálózatok alakzat). Minden cél rétegben ***y*** idegsejt idegsejt ***x*** , az adatforrás rétegben felel meg. Az aktiválás szint ***y*** képlettel a következő képletet, ahol ***f*** idegsejt aktiválási szintjét, pedig ***Nx*** a kernel (vagy az ***x***a helyek a idegsejtek csoportjának viselkedését tartalmazó megadása), a következő convolutional struktúra által meghatározott:  

![][1]  

Válasz normalizálás kötegeket támogatja a convolutional tulajdonságait, **súlyok**, **megosztás**és **MapCount**kivételével.  
 
-   Ha a kernel idegsejtek csoportjának viselkedését az ***x***ugyanazt a térképet tartalmaz, a normalizálás rendszer nevezik **azonos térkép normalizálás**. Azonos térkép normalizálás megadásához az első koordináta **InputShape** az 1 értéket kell rendelkeznie.
-   Ha a kernel idegsejtek csoportjának viselkedését ugyanott térbeli ***x***tartalmazza, de a idegsejtek csoportjának viselkedését más térképeket, a normalizálás rendszer neve **térképek normalizálás keresztül**. Válasz normalizálás ilyen típusú űrlap létrehozása az egyik ablaktábláról a másikra idegsejt kimeneti értékeket különböző térképeken számított nagy aktiválási szintek versenyhelyzetből valós idegsejtek csoportjának viselkedését található típus szerint ösztönözve oldalsó gátlási hajtja végre. Annak meghatározásához, térképek normalizálás át, az első koordináta a nagyobb, mint egy és nem nagyobb, mint a térképek számát egész számnak kell lennie, és a koordináták a többi kell rendelkeznie az 1 értéket.  

Válasz normalizálás kötegeket előre definiált függvényt alkalmaz a forrásértékeket csomópont határozza meg a célként megadott csomópontot, mivel nincsenek trainable állapota (súlyok vagy tömegmérés) rendelkeznek.   

**Értesítés**: a cél rétegben a csomópontok idegsejtek csoportjának viselkedését, amelyek a mag központi csomópontjai felelnek meg. Ha például KernelShape [d] argumentuma páratlan, majd _KernelShape [d] / 2_ felel meg a központi kernel csomópontot. Ha _KernelShape [d]_ még akkor is, a közép csomópont jelenleg _KernelShape [d] / 2-1_. Ezért ha a **belső margók**[d] hamis, az első és utolsó _KernelShape [d] / 2_ csomópontok nem rendelkezik a megfelelő csomópontok a cél rétegben. Ebben az esetben elkerülése érdekében határozza meg, mint a **belső margók** [értéke igaz, igaz,..., igaz].  

A korábban ismertetett négy attribútumok mellett válasz normalizálás kötegeket is a következő attribútumok támogatja:  

-   **Alfa**: (kötelező) adja meg, amely megfelel ***α*** az előző képlet lebegőpontos értéket. 
-   **A béta**: (kötelező) adja meg, amely megfelel ***β*** az előző képlet lebegőpontos értéket. 
-   **Eltolás**: (nem kötelező) adja meg, amely megfelel az előző képlethez ***k*** lebegőpontos értéket. 1 lesz az alapértelmezett.  

Az alábbi példa a választ normalizálás az első lépésekhez használatáról a következő attribútumok határozza meg:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   A forrás réteg öt 12 x 12 1440 csomópontok az összesítés aof dimenziójú térképek tartalmazza. 
-   **KernelShape** értékének azt jelzi, hogy ez egy azonos térkép normalizálás réteget, hol található a helyek a 3 x 3 téglalapot. 
-   Az alapértelmezett **belső margók** értéke HAMIS, így a cél réteghez hozzárendelt minden irányban csak 10 csomópontot. A cél réteget, amely megfelel a forrás réteg minden csomóponthoz felvenni egy csomópontot, belső margóinak hozzáadása = [igaz, igaz, a true]; és méretének módosítása: RN1 [5, 12, 12].  

## <a name="share-declaration"></a>Megosztás deklaráció 
Nettó # tetszés szerint is támogatja a megosztott súlyok több kötegeket. A súlyok bármely két kötegeket is lehet megosztani, ha az azok szerkezetek azonosak. A következő szintaxissal határozza meg a megosztott súlyok kötegeket:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Például a következő megosztás nyilatkozat adja meg a réteg nevét, jelezve, hogy a súlyok és a tömegmérés megosztott:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   A beviteli szolgáltatások vannak partícionálni két egyenlő szélességű beviteli rétegek. 
-   A rejtett rétegek majd magasabb szintű szolgáltatások, a két bemeneti rétegek számítja ki. 
-   A megosztás nyilatkozat Itt adhatja meg, hogy _H1_ és _H2_ kell számítani az illető megfelelő ráfordítások megegyező módon.  
 
Azt is megteheti Ez lehet kell megadni a két külön megosztás deklarációs az alábbi képlettel történik:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

A rövid űrlapon is használhatja, csak akkor, ha a rétegeket egyetlen az első lépésekhez tartalmaz. Az általános megosztása mindenre csak, ha a megfelelő struktúra, ami azt jelenti, hogy rendelkeznek az azonos méretű, azonos convolutional mértani és így tovább.  

## <a name="examples-of-net-usage"></a>Példák a nettó # használatát
Ebben a részben néhány példával szemlélteti, hogyan használhatja nettó # rejtett rétegek hozzáadásához definiálja, hogyan rejtett rétegek más rétegek másokkal, és convolutional hálózatok összeállítása.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Egyszerű egyéni adagolás hálózat meghatározása: "Helló, világ" példa
Ez az egyszerű példa bemutatja, hogyan hozhat létre, amely tartalmaz egy rejtett rétegben adagolás hálózati modell.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

A példa azt mutatja be, néhány alapvető parancsok az alábbi képlettel történik:  

-   Az első sor határozza meg, a bemeneti réteg (az _adatok_nevű). Az **automatikus** kulcsszó használata esetén az adagolás hálózati automatikusan tartalmazza az összes szolgáltatás oszlop beviteli példáiban szereplő. 
-   A második sorban a Rejtett réteg hoz létre. A _H_ nevét a rejtett réteget, amelynek 200 csomópontok van rendelve. A réteg teljesen csatlakozik a bemeneti réteget.
-   A harmadik sorában a kimeneti réteg (más néven _O_), mely tartalmazza a 10 kimeneti csomópontok határozza meg. Az adagolás hálózati osztályozási használata esetén nem per osztály egy kimenet csomópontot. A kulcsszó **sigmoid** jelzi, hogy a kimenet függvény a kimeneti réteg van hozzárendelve.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Több rejtett rétegek meghatározása: számítógépen elérhetők például
A következő példa bemutatja, hogyan lehet több egyéni rejtett rétegek egy kicsit összetettebb adagolás hálózati megadása.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Ez a példa bemutatja az adagolás hálózatok nyelv több lehetőségei:  

-   A szerkezet két bemeneti réteget, _képpont_ és a _metaadatokat_tartalmaz.
-   A _képpont_ réteg cél rétegeket, _ByRow_ és _ByCol_két kapcsolat kötegeket, az adatforrás réteg.
-   A rétegeket, _összegyűjtése_ és az _eredmény_ több kapcsolat kötegeket cél sorokba.
-   A kimeneti réteget, az _eredmény_, akkor a két kapcsolat kötegeket; cél réteg egy, a második szintű rejtett (összegyűjtése) cél réteg, és a másik a cél réteg beviteli rétegen (metaadatok).
-   A rejtett rétegeket, _ByRow_ és _ByCol_, szűrt kapcsolódási predikátumalakzat kifejezések használatával adja meg. Pontosabban a csomóponthoz _ByRow_ az [x, y], amelyekre a csomópont első koordináta, egyenlő első index koordináta csomópontok _képpont_ csatlakozik x. Hasonlóképpen, a _ByCol az [x, y] csomóponthoz csatlakozik egy második koordináta a csomópontot, koordinálása második indexűnek csomópontok _képpont_ y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Multiclass osztályozási convolutional hálózaton meghatározása: számjegy felismerés példa
A következő hálózati meghatározása az célja, hogy mindig ismerik fel a számokat, és azt szemlélteti, hogy egy adagolás hálózati testreszabásához speciális módszerrel.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   A szerkezet tartalmaz egy beviteli rétegben, _kép_.
-   A kulcsszó **convolve** azt jelzi, hogy a rétegeket, _Conv1_ és _Conv2_ nevű convolutional rétegek. Minden egyes e réteg nyilatkozatokat követi a konvolúció attribútum listáját.
-   A nettó egy harmadik rejtett réteg, _Hid3_, mely teljes mértékben a második _Conv2_rejtett réteget.
-   A kimenet réteget, _számjegy_, csak a harmadik rejtett réteg _Hid3_kapcsolódik. A kulcsszó **összes** azt jelzi, hogy a kimeneti réteg _Hid3_teljesen csatlakozik.
-   Az a konvolúció aritása három (a kockára **InputShape**, a **KernelShape**, a **lépésköz**és a **megosztás**hosszát). 
-   A használati kernel súlyok egy _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Vagy 26 * 50 = 1300_.
-   A csomópontok rejtett rétegekre a következőképpen számíthatja:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
-   A teljes oldalszám csomópontok számítható ki a réteg, a feltüntetett dimensionality használatával [50, 5, 5], az alábbi képlettel történik: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Mivel a **megosztás**[d] hamis csak _d == 0_, mag száma _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Nyugták

A nettó # nyelvet az adagolás hálózat architektúrája testreszabásához Shon Katzenberger (Architect, gépi tanulási) és ALEKSZEJ Kamenev (szoftver adatbázismodellbe, Microsoft Research) által kifejlesztett Microsoft. Gépi tanulási projektek és a szöveg analytics kép észlelési kezdve alkalmazások belső használatos. További tudnivalókért lásd: az [Adagolás háló az Azure Machine Learning - nettó # bemutatása](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
