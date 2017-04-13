<properties
    pageTitle="Felkészülés a hálózati hozzárendelést a Hyper-V virtuális gép védelme az Azure webhely helyreállítás VMM |} Microsoft Azure"
    description="Állítsa be a Hyper-V virtuális gép replikáció egy helyszíni adatközponthoz az Azure, illetve egy másodlagos webhelyen hálózati hozzárendelést."
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
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Felkészülés a Hyper-V virtuális gép védelmet az Azure webhely helyreállítási VMM hálózati hozzárendelése

Azure webhely helyreállítási hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátadási és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating.

Ez a cikk ismerteti a hálózati hozzárendelést, amely segít, optimálisan hálózat beállításainak konfigurálása webhely helyreállítási való replikáció a Hyper-V virtuális gépeken futó használata esetén a VMM felhőket két helyszíni adatközpont esetén, vagy egy helyszíni adatközponthoz és Azure között található. Megjegyzendő, hogy ha, használja a Hyper-V VMs VMM felhő nélkül, végre vagy replikálása VMware VMs vagy a fizikai kiszolgálók, ez a cikk nem releváns.

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.


## <a name="overview"></a>– Áttekintés

Hálózati hozzárendelést Azure webhely helyreállítási telepítik való replikáció a Hyper-V virtuális gépeken futó Azure vagy egy másodlagos adatközponthoz használata a Hyper-V kópia vagy SAN replikációs használatos.

- **A VMM felhő között két replikálása a Hyper-V virtuális gépeken futó helyszíni adatközpontokkal**– hálózati megfeleltetés térképek VMM forráskiszolgáló virtuális hálózatok és hajtsa végre a következő a cél VMM kiszolgálón virtuális hálózatok között:

    - **Kapcsolódás virtuális gépeken futó feladatátvétel után**– biztosítja, hogy a rendszer virtuális gépeken futó csatlakozik megfelelő hálózatok feladatátvétel után. A replika virtuális gép fog csatlakoznia a cél hálózat, amelyhez a forrás hálózathoz van rendelve.
    - **Hely replika virtuális gépeken futó host kiszolgálókon**– replika virtuális gépeken futó optimálisan elhelyezése a Hyper-V kiszolgálók. Replika virtuális gépeken futó férhet hozzá a megfeleltetett virtuális hálózatok állomáson kerül.
    - **Hálózati leképezéssel**– Ha hálózati hozzárendelést nem adja meg, replikált virtuális gépeken futó nem kell minden olyan virtuális hálózatokhoz kapcsolódó feladatátvétel után.

- **Replikálása a Hyper-V virtuális gépeken futó egy helyszíni VMM az a felhő az Azure**– megfeleltetés térképek virtuális hálózatok a forrás VMM kiszolgáló közötti hálózati és adja meg a célként Azure hálózatok, tegye a következőket:
    - **Kapcsolódás virtuális gépeken futó feladatátvétel után**– a hálózaton mely feladatátvevő csatlakozhat egymással, mely helyreállítási terv függetlenül azok minden gép.
    - **Hálózati átjáró**– Ha a hálózati átjáró a cél Azure hálózati állította be, a virtuális gépeken futó többi helyszíni virtuális gépeken futó csatlakozhat.
    - **Hálózati leképezéssel**– hálózati hozzárendelést nem állítja be, ha csak a virtuális gépeken futó helyreállítási megegyező csomagot az adott feladatátvevő tudják egymás után fellépő hibák keresztül kapcsolódik az Azure.


## <a name="network-mapping-example"></a>Hálózati hozzárendelése például

Hálózati hozzárendelést beállíthatók két VMM kiszolgálókon, vagy ha két webhelyből kezelhetők a azonos kiszolgáló által egyetlen VMM kiszolgálón virtuális hálózatok között. Megfeleltetés helyesen van beállítva, és replikációs engedélyezve van, a fő tartózkodási helyén virtuális gép kapcsolódik hálózatra, valamint annak a célhelyen replika a csatlakoztatott hálózati kell csatlakoztatni.

Ha hálózatok van beállítva megfelelően VMM, a cél virtuális hálózaton során hálózati hozzárendelést kijelölésekor, használja a forrás virtuális network VMM forrás felhőket ábrázoló jelennek meg, együtt a rendelkezésre álló cél virtuális hálózatok a cél felhőket védelem határozza meg.

