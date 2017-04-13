<properties
   pageTitle="Hozzon létre, és töltse fel az FreeBSD virtuális képet |} Microsoft Azure"
   description="Ismerje meg, hozhat létre, és töltse fel a virtuális merevlemez (virtuális), amely tartalmazza az FreeBSD operációs rendszer egy Azure virtuális gép létrehozása"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Hozzon létre, és töltse fel a FreeBSD virtuális Azure

Ez a cikk bemutatja, hogyan hozhat létre, és töltse fel a virtuális merevlemez (virtuális), amely tartalmazza az FreeBSD operációs rendszer. Töltse fel, miután vele, a saját kép egy virtuális gép (virtuális) létrehozása az Azure-ban.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy rendelkezik-e az alábbiakat:

- **Az Azure-előfizetés**– Ha nem rendelkeznek fiókkal, létrehozhat egyet a mindössze néhány perc. Ha az MSDN előfizetéssel rendelkezik, olvassa el a [Visual Studio előfizetők számára havi Azure hitelképesség.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Megtudhatja, hogyan hozhat [létre egy ingyenes próba-fiókkal](https://azure.microsoft.com/pricing/free-trial/).  

- **Azure PowerShell eszközök**– az Azure PowerShell modul kell telepítenie és Azure-előfizetése segítségével van konfigurálva. A modul letöltésére, olvassa el a [Azure letöltések](https://azure.microsoft.com/downloads/)című témakört. Egy oktatóprogram ismerteti, hogy hogyan telepítse és állítsa be a modulba érhető el az alábbi. Az [Azure letöltések](https://azure.microsoft.com/downloads/) parancsmag használatával a virtuális feltöltése.

- Virtuális merevlemez **.vhd fájlban FreeBSD operációs rendszer**– A támogatott FreeBSD operációs rendszert kell telepíthető. Több eszközök létezik .vhd fájlok létrehozásához. Ha például hozza létre a .vhd fájlt, és telepítse az operációs rendszer virtualizációs megoldást, például a Hyper-V szolgáltatást úgy is használhatja. Arról, hogy miként szeretné telepíteni és használni a Hyper-V című témakör nyújt tájékoztatást [telepítése a Hyper-V, és hozzon létre egy virtuális gép](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Az új VHDX formátumot Azure-ban nem támogatott. A lemez konvertálhatók virtuális formázása, ha a Hyper-V-kezelő vagy a parancsmag [Konvertálás-virtuális](https://technet.microsoft.com/library/hh848454.aspx)segítségével. Ezenkívül van [oktatóanyaga a Hyper - v FreeBSD használatáról az MSDN webhelyen](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Ezt a feladatot a következő lépésekből öt.

## <a name="step-1-prepare-the-image-for-upload"></a>Lépés: 1: A kép előkészítése a feltöltés

A virtuális gépen, amelyen telepítette az FreeBSD operációs rendszer hajtsa végre az alábbi eljárások valamelyikét:

1. Engedélyezze a DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Engedélyezze a SSH.

    SSH alapértelmezés szerint engedélyezve van a telepítés lemezről után. Ha valamilyen oknál fogva nincs engedélyezve, vagy ha FreeBSD virtuális használnia közvetlenül, írja be a következőt:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Állítsa be a soros konzolt.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Telepítse a sudo.

    A legfelső szintű fiók le van tiltva, Azure-ban. Ez azt jelenti, hogy kell sudo egy jogosultságok parancsai futtathatók magasabb szintű jogosultságokkal rendelkező felhasználó a csatlakozást.

        # pkg install sudo
;
5. Azure ügynök előfeltételei.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Telepítse az Azure ügynök.

    A legújabb verzióját az Azure ügynök [github](https://github.com/Azure/WALinuxAgent/releases)mindig megtalálható. A 2.0.10 + hivatalos FreeBSD 10 támogatja és 10.1, és a változat 2.1.4 hivatalos támogatja FreeBSD 10.2 és a későbbi kiadásokban.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    A 2.0-s használata 2.0.16 példaként:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1-es használata 2.1.4 példaként:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Miután telepítette az Azure ügynök, célszerű ellenőrizni, hogy fut:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. A rendszer deprovision.

    A rendszer jelenjenek meg, és ellenőrizze újra kiépítési alkalmas deprovision. A következő parancs is törli az utolsó kiépített felhasználói fiókot, és a kapcsolódó adatok:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Most, állítsa le a virtuális.

## <a name="step-2-create-a-storage-account-in-azure"></a>Lépés: 2: Tárterület-fiók létrehozása az Azure-ban ##

.Vhd fájl feltöltése, így hozhat létre virtuális gép használható Azure-ban a tárterület-fiókra van szüksége. Az Azure klasszikus portal segítségével tárterület-fiók létrehozása.

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).

2. A parancssávon válassza az **Új**.

3. Válassza a **Data Services** > **tároló** > **gyors létrehozásához**.

    ![Gyors tárterület-fiók létrehozása](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Töltse ki a mezők az alábbi képlettel történik:

    - Az **URL-címe** mezőbe írjon be egy altartomány nevet tároló fiók URL-címét. A szöveg a 3-24 számokat és a kisbetűket is tartalmazhat. Ez a név lesz az állomásnév belül, amely a cím Azure Blob-tárolóhoz, Azure várólista-tároló vagy tároló-erőforrások Azure-táblából az előfizetéshez tartozó URL-CÍMÉT.

    - **Hely/affinitás csoport** legördülő menüben válassza a **hely vagy affinitás csoport** a tárterület-fiókom. Az azonos adatközpont helyezi el a felhőszolgáltatások és a tárhely affinitás alkalmazás segítségével.

    - A **replikáció** mezőben, hogy a **Geo felesleges** replikációs a tárterület-fiókhoz használt-e. A replikáció GEO alapértelmezés szerint be van kapcsolva. Ez a beállítás, hogy a tárhely átvétele erre a helyre a fő hiba esetén a fő tartózkodási helyén replikálja egy másodlagos helyre, Önnek, áttérhet az adatok. A másodlagos helyre automatikusan hozzá van rendelve, és nem módosíthatók. Ha további beállítási lehetőségekre jogi követelményeknek vagy szervezeti házirend miatt a felhőalapú tárolási helyét, kikapcsolhatja a replikáció geo. Vigyázzon, hogy később mégis aktiválja a replikáció geo, ha meg fog kell díjat egyszeri adatok átadás való replikáció a meglévő adatok a másodlagos helyre. Tárterület-szolgáltatások geo-varianciaanalízis ismétlések nélkül árengedménnyel ajánlja fel. Tárterület-fiókok geo replikációs kezelésével kapcsolatban további részleteket megtalálható itt: [jelentések létrehozása, kezelése, és a tárhely fiók törlése](../storage-create-storage-account/#replication-options).

    ![Írja be a tárhely számla részletei](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Válassza a **tárterület-fiók létrehozása**. A fiók megjelenik a **tárhely**.

    ![Tárterület-fiókot sikeresen létre](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Ezután hozzon létre egy tároló feltöltött .vhd fájlok. Jelölje ki a tárhely fiók nevét, és válassza a **tárolók**.

    ![Tárterület számla részletei](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Jelölje ki **a tároló létrehozása**.

    ![Tárterület számla részletei](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. A **név** mezőbe írja be a tároló nevét. Az **Access** legördülő menüjében válassza milyen típusú hozzáférési házirendet szeretne.

    ![Tároló neve](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Alapértelmezés szerint a tároló privát, és csak a fiók tulajdonos is elérhető. Szeretné, hogy a tárolóban a BLOB, de nem a tároló tulajdonságainak és a metaadat-alapú nyilvános olvasási hozzáférést, adja meg a **Nyilvános Blob** beállítást. Szeretné, hogy a tároló és BLOB teljes körű nyilvános olvasási hozzáférést, adja meg a **Nyilvános tároló** beállítást.

## <a name="step-3-prepare-the-connection-to-azure"></a>3 lépés: A kapcsolat Azure előkészítése

Mielőtt .vhd fájlokat feltöltheti, kell a számítógép és az Azure előfizetés közötti biztonságos kapcsolatot létesíteni. Ehhez az Azure Active Directory (Azure Active Directory) módszer vagy a tanúsítvány módszer használható.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Az Azure Active Directory módszert .vhd fájl feltöltése

1. Nyissa meg az Azure PowerShell konzolt.

2. Írja be a következő parancsot:  
    `Add-AzureAccount`

    Ez a parancs megnyitja a bejelentkezési ablak jelentkezhetnek, ahol be a munkahelyi vagy iskolai fiókjával.

    ![A PowerShell-ablak](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure hitelesíti, és menti a hitelesítő adatokat. Kattintson az ablak bezárása.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>A tanúsítvány módszert .vhd fájl feltöltése

1. Nyissa meg az Azure PowerShell konzolt.

2. A Type mezőben  `Get-AzurePublishSettingsFile`.

3. Böngészőablakban nyílik meg, és kéri, hogy letölti-e egy .publishsettings fájlt. A fájl tartalmaz információkat és Azure előfizetéshez tartozó tanúsítványt.

    ![Böngésző letöltési lapjára](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Mentse a .publishsettings fájlt.

4. A Type mezőben  `Import-AzurePublishSettingsFile <PathToFile>`, ahol `<PathToFile>` van a fájl teljes elérési útját a .publishsettings.

   További tudnivalókért olvassa el a [Azure parancsmagok – első lépések](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx)című témakört.

   További információt a való telepítéséről és konfigurálásáról PowerShell megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Lépés: 4: A .vhd fájl feltöltése

A .vhd fájl feltöltésekor is elhelyezhet, tetszőleges Blob-tárolóhoz belül. Az alábbiakban néhány kifejezéseket kell használni, amikor a fájl feltöltése:
-  **BlobStorageURL** a tárterület-fiókot, a 2 létrehozott URL-CÍMÉT.
-  **YourImagesFolder** , amelyre a képeket tárolja a tároló Blob-tárolóhoz belül.
- **VHDName** az Azure klasszikus portálon azonosítja a virtuális merevlemez megjelenő címkét.
- **PathToVHDFile** , a teljes elérési útját és nevét a .vhd fájlt.


Az ablakból Azure Powershellt használta az előző lépésben írja be:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Lépés 5: Hozzon létre egy virtuális a feltöltött .vhd fájl
A .vhd fájl feltöltésekor felveheti képként egyéni a képeket, amelyek társítva az előfizetéshez, és hozzon létre egy virtuális gép ezen a képen egyéni listáját.

1. Az ablakból Azure Powershellt használta az előző lépésben írja be:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Az operációs rendszer típusa Linux használja. Az aktuális Azure PowerShell-verzió paraméterként csak "Linux" vagy "Windows" fogadja el.

2. A fenti lépések elvégzése után az új kép szerepel, ha úgy dönt, hogy az Azure klasszikus portálon **képek** fülre.  

    ![Válassza a kép](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Hozzon létre egy virtuális gép a gyűjteményből. Ezen a képen a **Saját kép**most érhető el.
4. Jelölje be az új kép. Ezután hajtsa végre az utasításokat állíthat be egy állomásnév, jelszót, SSH billentyűt, és így tovább.

    ![Egyéni képe](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Miután a kiépítési, látni fogja az Azure-ban futó FreeBSD virtuális.

    ![Azure-ban FreeBSD képe](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
