<properties
    pageTitle="Mi a teendő az Azure megelőzve a szolgáltatási zavarok, amely hatással van az Azure Cloud Services |} Microsoft Azure"
    description="Ismerje meg, mi a teendő az Azure szolgáltatási zavarok, amely hatással van az Azure Cloud Services megelőzve."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Mi a teendő az Azure megelőzve a szolgáltatási zavarok, amely hatással van az Azure Cloud Services

A Microsoft azt munkára szigorú ellenőrizze, hogy a szolgáltatások mindig elérhető szükség szerint csoportokat. Arra utasítja a vezérlő túl néha hatással lehet us nem tervezett szolgáltatás megszakadása okozó módon.

A Microsoft kötelezettséget üzemidőt és a kapcsolatok esetén, egy szolgáltatási szerződés SZOLGÁLTATÁSISZINT-kínál szolgáltatásait. Az egyes Azure szolgáltatások SLA [Azure Service szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)találhatók.

Azure már számos beépített platform funkciók, amelyek támogatják a könnyen hozzáférhető alkalmazásokat. További információt az alábbi szolgáltatások, olvassa el a [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetőségét](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Ez a cikk bemutatja az olyan igaz visszaállításához összeomlást követő helyreállítás során, ha egy teljes terület egy üzemszünetek fő természeti katasztrófa vagy kiterjedt szolgáltatáskimaradás miatt. Ezek a ritka előfordulások, de el kell készítenie, hogy van-e egy üzemszünetek egy teljes terület lehetőséget. Ha egy teljes terület a szolgáltatási zavarok, az adatok helyileg felesleges példányainak volna átmenetileg nem érhető el. Ha engedélyezte a replikáció geo, táblázatok és tároló Azure BLOB három további másolatok egy másik régióbeli tárolja. Egy teljes területi üzemszünetek vagy az elsődleges régió nem helyreállítható katasztrófa esetén Azure amelyek összes a DNS-bejegyzések a geo replikált területére.

>[AZURE.NOTE]Ne feledje, hogy keresztül folyamathoz vezérlőhöz nem rendelkezik, és csak bekövetkezik az Adatközpont szintű szolgáltatás megszakadása. Emiatt kell is használja arra, a többi az alkalmazás-specifikus biztonsági stratégiák elérhetőségét a legmagasabb szintű eléréséhez. További információ című kapcsolatos [adatok stratégiák vészhelyreállítás](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Ha szeretné engedélyezni szeretné a saját feladatátvevő hatással, érdemes lehet figyelembe [olvasásra geo felesleges tároló (TS-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), amely egy másik tartományban lévő hoz létre egy írásvédett példányát az adatok felhasználása.

Segít a ritka előfordulások kezelni, kínálunk, amely Azure virtuális gépeken futó (VMs) az alábbi útmutatást az esetében a teljes régió, ahol az Azure virtuális alkalmazás telepítve van a szolgáltatási zavarok.

##<a name="option-1-wait-for-recovery"></a>1 beállítást: Helyreállítási megvárja, amíg
Ebben az esetben nincs további teendő hajtana szükség. Ismerkedjen meg, hogy a csoportoknak Azure szolgáltatáselérhetőség visszaállítása szorgalmasan működnek. Az aktuális szolgáltatásállapot az [Azure állapotjelző irányítópultja](https://azure.microsoft.com/status/)megjelenik.

>[AZURE.NOTE]Ha egy ügyfél nem konfigurálta az Azure webhely helyreállítás vagy egy másodlagos telepítési egy másik régióbeli Ez a legjobb megoldást.

Azonnali hozzáférést szeretne a telepített felhőszolgáltatások felhasználókat az alábbi lehetőségek állnak rendelkezésre.

>[AZURE.NOTE]Ne feledje, hogy ezek a beállítások adatvesztést lehetősége van.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>2 lehetőség: Újra üzembe a felhőalapú szolgáltatás konfigurációja új területére

Ha az eredeti kód, egyszerűen csak újratelepítése is az alkalmazás, a társított konfigurációs, valamint a kapcsolódó erőforrásokat egy új tartomány új felhőszolgáltatásokba.  

Arról, hogy miként hozhat létre, és egy felhőalapú szolgáltatás alkalmazás telepítéséhez részletesebben tájékozódhat [készíthetnek és helyezhetnek üzembe a egy felhőalapú szolgáltatásba](./cloud-services-how-to-create-deploy-portal.md).

Attól függően, hogy az alkalmazás adatforrások szüksége lehet jelölje be az alkalmazás adatforrás helyreállítási műveleteket.
  * Azure tároló adatforrások esetén lásd: [Azure tároló replikációs](../storage/storage-redundancy.md#read-access-geo-redundant-storage) a rendelkezésre álló beállítások ellenőrzéséhez az alkalmazás a kiválasztott replikációs modellek alapján.
  * SQL-adatbázis-erőforrások esetén olvassa el a [áttekintése: a felhő üzleti folytonosságot és az adatbázis vészhelyreállítás SQL-adatbázissal](../sql-database/sql-database-business-continuity.md) ellenőrizni a rendelkezésre álló beállítások a kiválasztott replikációs modell az alkalmazás alapján.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Lehetőség 3: Azure forgalom-kezelővel biztonsági mentés telepítéshez használni
Ezt a beállítást feltételezi, hogy már megtervezni a területi vészhelyreállítás szem előtt az alkalmazás-megoldás. Használhatja ezt a lehetőséget, ha már van egy másodlagos cloud services alkalmazás telepítése, amely egy másik régióbeli fut, és a forgalom manager csatornán keresztül csatlakozik. Ebben az esetben a másodlagos telepítési állapotának ellenőrzése. Ha a megfelelő irányíthatja át forgalom rá Azure forgalom-kezelővel. Ez a stratégia a kihasználhatja a forgalom útválasztási módszer és feladatátvevő sorrendben konfigurációk Azure forgalom Manager. További információért megtudhatja, [hogy miként forgalom Manager beállításainak konfigurálása](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Nagyfokú Azure Cloud Services különböző régiók Azure forgalom-kezelővel](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Következő lépések

Egy vészhelyreállítás és magas elérhetőség stratégiát megvalósításáról kapcsolatos további információért lásd: [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetőségét](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Egy felhőalapú platform funkciók részletes technikai megértése fejleszt, olvassa el az [Azure tűrőképessége technikai útmutatást](../resiliency/resiliency-technical-guidance.md).

Ha nem, törölje a jelet, vagy ha szeretné, hogy az Ön nevében műveletek elvégzésére Microsoft kérjük, forduljon a [Támogatási](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)a képernyőn megjelenő utasításokat.
