<properties
    pageTitle="Mi az webhely helyreállítási? | Microsoft Azure"
    description="Áttekintést nyújt az Azure webhely helyreállítási szolgáltatás, és telepítési forgatókönyvek összegzi."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Mi az webhely helyreállítási?

Üdvözli a Azure webhely helyreállítás! Ez a cikk a webhely helyreállítási szolgáltatást, és hogyan beleszámít üzleti gyors áttekintést nyújt.

A szervezet egy üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, amely megtartása alkalmazások, feladatok és adatok biztonságos és a rendelkezésre álló tervezett és a tervezett legrövidebb leállás közben van szüksége, és helyreállítása a normál munka feltétel minél korábban beállítást. Webhely-helyreállítás egy Azure szolgáltatás, amely a stratégia beleszámít.

Webhely helyreállítási orchestrates futó helyszíni fizikai kiszolgálók és virtuális gépeken futó feladatok a replikáció. Hogy bizonyos kiszolgálók és VMs egy elsődleges adatközponthoz az a felhő (Azure), illetve egy másodlagos adatközponthoz. Kimaradások végrehajtásakor az elsődleges webhelyen, átadni a másodlagos webhely megtartása alkalmazások és -munkaterhelésekből, könnyen kezelhető és elérhető. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek.

