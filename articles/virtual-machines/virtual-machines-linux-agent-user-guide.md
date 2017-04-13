<properties 
    pageTitle="Linux ügynök útmutatója |} Microsoft Azure" 
    description="Megtudhatja, hogy miként telepítheti, állíthatja Linux ügynök (waagent) kezelése a virtuális gép interakció Azure háló vezérlő." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure Linux ügynök útmutatója

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>– Bevezetés

A Microsoft Azure Linux ügynök (waagent) kezeli a Linux és kiépítési FreeBSD és az Azure háló vezérlő virtuális interakció. Biztosít a következő funkciók Linux és FreeBSD IaaS telepítések:

> [AZURE.NOTE] További információt az Azure Linux ügynök [– Fontos fájl](https://github.com/Azure/WALinuxAgent/blob/master/README.md) megtekintése

* **Kép kiépítése**
  - A felhasználói fiók létrehozása
  - SSH hitelesítéstípusok konfigurálása
  - Nyilvános kulcs SSH és a fő pár
  - Az állomásnév beállítása
  - Az állomásnév közzététele a DNS platform
  - Jelentéskészítési SSH host kulcs ujjlenyomat a platformra
  - Erőforrások lemez kezelése
  - Formázás és a lemez csatlakoztatása
  - Felcserélése hely beállítása

* **Hálózati**
  - Platform Serveren kompatibilitásának javítására útvonalak kezelése
  - A stabilitás, a hálózati kapcsolat neve biztosítja

* **Kernel**
  - Konfigurálja a virtuális NUMA (kernel letiltása < 2.6.37)
  - A Hyper-V entrópiájának az /dev/random fogyasztása
  - A legfelső szintű eszköz (amely lehet távoli) SCSI időtúllépései konfigurálása

* **Diagnosztikai**
  - A soros porthoz konzol átirányítása

* **SCVMM telepítések**
    - Észleli és a VMM agent Linux betöltéséhez, System Center virtuális gép Manager 2012 R2 környezetben fut

* **Virtuális bővítmény**
    - Linux virtuális (IaaS) ahhoz, hogy szoftvert, és a konfigurációs automatizálási a Microsoft és a partnerek által létrehozott összetevő behelyezése
    - Virtuális bővítmény hivatkozás végrehajtása a [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Kommunikáció
A információáramlás a platform a ügynökének két csatornákon keresztül fordul elő:

* Egy rendszerindításkor IaaS telepítésekhez csatolt a DVD-t. A DVD-t egy OVF-kompatibilis fájl, amely tartalmazza a tényleges SSH keypairs kívül az összes létesítési információkat tartalmazza.
* A TCP-végpont közzéteszi REST API-val használt telepítési és topológiájának konfigurálása juthat.


## <a name="requirements"></a>Követelmények
Az alábbi rendszerek tesztelte és tudjuk, hogy az Azure Linux ügynök használata:

> [AZURE.NOTE] Ez a lista eltérhetnek a a Microsoft Azure Platform támogatott rendszerek hivatalos listáját itt leírt módon: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Piros kalap vállalati Linux 6,7 +
* Debian 7.0-s +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Az Oracle Linux 6.4 +

Egyéb támogatott rendszerek:

* FreeBSD 10 vagy újabb (Azure Linux ügynök v2.0.10 +)

A Linux agent annak érdekében, hogy a megfelelő működéséhez néhány rendszer csomag függ:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Formázáshoz segédprogramok: sfdisk, fdisk, mkfs, válogatottak
* Jelszó eszközök: chpasswd, sudo
* Szöveg eszközök feldolgozása: sed, grep
* Hálózati eszközök: ip-továbbítására
* Kernel támogatása UDF filesystems csatlakoztatása.

## <a name="installation"></a>Telepítés
Egy RPM, illetve a DEB csomagot használ a terjesztési csomag tárházba a másik telepítés a telepítés és verziófrissítés az Azure Linux ügynök előnyben részesített metódusát. Az összes a [terjesztési szolgáltatók záradékkal](virtual-machines-linux-endorsed-distros.md) integrálása az Azure Linux ügynök csomag tárházakban, valamint a képek.

Olvassa el az [Azure Linux ügynök repó Github a](https://github.com/Azure/WALinuxAgent) speciális telepítési beállítások, például forrásból, vagy az egyéni helyekre vagy prefixumokban a dokumentáció.


## <a name="command-line-options"></a>Parancssori kapcsolók

### <a name="flags"></a>Különböző adatcsoportokat megjelenítő alakzatok

- részletes: növelése a megadott parancs
- kötelező: interaktív megerősítés egyes parancsok kihagyása

### <a name="commands"></a>Parancsok

- Súgó: felsorolja a támogatott parancsok és a jelölők.

- deprovision: próbálja meg a rendszerből, és ellenőrizze újra kiépítési alkalmas. Ez a művelet hagyni a következőket:
 * Az összes SSH host kulcs (ha Provisioning.RegenerateSshHostKeyPair a konfigurációs fájl "i")
 * /Etc/resolv.conf NameServer konfiguráció
 * Legfelső szintű jelszóra /etc/shadow (ha Provisioning.DeleteRootPassword a konfigurációs fájl "i")
 * Gyorsítótárazott DHCP ügyfélcímbérleteinek
 * A localhost.localdomain alaphelyzetbe állítása állomásnév


> [AZURE.WARNING] Megszüntetés nem garantálja az, hogy a kép-e bejelölve az összes bizalmas információkat, és a megfelelő további terjesztés céljából.


- deprovision + felhasználói: hajtja végre a-(fenti) deprovision minden is törli az utolsó kiépített fiók (/var/lib/waagent nyert), és kapcsolódó adatokat. Ezt a paramétert kiépítésekor vonja vissza, amelyek korábban Azure a kiépítési volt, akkor előfordulhat, hogy rögzítse és újra felhasználni.

- verzió: waagent verziójának megjelenítése

- serialconsole: LÁRVAJÁRAT szeretne megjelölni ttyS0 állítja be (az első soros portot) szerint az indítási konzol. Ez biztosítja, hogy a kernel indítási naplók a soros port küldött és elérhetővé tett hibakeresése során.

- démon: waagent futtathat egy szolgáltatás kezelése a platform interakció. Ezt az argumentumot megadja a waagent init parancsfájl waagent.

- Indítsa el: waagent futtatása a háttérben folyamat


## <a name="configuration"></a>Konfiguráció

Konfigurációs fájl (/ etc/waagent.conf) tevékenységéből fakadó waagent szabályozza. A minta konfigurációs fájl nézhet ki:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

A különböző beállítási lehetőségek témakörben olvashat részletesen. Beállítási lehetőségek állnak a következő három típusba; Logikai érték, karakterláncot vagy egész szám. A logikai konfigurációs beállítások "i" vagy "m" adható meg. A "Nincs" használhatók egyes karakterlánc típusú konfigurációs bejegyzés részletes, az alábbi speciális kulcsszó.

**Provisioning.Enabled:**  
Írja be: logikai  
Alapértelmezett: y

A felhasználó engedélyezése vagy letiltása a agent a kiépítési funkciókat. Érvényes értékei "i" vagy "m". Kiépítési le van tiltva, ha a kép SSH host és a felhasználó billentyűk megőrződnek, és a kiépítési API Azure-ban a megadott beállításokat a rendszer figyelmen kívül hagyja.

> [AZURE.NOTE] A `Provisioning.Enabled` Ubuntu felhő képek kiépítési felhő nicializálás használó "m" a paraméterek alapértelmezés szerint.

  
**Provisioning.DeleteRootPassword:**  
Írja be: logikai  
Alapértelmezett: n

Ha beállított, a legfelső szintű jelszót és egyéb elemek/árnyék fájl törlése a kiépítési folyamat során.


**Provisioning.RegenerateSshHostKeyPair:**  
Írja be: logikai  
Alapértelmezett: y

Ha a beállítás, az összes SSH host kulcs pár (ecdsa, dsa és rsa) a kiépítési során a/etc/ssh/törlődnek. És friss kulcs két jön létre.

A friss főbb pár titkosításának típusát a Provisioning.SshHostKeyPairType bejegyzés konfigurálható. Felhívjuk a figyelmét arra, hogy bizonyos terjesztését hozza létre SSH kulcs párban bármely hiányzó titkosítási típusú, a SSH démon (például biztonsági újraindításkor) újraindítása után.


**Provisioning.SshHostKeyPairType:**  
Írja be: karakterlánc  
Alapértelmezett: rsa

Ez a SSH démon a virtuális gépen által támogatott titkosítási algoritmust típusba állítható be. A szokásos támogatott értékek "rsa", "dsa" és "ecdsa". Figyelje meg, hogy a Windows "putty.exe" nem támogatja a "ecdsa". Igen ha putty.exe használható Windows rendszeren Linux telepítés csatlakozni kíván, használja az "rsa" vagy "dsa".


**Provisioning.MonitorHostName:**  
Írja be: logikai  
Alapértelmezett: y

Ha beállításához waagent ellenőrzi a Linux virtuális gép hostname (állomásnév) módosítások (mint például a "hostname" parancs által visszaadott) és automatikusan frissíti a hálózati konfigurációja a kép módosítása megfelelően. Annak érdekében, hogy a leküldéses nevének módosítása a DNS-kiszolgálók, hálózati újraindul a virtuális gépen. Ekkor röviden internetkapcsolat elvesztése.


**Provisioning.DecodeCustomData**  
Írja be: logikai  
Alapértelmezett: n

Ha beállításához waagent meg fog dekódolását Base64 a CustomData.


**Provisioning.ExecuteCustomData**  
Írja be: logikai  
Alapértelmezett: n

Ha beállításához waagent hajtja végre CustomData kiépítése után.


**Provisioning.PasswordCryptId**  
Karakterlánc típusú:  
Alapértelmezett: 6

Jelszó kivonat létrehozásakor használja a titkosítási algoritmust.  
 1 – MD5  
 2a - Blowfish  
 5 – SHA-256  
 6 – SHA-512  


**Provisioning.PasswordCryptSaltLength**  
Karakterlánc típusú:  
Alapértelmezés: 10

Jelszó kivonat létrehozásakor használatos véletlen só hosszát.


**ResourceDisk.Format:**  
Írja be: logikai  
Alapértelmezett: y

Ha beállításához a platform erőforrás szállított fog formázható, és ha a fájlrendszer "ResourceDisk.Filesystem" a felhasználó által igényelt bármi eltérő "ntfs" waagent csatlakoztatva. Egy olyan partíciót Linux (83) típusú lesz elérhető a lemezen. Figyelje meg, hogy ezt a partíciót fog formázása nem lesz, ha sikeresen csatlakoztatva.


**ResourceDisk.Filesystem:**  
Írja be: karakterlánc  
Alapértelmezett: ext4

Ez a fájlrendszer típusát a lemez határozza meg. Támogatott értékek Linux terjesztési típusától függően változnak. Ha a karakterlánc X, majd mkfs. X kell lennie a Linux képre. Képek SLES 11 általában kell használni a "ext3". Képek FreeBSD "ufs2" Itt kell használni.


**ResourceDisk.MountPoint:**  
Írja be: karakterlánc  
Alapértelmezés: / mnt/erőforrás 

Ez a adja meg az elérési utat, amelynél az erőforrás lemez csatlakoztatva van. Ne feledje, hogy az erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük.


**ResourceDisk.MountOptions**  
Írja be: karakterlánc  
Alapértelmezett: nincs

Adja meg a lemez csatlakozási beállítások átadni a csatlakozási -o parancsot. Utólagos az értékeket vesszővel tagolt listáját. "nodev, nosuid". Lásd: További információ a mount(8).


**ResourceDisk.EnableSwap:**  
Írja be: logikai  
Alapértelmezett: n

Ha beállításához felcserélése fájl (/ lapozófájl) az erőforrás lemezen létrejön, és a rendszer felcserélése adhatja hozzá.


**ResourceDisk.SwapSizeMB:**  
Írja be: egész szám  
Alapértelmezett: 0

Az ideiglenes fájl megabájtban mérete.


**Logs.Verbose:**  
Írja be: logikai  
Alapértelmezett: n

Ha beállított, a naplófájl részletességi csillapítja van. Waagent /var/log/waagent.log jelentkezik, és a rendszer logrotate funkciókat, ha el szeretné forgatni a naplók használja.


**OPERÁCIÓS RENDSZER. EnableRDMA**  
Írja be: logikai  
Alapértelmezett: n

Ha beállításához a agent megpróbálja telepítése, és majd betölteni egy RDMA kernel-illesztőprogram megegyezzen a belső vezérlőprogram az alapul szolgáló hardveren.


**OPERÁCIÓS RENDSZER. RootDeviceScsiTimeout:**  
Írja be: egész szám  
Alapértelmezett: 300

Ez a SCSI időtúllépés a OS lemez és az adatok meghajtókon másodpercben állítja be. Ha nincs beállítva, a rendszer használt alapértelmezett.


**OPERÁCIÓS RENDSZER. OpensslPath:**  
Írja be: karakterlánc  
Alapértelmezett: nincs

Ez egy alternatív útvonalon, az a titkosítási műveletek használható bináris openssl megadását is használható.


**HttpProxy.Host, HttpProxy.Port**  
Írja be: karakterlánc  
Alapértelmezett: nincs

Ha beállításához a ügynök használni fog a proxykiszolgáló csatlakozik az internetre. 


## <a name="ubuntu-cloud-images"></a>Ubuntu felhő képek

Figyelje meg, hogy Ubuntu felhő képek csatlakozást [felhő nicializálás](https://launchpad.net/ubuntu/+source/cloud-init) számos, az Azure Linux ügynök ellenkező esetben kezelt konfigurációs feladatok elvégzéséhez.  Kérjük, vegye figyelembe az alábbi különbségek:


- "M", hogy kiépítési feladatok végrehajtásához használja a felhőben nicializálás Ubuntu felhő képek **Provisioning.Enabled** alapértelmezés szerint.

- Az alábbi konfigurációs paramétereket Ubuntu felhő képek kezelése a lemez és térköz felcserélése felhő nicializálás használó nincs hatással van:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Tanulmányozza az alábbi forrásokban a erőforrás lemezre csatlakozási pont és térköz Ubuntu felhő képek felcserélése kiépítési során:

 - [Ubuntu Wiki: Felcserélése partíciók konfigurálása](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Hogy egyéni adatok az Azure virtuális gépen](virtual-machines-windows-classic-inject-custom-data.md)

