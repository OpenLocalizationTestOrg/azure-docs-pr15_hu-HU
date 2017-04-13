<properties
   pageTitle="Azure virtuális gépeken futó biztonsági áttekintése |} Microsoft Azure"
   description=" Azure virtuális gépeken futó ad a virtualization rugalmasan anélkül, hogy vásárol, és a tényleges hardver a virtuális gépen futó karbantartása.  Ez a cikk áttekintést nyújt az alapvető Azure biztonsági funkciók Azure virtuális gépeken futó használható. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Azure virtuális gépeken futó biztonsági – áttekintés

Azure virtuális gépeken futó lehetővé teszi a számítások megoldások Agilis módon széles köre telepítésében. Microsoft Windows, Linux rendszerhez, a Microsoft SQL Server, az Oracle, az IBM, SAP és Azure BizTalk szolgáltatások támogatása telepítheti a bármely terhelést és bármely nyelvre szinte bármilyen operációs rendszeren.

Az Azure virtuális gép a rugalmasságot biztosít a virtualization anélkül, hogy vásárol, és a tényleges hardver a virtuális gépen futó karbantartása.  Össze, és az, hogy az adatok-e a védett és a nagyon biztonságos adatközpont esetén megbízható garancia az alkalmazás telepítéséhez.

Az Azure vagy nagyobb biztonságot, megfelelt megoldások, amelyek:

- A virtuális gépeken futó megvédését a vírusokkal, és a kártevők
- A bizalmas adatokat titkosítása
- Biztonságos hálózati forgalmának engedélyezésére
- Azonosítása és veszélyek feltárása
- Megfelelőségi követelmények betartását

Ez a cikk az a célja, hogy az alapvető áttekintést nyújtanak Azure biztonsági funkciók virtuális gépeken futó használható. Azt is, amely cikkekre, többet is megtudhat, adja meg a szolgáltatások adatait.  

Az alapvető Azure virtuális gép biztonsági funkciók nem vonatkozik a jelen cikkben:

- Antimalware
- Hardveres biztonsági modult
- Virtuális gép lemez titkosítás:
- Virtuális gép biztonsági mentése
- Azure webhely helyreállítás
- Virtuális hálózatok
- Biztonsági házirendek kezelése és jelentéskészítés
- Megfelelőség

## <a name="antimalware"></a>Antimalware

Az Azure-antimalware szoftver biztonsági szállítóktól, például a Microsoft, Symantec, Trend Micro, McAfee és Kaspersky védelme a virtuális gépeken futó rosszindulatú fájlokat, adatokat és más kockázatok is használhatja. Lásd: a további tudnivalók szakaszhoz az alábbi cikkekben partneren megoldások megkeresésére.

Az Azure Felhőszolgáltatások és a virtuális gépeken futó Microsoft Antimalware, amely segít a azonosítása és vírusok, a kémprogramok és egyéb rosszindulatú szoftverek eltávolítása egy valós idejű védelem lehetőséget.  A Microsoft Antimalware konfigurálható riasztások ismert rosszindulatú vagy a nem kívánt szoftver telepítése magát, és az Azure rendszeren futtatásához megpróbálja biztosít.

A Microsoft Antimalware használata az egyetlen-agent megoldás, az alkalmazások és a bérlői környezetekben futtathatók a háttérben fut, az emberi beavatkozás nélkül. Az alkalmazás-munkaterhelésekből, vagy egyszerű biztonságos--alapértelmezés szerint az igényeinek megfelelően vagy egyéni konfigurációs, beleértve az ellenőrzési antimalware speciális védelem telepítheti.

Üzembe helyezéséhez és Microsoft Antimalware engedélyezése, ha az alábbi alapvető funkciók állnak rendelkezésre:

- Valós idejű védelem – Felhőszolgáltatások, majd a virtuális gépeken futó felderítése és blokkolása kártevőt végrehajtása a monitorok tevékenység.
- Ütemezett végzett vizsgálatot – rendszeresen hajt végre kártevőt, beleértve a aktívan futó programok feltárása célzott végzett vizsgálatot.
- Kártevőt remediation - automatikusan észlelt kártevő, például a törlés vagy rosszindulatú fájlokat quarantining rosszindulatú bejegyzéseket megtisztítása a hajt végre műveletet.
- Aláírás frissítések – automatikusan telepíti a legújabb védelem aláírások (vírus-definíciók) védelme érdekében naprakész-e a előre meghatározott gyakorisággal.
- Antimalware Engine automatikusan frissíti – a Microsoft Antimalware engine frissíti.
- Antimalware Platform automatikusan frissíti – a Microsoft Antimalware platform frissíti.
- Aktív védelem – észlelt veszélyek és gyanús erőforrásokról gyors választ biztosítása érdekében a Azure telemetriai metaadatok jelentést, és lehetővé teszi, hogy a szinkronizált aláírás valós idejű kézbesítési keresztül a Microsoft Active védelem rendszer (térképek).
- Mintát jelentése - biztosít, és a minták jelentések finomíthatja a szolgáltatást, és engedélyezze a hibaelhárítási segítséget a Microsoft Antimalware szolgáltatás.
- Kivételek – lehetővé teszi az alkalmazás és a szolgáltatás-rendszergazdák konfigurálása az egyes fájlokat, folyamatok, és az alábbi tevékenységeivel a kizárása védelme és a teljesítmény és más oka lehet végzett vizsgálatot.
- Antimalware esemény webhelycsoport - rögzíti a Szolgáltatásállapot antimalware, gyanús tevékenységek és az operációs rendszer eseménynaplójába remediation megtett és gyűjti össze őket, az ügyfél Azure tárterület-fiókba.

