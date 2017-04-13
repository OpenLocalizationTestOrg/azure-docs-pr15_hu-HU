<properties
   pageTitle="Eltérések a Felhőszolgáltatások és a szolgáltatás háló |} Microsoft Azure"
   description="Való áttelepítés elrendezését Cloud Services szolgáltatás háló alkalmazások."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Tudnivalók a Felhőszolgáltatások és a szolgáltatás háló közötti különbségek áttelepítése előtt alkalmazásokat.
Microsoft Azure Service háló erősen méretezhető, nagymértékben megbízható elosztott alkalmazásokhoz a következő generációs felhő alkalmazás platform. Bemutatja a ennyi csomagolása, telepítése, frissítése és elosztott felhő alkalmazások kezelése új szolgáltatásai. 

Ez a egy bevezető útmutató történő áttelepítéséhez Cloud Services szolgáltatás háló alkalmazások. Koncentrál elsősorban építészeti, és azt tervezése a Felhőszolgáltatások és a szolgáltatás háló közötti különbségeket.
 
## <a name="applications-and-infrastructure"></a>Alkalmazások és -infrastruktúrát

A Felhőszolgáltatások és a szolgáltatás háló alapvető különbség, a VMs, feladatok és az alkalmazások között a kapcsolatot. Itt a terhelést a kód egy meghatározott tevékenység végrehajtása, vagy adja meg a szolgáltatás írási definíciója.
 
 - **Cloud Services az alkalmazások, például VMs telepítésével.** A kód, akkor egy virtuális példányhoz, például egy webhelyre vagy dolgozó szerepkör szorosan ahhoz. A terhelést a Cloud Services üzembe van telepítéséhez futtassa a terhelést a egy vagy több virtuális példányát. Az alkalmazások és VMs nincs elkülönítésének nem, és így nem hivatalos definíciója van az alkalmazás. Az alkalmazások is gondolatot az interneten vagy dolgozó szerepkör példányok Cloud Services telepítés belül vagy egy teljes Cloud Services telepítési. Ebben a példában az alkalmazások szerepkör-példányok jelenik meg.
 
![Cloud Services-alkalmazások és a topológia][1]

 - **Szolgáltatás háló az alkalmazások meglévő VMs vagy szolgáltatás háló futó Windows vagy Linux rendszerhez gépek üzembe helyezése.** A szolgáltatások, akkor is teljesen leválasztott, az alapul szolgáló infrastruktúra, amelyeket nem vagyok a gépnél szolgáltatás háló alkalmazás platform, így több környezetekben telepíthető az alkalmazás a. A szolgáltatás háló terhelési "szolgáltatás" nevezik, és egy vagy több szolgáltatások vannak csoportosítva, a szolgáltatás háló alkalmazás platformon futó hivatalos definiált alkalmazásban. Több alkalmazást egyetlen szolgáltatás háló fürtre elvégezhető.
 
![Szolgáltatás háló alkalmazások és a topológia][2]
 
Szolgáltatás háló magát egy alkalmazás platform réteget, amelyet futtat Windows vagy Linux rendszerhez, a, mivel a Cloud Services Azure felügyelt VMs üzembe helyezése a csatolt munkaterhelésekből a rendszer.
A szolgáltatás háló alkalmazásmodell számos előnye van:

 - Gyors telepítési időpontok. Virtuális példányok létrehozására időigényes lehet. A szolgáltatás háló VMs csak telepítését követően a szolgáltatás háló alkalmazás platform tárolja a fürtre űrlap. Ettől kezdve alkalmazáscsomagok elvégezhető a fürthöz nagyon gyorsan.
 - Nagysűrűségű szolgáltatója. A Cloud Services a dolgozó szerepkör virtuális egy terhelést tárolja. A szolgáltatás háló alkalmazások olyan külön értendő a VMs futó őket, ami azt jelenti, telepítheti a sok kisszámú VMs, amely nagyobb telepítésekhez teljes költsége csökkentheti az alkalmazások.
 - A szolgáltatás háló platform futtathat tetszőleges Windows Server vagy Linux gépek rendelkezik, kerül Azure vagy a helyszíni. A platform, az alkalmazás futtatását is lehetővé teszi, a különböző környezetekben nyújt az alapul szolgáló infrastruktúra-hardverabsztrakciós réteg. 
 - Megosztott alkalmazások kezelése. Szolgáltatás háló nemcsak hosts elosztott alkalmazásokat, de is kezelheti a segít, függetlenül a használat virtuális életciklusuk vagy gépi életciklus platformot.

## <a name="application-architecture"></a>Alkalmazás-architektúra

A Cloud Services alkalmazás architektúráját általában számos külső szolgáltatás függőségek, például szolgáltatás Bus, Azure-táblából és Blob-tárolóhoz, SQL, vgx.dll és mások az állapotot, és az alkalmazás és a Cloud Services környezetben Web és a dolgozó szerepkörök közötti kommunikáció adatok kezelése tartalmazza. Példa egy kész Cloud Services alkalmazás előfordulhat, hogy néz ki:  

