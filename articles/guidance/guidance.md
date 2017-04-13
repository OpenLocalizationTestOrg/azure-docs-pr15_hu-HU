
<properties
   pageTitle="Azure útmutatást |} minták és eljárások |} Microsoft Azure"
   description="Útmutató a Azure és gyakorlati tanácsok"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure útmutató

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

A Microsoft mintázatok és eljárások csoportja az Azure-ügyfél figyelmeztető csoport része. A célja, hogy a fejlesztők, építészek és a Microsoft Azure platform a sikeres informatikai szakemberek számára. Gyakorlati tanácsok a felhőben megoldások fejlesztésére a Azure megjelenítő útmutatást kidolgozása azt.

## <a name="checklists"></a>Feladatlisták

Ezeket a listákat is rendelkezésre állásának és méretezhetőség alapvető tulajdonságát megtekintésével rövid összefoglalása. 

- [Elérhetőség ellenőrzőlista][AvailabilityChecklist] 

    Ajánlott eljárások a elérhetőség biztosítása összefoglalását.

- [Méretezhetőség ellenőrzőlista][ScalabilityChecklist]

    Ajánlott eljárások a tervezése és végrehajtási méretezhető szolgáltatások és kezelése az adatkezelési összefoglalását.

## <a name="best-practices-articles"></a>Gyakorlati tanácsok a cikkek

Ezek a cikkek, adja meg egy felhőalapú gyakran társított lényeges fogalmat is meg szeretné vizsgálni megvitatásában számítások. 

- [API-Tervező][APIDesign] 

    Vitafórum tervezés problémák webes API tervezésekor figyelembe.

- [API végrehajtása][APIImplementation] 

    Ajánlott eljárások végrehajtása és a webes API közzétételi csoportja.