További információ: a virtuális gépeken futó védelme antimalware szoftverrel kapcsolatos további tudnivalókért lásd:

- [Az Azure Felhőszolgáltatások és a virtuális gépeken futó Microsoft Antimalware](../security/azure-security-antimalware.md)
- [Azure virtuális gépeken Antimalware megoldások telepítése](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Telepítse és állítsa be a Trend Micro-mély biztonsági kattintson egy Windows virtuális szolgáltatásként hogyan](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Telepítése és beállítása a Windows virtuális Symantec Endpoint Protection](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Új Antimalware beállításainak Azure virtuális gépeken futó – McAfee Endpoint Protection védelme](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [A Microsoft Azure piactéren található biztonsági megoldások](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardveres biztonsági modul

Titkosítási és a hitelesítési nem növelheti biztonsági kivéve, ha a saját maguk billentyűk-e védelemmel ellátva. Az adatkezelési és a kritikus titkos kulcsok és a billentyűk biztonsága egyszerűsítheti tárolja őket Azure kulcs tárolóból elemre. A tanúsítvánnyal FIPS 140-2 szintet 2 szabványok hardver (HSMs) biztonsági modulban kulcsok tárolására lehetőséget nyújt a kulcs tárolóból elemre. Az SQL Server titkosítási kulcsok biztonsági mentése vagy [átlátszó adatok titkosítása](https://msdn.microsoft.com/library/bb934049.aspx) összes tárolhatók kulcs tárolóból elemre a billentyűk vagy titkos kulcsok alkalmazásából. Engedélyek és hozzáférés e védett elemekhez [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)segítségével kezelhetők.

tudj meg többet:

- [Mi az Azure kulcs tárolóra?](../key-vault/key-vault-whatis.md)
- [Első lépések az Azure kulcs tárolóból elemre](../key-vault/key-vault-get-started.md)
- [Azure kulcs tárolóra-blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuális gép lemez titkosítás:

Azure lemez titkosítási egy új szolgáltatása, amellyel a Windows és Linux Azure virtuális gép merevlemezeken titkosítása. Azure lemez titkosítás a iparágban szabványos [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkció a Windows és a [dm – a titkosítási](https://en.wikipedia.org/wiki/Dm-crypt) szolgáltatás a Linux biztosítására használja az operációs rendszer és az adatok lemezt mennyiségi titkosítás.

A megoldás integrálva van segít határozzák meg, és kezelheti a lemez titkosítási kulcs és titkos kulcsok kulcs tárolóból elemre az előfizetésben, biztosítva, hogy a virtuális gép lemezen az összes adat titkosítva vannak a Azure-tárolóban lévő többi Azure kulcs tárolóból elemre.

tudj meg többet:

- [A Windows és Linux IaaS VMs Azure lemez titkosítás](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure lemez titkosítás Linux és a Windows virtuális gépeken futó](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [A virtuális gép titkosítása](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Virtuális gép biztonsági mentése

Azure biztonsági másolat védi az alkalmazás adatokat nulla tőkeberuházási és minimális működési költségek méretezhető megoldás. Alkalmazáshibák is sérült az adatok, és az emberi hibák hibák kiegészíteni a az alkalmazások. Azure mentéssel a virtuális gépeken futó Windows és Linux fut-e védelemmel ellátva.

tudj meg többet:

- [Mi az Azure biztonsági másolat?](../backup/backup-introduction-to-azure-backup.md)
- [Azure biztonsági tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure biztonsági szolgáltatás – gyakori kérdések](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure webhely helyreállítás

A szervezet BCDR stratégia fontos része tudja, hogy miként nyilvántartása vállalati munkaterhelésének és alkalmazások, és a tervezett és a tervezett kimaradások futnak. Azure webhely helyreállításhoz szükséges adatok téve a replikáció, feladatátvevő és helyreállítási munkaterhelésének és az alkalmazások az, hogy azok elérhetők egy másodlagos helyről Ha megszakad a fő tartózkodási helyén.

Webhely-helyreállítás:

- **A BCDR vonatkozó stratégia Simplifies** – webhely helyreállítási megkönnyíti a replikáció, feladatátvevő és helyreállítási több business munkaterhelésekből és alkalmazások egyetlen helyről kezelheti. Webhely helyreállítási orchestrates replikációs és feladatátvételi, de nem az alkalmazás adatok metsz vagy kell minden információt.
- **Rugalmas replikációs tartalmaz** – webhely helyreállítási használatával, hogy bizonyos a Hyper-V virtuális gépeken futó, VMware virtuális gépeken futó és Windows vagy Linux fizikai kiszolgálón futó feladatok.
- **Támogatja a feladatátvételt és helyreállítási** – webhely helyreállítási biztosít próba feladatátadás katasztrófa helyreállítási gyakorlatokat támogatása gyártási környezetekben megtartásával. A minimális adatvesztés (attól függően, hogy a replikáció gyakoriság) váratlan katasztrófák várható kimaradások nulla-adatok veszteséggel tervezett feladatátadás, vagy nem tervezett feladatátadás is futtatható. A feladatátvétel után visszaállás elsődleges webhelyekhez való is. Webhely-helyreállítás helyreállítási-előfizetésekhez is parancsfájlok és Azure automatizálási munkafüzetek, hogy testre szabhatja a feladatátvevő és helyreállítási a több szálon futó alkalmazások biztosít.
- **Másodlagos adatközponthoz szükségtelenné teszi** –, hogy bizonyos, a másodlagos a helyszíni webhely, illetve Azure. Azure használata a célként vészhelyreállítás megszünteti a költség és egy másodlagos webhely karbantartása komplexitását. Azure tároló replikált adatokat tárolja.
- **Meglévő BCDR technológiákkal lehetőségeinek integrálása** – webhely helyreállítási partnerek más alkalmazás BCDR funkcióival. Például SQL Server vissza végén vállalati munkaterhelésekből védelme webhely helyreállítási is használhatja. Ez támogatja natív SQL Server AlwaysOn rendelkezésre állási csoportok feladatátvételének kezeléséhez.

tudj meg többet:

- [Mi az Azure webhely helyreállítási?](../site-recovery/site-recovery-overview.md)
- [Hogyan működik az Azure webhely helyreállítási?](../site-recovery/site-recovery-components.md)
- [Mi Munkaterhelésekből eső Azure webhely helyreállítási?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuális hálózatok

Virtuális gépeken futó a hálózati kapcsolat szükséges. Ezt a követelményt támogatásához Azure virtuális gépeken futó Azure virtuális hálózathoz kapcsolódik van szükség. Az Azure virtuális hálózaton egy logikai szerkezetet, a fizikai Azure hálózati háló épülő. Az összes olyan logikai Azure virtuális hálózat legyen elkülönítve más Azure virtuális hálózatokon. A elkülönítési segítséget nyújt a biztosítására, hogy a forgalmat a központi telepítés nem érhető el egyéb Microsoft Azure-ügyfeleknek.

tudj meg többet:

- [Azure hálózati biztonsági – áttekintés](security-network-overview.md)
- [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)
- [Hálózati szolgáltatásait és partneri nagyvállalati verzió felhasználási területei](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Biztonsági házirendek kezelése és jelentéskészítés

Azure biztonság otthon segítségével megakadályozhatja, hogy, észleli és veszélyek válaszolni, és itt növeli a betekintést kap abba, és szabályozni kell, az Azure erőforrások biztonsága. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

Azure biztonság otthon segítségével optimalizálása, és figyelemmel követheti a virtuális gép biztonsági szerint:

- Virtuális gép [biztonsági javaslatok](../security-center/security-center-recommendations.md) például nyújtó frissítéseket a rendszer, állítsa be a hozzáférés-vezérlési listák végpontok, antimalware engedélyezése, hálózati biztonsági csoportok engedélyezése és alkalmazása lemez titkosítás.
- A virtuális gépeken futó állapotának ellenőrzése

tudj meg többet:

- [Azure biztonság otthon – bevezetés](../security-center/security-center-intro.md)
- [Azure biztonság otthon gyakran ismételt kérdések](../security-center/security-center-faq.md)
- [Azure biztonság otthon tervezése és műveletek](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Megfelelőség

Azure virtuális gépeken futó FISMA, FedRAMP, HIPAA, 1-es szintű PCI DSS és más kulcs megfelelőségi programok igazolt. A megfelelőség megkönnyíti a saját megfelelőségi követelmények betartását Azure alkalmazások és a vállalati verzió – belföldi és nemzetközi szabályozói követelmények széles köre címet.

tudj meg többet:

- [A Microsoft adatvédelmi központ: megfelelőség](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Megbízható felhő: Microsoft Azure biztonság, adatvédelem és megfelelőség](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