![Felhőalapú szolgáltatások architektúra][9]

Szolgáltatás háló alkalmazások ugyanazok a külső szolgáltatások használata egy teljes alkalmazásban is választhat. Ebben a példában Cloud Services architektúrája, a legegyszerűbb áttelepítési útvonalat Cloud Services szolgáltatás háló való használata csak a Cloud Services példányban cseréje szolgáltatás háló alkalmazással megőrzési átfogó architektúrája azonos. A webhely és a dolgozó szerepkörök is átültetésre szolgáltatás háló állapot nélküli szolgáltatások minimális kód módosításokkal.

![Egyszerű az áttelepítés után szolgáltatás háló architektúra][10]

Ebben a szakaszban a rendszer továbbra is ugyanúgy működik, mint előtt. Szolgáltatás háló állapot-nyilvántartó szolgáltatások külső állam tárolja kihasználni is internalized, mint megbízható, szűrőként működő szolgáltatások, ahol erre lehetőség van. Ez a bonyolultabb egyszerű az áttelepítés dolgozó szerepkörök és Web Service háló állapot nélküli szolgáltatásokat, mint szükséges, adja meg az alkalmazás megfelelő funkciók, mint a külső szolgáltatások előtt egyéni szolgáltatások írása. Ha így előnyei többek között: 

 - A külső függőségeket eltávolítása 
 - A telepítés, kezelése és frissítési modellek egységesítésére használható. 
 
Egy példa eredményül kapott architektúráját az alábbi szolgáltatások internalizing ábrája ilyen:

![Teljes az áttelepítés után szolgáltatás háló architektúra][11]

## <a name="communication-and-workflow"></a>Kommunikáció és a munkafolyamat

A legtöbb felhő szolgáltatásalkalmazások egynél több réteg állnak. A szolgáltatás háló alkalmazások hasonlóan egynél több szolgáltatást (általában hány szolgáltatások) áll. Két közös kommunikáció modellek közvetlen kommunikációt és egy külső tartós tároló keresztül.

### <a name="direct-communication"></a>Közvetlen kommunikációt

Közvetlen kommunikációt rétegek is kommunikálhatnak egymással közvetlenül minden réteg által elérhetővé tett végpontot. Állapot nélküli környezetben, például Cloud Services, ez azt jelenti, jelölje ki a virtuális szerepkör egy példányának vagy véletlenszerűen vagy egyenleg terhelés ciklikus és végpontját közvetlenül csatlakozik.

![Cloud Services közvetlen kommunikációt][5]

 Közvetlen kommunikációt szolgáltatás háló a közös kommunikáció modelljének. A fő szolgáltatás háló és közötti Cloud Services különbség, hogy a Cloud Services csatlakozik egy virtuális, mivel a szolgáltatás háló kapcsolódni lehet egy szolgáltatás. Ez az egy fontos különbséget néhány okok miatt:

 - A szolgáltatás háló Services nem kötődnek a VMs, amely a szolgáltató őket; Előfordulhat, hogy a fürt mozgás Services és az valójában várhatóan különféle okok Navigálás: erőforrás terheléselosztás, feladatátvevő, alkalmazások és az infrastruktúra frissítések és elhelyezését vagy betöltése kényszerek. Ez azt jelenti, hogy a szolgáltatás példányának cím bármikor módosíthatja. 
 - A szolgáltatás háló egy virtuális üzemeltetheti több egyedi végpontjait a szolgáltatásban.

Háló szolgáltatás lehetővé teszi a szolgáltatás feltárás, nevű az elnevezési szolgáltatása, amelynek szolgáltatások végpontjának címe megoldásához használható. 

![Szolgáltatás háló közvetlen kommunikáció][6]

### <a name="queues"></a>Sorok

A közös kommunikáció mechanizmusa rétegek állapot nélküli környezetben, például a Cloud Services között használva egy külső tároló várólista tartósan feladatok az egy réteg között. A leggyakoribb helyzet egy webes réteg, amely feladatok küld egy Azure várólista vagy Bus szolgáltatás, ahol dolgozó szerepkör-példányok feldolgozásához és a feladatok feldolgozása.

![Cloud Services várólista kommunikáció][7]

Ugyanazt a kapcsolati modellt szolgáltatás háló használható. Ez akkor lehet hasznos, áttelepítéséhez egy meglévő Cloud Services szolgáltatás háló alkalmazást. 

![Szolgáltatás háló közvetlen kommunikáció][8]
 
## <a name="next-steps"></a>Következő lépések

A legegyszerűbb áttelepítési a Felhőbeli szolgáltatások szolgáltatás háló útvonala csak a Cloud Services példányban lecserélése szolgáltatás háló-alkalmazást, hagyja az alkalmazás általános architektúráját nagyjából ugyanúgy. A következő cikkben egy útmutatóra egy webhelyre vagy dolgozó szerepkör átalakítása szolgáltatás háló állapot nélküli szolgáltatás.

 - [Egyszerű áttelepítési: egy webhelyre vagy dolgozó szerepkör átalakítása szolgáltatás háló állapot nélküli szolgáltatás](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
