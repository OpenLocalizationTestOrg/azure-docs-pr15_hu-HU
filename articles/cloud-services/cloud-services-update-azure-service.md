<properties
pageTitle="Hogyan lehet frissíteni egy felhőalapú szolgáltatásba |} Microsoft Azure"
description="Megtudhatja, hogy miként frissítése a felhőbeli szolgáltatások Azure-ban. Megtudhatja, hogyan egy felhőalapú szolgáltatásba a frissítés során állásának."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Hogyan lehet frissíteni egy felhőalapú szolgáltatásba

## <a name="overview"></a>– Áttekintés
A 10 000 láb egy felhőalapú szolgáltatásba, vendégként való bekapcsolódáshoz OS, és együtt szerepkörök frissítése következő három lépést áll. Első lépésként a bináris és a konfigurációs fájljait az új felhőalapú szolgáltatásba vagy az operációs rendszer kell feltölteni. Ezután az Azure fenntartja a felhőbeli szolgáltatástól, az új felhőalapú szolgáltatás verzió az igényeknek megfelelően alakítható számítási és a hálózati anyagok. Végül az Azure-fokozatosan frissítése a bérlő Vendég OS, vagy új verzió megőrzése mellett az elérhetőségéről közbeni frissítés hajt végre. Ez a cikk azt ismerteti, hogy az utolsó lépésben – ez a működés közben frissítés részleteit.

## <a name="update-an-azure-service"></a>Frissítés az Azure szolgáltatás

Azure rendezi, a szerepkör-példányok frissítési tartományok (UD) nevű logikai csoportok be. Frissítés (UD) tartományai csoportként frissített szerepkör-példányok logikai csoportja.  Azure frissítéseket a felhőbe a részére, amely lehetővé teszi, hogy példányok más UDs továbbra is a forgalom felszolgálásához egy UD szolgáltatás.

