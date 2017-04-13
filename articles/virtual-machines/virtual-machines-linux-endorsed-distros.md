<properties
    pageTitle="A Linux terjesztését záradékkal |} Microsoft Azure"
    description="Linux megismerheti a Azure záradékkal terjesztését irányelvek Ubuntu, OpenLogic, Oracle és SUSE is beleértve."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>A felosztott Azure záradékkal Linux

> [AZURE.NOTE] Ha egy darabig, kérjük, segítse a szolgáltatás javítható az Azure Linux virtuális át ezt a [rövid felmérés](https://aka.ms/linuxdocsurvey) a tapasztalok vételével. Minden válasz segít a munka elvégzéséhez kaphat segítséget.

Egy számot a partnerek által biztosított a piactér vagy Azure gyűjteményében a Linux képeket, és dolgozunk a még több változatok hozzáadása a záradékkal terjesztési lista különböző Linux Közösségek. Időközben terjesztését nem érhető el a gyűjteményből az bármikor Előrehozás-a-Öné-Linux [ezen](virtual-machines-linux-classic-create-upload-vhd.md)a lapon az útmutatásokat követve.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Támogatott terjesztését és verziók ##

Az alábbi táblázat a Linux terjesztését és Azure a támogatott. Is találhat további információt a [támogatása Linux képeket a Microsoft Azure-ban](https://support.microsoft.com/en-us/kb/2941892) .

A Hyper-V és Azure Linux Integration Services (LIS) illesztőprogramjainak olyan Microsoft közvetlenül a felsőbb szintű Linux kernel beleszámít kernel modulokat.  A LIS illesztőprogramok vagy beépített az eloszlás kernel alapértelmezés szerint, illetve a régebbi RHEL/CentOS-alapú terjesztését külön letölthető [Itt](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Olvassa el a [jelen cikk](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) további információt a LIS illesztőprogramok.

Az Azure Linux ügynök már előre telepítve van az Azure gyűjtemény képek és a terjesztési csomagot tárházba általában érhető el.  Forráskód [GitHub](https://github.com/azure/walinuxagent)megtalálható.

Ki.|Verzió|Illesztőprogramok|Ügynök
---|---|---|---
CentOS OpenLogic szerint | CentOS 6.3 + 7.0-s + | CentOS 6.3: [LIS letöltése](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: A Kernel | Csomag: [OpenLogic repó](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) "WALinuxAgent" csoportban a <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Rendszerhéj | Forráskód: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7.9 +, 8.2 + | Rendszerhéj | Csomag: a "waagent" repó a <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent)
Az Oracle Linux | 6.4 +, 7.0-s + | Rendszerhéj | Csomag: a "WALinuxAgent" repó a <br/>Forráskód: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Piros kalap vállalati Linux | RHEL 6,7 + 7.1 + | Rendszerhéj|Csomag: a "WALinuxAgent" repó a <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux vállalati | SLES 11 SP4, SLES 12 SP1 + és <p> Az SAP 11 SP3 + SLES | Rendszerhéj | Csomag: A "python azure-ügynök" [Felhő: eszközök](https://build.opensuse.org/project/show/Cloud:Tools) repó a <br/>Forráskód: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | Rendszerhéj | Csomag: A "python azure-ügynök" [Felhő: eszközök](https://build.opensuse.org/project/show/Cloud:Tools) repó a <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Rendszerhéj | Csomag: a "walinuxagent" repó a <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partnerek

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic a vállalati Megnyitás megoldások a felhőben, és az Adatközpont vezető szolgáltatóként. OpenLogic segít több száz vállalati vezető keresztül biztonságosan szerezheti be, támogatja és Megnyitás software szabályozása ágazatok széles köre. OpenLogic kereskedelmi minőségű technikai támogatást és OpenLogic szakértő Közösség, beleértve a vállalati szintű CentOS támogatása, valamint a folyamatban az indítási partner CentOS-alapú képek képzések a Azure által támogatott 600 Megnyitás csomagokhoz kártalanításokról kínál.

### <a name="coreos"></a>CoreOS
[https://CoreOS.com/docs/running-CoreOS/cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

A CoreOS webhelyről:

*Biztonság, egységesebb és megbízhatóságára CoreOS készült. Helyett csomagok vagy apt yum keresztül telepíti, CoreOS Linux tárolók kezelésére használ a szolgáltatások hardverabsztrakciós magasabb szinten. Egy egyetlen szolgáltatáskód és az összes függőségek csomagolása egy vagy több CoreOS gépeken futó tároló belül.*


### <a name="credativ"></a>Credativ
[http://www.credativ.Co.uk/credativ-blog/debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ egy független tanácsadási és a cég szolgáltatásainak csecsemőgondozásra a fejlesztés és profi megoldások való ingyenes szoftvert végrehajtását. A kezdő Megnyitás szakértő, mint Credative támogatási használatával számos informatikai részleghez nemzetközi felismerés tartalmaz. Microsoft együtt Credativ jelenleg elkészítés megfelelő Debian képek Debian 8 (Jessie) és Debian előtt 7 (Wheezy), amely Azure futtathatnak kifejezetten és könnyen kezelhető a platformon keresztül. Credativ is támogatni fogja a hosszú távú karbantartási és az Azure keresztül, a Megnyitás forrás támogatási erőforrások Debian képre frissítése.

### <a name="oracle"></a>Az Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle stratégia egy nyilvános és titkos felhőket megoldások széles portfoliónyi kínálatát közben, így ügyfelei a választási lehetőségek, hogyan azok Szoftvertelepítés Oracle Oracle felhőket, valamint egyéb felhőket rugalmasság.  A Microsoft Oracle partnerség lehetővé teszi, hogy a Microsoft nyilvános és titkos felhőket minősítési bizalommal az Oracle-szoftvert telepíthet, és az Oracle támogatja az ügyfelek számára.  Oracle lekötési és befektetés Oracle nyilvános és titkos felhő megoldásokban nem változik.

### <a name="red-hat"></a>Piros kalap
[http://www.redhat.com/en/partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

A világ vezető szolgáltató Megnyitás megoldást, piros kalap segít több, mint 90 % Fortune 500-vállalatnál oldja meg a vállalati problémáit, azok informatikai igazítása és üzleti stratégiák, és a Felkészülés a tervek technológia. Piros kalap végzi, mert a biztonságos megoldások át egy megnyitott üzleti modell és az elérhető, kiszámíthatóan előfizetés modell.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux vállalati Server Azure szolgáltatás egy igazolt platform, felettes megbízhatósága és biztonsági funkciói cloud számítások biztosító. SUSE a sokoldalú Linux platform zökkenőmentesen együttműködik a könnyen kezelhető felhőalapú környezetben előadásához Azure felhőszolgáltatásaihoz. És több mint 9,200 tanúsított alkalmazásokkal több 1,800 független gyártók SUSE Linux vállalati kiszolgáló SUSE biztosítja, hogy az az Adatközpont támogatott futó feladatok nyugodtan telepíthetők Azure.

### <a name="canonical"></a>Kanonikus
[http://www.ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanonikus mérnöki és az ügyfél, a kiszolgáló és a felhő számítások, beleértve a fogyasztói személyes cloud services nyitott Közösség irányítási meghajtó Ubuntu sikeres. Egy egyesített ingyenes platform Ubuntu, a felhőbe, egy család összefüggő felületek a telefonon, táblagépen, TV és az asztal, a telefonról kanonikus a látás teszi Ubuntu a fogyasztói elektronikai készítői és felvétele a Kedvencek közé között egyes technologists nyilvános felhő szolgáltatóktól különböző intézmények az első választás.

A fejlesztők és a világ mérnöki központok Canonical egyedileg elhelyezett partner hardver-készítők, tartalom szolgáltatók és szoftverfejlesztőknek ahhoz, hogy Ubuntu megoldások piaci, a kiszolgáló és a kézi eszközök PC-re.

