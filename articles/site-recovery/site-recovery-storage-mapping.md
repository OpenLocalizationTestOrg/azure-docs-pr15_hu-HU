<properties
    pageTitle="Feleltesse meg a Hyper-V virtuális gép replikációs helyszíni adatközpontokkal között az Azure webhely helyreállítási tárterület |} Microsoft Azure"
    description="Felkészülés a Hyper-V virtuális gép replikációs között két helyszíni adatközpontokkal az Azure-webhely helyreállításhoz tároló hozzárendelése."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Felkészülés a Hyper-V virtuális gép replikációs között két helyszíni adatközpontokkal az Azure-webhely helyreállításhoz tároló hozzárendelése


Azure webhely helyreállítási hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátadási és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Ez a cikk ismerteti a tárhely hozzárendelése, mely gondoskodhat optimális használata tároló webhely helyreállítási való replikáció a Hyper-V virtuális gépeken futó között két helyszíni VMM adatközpontokkal használata esetén.

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="overview"></a>– Áttekintés

Tárterület-megfeleltetés csak akkor jelentősége, a VMM felhőket egy elsődleges adatközponthoz a Hyper-V kópia vagy SAN replikációs használatával az alábbiak szerint egy másodlagos adatközponthoz a Hyper-V virtuális gépeken futó verzióazonosítójának esetén:


- **Helyszíni replikációs a Hyper-V replikával helyszíni)** – Beállítása tároló megfeleltetés szerinti megfeleltetés tároló besorolása elemet a forrás és a cél VMM kiszolgálók tegye a következőket:

    - **Cél tárterület azonosítása replika virtuális gépeken futó**– virtuális gépeken futó fog replikált, a tárhely célba (kis-és Középvállalatok megosztása vagy megosztott kötet (CSVs) fürt) választott.
    - **Replika virtuális gép elhelyezése**– tárterület megfeleltetés replika virtuális gépeken futó optimálisan elhelyezése a Hyper-V kiszolgálók használják. Replika virtuális gépeken futó férhet hozzá a megfeleltetett tároló besorolás állomáson kerül.
    - **Tárterület leképezéssel**– tárterület megfeleltetés nem állítja be, ha kiszolgálón a Hyper-V szolgáltatást úgy a replika virtuális gép társított megadott alapértelmezett tárolási helyére virtuális gépeken futó replikált kell-e.

- **A helyszíni való replikáció helyszíni SAN**– beállítása tároló megfeleltetés szerinti megfeleltetés tároló tömbök készletek a forrás és a cél VMM kiszolgálók.
    - **Adja meg a készlet**– Itt adhatja meg, mely másodlagos tárolókészlethez kap replikációs adatok az elsődleges.
    - **Azonosítása cél tároló készletek**– biztosítja, hogy egy adatforrás replikációs csoport logikai replikált target egyeztetett tárhely kvótáját lehetőség.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Állítsa be a Hyper-V replikáció tároló sorosztály

A webhely helyreállítási névkiszolgálóit használata esetén a Hyper-V replika, leképezheti tároló besorolása elemet a forrás- és célwebhelyek VMM kiszolgálókon vagy egy VMM kiszolgálón között, ha két webhelyből kezelhetők a azonos VMM kiszolgáló által. Megjegyzés:

- Ha megfeleltetés helyesen van beállítva, és a replikáció engedélyezve van, a replikált tárolóhoz a megfeleltetett célhelyen egy virtuális gép a fő tartózkodási helyén virtuális merevlemez kell.
- Tárterület sorosztály érhető el, olvassa el a forrás- és célwebhelyek felhőket ábrázoló host csoportoknak kell lennie.
- Osztályozás nem kell a tároló azonos típusú van. Forrás osztályozás, amely tartalmazza a kis-és Középvállalatok megosztások CSVs tartalmazó cél osztályozási például képezhető.
- További [tárterület besorolása elemet a VMM létrehozása](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Példa

Osztályozás helyesen van beállítva a VMM kijelölésekor a forrás- és célwebhelyek VMM kiszolgálói tárhely leképezés során, ha a forrás- és célwebhelyek sorosztály jelennek meg. Íme egy példa tároló fájlok és az osztályozás két helyen is elérhető a New York és Chicago a szervezet.

**Hely** | **VMM kiszolgáló** | **Fájl megosztása (forrás)** | **Osztályozási (forrás)** | **Megfeleltetve** | **Fájl megosztása (cél)**
---|---|--- |---|---|---
New York | VMM_Source| SourceShare1 | ARANY | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | EZÜST | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRONZ | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Nincs megfeleltetve |
| | | SILVER_TARGET | Nincs megfeleltetve |
 | | | BRONZE_TARGET | Nincs megfeleltetve

A **Tároló kiszolgáló** lapon ezek az **erőforrások** lapon a webhely helyreállítási portál szeretné konfigurálása.

![Tárterület-megfeleltetés konfigurálása](./media/site-recovery-storage-mapping/storage-mapping1.png)

Az ebben a példában:
- Ha egy replika virtuális gép bármely virtuális gép GOLD tárolón (SourceShare1) hoz létre, a GOLD_TARGET tárolóhoz (TargetShare1) fog replikált.
- Virtuális gépi EZÜST tárolón (SourceShare2) replika virtuális gép létrehozásakor fog egy SILVER_TARGET (TargetShare2) tárolóhoz kell replikált, és így tovább.

A tényleges fájlmegosztások és a hozzárendelt besorolása elemet a VMM jelennek meg a következő képernyőképét.

![Tárterület besorolása elemet a VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Több tárolási helye

Ha a cél besorolás több kis-és Középvállalatok megosztások vagy CSVs van rendelve, az optimális tárhelyek automatikusan ki lesz választva amikor a virtuális gép védett. Nincs megfelelő cél tároló nem találhatók meg a megadott besorolás, ha az alapértelmezett tárolási helye a Hyper-V állomáson megadott helyezze a replika virtuális merevlemez szolgál.

Az alábbi táblázat jelenjen meg, hogyan tároló osztályozás és megosztott fürt kötet beállítása ebben a példában.

**Hely** | **Besorolás** | **Kapcsolódó tárhely**
---|---|---
New York | ARANY | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | EZÜST | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

Az alábbi táblázat összefoglalja a viselkedését virtuális gépeken futó (VM1 - VM5) ebben a példában környezetben védelmének engedélyezheti.

**Virtuális gépen** | **Forrás tárhely** | **Forrás besorolás** | **A target egyeztetett tárhely**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | ARANY | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Mindkét GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | ARANY | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Mindkét GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | EZÜST | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | EZÜST |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Mindkét SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | A #HIÁNYZIK | Leképezéssel, így az alapértelmezett tárolási helye a Hyper-V fogadó használják

## <a name="next-steps"></a>Következő lépések

Most, hogy tárterület-leképezés [felkészítése az Azure webhely helyreállítási üzembe](site-recovery-best-practices.md)jobb megértése.
