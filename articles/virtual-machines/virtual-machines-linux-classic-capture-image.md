<properties
    pageTitle="Készítsen egy képet egy Linux virtuális gép |} Microsoft Azure"
    description="Megtudhatja, hogy miként Linux-alapú Azure virtuális gép (virtuális) a klasszikus telepítési modell készült képet."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Hogyan képként klasszikus Linux virtuális gép rögzítése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](virtual-machines-linux-capture-image.md).

Ez a cikk bemutatja, hogyan Linux képként létrehozása más virtuális gépeken futó operációs rendszert futtató klasszikus Azure virtuális gép rögzítéséhez. Ezen a képen az operációs rendszer lemez és a virtuális géphez csatlakoztatott adatok lemezt tartalmazza. Hálózati konfigurációja, akkor nem tartalmazza, úgy kell beállítania, hogy a kép virtuális gépeken futó létrehozásakor.

Azure együtt az Ön által feltöltött képet a **képek**, képeket tárolja. Képek kapcsolatos további tudnivalókért lásd: a [Kapcsolatos virtuális gép képek Azure-ban] [].

## <a name="before-you-begin"></a>Első lépések

Ezek a lépések feltételezik, hogy már már létrehozott egy Azure virtuális gép a klasszikus telepítési modell használata és az operációs rendszer, beleértve a bármely adatok lemezt csatolása beállítva. Ha hozzon létre egy virtuális van szüksége, olvassa el a [hogyan hozhat létre virtuális Linux géphez] [].


## <a name="capture-the-virtual-machine"></a>A virtuális gép rögzítése

1. [Kapcsolódás virtuális gépen](virtual-machines-linux-mac-create-ssh-keys.md) egy SSH ügyfél lehetőség használatával.

2. A SSH ablakban írja be a következő parancsot. Kimenetét `waagent` kissé eltérő lehet attól függően, hogy a segédprogram verziója:

    `sudo waagent -deprovision+user`

    Ez a parancs a rendszerből, és hogy csoporttagokkal tartott reprovisioning megpróbálja. Ez a művelet az alábbi műveleteket végzi el:

    - Eltávolítja a SSH host billentyűk (ha Provisioning.RegenerateSshHostKeyPair a konfigurációs fájl "i")
    - Törli az /etc/resolv.conf nameserver konfiguráció
    - Eltávolítja a `root` felhasználói jelszót és egyéb elemek/árnyék (ha Provisioning.DeleteRootPassword "i" a konfigurációs fájl)
    - Eltávolítja a gyorsítótárazott DHCP ügyfélcímbérleteinek
    - A localhost.localdomain alaphelyzetbe állítása állomásnév
    - Az utolsó kiépített felhasználói fiók (/var/lib/waagent nyert) **és a kapcsolódó adatok**törlése

    >[AZURE.NOTE] Megszüntetés törli a fájlokat és a "generalize" a képet az adatokat. Ez a parancs csak futtassa a rögzíteni az új kép sablonként kívánja virtuális gépen. Nem garantálja, hogy a kép összes bizalmas információkat nincs bejelölve, vagy harmadik fél terjesztés céljából alkalmas.


3. Írja be az **y** továbbra is. Hozzáadhat a `-force` paraméter elkerülése érdekében a megerősítést kérő ezt a lépést.

4. Írja be a **Kilépés** a SSH ügyfél bezárásához.

    >[AZURE.NOTE] A hátralévő lépések feltételezik, hogy már [telepítve](../xplat-cli-install.md) van az Azure CLI az ügyfélgépen beállított. Az alábbi lépéseket az [Azure klasszikus portálon] []is elvégezhető.

5. Az ügyfélszámítógép nyissa meg az Azure CLI és a bejelentkezési Azure-előfizetéséhez. További információ: olvassa el a [Csatlakozás az Azure CLI az Azure-előfizetésbe](../xplat-cli-connect.md).

6. Ügyeljen arra, hogy éppen Szolgáltatáskezelés módban:

    `azure config mode asm`

7. Állítsa le a virtuális gép, amely már leépítjük az előző lépéseket:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Láthatja, hogy az előfizetése használatával létrehozott virtuális gépek`azure vm list`

8. Amikor leállítja a virtuális gép, rögzítés a paranccsal a képet:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Írja be a kép neve nevet, amelyet _Új kép név_helyett. Ez a parancs létrehoz egy általános OS képe. A `-t` parancs törli az eredeti virtuális gépet.

9.  Az új kép érhető el a képek minden új virtuális gépeken futó konfigurálásához használható listáját. Ez a parancs tekintheti meg:

    `azure vm image list`

    Az [Azure klasszikus portál] []megjelenik a **képek** listában.

    ![A sikeres rögzítési képe](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Következő lépések
A kép készen áll a virtuális gépeken futó létrehozására szolgál. Az Azure CLI paranccsal `azure vm create` , és adja meg a létrehozott kép nevét. Kapcsolatos további tudnivalók: [az Azure CLI és klasszikus telepítési modellt használja](../virtual-machines-command-line-tools.md) a parancsot. Azt is megteheti az [Azure klasszikus portal] [] segítségével a **Gyűjtemény a** módszerrel, és válassza a kép létrehozott egyéni virtuális gép létrehozása. Megtudhatja, [hogy miként hozhat létre egy egyéni virtuális gép] [] további információt.

**Lásd még:** [Azure Linux ügynök útmutatója](virtual-machines-linux-agent-user-guide.md)

[Azure klasszikus portál]: http://manage.windowsazure.com
[Tudnivalók a virtuális gép képek Azure-ban]: virtual-machines-linux-classic-about-images.md
[Hogyan hozhat létre egy egyéni virtuális számítógépre]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Hogyan hozhat létre virtuális Linux gépen]: virtual-machines-linux-classic-create-custom.md
