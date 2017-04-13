<properties
   pageTitle="Az Azure tároló szolgáltatás fürt telepítése |} Microsoft Azure"
   description="Telepítse az Azure tároló szolgáltatás fürt az Azure-portálra, az Azure CLI vagy a PowerShell használatával."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Az Azure tároló szolgáltatás fürtre terjesztése

Azure tároló szolgáltatás megoldásokat a népszerű Megnyitás-forrás tároló gyors telepítésének fürtözés és üzletifolyamat-tervező. Azure tároló szolgáltatás használatával telepítheti Adatközpont/OS és Azure erőforrás-kezelő sablonokkal Docker Swarm fürt vagy az Azure-portálra. Azure virtuális gép skála beállítja a következő fürt rendszerbe, és a fürt Azure hálózati és tárolási ajánlataiban előnyeit. Azure tároló szolgáltatás eléréséhez szükséges egy Azure-előfizetést. Ha nincs telepítve egyik, majd jelentkezzen az [ingyenes próbaverziót](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

A dokumentum végigvezeti az Azure tároló szolgáltatás fürt telepítése az [Azure portál](#creating-a-service-using-the-azure-portal), az [Azure parancssori kezelőfelületről](#creating-a-service-using-the-azure-cli)és az [Azure PowerShell-modult](#creating-a-service-using-powershell)használatával.  

## <a name="create-a-service-by-using-the-azure-portal"></a>A szolgáltatás létrehozása az Azure portál használatával

Jelentkezzen be az Azure-portálra, és válassza az **Új** **Azure tároló szolgáltatás**keresése a Microsoft Azure piactéren.

![Telepítési 1 létrehozása](media/acs-portal1.png)  <br />

Jelölje ki az **Azure tároló szolgáltatás**, és kattintson a **Létrehozás**gombra.

![Telepítés 2 létrehozása](media/acs-portal2.png)  <br />

Írja be az alábbi adatokat:

- **Felhasználónév**: a felhasználó nevét a az egyes a virtuális gépeken futó fiókjához használt és virtuális gép skála az Azure tároló szolgáltatás fürt állítja be.
- **Előfizetés**: Jelölje ki azt az Azure előfizetést.
- **Erőforráscsoport**: jelölje be a meglévő erőforráscsoport, vagy hozzon létre egy újat.
- **Hely**: válassza ki az Azure régió az Azure tároló szolgáltatás telepítéshez.
- **Nyilvános kulcs SSH**: Adja hozzá a nyilvános kulcs ellen Azure tároló szolgáltatás virtuális gépeken futó hitelesítéshez használt. Nagyon fontos, hogy a következő kulcsot nincs sortörések tartalmazza, és hogy tartalmazza az "ssh-rsa" előtag és a 'username@domain' utólagos javítás. Azt hasonlóan kell kinéznie a következők: **ssh-rsa AAAAB3Nz... <> …... UcyupgH azureuser@linuxvm **. Biztonságos rendszerhéj (SSH) billentyűk létrehozásával kapcsolatban, olvassa el a [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) és a [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) cikkeket.

Ha készen áll a folytatáshoz kattintson az **OK gombra** .

![Telepítés 3 létrehozása](media/acs-portal3.png)  <br />

Jelölje ki az üzletifolyamat-tervező típusát. A beállítások a következők:

- **Adatközpont/operációs rendszer**: a Adatközpont/OS fürtre üzembe helyezése.
- **Swarm**: üzembe helyezése a Docker Swarm fürtre.

Ha készen áll a folytatáshoz kattintson az **OK gombra** .

![4-es telepítési létrehozása](media/acs-portal4.png)  <br />

Írja be az alábbi adatokat:

- **Diaminta megszámolása**: minták a fürt számát.
- **Ügynök száma**: az Docker Swarm, erre akkor ügynökök ügynök skála megadása a kezdeti száma. Ez az Adatközpont/OS, lesz ügynökök egy privát skála megadása a kezdeti száma. Ezenkívül egy nyilvános méretarányra beállított jön létre, anyagok előre megadott számot tartalmaz, amelyek. A nyilvános skála kategóriához anyagokkal száma szerint hány mesteralakzatok lettek létrehozva a fürt – egy fő egy nyilvános agent, és a két nyilvános anyagokkal három vagy öt mesteralakzatok határozza meg.
- **Ügynök virtuális gép méret**: a ügynök virtuális gépeken futó méretét.
- **DNS-előtag**: a szolgáltatás teljesen minősített tartománynév főbb elemeinek előtag használt nemzetközi egyedi nevet.

Ha készen áll a folytatáshoz kattintson az **OK gombra** .

![A telepítés 5 létrehozása](media/acs-portal5.png)  <br />

Szolgáltatás érvényességi befejeződése után kattintson az **OK gombra** .

![Telepítés 6 létrehozása](media/acs-portal6.png)  <br />

Kattintson a **Create** a telepítés megkezdéséhez.

![Telepítés 7 létrehozása](media/acs-portal7.png)  <br />

Ha korábban rögzítése az Azure-portálra a példányban van megadva, megjelenik a telepítési állapota.

![Telepítés 8 létrehozása](media/acs-portal8.png)  <br />

A telepítés befejezésekor a Azure tároló szolgáltatás fürt készen áll a használatra.

## <a name="create-a-service-by-using-the-azure-cli"></a>A szolgáltatás létrehozása az Azure CLI használatával

Használatával a parancssorban az Azure tároló szolgáltatás egy példánya létrehozásához szüksége van egy Azure-előfizetést. Ha nincs telepítve egyik, majd jelentkezzen az [ingyenes próbaverziót](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Is van szükség [telepítette](../xplat-cli-install.md) és [beállította](../xplat-cli-connect.md) az Azure CLI.

Egy Adatközpont/OS vagy Docker Swarm fürt üzembe helyezéséhez válasszon az alábbi sablonok közül GitHub. Ne feledje, hogy ezek a sablonok a azonos, alapértelmezett orchestrator kijelölést kivételével.

* [Adatközpont/OS sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Méhrajt sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Ezután győződjön meg arról, hogy az Azure CLI csatlakozott Azure-előfizetésbe. Ehhez használja a következő parancsot:

```bash
azure account show
```
Azure-fiók nem ad vissza, ha a következő parancs használatával jelentkezzen be a CLI az Azure.

```bash
azure login -u user@domain.com
```

Ezután konfigurálja az Azure CLI Azure erőforrás-kezelő eszközök.

```bash
azure config mode arm
```

Hozzon létre egy Azure erőforráscsoport és tároló szolgáltatás fürt a következő parancsot, ha:

- **RESOURCE_GROUP** a csoport nevét, az erőforrás, amelyet használni ehhez a szolgáltatáshoz.
- **Hely** az Azure régió, ahol az erőforráscsoport és Azure tároló szolgáltatás telepítési létrejön egy.
- **TEMPLATE_URI** a telepítési fájl helyét. Figyelje meg, hogy ez a nyers fájlt, a felhasználói felület GitHub mutató nem kell lennie. Keresse meg az URL-cím, jelölje ki a azuredeploy.json fájlt GitHub, és kattintson a **nyers** gombra.

> [AZURE.NOTE] Ez a parancs futtatásakor a rendszerhéj kérni fogja telepítési paraméterértékeket.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Adja meg a sablon paramétereket

Ez a parancs verziója megköveteli a paraméterek interaktív módon. Ha paramétereket, például a JSON-formátumú karakterláncként adja meg szeretne ehhez használatával a `-p` válthat. Példa:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Másik lehetőségként biztosít a paraméterek JSON-formátumú fájl használatával a `-e` válthat:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Egy példa-paraméterek nevű fájl megtekintéséhez `azuredeploy.parameters.json`, keresse meg azt a GitHub Azure tároló szolgáltatás sablonokkal.

## <a name="create-a-service-by-using-powershell"></a>Szolgáltatás hozzon létre a PowerShell használatával

A PowerShell-Azure tároló szolgáltatás fürt is telepítheti. A dokumentum 1.0 verziójú alapuló [Azure PowerShell-modult](https://azure.microsoft.com/blog/azps-1-0/).

Egy Adatközpont/OS vagy Docker Swarm fürt üzembe helyezéséhez jelölje be az alábbi sablonok közül. Ne feledje, hogy ezek a sablonok a azonos, alapértelmezett orchestrator kijelölést kivételével.

* [Adatközpont/OS sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Méhrajt sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Fürt létrehozása az Azure-előfizetése, előtt ellenőrizze, hogy a PowerShell-munkamenetet már bejelentkezett az Azure. Az ezt megteheti a `Get-AzureRMSubscription` parancsot:

```powershell
Get-AzureRmSubscription
```

Ha Azure bejelentkezni, használja a `Login-AzureRMAccount` parancsot:

```powershell
Login-AzureRmAccount
```

Ha új erőforráscsoport esetén telepítése, először létre kell hoznia az erőforráscsoport. Hozzon létre egy új erőforráscsoport, használja a `New-AzureRmResourceGroup` parancsot, és adjon meg egy erőforrás csoport nevét és a cél régiót:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Miután létrehozott egy erőforráscsoport, a következő parancsot a fürt hozhat létre. A kívánt sablon URI adható a `-TemplateUri` paraméter. Ez a parancs futtatásakor PowerShell kérni fogja telepítési paraméterértékeket.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Adja meg a sablon paramétereket

Ha ismeri a PowerShell, tudni fogja, hogy a mínuszjel (-) beírásával, és kattintson a TAB billentyű a rendelkezésre álló paraméterek parancsmag válthat. Ez a funkció akkor is használható, amely csak a sablon paraméterrel. Amint a sablon nevét a szó beírásakor az parancsmagot beolvassa a sablon elemzi a paraméterek és dinamikusan a sablon paramétereket ad a parancsot. Ezzel megkönnyíti nagyon adja meg a sablon paraméterértékeket. És, ha egy kötelező paraméter elfelejti, PowerShell felajánlja az érték.

Az alábbi a teljes parancs, paraméterekkel tartalmazza. Az erőforrások nevei lehet nyújtani a kívánt értékét.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Következő lépések

Most, hogy van egy működő fürtre, ezeket a dokumentumokat a kapcsolat és kezelési részleteit lásd:

- [Csatlakozzon az Azure tároló szolgáltatás fürthöz](container-service-connect.md)
- [Azure tároló szolgáltatás és az Adatközpont/operációs rendszer használata](container-service-mesos-marathon-rest.md)
- [Azure tároló szolgáltatás és Docker méhrajt használata](container-service-docker-swarm.md)
