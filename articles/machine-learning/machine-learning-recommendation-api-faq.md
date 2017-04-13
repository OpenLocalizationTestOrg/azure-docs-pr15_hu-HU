<properties 
    pageTitle="Beállítása és használata a gépi tanulási javaslatok API |} Microsoft Azure" 
    description="A Microsoft javaslatok API épülő Azure gépi tanulási – gyakori kérdések" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Beállításával és használatával gépi tanulási javaslatok API – gyakori kérdések


**Mi az javaslatok?**

>[AZURE.NOTE]Ez a verzió nem kell a javaslatok kognitív API-szolgáltatás használatbavétele. A javaslatok kognitív szolgáltatást fogja cseréje ezt a szolgáltatást, és az új szolgáltatások vannak készüljön. Új funkciók támogatása, jobban API Explorer, a tisztább API felületre, egységesebb előfizetési és számlázási élmény stb kötegelés például rendelkezik.
> További tudnivalók [az új kognitív szolgáltatás áttelepítése](http://aka.ms/recomigrate)

Szervezetek, és idegen-értékesítés javaslatok és felfelé-értékesítés termékek az ügyfelek számára nyújtott szolgáltatások támaszkodó vállalkozások az Azure gépi tanulási javaslatok biztosít a javaslatok önkiszolgáló motor. Az alapvető algoritmus mátrix factorization használó együttműködésen alapuló szűrés megvalósítása. Alkalmazások fejlesztői számára is elérhessék javaslatok REST API-khoz. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Mire van lehetőségem a javaslatok?**

JAVASLATOK beviteli elemet vagy elemeket halmazának foglal, és a vonatkozó javaslatok listáját adja vissza. Példa: egy ügyfél, egy online viszonteladótól termék gombra kattint. Az online viszonteladótól küld az adott termék bemenetként átadja javaslatok, cserébe megkapja a termékek listáját, és úgy dönt, hogy melyik e termékek is megjelenik a felhasználói. Érdemes használni a javaslatok optimalizálhatja az online áruházba, vagy akár tájékoztatja a belső értékesítési részlege vagy -hívás központ.

**Vannak olyan korlátozásait használatát?**

Javaslatok korlátai a következő használatát:
* Maximális száma előfizetésenként modellek: 10
* A katalógus élvező elemek maximális száma: 100 000
* Maximális száma, melyek folyamatosan használatát pontok ~ 5,000,000. A legrégebbi is törli, ha újakat fog tölthetők fel vagy jelentett.
* Adatok (például katalógus adatok importálása párbeszédpanelen használati adatok importálása) e-mailben elküldött maximális mérete 200 MB
* Egy ~ 2 TP-k / szekundum (TP-k) a javaslatok modell fejlesztése, amely nem aktív a tranzakciók. Aktív javaslatok modell építés TP legfeljebb 20-k tárolhat.

##<a name="purchase-and-billing"></a>Vásárlás és számlázás 


**Mennyibe kerül az javaslatok költség az indítási időszakban?**

Javaslatok egy olyan előfizetés-alapú szolgáltatás. Töltődik havonta tranzakciók mennyisége alapján. Ellenőrizheti, hogy a [ajánlat lap] (https://datamarket.azure.com/dataset/amla/recommendations) a Microsoft Azure piactérről a árinformációkat.

**Olyan bármely javaslatok problémákat társított költségek nyomon követése és felhasználói tevékenység tárolni a számomra?**

Nem pillanatában.

**Muszáj javaslatok az ingyenes próbaverzióra?**

Van egy ingyenes pontosan, amelynek 10 000 tranzakciók havonta korlátozódik.

**Mikor fog lehet számlát kapni ajánlások?**

A fizetős verzióra bármely előfizetést, amelyhez nincs havi díjat. A fizetős verzióra vásárlásakor közvetlenül az előfizetést terhelő az első hónapban használatát. Az összeg, az ajánlatot az előfizetések lapon (plusz alkalmazandó áfával) a társított terheli. E havi díj lett ugyanazt a naptári dátumot, az eredeti vásárolta havonta mindaddig, amíg az előfizetés lemondása. 

**Hogyan frissíthetem magasabb réteg szolgáltatás?**

Vásároljon, vagy frissítse a lapról [ajánlat] az előfizetés a Microsoft Azure piactéren (https://datamarket.azure.com/dataset/amla/recommendations) lap.

Előfizetés frissítéskor:

* A régi előfizetés hátralévő tranzakciók nem bekerülnek az új előfizetésbe. 
* Fizet teljes ár az új előfizetés, akkor is, ha rendelkezik a régi előfizetés még nem használt tranzakciók.

A folyamat előfizetés frissítése:

* A [ajánlat lapra] Nevigate (https://datamarket.azure.com/dataset/amla/recommendations).
* Ha még nem jelentkezett be, jelentkezzen be a piactérről.
* A jobb oldali sávban a rendelkezésre álló csomagok jelennek meg. Kattintson a frissíteni kívánt csomagot a választógombok gombra.
* Ha frissíteni szeretné, kattintson az **OK gombra**. Ha nem szeretné frissíteni, kattintson a **Mégse gombra**.

**Fontos** Gondosan olvassa el a párbeszédpanel, mert nincsenek számlázás és használatának következmények frissítése előtt.

**Mikor érjen véget ajánlások az előfizetésem?**

Az előfizetés befejeződik, amikor szakítsa meg. Ha szeretné, hogy az előfizetés lemondásáról, olvassa el a következő útmutatót.

**Hogyan mondhatom le javaslatok előfizetésem?**

Az előfizetés lemondásáról, kövesse az alábbi lépéseket. Ha a jelenlegi előfizetését a fizetős verzióra, az előfizetés továbbra is a gyakorlatilag az aktuális számlaidőszakra végéig. Ha módosítania kell a lemondás azonnal érvénybe, kapcsolatfelvétel a [Microsoft-támogatást](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Megjegyzés:** Ha a számlázási időszak vagy még nem használt tranzakciók számlázási időszak vége előtt lemondja visszatérítés nem kap.

* Nyissa meg a [ajánlat lapot] (https://datamarket.azure.com/dataset/amla/recommendations).
* Ha még nem jelentkezett be, jelentkezzen be a piactérről.
* Jobbra lévő az adatkészlet nevét, és állapota a **Mégse** gombra. Használhatja ezt az előfizetést, az aktuális számlaidőszakra végéig, vagy a tranzakció letelik (amelyik előbb letelik).

Ha szeretné, hogy az előfizetés lemondásáról, hogy vásároljon új előfizetést közvetlenül, a fájl a [Microsoft terméktámogatási](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn)jegy.

##<a name="getting-started-with-recommendations"></a>Javaslatok – első lépések

**Javaslatok az számomra?** 

A gépi tanulási javaslatok szervezetek és javaslatok határokon-értékesítés és a felfelé-értékesítés termékek vagy a szolgáltatások ügyfeleik támaszkodó vállalkozások szolgál. Ha van egy ügyfél webhelye, egy értékesítés, belső értékesítés, vagy a telefonos ügyfélszolgálat és, ha több mint néhány dozen termékek és szolgáltatások katalógushoz, az utolsó sor előfordulhat, hogy élvezhetik javaslatok használatával. 

Javaslatok kísérletezzen lett tervezve meglehetősen egyszerű. Az aktuális API-alapú verzióját egyszerű programozási ismeretek szükséges. Ha segítségre van szüksége, forduljon a alvállalkozó, akinek a webhely fejlesztett. Ha belső informatikai részleggel vagy egy belső fejlesztő kell személyre javaslatok használhat. 

**Mik azok a javaslatok beállítása előfeltételei**

Javaslatok, hogy kell rendelkeznie a naplózási felhasználói választási lehetőségek, a katalógus vonatkozik. Ha t van ilyen a naplózási és van ügyfél szemben, javaslatok összegyűjtheti felhasználói tevékenység meg. 

Javaslatok a katalógus a termékek és szolgáltatások is szükség van. Ha t van a katalógus, javaslatok ment egy katalógushoz és használhatja a tényleges ügyféladatok használatát. Egy implicit katalógus nem fogják tartalmazni a nem felhasználói tranzakciók részeként jelentő elemek.

**Hogyan tegye beállítani javaslatok az első alkalommal?**

Utána [előfizetés] (https://datamarket.azure.com/dataset/amla/recommendations) javaslatok, használjon API át az [Azure gépi tanulási javaslatok rövid útmutató az első lépésekhez](machine-learning-recommendation-api-quick-start-guide.md) állíthatja be a szolgáltatás.

**Hol találom meg a dokumentáció API-val?** 

Az API-dokumentáció [Azure gépi tanulási javaslatok rövid útmutató az első lépésekhez](machine-learning-recommendation-api-quick-start-guide.md).

**Milyen lehetőségeket katalógus és használati adatainak feltöltése javaslatok van?**

A katalógustétel- és használati adatainak feltöltése két lehetőség közül választhat: az adatok exportálása a CRM rendszerrel vagy más naplók, és töltse fel a javaslatok, vagy hozzáadhat a címkéket a webhelyét, amelyek a felhasználói tevékenységek figyelemmel kísérheti. Az utóbbi módszert használja, ha az adatokat az Azure tárolhatók.

##<a name="maintenance-and-support"></a>Karbantartási és támogató forrásaihoz.

**Milyen nagy az adatkészlet lehet?**

Az adatkészletek legfeljebb 100 000 katalógustételek tartalmaz, és a másolatot használati adatok 2048 MB.
Ezeken kívül előfizetés legfeljebb 10 adatkészletek (modellek) is tartalmazhatnak.

**Hol kaphatok technikai támogatást a javaslatok?**

Technikai támogatás érhető el a [Microsoft Azure támogatja a](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) webhelyen.

**Hol találom meg a használati feltételei?**

[Microsoft Azure gépi tanulási javaslatok API szolgáltatási feltételeket](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
