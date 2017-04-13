<properties
   pageTitle="Tűrőképessége technikai útmutató index |} Microsoft Azure"
   description="Index technikai cikkek ismertetése és tervezése rugalmassá, könnyen hozzáférhető, hibafa tervezett alkalmazások, valamint katasztrófa helyreállítási és üzleti folytonosságot megtervezése"
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

#<a name="azure-resiliency-technical-guidance"></a>Azure tűrőképessége technikai útmutató

##<a name="introduction"></a>– Bevezetés

Kétféle Tudásbázis magas rendelkezésre állásának és katasztrófa helyreállítási követelmények van szükség:

- Egy felhőalapú platform funkciók részletes technikai ismertetése
- Hogyan megfelelően tervezővel elosztott szolgáltatás ismerete

Ez a cikksorozat a korábbi terjed ki: a jellemzői és korlátai az Azure platform részletez tűrőképessége (más néven üzemkészségének). Ha érdekli a az utóbbi, című a cikk sorozat [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége](https://aka.ms/drtechguide)összpontosít. Ez a cikk sorozat úgy elérje a minták architektúrája és a tervezés, bár ez nem a fókuszt a sorozat. A tervezési útmutató olvassa el az anyag [További források](#additional-resources) szakaszában.

Az adatok az alábbi cikkekben sorolhatók:

- [Helyi hibaelhárítás](resiliency-technical-guidance-recovery-local-failures.md).
Hiba történhet a fizikai hardver (például meghajtók, kiszolgálók és hálózati eszközök). Erőforrások is felhasználhatók, amikor a betöltés napra. Ez a cikk ismerteti az Azure ad magas elérhetőségének e feltételek alapján karbantartása funkcióját.

- [Az Azure régió szintű a szolgáltatási zavarok helyreállítása](resiliency-technical-guidance-recovery-loss-azure-region.md).
Széles körű hibák ritka, de azok elméletileg lehetséges. Teljes régiók előfordulhat, hogy hálózati hibák miatt elszigetelt, vagy azok is fizikailag sérült a természeti katasztrófák. Ez a cikk ismerteti, hogyan használhatja az Azure földrajzilag különböző régiók átterjedő alkalmazások létrehozásához.

- [A helyszíni az Azure helyreállítása](resiliency-technical-guidance-recovery-on-premises-azure.md).
Az a felhő jelentősen megváltoztatja a közgazdasági vészhelyreállítás Azure-helyreállítás második webhely létrehozásához használandó szervezetek engedélyezése a. Ezt megteheti a a összeállítását, és egy másodlagos adatközponthoz karbantartási költség egy törtet. Ez a cikk ismerteti a lehetőségeket, amelybe a felhőbe egy helyszíni adatközponthoz bővítése Azure.

- [Helyreállítási adatsérülésekkel vagy véletlen törlését](resiliency-technical-guidance-recovery-data-corruption.md).
Alkalmazások beállíthatja, hogy hibák sérült adatokat. Operátorok helytelenül törölheti a fontos adatokat. Ez a cikk ismerteti, hogy mi Azure nyújt a adatok biztonsági mentése és visszaállítása egy korábbi állapotra, időt.

##<a name="additional-resources"></a>További források

- [Vészhelyreállítás és a Microsoft Azure-ra épülő alkalmazások magas elérhetőségét](resiliency-disaster-recovery-high-availability-azure-applications.md).
Ez a témakör az elérhetőség és a helyreállítás részletes áttekintése. A kérdés hivatkozás és tranzakció alapú adatok manuális replikációs lefedi. A végleges szakaszokban katasztrófa helyreállítási topológiák elérhetőségét a legmagasabb szintű Azure régiók átterjedő különféle összegzéseinek.

- [Magas rendelkezésre állás feladatlista](resiliency-high-availability-checklist.md).
Ez a cikk szolgáltatások, a szolgáltatások listáját, és látványtervek, amelyek segítséget nyújtanak a tűrőképessége és az alkalmazás elérhetősége növeléséhez.

- [Microsoft Azure service tűrőképessége útmutatást](resiliency-service-guidance-index.md).
Ez a cikk az Azure szolgáltatások index és katasztrófa helyreállítási útmutatást és a tervezési útmutató mutató hivatkozásokat tartalmaz.

- [Áttekintése: a felhő üzleti folytonosságot és az adatbázis vészhelyreállítás SQL-adatbázissal](../sql-database/sql-database-business-continuity.md).
Ebben a cikkben Azure SQL-adatbázis technikákat az elérhetőség. A középre elsősorban a biztonsági mentés és visszaállítás stratégiák igazítása. Azure SQL-adatbázis a felhőalapú szolgáltatást használ, érdemes áttekintenie, ha ez a papír és a kapcsolódó erőforrásokat.

- [Magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
A cikkből megismerheti, használatakor infrastruktúra szolgáltatásként (IaaS) tárolni az adatbázis-szolgáltatások elérhetősége beállításokat. Azt ismerteti, hogy AlwaysOn rendelkezésre állási csoportok, az adatbázis-tükrözéshez, a napló szállítási és a biztonsági mentési és visszaállítási. Több oktatóanyagok használatáról az alábbi eljárások megjelenítése.

- [Gyakorlati tanácsok Azure Cloud Services nagyméretű szolgáltatások tervezéséhez](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
Ez a cikk a nagyon méretezhető felhő architektúrákban elkészítésének koncentrál. A módszereket, amelyek akkor alkalmaznia méretezhetőség javítása érdekében számos növelheti elérhetősége is. Is ha az alkalmazás nem méretezheti nagyobb terhelés alatt, méretezhetőség elérhetősége problémát lesz.

- [Biztonsági mentési és visszaállítási az Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Ez a cikk ismerteti technikai biztonsági mentése és visszaállítása a Microsoft SQL Server Azure virtuális gépeken futó a futtatása.

- [Failsafe: rugalmassá felhő architektúrákban útmutatást](https://channel9.msdn.com/Series/FailSafe).
Ez a cikk ismerteti létrehozásának rugalmassá felhő architektúrákban, Microsoft technológiák, és ezeket a különböző forgatókönyvekben architektúrákban végrehajtásához recepteket e architektúrákban végrehajtási útmutatást.

- [Technikai esettanulmány: Felhő technológiák segítségével javítható vészhelyreállítás](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
A esettanulmány mutatja, hogy hogyan Microsoft IT használt vészhelyreállítás javítható Azure.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a technikai útmutató Azure tűrőképessége sorozat része. Ha érdekli a sorozat más cikkek olvasása, kiindulhat [helyi hibaelhárítás](resiliency-technical-guidance-recovery-local-failures.md).