## <a name="site-recovery-in-the-azure-portal"></a>Webhely-helyreállítás az Azure-portálon

Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) az erőforrások létrehozásáról és használatáról. Az erőforrás-kezelő Azure modell és a klasszikus szolgáltatások kezelése modell. Azure szintén két portálokról – az [Azure klasszikus portál](https://manage.windowsazure.com/) , amely támogatja a klasszikus telepítési modell, és az [Azure portál](https://portal.azure.com) klasszikus, mind az erőforrás-kezelő az adatmodellek támogatása.

- Webhely-helyreállítás az a klasszikus portál és az Azure portál is elérhető.
- Az Azure klasszikus portálon a klasszikus szolgáltatások kezelése modellel támogatja a webhely helyreállítási.
- Az Azure-portálon támogatja a Klasszikus modell vagy erőforrás-kezelő telepítések. 

Az információk ebben a cikkben classic és az Azure portál telepítések vonatkozik. Különbségek vannak jegyezni, ahol erre lehetőség van.


## <a name="why-deploy-site-recovery"></a>Miért üzembe webhely helyreállítási?

Az alábbiakban webhely helyreállítási mire alkalmas a vállalati:

- **Egyszerűsítése BCDR**– képes kezelni a replikáció, feladatátvétel és helyreállítása több munkaterhelésekből egyetlen helyen, az Azure-portálon. Webhely helyreállítási orchestrates replikációs és feladatátvételi, de nem az alkalmazás adatok metsz vagy kell minden információt.
- **Rugalmas replikációs Provide**– webhely helyreállítási használatával, hogy bizonyos támogatott a Hyper-V VMs, VMware VMs és Windows vagy Linux fizikai kiszolgálón futó feladatok.
- **Tesztelés végrehajtása egyszerűen replikációs**– webhely helyreállítási biztosít próba feladatátadás a támogatási katasztrófa helyreállítási gyakorlatokat, gyártási környezetekben megtartásával.
- **Nem sikerül fölé, és helyreállítása**– minimális adatvesztés (attól függően, hogy a replikáció gyakoriság) váratlan katasztrófák tervezett feladatátadás várható kimaradások a nulla adatvesztés-, illetve nem tervezett feladatátadás hajthat végre. A feladatátvétel után visszaállás elsődleges webhelyekhez való is. Webhely helyreállítási biztosít a helyreállítási-előfizetésekhez is parancsfájlok és Azure automatizálási munkafüzetek, hogy testre szabhatja a feladatátvevő és helyreállítási a több szálon futó alkalmazások.
- **A másodlagos adatközponthoz kiküszöbölheti**–, hogy bizonyos feladatok Azure helyett egy másodlagos webhelyet. Ez megszünteti a költség és összetettsége egy másodlagos adatközponthoz fenntartása. Replikált adatok, ahol minden címtárfrissítések az Azure-tárolóban lévő vannak tárolva. Amikor feladatátadás VMs létre a replikált adatokat.
- **A meglévő BCDR technológiák**– webhely helyreállítási integrálódik más BCDR funkciók. Például az SQL Server kódmentes vállalati munkaterhelésekből, például SQL Server AlwaysOn rendelkezésre állási csoportok feladatátvételének kezelése natív támogatása a védelemmel ellátni webhely helyreállítási is használhatja.

## <a name="what-can-i-replicate"></a>Mi hogy bizonyos?

Mi, hogy bizonyos összefoglalása az alábbiakban webhely helyreállítás segítségével.

![A helyszíni helyszíni](./media/site-recovery-overview/asr-overview-graphic.png)

**BIZONYOS** | **VALÓ REPLIKÁCIÓ** 
---|---
A helyszíni VMware VMs futó feladatok | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Másodlagos webhely](site-recovery-vmware-to-vmware.md)
A helyszíni a Hyper-V VMs futó feladatok VMM felhőket kezelése  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Másodlagos webhely](site-recovery-vmm-to-vmm.md) 
A helyszíni a Hyper-V VMs futó terhelést VMM felhőket SAN adathordozós kezelése|  [Másodlagos webhely](site-recovery-vmm-san.md)
A helyszíni a Hyper-V VMs VMM nélkül futó feladatok | [Azure](site-recovery-hyper-v-site-to-azure.md)
A helyszíni fizikai Windows vagy Linux kiszolgálón futó feladatok | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Másodlagos webhely](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Milyen munkaterhelésekből is védelme?

Webhely helyreállítási lehetővé teszi, hogy alkalmazás tudatában BCDR, így munkaterhelésének és alkalmazások továbbra is kimaradások fordulhat elő, amikor egy következetes módszert futtatásához. Webhely helyreállítási nyújtja:

- **Alkalmazás egységes pillanatképek**– gépek replikáció alkalmazás egységes pillanatfelvételek, egy vagy több szintű alkalmazásai. Nemcsak a lemez adatok rögzítése, alkalmazás egységes pillanatképek rögzítési rögzítése a memória az összes adat és a folyamat minden tranzakciók.
- **Közelében szinkron replikációs**– webhely helyreállítási előírja replikációs gyakoriság a lehető a Hyper-V 30 másodperc és VMware a folyamatos replikáció.
- **Rugalmas helyreállítási csomagok**– létrehozhat és helyreállítási csomagok külső parancsfájlok és a kézi műveletek testreszabása. Integrálása az Azure automatizálási runbooks lehetővé teszi helyreállítása egy teljes alkalmazás Papírhalom egyetlen kattintással.
- **Az SQL Server AlwaysOn integrációja**– rendelkezésre állási csoportok feladatátvételének kezelheti a webhely helyreállítási helyreállítási tervek.
- **Automatizálási tár**– gazdag Azure automatizálási tár gyártási – kész, az alkalmazás-specifikus parancsprogramok letöltött, és webhely-helyreállítás integrálódik biztosít.
- **Egyszerű hálózatkezelő**– speciális hálózatkezelő webhely helyreállítási és Azure leegyszerűsíti alkalmazás hálózati követelményeknek, ideértve a lefoglalására IP-címek, terheléselosztókkal beállítása és Azure forgalom Manager integrálása a hatékony hálózati switchovers.


## <a name="next-steps"></a>Következő lépések

- További információ a [milyen munkaterhelésekből webhely helyreállítási megvédheti?](site-recovery-workload.md)
- További tudnivalók a webhely helyreállítási architektúra [webhely helyreállítási működése?](site-recovery-components.md)
 