Íme egy példa mutatja be, ez az eljárás. Nézzük meg szervezeten a New York és Chicago két helyen.

**Hely** | **VMM kiszolgáló** | **Virtuális hálózatok** | **Megfeleltetve**
---|---|---|---
New York | VMM-NewYork| VMNetwork1-NewYork | VMNetwork1-Chicago megfeleltetve
 |  | VMNetwork2-NewYork | Nincs megfeleltetve
Chicago | VMM-Chicago| VMNetwork1-Chicago | VMNetwork1-NewYork megfeleltetve
 | | VMNetwork2-Békéscsabai | Nincs megfeleltetve

Az ebben a példában:

- Virtuális gépi VMNetwork1-NewYork csatlakoztatott replika virtuális gép létrehozásakor fog kell csatlakoztatva VMNetwork1-Békéscsabai.
- Replika virtuális gép VMNetwork2-NewYork vagy VMNetwork2-Chicago létrehozásakor meg fog nem csatlakozik olyan hálózat.

Az alábbiakban hogyan VMM felhőket példa tagként a szervezet és az a felhő társított logikai hálózatok beállítani.

### <a name="cloud-protection-settings"></a>Felhőalapú adatvédelmi beállítások

**Védett felhő** | **Felhőalapú védelme** | **Logikai hálózati (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>HIÁNYZIK</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>HIÁNYZIK</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Logikai és a virtuális hálózati beállítások

**Hely** | **Logikai hálózati** | **Hozzárendelt virtuális hálózati**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-networks"></a>Cél hálózatok

Ezeket a beállításokat a alapján, akkor jelölje be a cél virtuális hálózat, a következő táblázat mutatja a választási lehetőségek érhetők el.

**Válassza a** | **Védett felhő** | **Felhőalapú védelme** | **Cél hálózati érhető el**
---|---|---|---
VMNetwork1-Békéscsabai | SilverCloud1 | SilverCloud2 | Érhető el
 | GoldCloud1 | GoldCloud2 | Érhető el
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Nem érhető el
 | GoldCloud1 | GoldCloud2 | Érhető el



## <a name="multiple-subnets"></a>Több alhálózat

A cél hálózatnak több alhálózat és az egy adott alhálózat neve megegyezik az alhálózat, amelyen a forrás virtuális gép található, majd a replika virtuális gép fog csatlakoznia cél alhálózat feladatátvétel után. Ha nincs cél alhálózat egyező nevű, a virtuális gép csatlakozik az első alhálózat a hálózaton.


### <a name="failback"></a>Visszaállás

Mi történik az visszaállás (fordított replikáció) esetében megtekintéséhez tegyük fel, hogy VMNetwork1-NewYork van rendelve VMNetwork1-Chicago, az alábbi beállításokkal.


**Virtuális gépen** | **Virtuális hálózathoz csatlakozik**
---|---
VM1 | Hálózati VMNetwork1
VM2 (VM1 replikáját) | VMNetwork1-Chicago

Ezekkel a beállításokkal tanulmányozzuk mi történik a lehetséges forgatókönyvek néhány.

**Eset** | **Eredmény**
---|---
Ne módosítsa a hálózat tulajdonságai párbeszédpanelen a virtuális-2 feladatátvétel után. | Virtuális-1 marad a forrás hálózathoz.
Virtuális-2 hálózati tulajdonságainak feladatátvétel után megváltoznak, és nem kapcsolódik. | Virtuális-1 le van választva.
Virtuális-2 hálózati tulajdonságainak feladatátvétel után megváltoznak, és VMNetwork2-Chicago csatlakozik. | Ha a VMNetwork2-Chicago nincs megfeleltetve, virtuális-1 megszakad.
Hálózati megfeleltetésének VMNetwork1-Chicago módosul. | A hálózat VMNetwork1-Chicago most megfeleltetve virtuális-1 lesz csatlakoznia.


## <a name="next-steps"></a>Következő lépések

Most, hogy ha a hálózati hozzárendelést [webhely helyreállítási telepítési – első lépések](site-recovery-best-practices.md)jobb megértése.
