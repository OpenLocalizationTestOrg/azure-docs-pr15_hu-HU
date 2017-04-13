<properties
   pageTitle="Szolgáltatási tűrőképessége útmutatást |} Microsoft Azure"
   description="Megelőző tűrőképessége és elérhetőség útmutató a Microsoft Azure-szolgáltatások és vészhelyreállítás mutató hivatkozásokat tartalmaz."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Microsoft Azure szolgáltatás tűrőképessége útmutató
Microsoft Azure készült, küldje el a szükséges erőforrások, szükség szerint csoportokat. Jó tervezési és működési gyakorlat részeként egyaránt hogyan tervezővel magas rendelkezésre állás érdekében Azure szolgáltatások használatára, valamint a Mi a teendő, ha az alkalmazás hatással van a szolgáltatási zavarok kell figyelembe. Ehhez a dokumentumhoz a támogatási, az ezt a folyamatot, a katasztrófa helyreállítási útmutatást, valamint a tervezési útmutató az Azure-szolgáltatások különböző hivatkozásokat is tartalmaz.

##<a name="disaster-recovery-guidance"></a>Katasztrófa helyreállítási útmutató
Az alábbi hivatkozások akkor katasztrófa helyreállítási útmutatást adja meg az adatokkal, ha segítségre van szüksége, az alkalmazás vissza online gyorsan letölthető és, ha Ön az Azure a szolgáltatási zavarok hatással vannak. A kérdés segítségével létrehozott ezeket a hivatkozásokat, "E vagyok éppen hatással az Azure szolgáltatási zavarok Mit tehetek?"

##<a name="design-guidance"></a>Tervezési útmutató
Az alábbi tervezési útmutató hivatkozásokat, hogy tervezés és útmutatóból megtudhatja, hogyan lehet a legjobban, ha az alkalmazás üzemidőt maximalizálja minden Azure szolgáltatás használatához a létrehozott építészeti útmutatást. Ezeket a hivatkozásokat "Hogyan tudható, hogy egy hiba, hardver hiba, a szolgáltatási zavarok vagy egyéb hiba az alkalmazás általános elérhetősége nem hatással?" kérdés megválaszolásához segítséget által létrehozott Ha nem adott a szolgáltatás jelenleg keres útmutatást, a [Microsoft Azure-ra épülő alkalmazások magas elérhetősége](./resiliency-high-availability-azure-applications.md) cikk lehet további információt a tervező segítségével. 

##<a name="service-guidance"></a>Szolgáltatás-útmutató
| Szolgáltatás  | Katasztrófa helyreállítási útmutató | Tervezési útmutató |
|:---------|:--------------------------:|:------------------:|
| [Cloud Services] (https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services")       | [hivatkozás] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure Cloud Services katasztrófa helyreállítási útmutató")   | Nem érhető el |
| [Fő tárolóból elemre] (https://azure.microsoft.com/services/key-vault/ "Azure kulcs tárolóból elemre")                      | [hivatkozás] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure kulcs tárolóra katasztrófa helyreállítási útmutató")        | Nem érhető el |
| [Tárhely] (https://azure.microsoft.com/services/storage/ "Azure tárhely")                            | [hivatkozás] (../storage/storage-disaster-recovery-guidance.md "Azure tároló katasztrófa helyreállítási útmutató")          | Nem érhető el |
| [SQL-adatbázisait] (https://azure.microsoft.com/services/sql-database/ "Azure SQL-adatbázisait")           | [hivatkozás] (../sql-database/sql-database-disaster-recovery.md  "Azure SQL-adatbázis katasztrófa helyreállítási útmutató")    | [hivatkozás] (../sql-database/sql-database-business-continuity.md "Az SQL Azure-adatbázis üzemkészségének áttekintése") |
| [Virtuális gépeken futó] (https://azure.microsoft.com/services/virtual-machines/ "Azure virtuális gépeken futó") | [hivatkozás] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure virtuális gépeken futó katasztrófa helyreállítási útmutató") | Nem érhető el |
| [Virtuális hálózati] (https://azure.microsoft.com/services/virtual-network/ "Azure virtuális hálózati")    | [hivatkozás] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure virtuális hálózati katasztrófa helyreállítási útmutató")  | Nem érhető el |

##<a name="next-steps"></a>Következő lépések
Ha a keres útmutatást, amely szélesebb körben koncentrál rendszerek és megoldások, olvassa el a [vészhelyreállítás és a Microsoft Azure-ra épülő alkalmazások magas elérhetőségét](https://aka.ms/drtechguide).
