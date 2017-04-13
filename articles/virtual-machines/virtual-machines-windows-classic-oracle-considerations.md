<properties
pageTitle="Az Oracle virtuális képek használatával kapcsolatos szempontok |} Microsoft Azure"
description="Tudjon meg többet a támogatott beállításokat és a Windows Server Azure-ban egy Oracle virtuális korlátozások, mielőtt beállítaná."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Az Oracle virtuális gép képek egyéb szempontok



Ez a cikk bemutatja a szempontjait az Oracle virtuális gépeken futó az Azure, amelyek Oracle szoftver képek Oracle által biztosított alapulnak.  

-  Az Oracle-adatbázis virtuális gép képek
-  Az Oracle WebLogic kiszolgáló virtuális gép képek
-  Az Oracle JDK virtuális gép képek

##<a name="oracle-database-virtual-machine-images"></a>Az Oracle-adatbázis virtuális gép képek
### <a name="clustering-rac-is-not-supported"></a>Nem támogatott fürtképző (RAC)

Azure jelenleg nem támogatja az Oracle valós alkalmazás fürt (RAC) az Oracle-adatbázis. Oracle-adatbázis csak különálló példányai lehetségesek. Ennek oka az Azure jelenleg nem támogatja a virtuális merevlemez-megosztás olvasási/írási módon több virtuális gép példányok között. Csoportos UDP még nem támogatott.

### <a name="no-static-internal-ip"></a>Nincs statikus belső IP-cím

Azure rendeli hozzá minden virtuális gépi belső IP-címet. A virtuális gép virtuális hálózat része, kivéve a virtuális gép IP-címe dinamikus, és a virtuális gép újraindítása után megváltozik. Ez problémákat okozhatják, mert az Oracle-adatbázishoz vár statikus IP-címét. A probléma elkerülése érdekében fontolja meg a virtuális gép hozzáadása Azure virtuális hálózathoz. [Virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/) és [létrehozása az Azure virtuális hálózat](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) talál további információt.

### <a name="attached-disk-configuration-options"></a>Csatlakoztatott lemezre beállítási lehetőségek

A virtuális gép operációs rendszer lemezen vagy csatolt lemezen, más néven adatok lemezt is elhelyezhet adatfájlokat. Csatolt lemezre jobb teljesítmény és a méret rugalmasság, mint az operációs rendszer lemezen is felajánl. Lehet, hogy az operációs rendszer lemezen csak olyan adatbázisokhoz a 10 gigabájtos (GB) helyett.

Csatolt lemezre az Azure Blob-tároló szolgáltatás támaszkodhat. Minden egyes lemez is alkalmas elméleti legfeljebb 500 KB bemeneti és kimeneti másodpercenként művelet (IOPS). A csatolt lemezt a teljesítmény nem optimális kezdetben, és IOPS teljesítmény körülbelül 60-90 perc folyamatos művelet "írási a" elteltével jelentősen javítható. Lemezen később tétlen, ha a IOPS teljesítmény előfordulhat, hogy előrehaladtával csökkenni addig, amíg a másik írási az időszak folyamatos művelet. Röviden az további aktív a lemezen, annál nagyobb az esélye azt, hogy megközelítés IOPS teljesítményének.

Bár a legegyszerűbb egyetlen lemez csatolni virtuális gépen és helyezi el a lemezen adatbázisfájlok, ezzel a módszerrel is teljesítmény értelmez a legtöbb korlátozó. Ehelyett gyakran javíthatja a hatékony IOPS teljesítmény, ha több csatolt lemez használata, húzza szét az adatbázis adatainak át őket, és írja be az Oracle automatikus tárhely kezelése (ASM). További információt a [Oracle automatikus tároló áttekintése](http://www.oracle.com/technetwork/database/index-100339.html) című témakörben találhat. Több lemezre csíkozást használható operációs rendszer szintre, de ezt a megközelítést nem javasolt, mert nem ismert IOPS a teljesítmény javítása érdekében.

Fontolja meg az e szeretne olvasási műveletek teljesítményét rangsorolása, vagy írja be az adatbázis-műveletek alapján több lemezre csatolása két különböző megközelítés:

- **Oracle ASM saját maga** valószínűleg jobb írási művelet teljesítmény, de az olvasási műveletek szemben a lemez tömbök használata megközelítés ami IOPS még rosszabb eredményez. Az alábbi ábra szemlélteti a ennél a módszernél logikailag.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] A teljesítmény írási és olvasási teljesítmény-eseti alapon között a csökkentés felmérése. Ha ezt a tényleges eredmények változhat.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Magas rendelkezésre állásának és katasztrófa helyreállítási kapcsolatos szempontok

Oracle-adatbázis Azure virtuális gépeken futó használatakor is felelősséggel tartozik egy nagy rendelkezésre állásának és katasztrófa helyreállítási megoldás bármely legrövidebb leállás elkerülése érdekében. Akkor is felelős biztonsági másolat készítése saját adatok és az alkalmazás.

