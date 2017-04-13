<properties
    pageTitle="Bevezetés Linux Azure-ban |} Microsoft Azure"
    description="Tudjon meg többet a Azure Linux virtuális gépeken futó."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>A Azure Linux – bevezetés

Ez a témakör áttekintést nyújt Azure felhőben Linux virtuális gépeken futó használata bizonyos szempontjait. Linux virtuális gép üzembe helyezése a következő egyszerű áll egy kép felhasználásával a gyűjteményből.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Hitelesítés: Felhasználónevét, a jelszavakat és a SSH kulcsok

Az Azure klasszikus portálon Linux virtuális gép létrehozásakor a rendszer kéri, hogy adja meg a felhasználónevét, a jelszó vagy a egy SSH nyilvános kulcs. A következő kényszer fizetnie van az üzembe helyezése a Azure virtuális Linux gép felhasználónév választható: rendszer fiókok nevét (UID < 100) már szerepel a virtuális gép nem használhatók, "legfelső szintű" például.


 - Lásd: a [virtuális gép Linux operációs rendszert futtató létrehozása](virtual-machines-linux-quick-create-cli.md)
 - Megismerkedhet [a Azure Linux SSH használatával](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Rendszeradminisztrátori jogosultságokkal használatával beszerzése`sudo`

A felhasználó a Azure virtuális gép példány a telepítés során megadott egy olyan jogosultsággal rendelkező fiók. Ehhez a fiókhoz az Azure Linux ügynök engedélyezni szeretné a legfelső szintű (rendszeradminisztrátori fiókot) használ szintű jogosultságokkal állítható be a `sudo` segédprogramot. Miután bejelentkezett, az a felhasználói fiókot használ, akkor fogja tudni parancsokat a parancs szintaxissal legfelső szintű

    # sudo <COMMAND>

Másik lehetőségként a legfelső szintű rendszerhéj **sudo -s**használatával szerezhet be.

- Lásd: [A legfelső szintű jogosultságok Linux virtuális gépeken Azure-ban](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>A tűzfal beállításainak

Azure biztosít a bejövő csomag szűrő, amely a kapcsolat korlátozza az Azure klasszikus portálon megadott portot. Alapértelmezés szerint a csak engedélyezett portja SSH. Előfordulhat, hogy megnyitható további portokat elérésének a Linux virtuális gépen végpontok konfigurálásával az Azure klasszikus portálon:

 - Lásd:, [hogy miként állíthat be egy virtuális gép végpontok](virtual-machines-windows-classic-setup-endpoints.md)

Az Azure gyűjtemény Linux képek alapértelmezés szerint nem engedélyezi a *iptables* tűzfal. Ha azt szeretné, a tűzfal beállítható a további szűrhetők.


## <a name="hostname-changes"></a>Hostname (állomásnév) módosítása

Amikor egy Linux képe egy példányának telepít host nevezze el a virtuális gép szükségesek. Ha a virtuális gépen fut, a hostname (állomásnév) a történő közzétételekor platform DNS-kiszolgálók, hogy több virtuális gépeken futó egymáshoz kapcsolódó hajthatják végre IP-cím keresések állomásnevekké használatával.

Hostname (állomásnév) módosítások virtuális gép telepítését követően van szükség esetén adjon a paranccsal

    # sudo hostname <newname>

Az Azure Linux ügynök a név megváltoztatása automatikus észlelése és megfelelően állítsa be a virtuális gép marad meg ezt a módosítást, és tegye közzé a módosítás platform DNS-kiszolgálók funkciókat tartalmaz.

 - [Azure Linux ügynök útmutatója](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Felhőalapú nicializálás
Képek **Ubuntu** és **CoreOS** felhő nicializálás Azure, amely egy virtuális gép rendszerindításáért további lehetőségeket biztosít a csatlakozást.

 - [Hogyan lehet egyéni adatok a beillesztendő](virtual-machines-windows-classic-inject-custom-data.md)
 - [Egyéni adatok és a felhő nicializálás a Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Azure felcserélése partíciók felhő nicializálás használatával létrehozása](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Azure CoreOS használatáról](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Virtuális gép kép rögzítése

Azure lehetővé teszi a rögzíthet egy meglévő virtuális gép állapotát képbe, ezt követően további virtuális gép példányok üzembe használható. Lehetséges, hogy az Azure Linux ügynök visszaállítás használt a testre szabott a kiépítési folyamat során végrehajtott részét. Előfordulhat, hogy hajtsa végre az alábbi lépéseket követve rögzíthet egy virtuális gép képként:

1. Futtatása **waagent-deprovision** kiépítési testreszabási visszavonásához. Vagy **waagent-deprovision + felhasználói** tetszés szerint a kiépítési és kapcsolódó adatok során megadott felhasználói fiók törlése.

2. Állítsa le/power ki a virtuális gépen.

3. Kattintson a *Rögzítés* az Azure klasszikus portálon, vagy a Powershell vagy CLI eszközök segítségével rögzítheti a virtuális gép képként.

 - Lásd: [hogyan sablonként használandó Linux virtuális gép rögzítése](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Feltüntetésével lemezen

Virtuális gépeken csatolt ideiglenes, helyi *erőforrás lemezre* tartalmaz. Egy erőforrás lemezen adatok nem feltétlenül tartós újraindul között, mert gyakran használt alkalmazások és a tranziens és **átmeneti** tárolására szolgáló adatok virtuális gépen futó folyamatok. Azt is tárolni a lapot, illetve az operációs rendszer fájlok cserélheti használja.

Linux az erőforrás lemez általában az Azure Linux ügynök kezeli, és automatikusan csatlakoztatott **/mnt/resource** (vagy **/mnt** Ubuntu képek).


>[AZURE.NOTE] Figyelje meg, hogy az erőforrás lemez **ideiglenes** lemezen, és előfordulhat, hogy is törlődik, és formázni, amikor a virtuális újraindítása után.

Linux az adatok lemez előfordulhat, hogy nevű, mint a kernel `/dev/sdc`, és a felhasználók kell partition, formázhatja, és csatlakoztassa az adott erőforrás. Ez az oktatóprogram lépésenkénti vonatkozik: [hogyan csatolhat virtuális géphez adatok lemezen](virtual-machines-linux-classic-attach-disk.md).

 - **Lásd még:** [A szoftver Linux RAID konfigurálása](virtual-machines-linux-configure-raid.md)  &  [Egy Linux virtuális Azure-ban a LVM konfigurálása](virtual-machines-linux-configure-lvm.md)