- [API biztonsági útmutató](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Vitafórum hitelesítési és engedélyezési kérdések (például típusok jogkivonat, engedélyezési protokollok, engedélyezési flow és veszélyt kezelési).

- [Autoscaling útmutató][AutoscalingGuidance] 

    Megfontolandó szempontok meg beavatkozás nélkül megoldások méretezés összefoglalását.

- [Háttér feladatok útmutató][BackgroundJobsGuidance] 

    Rendelkezésre álló beállítások és ajánlott eljárások a háttérben fut, egymástól függetlenül az előtér- vagy interaktív műveletek elvégzendő feladatok végrehajtására leírását.

- [Tartalom kézbesítési hálózati (CDN) útmutató][CDNGuidance] 

    Szóló általános útmutatás és ajánlott eljárás a CDN használni a betöltés az alkalmazások kisméretűvé alakítása és maximalizálása elérhetőségét, és a teljesítmény.

- [Gyorsítótár-útmutató][CachingGuidance] 

    Hogyan gyorsítótárazás használatával javíthatja a teljesítmény és méretezhetőség: a rendszer összegzése.

- [Adatok particionálására útmutató][DataPartitioningGuidance]

    Stratégiák használható partíciót adatok méretezhetőség, javítása érdekében kérelem csökkentése és optimalizálhatja teljesítményét.

- [Figyelés és diagnosztika útmutató][MonitoringandDiagnosticsGuidance] 

    Hogyan a felhasználók a rendszer csatlakozást, erőforrás-kihasználtság nyomon követése és általában figyelni a rendszerállapot és a rendszer teljesítményét követés útmutatást.

- [Ajánlott elnevezési szabályai][naming-conventions] 

    Azure erőforrások elnevezési szabályai ajánlott.

- [Ismételje meg az általános útmutató][RetryGeneralGuidance] 

    Vitafórum általános fogalmak tranziens hibák kezelésére.

- [Szolgáltatásspecifikus útmutatásának újrapróbálkozási][RetryServiceSpecificGuidance]

    Azure szolgáltatások, például az segít használata, igazítani vagy kiterjesztése a újrapróbálkozási mechanizmusa, hogy a szolgáltatás számos újrapróbálkozási funkcióinak összefoglalását.

## <a name="scenario-guides"></a>Forgatókönyv segédvonalak

- [Azure Elasticsearch fut][elasticsearch] 
    
    Elasticsearch pedig egy erősen méretezhető Megnyitás-forrás keresőmotor adatbázis. Alkalmas helyzetek gyors elemzése és nagy adatkészletek tárolt információk felfedezése igénylő. Ez az útmutató vizsgálja meg néhány főbb tényezők egy Elasticsearch fürthöz tervezésekor figyelembe.

- [Az Identitáskezelés multitenant alkalmazások][identity-multitenant] 
    
    Multitenancy-architektúra, ahol több bérlők az alkalmazások megosztani, de egymástól elszigetelt. Ez az útmutató megtudhatja, hogy miként kezelheti a felhasználói identitások multitenant alkalmazásban, használja az [Azure Active Directory] [ AzureAD] bejelentkezési és a hitelesítéshez kezelheti.
    
- [Nagy adatok megoldások fejlesztésére](https://msdn.microsoft.com/library/dn749874.aspx)

    Ez az útmutató ismerteti, például ismétlődő feltárása, mint egy adatraktár ETL folyamatok és a meglévő BI rendszerek integrálása az esetek HDInsight is használja. Útmutatást a nagy adatok, a tervezés és a tervezése nagy adatok megoldások fogalmak ismertetése, és végrehajtása az alábbi lehetőségeket is bemutat.
    
## <a name="patterns"></a>Mintázatok

- [Felhőalapú tervezés mintázatok: Elfogadott architektúra útmutató a felhőben alkalmazások](https://msdn.microsoft.com/library/dn568099.aspx)

    Felhőalapú tervezés mintázatok tervezés mintázatok és a kapcsolódó útmutatást témakörök tárban. Azt articulates mintázatok alkalmazásával hogyan minden darab felhő alkalmazás architektúrákban elfér megjelenítésével előnyeit.
    
- [Felhőalapú alkalmazások teljesítmény optimalizálása](https://github.com/mspnp/performance-optimization)

    Ez az útmutató közös levélszemét mintázatok, amelyek megakadályozzák a alkalmazások terhelés alatt a méretezés-feltárási. Nyolc levélszemét mintázatok és a [teljesítmény elemzése alapozó](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) és a [teljesítmény összevetése kulcs mértékek értékelő](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md)útmutató igazoló mintát is tartalmaz.

## <a name="reference-architectures"></a>Hivatkozás architektúrákban

A hivatkozás architektúrákban forgatókönyv szerint vannak rendezve.
Minden egyes architektúra ajánlott eljárások és elfogadott lépéseket, és egy végrehajtható összetevő, amely a javaslatok, úgy kínál.

A hivatkozás architektúrákban aktuális könyvtár [http://aka.ms/architecture](http://aka.ms/architecture)címen érhető el.

## <a name="resiliency-guidance"></a>Tűrőképessége útmutató

Az alábbi témakörök tervezése rugalmassá hiba elosztott felhőalapú környezetben való alkalmazásokat ismertetik.   

- [Tűrőképessége – áttekintés][ResiliencyOvervew]

     Az Azure platform-alkalmazásokkal, amelyek képesek helyreállítása a hibák és továbbra is működnek létrehozásának módját. A strukturált megközelítés tűrőképessége, a Tervezés fülre kattintva végrehajtása telepítésének és műveletek elérése érdekében ismerteti.

- [Tűrőképessége ellenőrzőlista][resiliency-checklist]

    A tevékenység-ellenőrzőlistát, amely segít javaslatok fellépő hiba üzemmódok számos megtervezése.

- [Hiba mód elemzése][resiliency-fma] 

    A következő készítéséhez tűrőképessége rendszerbe, esetleg nem sikerül pontok azonosításával áll hiba mód analysis (FMA). Kiindulási pontként a FMA folyamatot a cikkben egy katalógushoz, a potenciális hiba üzemmódok és a saját megoldásokkal kapcsolatban. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