Az alapértelmezett szám frissítési tartományok az 5. A szolgáltatás-definíciós fájl (.csdef) a upgradeDomainCount attribútum felvételével különféle frissítési tartományok is megadhat. Többet szeretne tudni a upgradeDomainCount attribútum olvassa el a [WebRole séma](https://msdn.microsoft.com/library/azure/gg557553.aspx) vagy [WorkerRole séma](https://msdn.microsoft.com/library/azure/gg557552.aspx)című témakört.

A szolgáltatás végrehajtásakor egy vagy több szerepkörök helyi frissítés Azure frissíti a aszerint, hogy melyik kategóriához tartoznak frissítési tartomány szerepköre példányok csoportja. Minden olyan tartományban megadott frissítési – leállítása őket, frissíteni őket, arra előfordulása biztonsági online – Azure frissítések alakzatot a következő tartomány lép. Csak az aktuális frissítési tartományban futó példányok leállítása által Azure azt ellenőrzi, hogy frissítés fordul elő a lehető legkisebb hatása a futó szolgáltatás. További tudnivalókért lásd [hogyan a frissítés során](#howanupgradeproceeds) a jelen cikk.

> [AZURE.NOTE] A kifejezések, **frissítése** és **frissítése** jelentése némileg eltér az Azure környezetben, miközben felhasználhatók szót azonos értelemben folyamatok és a dokumentumban a szolgáltatások leírása.

A szolgáltatás meg kell határoznia, hogy szerepkör frissíteni kell a szerepkör legalább két példánya helyi legrövidebb leállás nélkül. Ha a szolgáltatás csak egy példánya egy szerepkört áll, a szolgáltatás nem érhető el mindaddig, amíg befejeződik a helyi frissítés.

Ez a témakör bemutatja az Azure frissítéseit alábbi adatait:

-   [A szolgáltatás módosítása engedélyezett a frissítés során](#AllowedChanges)
-   [Hogyan halad a frissítés](#howanupgradeproceeds)
-   [A frissítés visszaállítása](#RollbackofanUpdate)
-   [Folyamatban lévő telepítéseken több mutating műveletek kezdeményezése](#multiplemutatingoperations)
-   [Frissítési tartományok szerepkörök terjesztése](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>A szolgáltatás módosítása engedélyezett a frissítés során
A következő táblázat mutatja a szolgáltatás engedélyezett módosításai frissítés során:

|Módosítások megengedett szolgáltatónál, szolgáltatások és szerepkörök|A helyi frissítés|Szakaszos (virtuális felcserélése)|Törlés és újból telepíteni|
|---|---|---|---|
|Operációs rendszer verziója|igen|igen|igen
|.NET adatvédelmi szint|igen|igen|igen|
|Virtuális gép méret<sup>1</sup>|Igen,<sup>2</sup>|igen|igen|
|Helyi tárhely beállításai|<sup>2</sup> csak növelése|igen|igen|
|Hozzáadása és eltávolítása szerepkörök szolgáltatás|igen|igen|igen|
|Az adott jogosultság példányainak száma|igen|igen|igen|
|Szám vagy egy szolgáltatás végpontok típus|Igen,<sup>2</sup>|nem|igen|
|Nevek és értékek konfigurációs beállítások|igen|igen|igen|
|Értékek (de nem nevek) konfigurációs beállítások|igen|igen|igen|
|Új tanúsítványok hozzáadása|igen|igen|igen|
|Meglévő tanúsítványok módosítása|igen|igen|igen|
|Új kód terjesztése|igen|igen|igen|
<sup>1</sup> Csak korlátozottan érhető el a felhőbeli szolgáltatástól méretű részét méretének módosítása

<sup>2</sup> Azure SDK 1.5-ös vagy újabb verzió szükséges.

> [AZURE.WARNING] A virtuális gép méretének módosítása fog destroy helyi adatok.


Az alábbi elemek a frissítés során nem támogatottak:

-   Szerepkör nevének módosítása. Távolítsa el, és az új névvel ellátott szerepkör fel.
-   A tartomány frissítése számának módosítása
-   A helyi erőforrások méretének csökkentése.

Ha más frissítéseket a szolgáltatás definition, például a helyi erőforrás, méretének csökkentése egy virtuális felcserélése frissítés inkább kell végrehajtania. További információ a [Felcserélése telepítési](https://msdn.microsoft.com/library/azure/ee460814.aspx)témakörben talál.

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Hogyan halad a frissítés
Eldöntheti, hogy szeretné-e a szerepkörök, a szolgáltatásban vagy a szolgáltatás egyetlen szerepkör az összes frissítése. Bármelyik lehetőséget választja frissítendő van, és az első frissítési tartományhoz tartozik szerepkörökhöz összes előfordulását le, frissíteni, és vissza online állapotba. Visszatérés online, miután a második frissítési tartományban példányok leállt, frissíteni, és vissza online állapotba. Egy felhőalapú szolgáltatásba beállíthatja, hogy legfeljebb egy frissítés aktív egyszerre. A frissítés mindig a legújabb a felhőbeli szolgáltatástól hajtja végre.

Az alábbi ábra bemutatja, hogyan halad a frissítés, ha frissít összes a szerepkör a szolgáltatásban:

![Frissítési szolgáltatás] (media/cloud-services-update-azure-service/IC345879.png "Frissítési szolgáltatás")

A következő ábra bemutatja, hogyan halad a frissítés, ha csak egyetlen szerepkör:

![Szerepkör frissítése] (media/cloud-services-update-azure-service/IC345880.png "Szerepkör frissítése")  

> [AZURE.NOTE] Frissítéskor szolgáltatás egy példányát a több példányban a szolgáltatás állapotba kerül, amíg megtörténik a frissítés módja Azure frissítések szolgáltatások miatt. A szolgáltatás szolgáltatásiszint-szerződés garanciavállaló szolgáltatáselérhetőség csak több példányával telepített szolgáltatások vonatkozik. Az alábbi lista bemutatja, hogyan befolyásolják az egyes meghajtón lévő adatok az egyes Azure szolgáltatás frissítési forgatókönyv:
>
>Virtuális újraindítása:
>
-   C: megmaradnak
-   D: megmaradnak
-   E: megmaradnak
>
>Portál újraindítása:
>
-   C: megmaradnak
-   D: megmaradnak
-   E: semmisíteni
>
>Portál Reimage:
>
-   C: megmaradnak
-   D: semmisíteni
-   E: semmisíteni

>A helyi frissítés:
>
-   C: megmaradnak
-   D: megmaradnak
-   E: semmisíteni
>
>Csomópont áttelepítési:
>
-   C: semmisíteni
-   D: semmisíteni
-   E: semmisíteni

>Ne feledje, hogy a fenti listában a E: meghajtó a szerepkör a legfelső szintű meghajtó, nem kell csomagolásukkor. A meghajtó csatlakozik, használja a % RoleRoot % környezeti változó.

>Minimalizálásához az állásidőt szolgáltatás egyetlen-példány frissítésekor, új több példány szolgáltatás telepítése az átmeneti tárolásra szolgáló kiszolgálóra, és végezze el a virtuális felcserélése.

Az automatikus frissítése közben az Azure háló vezérlő rendszeres kiértékeli a felhőbeli szolgáltatástól megállapítani, hogy mikor biztonságos elemre a következő UD állapotának. Ez az állapot értékelés szerepkör alapon történik, és csak a legújabb verziójában (tehát, hogy már üzlet elévült UDs példányok) példányok számításba veszi. Ellenőrzi, hogy a minimálisan szükséges szerepkör-példányok, minden szerepkör esetében van elért kielégítő Terminálszolgáltatások állapotba.

### <a name="role-instance-start-timeout"></a>Szerepkör példány kezdő időkorlát
A háló vezérlő az egyes szerepkör-példány elérje a lépések állapot 30 percig várakozik. A program az időtúllépés időtartam, ha a háló vezérlő továbbra is a következő szerepkört többszörösre kiállniuk.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>A frissítés visszaállítása
Azure szolgáltatások kezelése a frissítés során, hogy akkor kezdeményez egy szolgáltatást, a további műveletek, az Azure háló adatkezelő a kezdeti frissítési kérelem elfogadása után rugalmasságot biztosít. Egy visszaállítása csak hajtható végre, ha a frissítés (konfigurációs módosítás), vagy a frissítés a telepítési **folyamatban** állapotban van. Egy frissítést, illetve minősül a folyamatban lévő mindaddig, amíg a szolgáltatást, amely még nem lettek frissítve az új verzió legalább egy példánya. Tesztelése, hogy egy visszaállítása engedélyezett, jelölje be az [Első telepítési](https://msdn.microsoft.com/library/azure/ee460804.aspx) és az [Első felhőalapú szolgáltatás tulajdonságai](https://msdn.microsoft.com/library/azure/ee460806.aspx) műveletek által visszaadott RollbackAllowed jelölő értékét, értéke igaz.

> [AZURE.NOTE] Csak célszerű visszaállítás hívja meg **a helyi** frissítés vagy frissíteni, mert a virtuális felcserélése frissítések foglalnak magukban, egy teljes futó-példányt tartson a szolgáltatás cseréje egy másikra.

A folyamatban lévő frissítés visszaállítása az alábbiakat tapasztalhatja az példányban foglalja magában:

-   Bármely szerepkör példányok, amelyek volna még nem frissülnek vagy frissíteni kell az új verzió nem frissíthető, illetve frissítve, mert adott példányok már a szolgáltatás cél verziót futtatja.
-   Volt már nem frissülnek vagy frissíteni kell a service csomag az új verzió szerepkör példányok (\*.cspkg) fájl vagy a szolgáltatás beállításai (\*.cscfg) fájlt (vagy mindkettőt) vissza ezeket a fájlokat a frissítés előtti verzióját.

Ez funkcionális által biztosított a következő funkciók:

-   A [Visszaállítás módosítása vagy frissítése a](https://msdn.microsoft.com/library/azure/hh403977.aspx) művelet, amely hívható a konfigurációs frissítést (hívó [Módosítása konfigurációs](https://msdn.microsoft.com/library/azure/ee460809.aspx)által indított), illetve egy (indított hívó [Telepítési frissítése](https://msdn.microsoft.com/library/azure/ee460793.aspx)) mindaddig, amíg legalább egy példánya van a szolgáltatás, amely még nem lettek frissítve az új verzió.
-   A zárolt elemet, valamint az RollbackAllowed elemet, amely az [Első telepítési](https://msdn.microsoft.com/library/azure/ee460804.aspx) és az [Első felhőalapú szolgáltatás tulajdonságai](https://msdn.microsoft.com/library/azure/ee460806.aspx) műveletek válasz törzsében részeként a visszaadott:
    1.  Zárolt elem észleli, ha egy mutating művelet hívható egy adott példányban teszi lehetővé.
    2.  A RollbackAllowed elem lehetővé teszi, a [Visszaállítás frissítése vagy frissítése](https://msdn.microsoft.com/library/azure/hh403977.aspx) művelet egy adott példányban hívásakor feltárása.

    A visszaállítás végrehajtásához nem rendelkezik a zárolt és a RollbackAllowed elemek ellenőrzéséhez. Elegendő, ha a győződjön meg arról, hogy RollbackAllowed értéke igaz. Ezeket az elemeket csak adja vissza, ha ezeket a módszereket a kérelem fejlécben használatával vételét "x-ms-verziója: 2011-10-01" vagy újabb verziója. Verziószámozás fejlécek kapcsolatos további tudnivalókért olvassa el a [Szolgáltatás kezelése a verziószámozás](https://msdn.microsoft.com/library/azure/gg592580.aspx)című témakört.

Vannak olyan helyzetek, ha a frissítés visszaállítását vagy frissítés nem támogatott, ezek a következők:

-   Helyi erőforrások – Ha a frissítés növeli a szerepkör a Azure platform helyi erőforrásokat csökkenése nem engedi visszaállítása. 
-   Kvóta korlátozások -, ha a frissítés le, előfordulhat, hogy már nem művelet skálával lett van-e elegendő számítási kvóta a visszaállítási művelet végrehajtásához. Egyes Azure előfizetés van társítva kvóta, amely meghatározza, amelynek tagja, hogy az előfizetés szolgáltatott szolgáltatások által igénybe vehető magmintákat maximális száma. Ha egy visszaállítása egy adott frissítés elvégzéséhez volna vigye az előfizetés kvóta, majd egy visszaállítása nem engedélyezi.
-   Versenyhelyzetből – Ha a kezdeti frissítés befejeződött, a visszaállítás esetén nem lehet.

Amikor a visszaállítás frissítés célszerű lehet egy példa használata a [Frissítés telepítési](https://msdn.microsoft.com/library/azure/ee460793.aspx) művelet vezérlőelemre, az árfolyam, amelyen a fő helyi rendszerre való frissítés az Azure service üzemeltetett közzétételének kézi módban.

A Bevezetés a frissítés során hívja fel a [Telepítés frissítése](https://msdn.microsoft.com/library/azure/ee460793.aspx) kézi módban, és kezdje el a frissítési tartományok elemre. Ha bizonyos pontján, közben figyelheti a frissítés, jegyezze fel van, hogy az első frissítési tartományok, akkor vizsgálja meg a szerepkör esetenként nem válaszol, felhívhatja telepítését, és a példányok, amelyek volna még nem frissített és visszaállítása példányok, amely az előző szolgáltatás csomag és konfigurációs frissített változatlan marad a [Visszaállítás frissítése vagy frissítése a](https://msdn.microsoft.com/library/azure/hh403977.aspx) műveletet.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Folyamatban lévő telepítéseken több mutating műveletek kezdeményezése
Egyes esetekben előfordulhat, hogy szeretne indítása folyamatban lévő telepítéseken több egyidejű mutating műveletek. Például a szolgáltatás frissítését teljesítményt és, frissítést végig a szolgáltatás van görgetve, miközben szeretne módosítás az egyes, például görgetheti újra a frissítést, egy másik frissítés alkalmazása és a telepítés még törlése. Eset, amelyben erre akkor lehet szükség, ha a szolgáltatásfrissítés frissített szerepkör példány többször lefagyását okozza buggy kódot tartalmazza. Ebben az esetben az Azure háló vezérlőhöz nem tudnak haladást alkalmazásában, mert a frissítendő tartomány példányok elegendő számú megfelelő frissítése. Ez az állapot nevezik *problémákat tapasztal a telepítés*. A frissítés visszaállítása vagy egy friss frissítés alkalmazása a hibás fölé is húznia a a telepítési egyet.

Amikor az eredeti értekezlet-összehívást frissítése vagy a szolgáltatás frissítése az Azure háló vezérlő kapott, megkezdheti mutating a további műveletek. Ez azt jelenti, hogy nem rendelkezik a kezdeti művelet előtt: egy másik mutating művelet elvégzéséhez léptetése.

A visszaállítási művelet hasonló végez kezdeményezése egy második frissítési művelet, miközben az első frissítés folyamatban. Ha a második frissítés automatikus módban van, az első frissítési tartomány lesznek frissítve azonnal, esetleg vezető példányok időben ugyanazt a kapcsolat nélküli állapotban több frissítési tartományt.

A mutating műveletek a következők: [Módosítása konfigurációs](https://msdn.microsoft.com/library/azure/ee460809.aspx), a [Telepítés frissítése](https://msdn.microsoft.com/library/azure/ee460793.aspx), a [Telepítési állapotának frissítése](https://msdn.microsoft.com/library/azure/ee460808.aspx), a [Telepítési törlése](https://msdn.microsoft.com/library/azure/ee460815.aspx)és a [Visszaállítás frissítése vagy frissítése](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Két művelet [Első telepítési](https://msdn.microsoft.com/library/azure/ee460804.aspx) és az [Első felhőalapú szolgáltatás tulajdonságait](https://msdn.microsoft.com/library/azure/ee460806.aspx), a Locked jelzőt, amely határozza meg, hogy egy mutating műveletet is hívjon meg egy adott környezetben is vizsgálni adja eredményül.

Annak érdekében, hogy hívja fel az alábbi módszerek egyikét adja vissza az zárolt jelző verziójának, be kell kérés fejléce "x-ms-verziója: 2011-10-01" vagy egy újabb. További információt a verziószámozás fejlécek [Szolgáltatás kezelése a verziószámozás](https://msdn.microsoft.com/library/azure/gg592580.aspx)megtekintése

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Frissítési tartományok szerepkörök terjesztése
Azure elosztja szerepkörbe előfordulását egyenletesen állítható be a szolgáltatás definition (.csdef) fájl részeként frissítési tartományok beállítása száma. Frissítési tartományok maximális száma 20 és az alapértelmezett érték 5. A szolgáltatás-definíciós fájl módosításával kapcsolatos további tudnivalókért lásd: [Azure Service Definition séma (.csdef fájl)](cloud-services-model-and-package.md#csdef).

Például ha szerepköre rendelkezik tíz példányok, alapértelmezés szerint minden frissítési tartomány tartalmazza két példánya. Ha szerepköre rendelkezik 14 példányok, ezután három példányok tartalmazza-e négy frissítési a tartomány, és ötödik tartomány két tartalmazza.

Frissítési tartományok azonosítja a nulla alapú index létrehozása: az első frissítési tartomány van a 0-azonosító, és a második frissítési tartománynak Azonosítóval és 1, és így tovább.

Az alábbi ábra szemlélteti, hogyan kerülnek terjesztésre egy szolgáltatást, mint a két szerepköröket tartalmaz a szolgáltatás két frissítési tartomány határozza meg. A webes szerepkör nyolc példányok és a dolgozó szerepkör kilenc példánya fut a szolgáltatás.

![A frissítési tartományok eloszlás] (media/cloud-services-update-azure-service/IC345533.png "A frissítési tartományok ki.")

> [AZURE.NOTE] Fontos tudni, hogy az Azure vezérlők hogyan példányok frissítési tartományban van rendelve. Még nem is megadhat, előfordulások melyik tartományhoz van rendelve.

## <a name="next-steps"></a>Következő lépések
[Cloud Services kezelése](cloud-services-how-to-manage.md)  
[Cloud Services figyelése](cloud-services-how-to-monitor.md)  
[Cloud Services konfigurálása](cloud-services-how-to-configure.md)  
