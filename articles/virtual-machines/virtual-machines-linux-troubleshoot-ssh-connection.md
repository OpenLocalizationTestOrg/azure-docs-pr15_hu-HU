<properties
    pageTitle="Egy virtuális SSH csatlakozási problémák elhárítása |} Microsoft Azure"
    description="Hogyan kapcsolatos problémák, például a "Nem sikerült SSH kapcsolat" vagy 'SSH kapcsolat elutasítva' az Azure virtuális Linux operációs rendszert futtató."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Az Azure Linux virtuális, amely nem sikerül, hibákat, vagy elutasítják SSH kapcsolatok – problémamegoldás
Okból különböző tapasztal, biztonságos rendszerhéj (SSH) hibák, SSH csatlakozási hibák, vagy a SSH elutasítják, amikor Linux virtuális géphez (virtuális) csatlakozzon. Ez a cikk segít megtalálni, és a hibák kijavításának. Kapcsolódási problémák elhárítására Azure CLI vagy Linux virtuális Access bővítményének Azure portálon is használhatja.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet [az MSDN Azure és fórumok Papírhalom túlcsordulás](http://azure.microsoft.com/support/forums/)Azure szakértői. Másik lehetőségként a be Azure támogatási kérelmeiket is fájl. Nyissa meg az [Azure támogatja a webhelyet](http://azure.microsoft.com/support/options/) , és válassza a **technikai támogatás kérése**. Támogatja az Azure használatával kapcsolatos információkért olvassa el a [Microsoft Azure támogatja a gyakori kérdések](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Rövid hibaelhárítási lépések
Minden hibaelhárítási lépés után próbálja meg, a virtuális való csatlakozáshoz.

1. A SSH konfiguráció visszaállítása.
2. Állítsa alaphelyzetbe a felhasználó hitelesítő adatait.
3. Ellenőrizze, hogy a [Hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) szabályokat SSH forgalom engedélyezése.
    - Győződjön meg arról, hogy létezik-e egy hálózati biztonsági csoport szabály SSH forgalom engedélyezéséhez (Ez alapértelmezés szerint 22-es TCP-port).
    - Nem használható a port átirányításának / hozzárendelés az Azure terheléselosztó használata nélkül.
4. A [virtuális erőforrás állapot](../resource-health/resource-health-overview.md)ellenőrzése 
    - Győződjön meg arról, hogy a virtuális, hogy a megfelelő jelentéseket.
    - Ha indítási diagnosztika engedélyezve van, ellenőrizze a virtuális nem jelez a naplókat indítási hibáinak.
5. Indítsa újra a virtuális.
6. A virtuális újratelepítése.

Továbbra is részletesebb hibaelhárítási lépések és magyarázatokat az olvasóablakban.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Rendelkezésre álló módszerek SSH kapcsolódási problémák elhárítása

Visszaállíthatja a hitelesítő adatok vagy SSH konfigurációs az alábbi módszerek egyikével:

- [Azure portál](#using-the-azure-portal) - remek, ha módosítani szeretné, hogy gyorsan alaphelyzetbe állítása az SSH konfiguráció vagy SSH billentyűt, és nincs telepítve az Azure eszközök.
- [Azure CLI parancsok](#using-the-azure-cli) – Ha már a parancssorban gyorsan állítsa alaphelyzetbe a SSH konfigurációs vagy a hitelesítő adatokat.
- [Azure VMAccessForLinux bővítmény](#using-the-vmaccess-extension) - létrehozása, és újra felhasználhatja a json-definíciós fájlok SSH konfigurációs vagy a felhasználó hitelesítő adatok alaphelyzetbe állítása.

Minden hibaelhárítási lépés után próbálja a virtuális újra. Ha még mindig nem tud csatlakozni, próbálkozzon a következő lépéssel.


## <a name="using-the-azure-portal"></a>Az Azure portál használatával
Az Azure portál gyorsan bármilyen eszközök telepítése a helyi számítógépen nélkül a SSH konfigurációs vagy a felhasználó hitelesítő adatok alaphelyzetbe állítása.

Jelölje ki a virtuális az Azure-portálon. Görgessen le a **támogatási + hibaelhárítás** szakaszt, és jelölje be a **jelszó alaphelyzetbe állítása** az alábbi példának megfelelően:

![SSH konfigurációs vagy az Azure-portálon hitelesítő adatok alaphelyzetbe állítása](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>A SSH konfiguráció visszaállítása
Első lépésként válassza ki a `Reset SSH configuration only` a **mód** legördülő menü, ahogy a fenti képernyőképet, majd kattintson az **Alaphelyzet** gombra. Ez a művelet befejezése után próbálja meg a virtuális ismételt eléréséhez.

### <a name="reset-ssh-credentials-for-a-user"></a>A felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
Hozzon létre egy új meglévő felhasználó hitelesítő adatait, jelölje be `Reset SSH public key` vagy `Reset password` ahogy a fenti képernyőképet **mód** legördülő menüből. Adja meg a felhasználónév és SSH billentyűt, vagy új jelszót, majd kattintson az **Alaphelyzet** gombra.

A felhasználó a virtuális ebben a menüben a sudo jogosultságokkal rendelkező is létrehozhat. Adjon meg egy új felhasználónév és az ahhoz tartozó jelszót vagy a SSH billentyűt, és kattintson az **Alaphelyzet** gombra.


## <a name="using-the-azure-cli"></a>Az Azure CLI használatával
Ha még nem tette meg, [telepítse az Azure CLI, és csatlakoztassa az Azure-előfizetésébe](../xplat-cli-install.md). Győződjön meg arról, hogy, erőforrás-kezelő üzemmód használata az alábbi képlettel történik:

```
azure config mode arm
```

Ha a feltöltés egyéni Linux lemez kép, ellenőrizze, hogy a [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) 2.0.5 verziója, vagy újabb verziójának telepítése. A gyűjtemény képek használatával létrehozott VMs a hozzáférés-bővítmény már telepített és beállítja.

### <a name="reset-ssh-configuration"></a>SSH konfigurációs alaphelyzetbe állítása
A SSHD konfiguráció magát nincs megfelelően konfigurálva, vagy a szolgáltatás hibába ütközött. SSHD, hogy érvényes a SSH konfigurációja magát, visszaállíthatja. SSHD visszaállítására, az Ön által első hibaelhárítási lépést kell lennie.

A következő példa alaphelyzetbe állítása meg egy virtuális nevű SSHD `myVM` az erőforráscsoport nevű `myResourceGroup`. Használja a saját virtuális és az erőforrás neve az alábbiak szerint:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>A felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
SSHD megjelenik a helyesen fog működni, ha, előállító felhasználó jelszavának alaphelyzetbe. Az alábbi példa a hitelesítő adatok alaphelyzetbe állítása `myUsername` a megadott értékkel `myPassword`, kattintson a virtuális nevű `myVM` a `myResourceGroup`. A kívánt értékét használja az alábbiak szerint:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Ha SSH kulcsú hitelesítést használ, akkor kérhet új a SSH billentyűt az adott felhasználó számára. A következő példa frissíti a tárolt SSH billentyűt `~/.ssh/azure_id_rsa.pub` a felhasználó nevű `myUsername`, kattintson a virtuális nevű `myVM` a `myResourceGroup`. A kívánt értékét használja az alábbiak szerint:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>A VMAccess bővítmény használatával
A virtuális Access kiterjesztése Linux felolvassa json fájlban, amely definiálja műveletek elvégzéséhez. Az alábbi műveletek SSHD visszaállítására, alaphelyzetbe állítása egy SSH billentyűt vagy a felhasználó hozzáadásának tartalmazzák. Továbbra is használhatja az Azure CLI hívja fel a VMAccess bővítmény, de, így újból felhasználhatja a json-fájlokat különböző több VMs ha szükségesnek látja. Ezt a megközelítést lehetővé teszi, hogy hozzon létre egy json fájlokat, majd hívható az adott felhasználási területei tárházba.

### <a name="reset-sshd"></a>SSHD alaphelyzetbe állítása
Hozzon létre egy nevű fájlt `PrivateConf.json` a következő tartalommal:

```bash
{  
    "reset_ssh":"True"
}
```

Az Azure CLI használ, majd hívja a `VMAccessForLinux` kiterjesztése a SSHD kapcsolat alaphelyzetbe json fájljait megadásával. A következő példa alaphelyzetbe állítása meg a virtuális nevű SSHD `myVM` a `myResourceGroup`. A kívánt értékét használja az alábbiak szerint:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>A felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
Ha SSHD jelenik meg megfelelően működjön, átállíthatja előállító felhasználó hitelesítő adatait. Felhasználó jelszavának alaphelyzetbe állítása, hozzon létre egy nevű fájlt `PrivateConf.json`. Az alábbi példa a hitelesítő adatok alaphelyzetbe állítása `myUsername` a megadott értékkel `myPassword`. Írja be a következő sort a `PrivateConf.json` fájl saját értékei alapján:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Hozzon létre egy új felhasználó a SSH billentyűjét, először hozzon létre egy nevű fájlt vagy `PrivateConf.json`. Az alábbi példa a hitelesítő adatok alaphelyzetbe állítása `myUsername` a megadott értékkel `myPassword`, kattintson a virtuális nevű `myVM` a `myResourceGroup`. Írja be a következő sort a `PrivateConf.json` fájl saját értékei alapján:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

A json-fájl létrehozása után az Azure CLI segítségével hívja fel a `VMAccessForLinux` kiterjesztése a SSH felhasználói hitelesítő adatok alaphelyzetbe json fájljait megadásával. Az alábbi példa a virtuális nevű a hitelesítő adatok alaphelyzetbe állítása `myVM` a `myResourceGroup`. A kívánt értékét használja az alábbiak szerint:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Indítsa újra a virtuális
Ha a SSH konfigurálása és a felhasználó hitelesítő adatok alaphelyzetbe állítása, vagy ezzel hibába ütközött, próbálja meg újraindítani a virtuális alapjául szolgáló számítási problémák címére.

### <a name="azure-portal"></a>Azure portál
Indítsa újra az Azure portálon egy virtuális, jelölje ki a virtuális, és kattintson a ***Indítsa újra a** gombra a következő példának megfelelően:

![Indítsa újra a virtuális az Azure-portálon](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Az alábbi példa a virtuális nevű újraindul `myVM` az erőforráscsoport nevű `myResourceGroup`. A kívánt értékét használja az alábbiak szerint:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>A virtuális újratelepítése
Egy másik csomópontra belül Azure, előfordulhat, hogy javítsa a mögöttes hálózati hibát tapasztal, amely egy virtuális is telepítsen újra. Egy virtuális újbóli tudni olvassa el a [virtuális gép új Azure csomópontra telepítsen újra](virtual-machines-windows-redeploy-to-new-node.md)című témakört.

> [AZURE.NOTE] Ez a művelet befejezése után rövid életű lemez adatok elvesznek, és dinamikus IP-címek a virtuális gép társított frissülnek.

### <a name="azure-portal"></a>Azure portál
Telepítsen újra egy virtuális az Azure portálon, jelölje be a virtuális, és görgessen lefelé a **támogatási + hibaelhárítás** szakaszig. A **telepítsen újra** gombra a következő példának megfelelően:

![Telepítsen újra egy virtuális az Azure-portálon](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Az alábbi példa a virtuális nevű redeploys `myVM` az erőforráscsoport nevű `myResourceGroup`. A kívánt értékét használja az alábbiak szerint:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>A klasszikus telepítési modell használatával létrehozott VMs

Próbálja meg ezeket a lépéseket a klasszikus telepítési modell használatával létrehozott VMs az a leggyakoribb SSH csatlakozási hibák megoldására. Minden egyes lépés után próbálja meg, a virtuális való csatlakozáshoz.

- Állítsa alaphelyzetbe az [Azure portál](https://portal.azure.com)távelérési. Az Azure portálon jelölje ki a virtuális, és kattintson az **Alaphelyzet távoli...** gombra.

- Indítsa újra a virtuális. Az [Azure portál](https://portal.azure.com)jelölje ki a virtuális, és kattintson a **Indítsa újra a** gombra.

    – VAGY –

    Az [Azure klasszikus portál](https://manage.windowsazure.com), jelölje ki a **virtuális gépeken futó** > **példányok** > **Indítsa újra**.

- Telepítsen újra a virtuális új Azure csomópontra. Információt arról, hogy miként egy virtuális újratelepítése című témakörben talál [telepítsen újra virtuális gép új Azure csomópontra](virtual-machines-windows-redeploy-to-new-node.md).

    Ez a művelet befejezése után rövid életű lemez adatok elvesznek, és dinamikus IP-címek a virtuális gép társított frissülnek.

- Kövesse a [jelszó vagy Linux-alapú virtuális gépekhez SSH alaphelyzetbe állítása](virtual-machines-linux-classic-reset-access.md) :
    - Állítsa alaphelyzetbe a jelszó vagy a SSH billentyűt.
    - _Sudo_ felhasználói fiók létrehozása.
    - A SSH konfiguráció visszaállítása.

- Jelölje be a virtuális erőforrás állapot-platform problémák megoldásához.<br>
  Jelölje ki a virtuális, és görgessen lefelé a **Beállítások** > **Állapot ellenőrzése**.


## <a name="additional-resources"></a>További források

- Ha Ön SSH sem tud a virtuális gép után lépések végrehajtása után, című témakörben [részletesebb hibaelhárításhoz keres útmutatást](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) , tekintse át a további lépéseket a probléma megoldásához.

- Többet szeretne tudni az alkalmazás elérésével kapcsolatos problémák elhárítása olvassa el a [Hibaelhárítás access-Azure virtuális gépen futó alkalmazás](virtual-machines-linux-troubleshoot-app-connection.md) című témakört.

- További információt a virtuális gépeken futó a klasszikus telepítési modell használatával létrehozott hibaelhárítási megtudhatja, [hogy miként állítsa alaphelyzetbe a jelszó vagy Linux-alapú virtuális gépekhez SSH](virtual-machines-linux-classic-reset-access.md).