Az Oracle adatbázis Enterprise Edition (nélkül RAC) Azure a magas rendelkezésre állásának és katasztrófa helyreállítási [adatok Guard, az aktív adatok Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)vagy [Oracle Fülöp kapu](http://www.oracle.com/technetwork/middleware/goldengate), használja a két adatbázisok két külön virtuális gépeken futó lehet elérni. Mindkét virtuális gépeken futó elérhetik egymás felett a magánjellegű állandó IP-cím biztosítja az ugyanazt a [felhőbeli szolgáltatástól](virtual-machines-linux-classic-connect-vms.md) és ugyanazon a [virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/) kell lennie.  Azt javasoljuk, úgy, hogy a virtuális gépeken futó azonos [elérhetőségének beállítása](virtual-machines-windows-manage-availability.md) helyezhet őket külön hibafa tartományok és frissítése a tartományok Azure engedélyezni. Csak az ugyanazt a felhőalapú szolgáltatást a virtuális gépek elérhetősége mindazon vehet részt. Virtuális gépeken 2 GB memóriát és 5 GB szabad lemezterület kell rendelkeznie.

Az Oracle-adatok Guard, nagy elérhetősége érhető el egy virtuális gép, a másodlagos (készenléti) adatbázis virtuális egy másik számítógépen, egy elsődleges adatbázist, és állítsa be a közöttük lévő egyirányú replikációs. Az adatbázis másolati példányának olvasási hozzáférést eredménye. Az Oracle GoldenGate beállíthatja a két adatbázis közötti kétirányú replikáció. Megtudhatja, hogy miként állíthatja be a következő eszközökkel adatbázisok magas rendelkezésre állás megoldást, dokumentációjában olvasható [Az aktív adatok Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) és [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) az Oracle-webhelyén. Ha meg kell írási és olvasási hozzáférést az adatbázis azon példányára, [Aktív az Oracle-adatok Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html)is használhatja.

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Az Oracle WebLogic kiszolgáló virtuális gép képek

-  **Fürtözés támogatott Enterprise Edition csak.** Akkor is rendelkezik licenccel WebLogic fürtözés csak a vállalati Edition a WebLogic kiszolgáló használata esetén. Ne használjon WebLogic Server Standard Edition fürtözés.

-  **Kapcsolat időtúllépései:** Ha az alkalmazás az Azure valamelyik másik felhőszolgáltatásával (például egy adatbázis-réteg-szolgáltatása) nyilvános végpontok kapcsolatok támaszkodik, Azure előfordulhat, hogy zárja be a ezek nyissa meg a kapcsolatok négy perc inaktivitás után. Hatással lehet szolgáltatások és alkalmazások használna kapcsolat-készletek, mivel előfordulhat, hogy ezt a korlátot több inaktív kapcsolatok már nem érvényben marad. Ha ez hatással van az alkalmazás, fontolja meg a kapcsolat készletek "életben" logika.

    Ha a végpontok *belső* az a felhő Azure szolgáltatás üzembe (például egy különálló adatbázis virtuális számítógép *ugyanazt* a felhőalapú szolgáltatást, mint a WebLogic virtuális gépeken futó belül), majd a kapcsolat közvetlen és nem támaszkodik az Azure terheléselosztó, és ezért nem a kapcsolat időtúllépési fizetnie.

-  **UDP csoportos nem támogatott.** Azure UDP történő, de nem csoportos vagy közvetítése támogatja. WebLogic kiszolgáló az Azure UDP egyedi funkciók hivatkozhatnak. A legjobb használna UDP egyedi eredmény érdekében azt javasoljuk, hogy a WebLogic fürt mérete megmaradjanak statikus, vagy a fürt szereplő legfeljebb 10 felügyelt kiszolgálókkal kell tartani.

-  **WebLogic kiszolgáló vár nyilvános és titkos portokat, hogy ugyanazok a T3 hozzáférés (például vállalati JavaBeans használata esetén).** Fontolja meg a két vagy több felügyelt kiszolgálók, egy névvel ellátott **SLWLS**felhőszolgáltatásában álló WebLogic kiszolgáló fürthöz réteg (EJB) szolgáltatásalkalmazás futtató több szálon példa. Az ügyféloldali réteg valamelyik másik felhőszolgáltatásában, egy egyszerű Java-program, amelyet fel szeretne hívni a szolgáltatás réteg EJB futó van. Mivel terheléselosztó a szolgáltatás réteget, egy nyilvános terheléselosztás végpontot kell a virtuális gépeken futó a WebLogic kiszolgáló fürt jön létre. Ha a saját olyan portot végpontra megadott eltér a nyilvános port (például 7006:7008), például a következő hiba történik:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Ennek az oka T3 távelérési WebLogic kiszolgáló vár a betöltés terheléselosztó port és a felügyelt WebLogic kiszolgáló port azonosnak kell lennie. A fenti lehetőséget választja az ügyfél port 7006 (a betöltés terheléselosztó port) éri el, és figyeli a felügyelt kiszolgáló 7008 (a személyes port). Ez a korlátozástípus akkor csak a T3 hozzáférést, a HTTP-nem alkalmazható.

    Ez a probléma elkerülése érdekében használjon egy, az alábbi lehetőségek közül:

    -  Használja a azonos személyes és nyilvános portszámokat terheléselosztása végpontok T3 access hozzárendelve.

    -  A következő JVM paraméter kihagyása WebLogic kiszolgáló indítása:

            -Dweblogic.rjvm.enableprotocolswitch=true

Kapcsolódó információk olvassa el a KB cikk **860340.1** a <http://support.oracle.com>című témakört.

-  **Dinamikus fürtözés és terheléselosztási korlátozások vonatkoznak.** Tegyük fel, hogy a dinamikus fürtre WebLogic Server-alapú és elérhetővé azt egy egyetlen, nyilvános terheléselosztás végpontot Azure-ban. Ez a mindaddig, amíg egy rögzített portszámot használni az egyes a felügyelt kiszolgálók (a tartomány nem dinamikusan kiosztott), és nem indul el további felügyelt kiszolgálók, mint a rendszergazda külön követi nyomon gépek elvégezhető (Ez azt jelenti, hogy több, mint egy felügyelt kiszolgáló egy virtuális gépen). Ha több, mint virtuális gépeken futó indítása folyamatban WebLogic kiszolgáló konfigurációjától eredménye (Ez azt jelenti, ahol több WebLogic Server-példány megoszthatja virtuális ugyanarra a gépre), majd még nem kötődnek egy adott portszámot WebLogic Server-kiszolgálók e példányainak egynél több lehetséges – a többi, hogy virtuális gépen sikertelen lesz.

    Kézzel Ha úgy állítja be a rendszergazda kiszolgáló automatikusan egyedi portszámokat hozzárendelése a kezelt kiszolgálók, majd terheléselosztás függvény nem lehetséges mivel Azure nem támogatja a megfeleltetéseket egyetlen nyilvános port több magánjellegű portokat és lenne a fenti konfiguráció szükséges.

-  **Több példányon Weblogic kiszolgáló virtuális gépen.** Attól függően, hogy a üzembe követelményeknek Ha elég nagy a virtuális gép érdemes WebLogic kiszolgáló több példánya fut ugyanazon a gépen virtuális, lehetőséget. Például Közepes méret virtuális gépen, amely a két grafikon tartalmazza, amelyet sikerült választva WebLogic kiszolgáló két példánya futtatható. Ne feledje, hogy továbbra is javasoljuk, hogy ne hiba egyetlen pontjai bemutatása a architektúrában vonatkozik arra az esetre, ha csak egy virtuális gép WebLogic kiszolgáló több példányával futtató használt be. Legalább két virtuális gépeken futó használatával lehet jobb megoldást jelenthet, és minden egyes e virtuális gépeken futó futtatására WebLogic kiszolgáló több példánya. Minden esetben WebLogic kiszolgáló talán még mindig ugyanazt a fürt része. Megjegyzés:, azonban azt jelenleg nem használható Azure terhelést végpontok keresztül ilyen WebLogic Server-telepítések belül ugyanazon virtuális gép, mivel az Azure terheléselosztó szükséges, a terheléselosztás kiszolgálók között egyedi virtuális gépeken futó felosztandó.

##<a name="oracle-jdk-virtual-machine-images"></a>Az Oracle JDK virtuális gép képek

-  **JDK 6 és 7 legújabb frissítéseket.** Azt javasoljuk, hogy Java (jelenleg Java 8), a legújabb nyilvános, a támogatott verzióját használja, miközben Azure is elérhetővé teszi JDK 6 és 7 képek. Ez szól, amelyek nem készen áll a verziófrissítésre JDK 8 régebbi alkalmazások. Miközben frissítések előző JDK képekre előfordulhat, hogy már nem érhetők el a nyilvánosság, a Microsoft partnerség Oracle, az adott az JDK 6 és 7 képek Azure által biztosított célja, hogy egy újabb nem nyilvános frissítés ajánlott általában az Oracle által kiválasztott csoportjára Oracle támogatott ügyfelek tartalmaz. Új verzió JDK képre lesz elérhető frissített verzióitól JDK 6, 7 és az idő múlásával.

    A használható ez JDK 6 és 7 képeket, és a virtuális gépeken futó és belőlük származtatott képek JDK csak akkor használható, Azure belül.

-  **64 bites JDK.** Az Oracle-kiszolgáló WebLogic virtuális gép képek és az Oracle JDK virtuális gép képek Azure által biztosított tartalmazzák a Windows Server és a a JDK 64 bites verziója.

##<a name="additional-resources"></a>További források
[Az Oracle Azure virtuális gép képe](virtual-machines-linux-classic-oracle-images.md)
