<properties
    pageTitle="Telepítse az Azure parancssor |} Microsoft Azure"
    description="Telepítse az Azure parancssor CLI () a Mac, az Linux és a Windows Azure-szolgáltatások használatának megkezdése"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Telepítse az Azure CLI

> [AZURE.SELECTOR]
- [A PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

Gyorsan telepítse az Azure parancssori kezelőfelületről Azure létrehozásához és kezeléséhez, a Microsoft Azure erőforrások Megnyitás-forrás shell-alapú parancsok használ. Az platformok eszközök telepítése a számítógépen több lehetőség közül választhat: 

* **npm csomag** - npm (a JavaScript csomag vezetője) futtatásával telepítse a legújabb Azure CLI-csomagot OS vagy Linux terjesztési. Node.js és npm a számítógépen van szükség.
* **Installer** – letöltés egy egyszerű telepítéshez, Mac vagy Windows installer.
* **Docker tároló** – a legújabb CLI kattintásra kész Docker tárolóban használatának megkezdése. A számítógépen Docker host szükséges.
    
További lehetőségek és a háttér [GitHub](https://github.com/azure/azure-xplat-cli)a projekt tárházba látható. 

Az Azure CLI telepítette, [csatlakoztassa az Azure-előfizetéséhez](xplat-cli-connect.md) , és a parancsokat **azure** (Bash, terminált, parancssor és stb.) a parancssori felületről készült Azure erőforrásait.



## <a name="option-1-install-an-npm-package"></a>1 beállítást: Egy npm csomag telepítéséhez

Egy npm csomagot a CLI telepít, győződjön meg arról, letöltött és telepítve van a [legújabb Node.js és npm](https://nodejs.org/en/download/package-manager/). Ezután futtassa a **npm telepítse** az azure-cli csomag telepítését: 

    npm install -g azure-cli

A Linux terjesztését akkor előfordulhat, hogy **sudo** használja a sikeres futtatásához a __npm__ parancs az alábbi képlettel történik:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Ha telepítése vagy Node.js és npm-OS vagy Linux terjesztési kell azt javasoljuk, hogy telepítse a legújabb verzió Node.js LTS (4.x). Ha egy régebbi verzióját használja, a telepítési hiba jelenhet meg. 

Ha inkább, töltse le a legújabb Linux [tar fájl] [ linux-installer] a npm csomaghoz helyileg. Ezt követően telepítse a letöltött npm csomag az alábbiak szerint (a Linux terjesztését **sudo**használni kell):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>2 beállítás: Az installer használata

Ha a Mac vagy Windows rendszerű számítógépen használja, az alábbi CLI telepítőfájlokat letölthetők:

* [Mac OS X installer][mac-installer]

* [A Windows MSI][windows-installer] 

>[AZURE.TIP]A Windows rendszeren is letöltheti a [Webes Platform telepítő](https://go.microsoft.com/?linkid=9828653) a CLI telepítéséhez. Ez a telepítő felajánlja a CLI telepítését követően telepítse a Azure SDK csomagjában talál további és parancssori eszközöket. 


## <a name="option-3-use-a-docker-container"></a>Lehetőség 3: Docker tároló használata

Ha a számítógépen, amelyen a [Docker](https://docs.docker.com/engine/understanding-docker/) szolgáltató beállította a legújabb Azure CLI Docker tárolóban futtathatja. Futtassa a következő parancs (Linux terjesztését **sudo**használni kell):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Azure CLI parancsokat.
Az Azure CLI telepítése után a **azure** művelet végrehajtása a parancssori kezelőfelületen (Bash, terminált, parancssor és stb.) A Súgó parancsot, írja be például a következőket:

```
azure help
```
> [AZURE.NOTE]Néhány Linux terjesztését a hasonló hibaüzenet jelenhet meg `/usr/bin/env: ‘node’: No such file or directory`. Ez a hiba telepíti a /usr/bin/nodejs Node.js legutóbbi példányainak származik. Szolgáló fix it, ez a parancs futtatásával hozzon létre egy /usr/bin/node szimbolikus hivatkozás:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Ha látni szeretné az Azure CLI telepített verziója, írja be a következőt:

```
azure --version
```

Most már készen áll! A saját erőforrásokat [csatlakoztatása az Azure előfizetést az Azure CLI](xplat-cli-connect.md)CLI parancsok elérése.

>[AZURE.NOTE] Azure CLI első használatakor megjelenik egy üzenet engedélyezem a Microsoftnak használatát vonatkozó információk gyűjtését kér. Részvétel az önkéntes. Ha úgy dönt, hogy részt, akkor leállíthatja bármikor futtatásával `azure telemetry --disable`. Ahhoz, hogy a részvételt bármikor, futtatása `azure telemetry --enable`.


## <a name="update-the-cli"></a>A CLI frissítése

A Microsoft gyakran elengedi az Azure CLI frissített verzióját. Telepítse újra a használatával a telepítőprogram az operációs rendszerének CLI, vagy futtassa a legújabb Docker tároló. Vagy, ha a legújabb Node.js és npm telepítve van, frissítse a beírásával (a Linux terjesztését **sudo**használni kell) a következő.

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Kiegészítés engedélyezése

A Mac- és Linux CLI parancsok lap kiegészítésének támogatott.

Engedélyezze a zsh, futtatása:

```
echo '. <(azure --completion)' >> .zshrc
```

Engedélyezze a bash, futtatása:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Következő lépések 

* [Csatlakozás az Azure-előfizetéséhez a CLI](xplat-cli-connect.md) létrehozása és kezelése az Azure erőforrások.

* Ha többet szeretne megtudni az Azure CLI, forráskód, a problémák bejelentésére és szerkeszthetik azokat a projekt letöltéséhez keresse fel [az Azure CLI GitHub tárháza](https://github.com/azure/azure-xplat-cli).

* Ha az Azure CLI vagy Azure használatával kapcsolatban, látogasson el a [Azure-fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Ha azt szeretné, próbálja meg a Python-alapú [Azure CLI 2.0-s előzetes](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
