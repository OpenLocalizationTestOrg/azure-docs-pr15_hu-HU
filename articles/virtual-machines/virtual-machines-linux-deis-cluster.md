<properties
   pageTitle="A 3-csomópont üzembe fürt Deis |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogy miként hozhat létre a 3-csomópont Deis fürt Azure erőforrás-kezelő Azure-sablon segítségével"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>A 3-csomópont üzembe Deis fürthöz

Ez a cikk bemutatja az kiépítési egy [Deis](http://deis.io/) Azure fürt. Bemutatja, hogy az ne hozzon létre a szükséges tanúsítványok üzembe helyezése, és egy minta **lépjen** alkalmazás méretezés az újonnan kiépített fürt a lépéseket.

Az alábbi ábra mutatja a telepített rendszer architektúráját. A rendszergazda kezeli a fürt használatával eszközök, például **deis** és **deisctl**Deis. Kapcsolatot létesít egy Azure terheléselosztó, amely a kapcsolatok továbbítja az egyik tagjának a fürt található csomópontok keresztül. Az ügyfelek access a terheléselosztó alkalmazások telepítését. Ebben az esetben a a terheléselosztó továbbítja a forgalmat a Deis útválasztó háló, amely további routs a fürt is megfelelő Docker tárolók forgalmat.

  ![A telepített Desis fürt architektúra ábrája](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Futtathatja az alábbi lépéseket, hogy szüksége lesz:

 * Azure active előfizetés. Ha nincs telepítve egyik, egy ingyenes pontosan a [azure.com](https://azure.microsoft.com/)elérheti.
 * Munkahelyi vagy iskolai azonosítója Azure erőforrás csoportok használatáról. Ha egy személyes fiók, és jelentkezzen be a Microsoft-azonosító van, [a személyes egy munka azonosítóra létrehozásához](virtual-machines-windows-create-aad-work-id.md)szükséges.
 * Bármelyik – az ügyfél operációs rendszertől függően – a [Azure PowerShell](../powershell-install-configure.md) vagy a [Mac, a Linux, és a Windows Azure CLI](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL létrehozásához szükséges tanúsítványok használatos.
 * Mely számjegy ügyfélprogram, például [Mely számjegy Bash](https://git-scm.com/).
 * A minta alkalmazás teszteléséhez is szüksége a DNS-kiszolgáló. Bármely DNS-kiszolgálók vagy helyettesítő A rekordok támogató szolgáltatások is használhatja.
 * A számítógép futtatásához Deis az ügyféleszközök elől. A helyi számítógép és a virtuális gép is használhatja. Futtatását is lehetővé teszi a szinte bármilyen Linux terjesztési ezeket az eszközöket, de az alábbi utasításokat Ubuntu használja.

## <a name="provision-the-cluster"></a>A fürt kiépítése

Ebben a részben egy [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) sablont, a Megnyitás tárházba [azure-quickstart útmutató-sablonok](https://github.com/Azure/azure-quickstart-templates)használni. Először meg fog másolja le a sablont. Egy új SSH kulcs pár hitelesítéshez ezt követően a kell létrehoznia. És ezt követően egy új azonosítót, fürt fogja beállítása. És végül kell megadnia a parancsprogram vagy a PowerShell-parancsprogramot, a fürt hozhatók létre.

1. A tár klónozhatja: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Nyissa meg a sablon mappája:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Hozzon létre egy új kulcs SSH pár ssh-keygen használata:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. A fenti titkos kulccsal tanúsítvány létrehozása:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Nyissa meg a [https://discovery.etcd.io/new](https://discovery.etcd.io/new) egy új fürt jogkivonat, amelyek úgy, mint ahogy látványtervek létrehozásához:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Minden CoreOS fürt egy egyedi jogkivonat, ez az ingyenes szolgáltatásból kell szerepelnie. További információt talál az [CoreOS dokumentációt](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Módosítsa a meglévő **Feltárás** jogkivonat cserélje ki az új jogkivonathoz **cloud-config.yaml** fájl:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Módosítsa a **azuredeploy-parameters.json** fájlt: Nyissa meg a tanúsítvány szövegszerkesztőben 4 lépésben létrehozott. Másolja a teljes szöveg közötti `----BEGIN CERTIFICATE-----` és `-----END CERTIFICATE-----` be a **sshKeyData** paraméter (kell eltávolítása az összes újsor karaktereket).

8. Módosítsa a **newStorageAccountName** paramétert. Ez a virtuális OS lemezt tároló fiókjában. Ez a fióknév nem lehet globálisan egyedi.

9. Módosítsa a **publicDomainName** paramétert. Ez a tartománynév a betöltés terheléselosztó szolgáltatása nyilvános IP társított része lesz. A végleges FQDN _[a paraméter értéke]_formátumának lesz. _[régió]_. cloudapp.azure.com. Például ha deishbai32 szerint adja meg a nevét, és az erőforráscsoport US nyugati régió telepíti, akkor a végleges FQDN a terheléselosztó lesz deishbai32.westus.cloudapp.azure.com.

10. Mentse a paraméter fájlt. És kattintson a fürt Azure PowerShell használatával is kiépítése:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  vagy Azure CLI:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Az erőforráscsoport már kiépítve, amikor megjelenik a csoport összes erőforrás Azure klasszikus portálon. Ahogy az alábbi képernyőképen látható, az erőforráscsoport egy virtuális elérhetősége mindazon tartományhoz három VMs-hálózaton tartalmazza. A csoport is tartalmaz egy terheléselosztó, amelynek társított nyilvános IP-címre.

  ![A kiépített erőforráscsoport Azure klasszikus portálon](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Az ügyfél telepítése

**Deisctl** vezérlőre van szüksége a fürt Deis. Bár az összes fürt csomópontok deisctl automatikusan települ, tanácsos deisctl használata egy külön felügyeleti gépen. Ezenkívül csomópontjait konfigurált saját IP-címek, mert kell keresztül van egy nyilvános IP-, csatlakozás a csomópont gépek terheléselosztó protokollbújtatás SSH használja. Az alábbiakban a fizikai egy külön Ubuntu a deisctl vagy virtuális gép beállításának lépéseit.

1. Telepítés deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. A titkos kulcs ssh ügynök hozzáadása:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Állítsa be a deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

A sablon feleltesse meg az 1, 2-példányhoz 2224 és 2225 példányához 3 példány 2223 bejövő hálózati Címfordítást szabályokat határozza meg. Ez a redundancia biztosít a deisctl eszközzel. Ellenőrizheti, hogy a Azure klasszikus portálon szabályok:

![A terheléselosztó hálózati Címfordítást szabályok](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] A sablon jelenleg csak 3-csomópont fürt támogatja. Ez az erőforrás-kezelő Azure sablon hálózati Címfordítást szabály definíciót, amely nem támogatja a leállításig szintaxis korlátozása miatt.

## <a name="install-and-start-the-deis-platform"></a>Telepítse és indítsa el a Deis platform

Most deisctl használatával telepítse és indítsa el a Deis platform:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Kezdve a platformot is eltarthat (szerint 10 perc). Különösen a szerkesztő szolgáltatás indítása hosszú ideig tarthat. És előfordul, hogy tart biztos, hogy a sikeres: Ha a művelet úgy tűnik, hogy lefagyhat, írjon be `ctrl+c` a parancs végrehajtásának megszakítása és próbálkozzon újra.

Használható `deisctl list` ellenőrzéséhez, ha az összes szolgáltatás fut:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Gratulálok! Most egy futó róla, hogy Deis a Azure clsuter! Ezután vegyük üzembe minta Ugrás alkalmazás működését a fürt megjelenítéséhez.

## <a name="deploy-and-scale-a-hello-world-application"></a>Üzembe helyezéséhez és egy Helló, világ alkalmazás méretezése

A következő lépések bemutatják, hogyan bevezetését tervezi a "Helló, világ" nyissa meg a fürthöz alkalmazást. A lépések alapján [Deis dokumentációt](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. A(z) működéséhez útválasztási háló kell egy helyettesítő A rekord a tartományátirányítás nyilvános IP-a terheléselosztó is. Az alábbi képernyőképen egy minta tartomány regisztrációs az A rekordot a GoDaddy jeleníti meg:

    ![Godaddy rekord](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Telepítés deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Hozzon létre egy új SSH kulcsot, majd adjon hozzá a nyilvános kulcs GitHub (természetesen is felhasználhatja a meglévő kulcsokat). Hozzon létre egy új SSH-pár, használhatja:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Id_rsa.pub és a nyilvános kulcs tetszőlegesen megválasztott felvétele GitHub. Ehhez a hozzáadása SSH gombra a SSH kulcsok a beállítások képernyőn használata:

  ![Github kulcs](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Új felhasználóként rögzítése:

        deis register http://deis.[your domain]
<p />
6. Adja hozzá a SSH kulcs:

        deis keys:add [path to your SSH public key]
  <p />      
7. Hozzon létre egy alkalmazást.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.A mely számjegy leküldéses elindítja a Docker képek épít fel és rendszerbe, amely néhány percig tart. A saját változat időnként, lépés 10 (Pushing kép a magánjellegű tárházba) leállhat. Ez történik, amikor a folyamat leállítása, az alkalmazás használatával eltávolítása "alkalmazások deis: destroy – a <application name> ` to remove the application and try again. You can use `deis apps:list" megtudhatja, hogy az alkalmazás nevét. A szolgáltatás működik, ha meg kell jelennie a következő parancs kimeneti értékeket végén hasonló:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Ellenőrizze, hogy működik-e az alkalmazást:

        curl -S http://[your application name].[your domain]
  Meg kell jelennie:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Az alkalmazás 3 példányaiban méretezheti:

        deis scale cmd=3
<p />
11. Másik lehetőségként használhatja az alkalmazás részleteit vizsgálja meg az információ deis. A következő kimeneti értékeket a saját alkalmazások telepítésének a következők:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Következő lépések

Ez a cikk az üzlet elévült meg egy új kiépítése a lépéseivel Deis fürt Azure erőforrás-kezelő Azure-sablon segítségével. A sablon redundancia támogatja a kapcsolatokat, valamint a telepített alkalmazások terheléselosztási szerszámok. A sablon is elkerülhető használata nyilvános IP-címei tag csomópontok, amely értékes nyilvános IP-erőforrások menti és host alkalmazásokhoz további védett környezetet nyújt. További tudnivalókért lásd: az alábbi cikkekben:

[Azure erőforrás szolgáltatásának áttekintése] [resource-group-overview]  
[Az Azure CLI használata] [azure-command-line-tools]  
[Azure PowerShell használatával a Azure-kezelő eszközzel] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
