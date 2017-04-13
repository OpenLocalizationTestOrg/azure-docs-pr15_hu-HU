<properties
   pageTitle="Tevékenységek kezelése csomagja (MOBILE) – áttekintés |} Microsoft Azure"
   description="Microsoft műveletek Management csomagja (MOBILE) használata a Microsoft felhőalapú informatikai kezelési megoldás, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra.  Ez a cikk a MOBILE szereplő más szolgáltatások azonosítja, és a hivatkozásokra kattintva részletes tartalmuk."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Mi az, hogy a tevékenységek kezelése csomagja (MOBILE)?

Microsoft műveletek Management csomagja (MOBILE) használata a Microsoft felhőalapú informatikai kezelési megoldás, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra.  Mivel a MOBILE történik, mint egy felhőalapú szolgáltatásba, is azt lépéseket gyorsan a minimális befektetés infrastruktúrájának szolgáltatásai.  Új funkciók kézbesíti a rendszer automatikusan menti, akkor a folyamatos karbantartást, és frissítse a költségeket.

Nemcsak a saját maga értékes szolgáltatásokat nyújtó, a MOBILE integrálhatja System Center elemekkel, például a rendszer központ Operations Manager, ha ki szeretné terjeszteni a meglévő management hasznosítása be a felhőben.  System Center és a MOBILE együttműködhetnek egy teljes hibrid adatkezelési folyamatok megadását.

A következő szakaszokban a MOBILE és a szolgáltatások, amelyek a végrehajtásukhoz másik érték részeinek magas szintű leírását.  Akkor is hivatkozhat MOBILE architektúra különböző MOBILE összetevői áttekintése előtt, hogy az egyes részletes dokumentációt áttekintése.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Betekintést és az elemzést](media/operations-management-suite-overview/icon-insight-analytics.png) Betekintést és az elemzést

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) segít összegyűjtése, összehangolására, kereshet és operációs rendszerek és alkalmazások által generált adatok napló és a teljesítmény működésbe lépnek. Beépített keresési lehetőség és egyéni irányítópultok használatával könnyen elemzése a rekordok milliónyi munkaterhelésekből és fizikai helyük függetlenül kiszolgálók közötti valós idejű műveleti háttérismeretek kínál fel.

Log Analytics gyűjtendő adatok meghatározó és az elemzés a logikája egyszerűen hozzáadhatja megoldások.  Megoldások tartalmazhat további funkciókat, amelyek a rendszer automatikusan ügynökök minimális a vagy beállítást.  Egyéni megoldások által biztosított elemzőeszközök használata esetén mellett hajthatja végre egyéni kereséseket át a teljes adatkészlet annak érdekében, hogy az adatokat a rendszerek és az alkalmazások között összehangolására.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatizálási és irányítása](media/operations-management-suite-overview/icon-automation-control.png) Automatizálási és irányítása

Azure automatizálási automatizálja [runbooks](../automation/automation-runbook-types.md) , amelyek alapján a PowerShell, és futtassa az Azure felhőben felügyeleti folyamatok.  Minden olyan termék vagy szolgáltatás, köztük az erőforrásokat más felhőket, például az Amazon Web Services (AWS) a PowerShell kezelhető Runbooks érheti el.  A helyi adatok központban helyi erőforrások kezelése kiszolgálón Runbooks is futtatható.

Azure automatizálási [PowerShell DSC](../automation/automation-dsc-overview.md)a modellkonfigurációk kezelésére nyújt.  Hozzon létre és kezelheti az Azure-ban tárolt DSC erőforrásokat, és őket a felhőben, és a helyszíni rendszerek meghatározása és a konfigurációs automatikusan hivatkozási alkalmazása.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Védelem és helyreállítása](media/operations-management-suite-overview/icon-protection-recovery.png) Védelem és a helyreállítás

[Azure biztonsági másolat](http://azure.microsoft.com/documentation/services/backup) védi az alkalmazás adatokat, és az év minden tőkeberuházási nélkül és minimális működési költségek megtartja azt.  Azt is fizikai és virtuális Windows kiszolgálók mellett az alkalmazás-munkaterhelésekből, például az SQL Server és SharePoint adatainak biztonsági másolatot.  Azt is használható rendszer központ adatok védelme Manager (DPM) védett adatok bizonyos az Azure redundancia és a hosszú távú tároló.

[Azure webhely helyreállítási](http://azure.microsoft.com/documentation/services/site-recovery) hozzájárul a üzemkészségének katasztrófa helyreállítási (BCDR) stratégia replikációs, feladatátadási és helyreállítási helyszíni a Hyper-V virtuális gépeken futó, VMware virtuális gépeken futó és fizikai Windows vagy Linux kiszolgálók orchestrating szerint. Az Adatközpont meghosszabbíthatja esetében, amelyek őket Azure, vagy bizonyos gépek egy másodlagos adatközpontja. Webhely helyreállítási is biztosít az egyszerű feladatátvételi és helyreállítási munkaterhelésekből. Katasztrófa helyreállítási mechanizmusok például SQL Server AlwaysOn integrálódik, és helyreállítási tervek biztosít, amely több számítógépen is többszintű munkaterhelésekből egyszerűen feladatátvételének.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![MOBILE biztonság és megfelelőség](media/operations-management-suite-overview/icon-security-compliance.png) Biztonság és megfelelőség
Biztonság és megfelelőség segítségével azonosítani felmérése és a infrastruktúrájába biztonsági kockázatok csökkentésében.  Ezek a funkciók a MOBILE keresztül, amely a napló adatot és konfigurációt, amely segít a folyamatban lévő biztonsági a környezet biztosítása ügynök rendszerekből elemzése napló Analytics több megoldások végrehajtását.

- A [Biztonság és naplózási megoldás](oms-security-getting-started.md ) gyűjti össze, és elemzi a biztonsági események gyanús tevékenység azonosítása felügyelt rendszeren.
- A felügyelt rendszerek antimalware a védelem állapotának [Antimalware megoldás](log-analytics-malware.md ) jelentéseket.  
- A rendszer frissítéseit megoldást a biztonsági frissítések elemzést végez, és egyéb frissíti a felügyelt rendszeren, hogy könnyen rendszerek azonosításához igénylő javítása.


## <a name="next-steps"></a>Következő lépések
- Tudjon meg többet a [napló Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
- További tudnivalók: [Azure automatizálási](../automation/automation-intro.md).
- További tudnivalók: [Azure biztonsági másolatot](http://azure.microsoft.com/documentation/services/backup).
- További tudnivalók: [Azure webhely helyreállítási](http://azure.microsoft.com/documentation/services/site-recovery).
