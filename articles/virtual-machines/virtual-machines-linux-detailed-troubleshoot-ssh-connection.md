<properties
    pageTitle="Részletes SSH kapcsolatos problémák megoldása az Azure virtuális |} Microsoft Azure"
    description="Részletesebb SSH hibaelhárítási lépéseket az Azure virtuális gép problémák"
    keywords="ssh kapcsolatot visszautasították, ssh hiba, azure ssh, SSH létrehozott kapcsolat"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Részletes SSH hibaelhárítási lépések

Nincsenek számos oka lehet, hogy a SSH ügyfél előfordulhat, hogy nem tudja elérni a virtuális a SSH szolgáltatást. Ha már elvégezte a további [Általános SSH hibaelhárítási lépéseket](virtual-machines-linux-troubleshoot-ssh-connection.md), további kapcsolatos hibák elhárítása a csatlakozási problémát szüksége. Ez a cikk részletes hibaelhárítási lépésekkel megállapíthatja, ahol a SSH kapcsolatot az Adatkapcsolat és megoldásáról végigvezeti Önt.

## <a name="take-preliminary-steps"></a>Első lépések készítése

Az alábbi ábra mutatja a kapcsolódó összetevőket.

![SSH szolgáltatás összetevői bemutató diagram](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Az alábbi lépéseket segítségével azonosítani a hiba forrását, és tudni, a megoldások és megoldásokat alkalmazhatja.

Első lépésként a portálon a virtuális állapotának ellenőrzése.

Az [Azure portálon](https://portal.azure.com):

1. A klasszikus telepítési modell használatával létrehozott VMs, válassza a **Tallózás** > **virtuális gépeken futó (klasszikus)** > *virtuális nevét*.

    – VAGY –

    Az erőforrás-kezelő modell használatával létrehozott VMs, válassza a **Tallózás** > **virtuális gépeken futó** > *virtuális nevét*.

    Az állapot ablakban a virtuális meg kell jelennie **operációs rendszert futtató**. Görgessen le a számítási, a tárhely és a hálózati erőforrások megjelenítése a legutóbbi tevékenység.

2. Kattintson a **Beállítások** vizsgálja meg a végpontok, IP-címek és más beállításokat.

    Erőforrás-kezelő használatával létrehozott VMs lévő azonosítása, győződjön meg arról, hogy egy [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) meg van adva. Is ellenőrizze, hogy a szabályok a hálózati biztonsági csoport alkalmazott, hogy azok által hivatkozott az alhálózathoz.

Az [Azure klasszikus portált](https://manage.windowsazure.com)a klasszikus telepítési modell használatával létrehozott VMs esetében:

1. Jelölje ki a **virtuális gépeken futó** > *virtuális nevét*.
2. Válassza ki a virtuális **Irányítópult** állapotának ellenőrzéséhez.
3. Jelölje ki a **Monitor** számítási, a tárhely és a hálózati erőforrások legutóbbi tevékenység megjelenítéséhez.
4. Jelölje ki a **Végpontok** arról, hogy van-e SSH forgalmához zárólap.

Ha ellenőrizni szeretné a hálózati kapcsolat, ellenőrizze a beállított végpontok és ha a virtuális egy másik, például a http- vagy más szolgáltatás protokollon keresztül érhető el.

Miután ezeket a lépéseket próbálkozzon a SSH ismételt kapcsolódási kísérlethez.


## <a name="find-the-source-of-the-issue"></a>Keresse meg a problémát forrása

A számítógépen a SSH ügyfél elérje a SSH szolgáltatást a Azure virtuális problémák vagy a következő felderítésére miatt előfordulhat, hogy nem:

- [SSH ügyfélszámítógépen](#source-1-ssh-client-computer)
- [Szervezeti él eszköz](#source-2-organization-edge-device)
- [A szolgáltatás végpontjának felhő és hozzáférés vezérlőelem-lista (vezérlés)](#source-3-cloud-service-endpoint-and-acl)
- [Hálózati biztonsági csoportok](#source-4-network-security-groups)
- [Azure virtuális Linux-alapú](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Forrás 1: SSH ügyfélszámítógépen

A számítógép kizárható a hiba forrását, győződjön meg arról, hogy azt fel, hogy egy másik a helyszíni Linux-alapú számítógépen SSH-kapcsolatot.

![Diagram, mely bemutatja az SSH ügyfél számítógép-összetevők](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Ha nem sikerül a kapcsolatot, győződjön meg a számítógépen a következőket:

- A beállítás, amely helyi tűzfal blokkolja a bejövő és kimenő SSH adatforgalom (TCP 22)
- A proxykiszolgáló ügyfélszoftver, ami miatt SSH kapcsolatok helyileg telepített
- Figyelés, ami miatt SSH kapcsolatok szoftver hálózati helyileg telepített
- Biztonsági szoftver más típusú figyelheti a forgalmat, és engedélyezheti/tilthatja, adott típusú forgalom

Ha e feltételek valamelyike vonatkozik, ideiglenesen tiltsa le a szoftvert, majd próbálkozzon SSH forrásból származó helyszíni számítógépre megtudhatja, hogy az az oka, a kapcsolat legyen blokkolva a számítógépen. Kattintson a hálózati rendszergazdához kell javítani a szoftver beállítások SSH kapcsolatok engedélyezésére dolgozhat.

Hitelesítés használatakor ellenőrizheti, hogy ezeket az engedélyeket a .ssh mappába otthoni címtárában:

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/\*.pub
- Chmod 600 ~/.ssh/id_rsa (vagy bármely más fájlokhoz tárolja őket, a titkos kulcsokat)
- Chmod 644 ~/.ssh/known_hosts (tartalmaz, amely a via SSH kapcsolt hosts)

## <a name="source-2-organization-edge-device"></a>Forrás 2: Szervezet él eszköz

A szervezet él eszköz kizárható a hiba forrását, győződjön meg arról, hogy a számítógép, amely közvetlenül csatlakozik az internethez SSH kapcsolatok végezhet az Azure virtuális. Ha egy webhely VPN vagy a készült Azure ExpressRoute származó érik el a virtuális, ugorjon [forrás 4: a biztonsági csoportok hálózati](#nsg).

![Diagram, mely bemutatja a szervezet él eszköz](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Ha nincs telepítve a számítógépen, amely közvetlenül csatlakozik az internethez, hozzon létre egy új Azure virtuális saját erőforráscsoport vagy szolgáltatás a felhő és használhatja azt. További tudnivalókért olvassa el a [Létrehozás virtuális gépen futó Linux Azure-ban](virtual-machines-linux-quick-create-cli.md)című témakört. Amikor befejezte a tesztelés erőforráscsoport vagy virtuális és felhőalapú szolgáltatást törléséhez.

SSH internetkapcsolattal rendelkező számítógépen közvetlenül csatlakozik az internethez hozhat létre, ha jelölje be a szervezet él eszköz:

- Egy belső tűzfala blokkolja a SSH forgalom az interneten
- A proxykiszolgáló, ami miatt SSH kapcsolatok
- Behatolási észlelési vagy hálózati megakadályozza, hogy a kapcsolatok SSH él hálózata eszközökön futó szoftver figyelése

Dolgozhat a hálózati rendszergazda kijavításához engedélyezze az Internet SSH forgalmat a szervezet él eszközök beállításait.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Forrás 3: Felhőalapú szolgáltatás végpontjának és vezérlés

> [AZURE.NOTE] A forrás csak a klasszikus telepítési modell használatával létrehozott VMs vonatkozik. Az erőforrás-kezelő használatával létrehozott VMs, ugorjon a [4 forrás: a biztonsági csoportok hálózati](#nsg).

A felhőalapú szolgáltatás végpontjának és vezérlés kizárható a hiba forrását, győződjön meg arról, hogy egy másik Azure virtuális virtuális ugyanabba a hálózatba SSH kapcsolatok végezhet a virtuális.

![Diagram, mely bemutatja a felhőbe szolgáltatás végpontjának és vezérlés](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Ha nem egy másik virtuális virtuális ugyanabba a hálózatba, egyszerűen hozhat létre egy újat. További információ a [egy használata a CLI Azure virtuális Linux gép létrehozása](virtual-machines-linux-quick-create-cli.md)című témakör tartalmaz. Amikor befejezte a tesztelés, törölje a felesleges virtuális.

Ha egy SSH kapcsolatot hozhat létre egy virtuális virtuális ugyanabba a hálózatba, ellenőrizze az alábbiakat:

- **A SSH forgalmat a cél virtuális végpont konfigurációját.** A magánjellegű portot végpontjának meg kell egyeznie a olyan portot, amelyet a SSH szolgáltatást a virtuális figyel. (Az alapértelmezett portja 22). Az erőforrás-kezelő telepítési modell használatával létrehozott VMs, ellenőrizze az Azure-portálon SSH TCP portszámot **Tallózás gombra**kattintva > **virtuális gépeken futó (v2)** > *virtuális neve* > **Beállítások** > **Végpontok**.

- **A hozzáférés-Vezérlési a cél virtuális gépen SSH forgalom végpontot.** Egy vezérlés lehetővé teszi, hogy adja meg engedélyezni vagy tiltani a bejövő forgalom az internetről, a forrás IP-címének alapján. Helytelenül vannak beállítva hozzáférés-vezérlési listák megakadályozhatja, hogy a végpontra bejövő SSH forgalmat. Jelölje be a hozzáférés-vezérlési listák annak érdekében, hogy érkező forgalmat a nyilvános IP-címét a proxykiszolgáló vagy más biztonsági kiszolgálójának engedélyezve van. További tudnivalókért olvassa el a [hálózati hozzáférés (-szabályozási listák)](../virtual-network/virtual-networks-acl.md)című témakört.

A végpont letiltjuk adatforrásként a problémát, távolítsa el az aktuális végpontot, hozzon létre egy új végpontot, és adja meg a SSH nevét (TCP portot 22 a nyilvános és titkos portszámot). További tudnivalókért olvassa el a [az Azure virtuális gépen végpont beállítása](virtual-machines-windows-classic-setup-endpoints.md)című témakört.

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Forrás 4: Hálózati biztonsági csoportok

Hálózati biztonsági csoportok lehetővé teszi engedélyezett bejövő és kimenő forgalmának pontosabban szabályozhatja. Időtartomány alhálózat és a felhőszolgáltatásokba az Azure virtuális hálózat szabályok hozhat létre. Jelölje be a hálózati biztonsági csoport szabályok annak érdekében, hogy engedélyezett-e SSH forgalom az internetről.
További információ című témakörben [olvashat a hálózati biztonsági csoportokat](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Forrás 5: Azure virtuális Linux-alapú számítógépen

Lehetséges problémák utolsó forrásának az Azure virtuális gép magát.

![Diagram, mely bemutatja az Azure virtuális Linux-alapú számítógépen](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Ha még nem tette meg, kövesse az utasításokat [alaphelyzetbe a jelszó vagy Linux-alapú virtuális gépekhez SSH](virtual-machines-linux-classic-reset-access.md).

Próbáljon újra a számítógépről. Ha továbbra is sikertelen, az alábbiakban a lehetséges problémák némelyike esetén:

- A SSH szolgáltatás nem fut a cél virtuális gépen.
- A SSH szolgáltatás nem figyel 22-es TCP-portot. Ennek ellenőrzéséhez telnet ügyfél telepítése a helyi számítógépen, és futtassa "telnet *cloudServiceName*. cloudapp.net 22-es". Ez határozza meg, ha a virtuális gép lehetővé teszi a bejövő és kimenő kommunikáció az SSH végpontot.
- A helyi tűzfal a cél virtuális gépen bejövő és kimenő SSH adatforgalom meggátolják szabályokat tartalmaz.
- Behatolási észlelési vagy hálózati figyelése a Azure virtuális gépen futó szoftver SSH kapcsolatok miatt.


## <a name="additional-resources"></a>További források
Többet szeretne tudni az alkalmazás elérésével kapcsolatos problémák elhárítása olvassa el a [Hibaelhárítás access-Azure virtuális gépen futó alkalmazás](virtual-machines-linux-troubleshoot-app-connection.md) című témakört.