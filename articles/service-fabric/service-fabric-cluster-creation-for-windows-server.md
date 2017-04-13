<properties
   pageTitle="Létrehozhatja és kezelheti a különálló Azure Service háló fürtre |} Microsoft Azure"
   description="Létrehozása és kezelése egy Azure Service háló fürthöz a bármely számítógép (fizikai vagy virtuális) futó Windows Serveren, kerül a helyszíni, vagy bármely a felhőben."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Létrehozása és kezelése a Windows Server operációs rendszerű fürtre

Azure Service háló szolgáltatás háló fürt létre virtuális gépeken futó vagy a Windows Servert futtató számítógépeken is használhatja. Ez azt jelenti, hogy lehet telepíteni és szolgáltatás háló alkalmazások futtatásához minden olyan környezetben, amelyben az összekapcsolt Windows Server számítógépek, legyen az helyszíni, vagy bármely felhő szolgáltatónál. Szolgáltatás háló a telepítőcsomag létrehozása a Windows Server önálló csomag nevű szolgáltatás háló fürt biztosít.

Ez a cikk végigvezeti a fürt létrehozásához szolgáltatás háló helyszíni, a különálló csomag használatával, hogy könnyen igazítható legyen bármely olyan környezetben, például más felhő szolgáltatók számára.

>[AZURE.NOTE] A különálló Windows Server csomag is tartalmazhat, amely jelenleg előzetes verzióban és az üzleti felhasználását nem támogatottak. Szolgáltatásokat, amelyeket a képen listájának megtekintéséhez kattintson a "előzetes verzió szolgáltatások szerepel ebben a csomagban." Is megtekintheti [a végfelhasználói egy példányának letöltése](http://go.microsoft.com/fwlink/?LinkID=733084) most.


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Technikai támogatás kérése a szolgáltatás háló önálló csomag

- A szolgáltatás háló önálló csomag a Windows Server [Azure Service háló fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?)a Közösség kérdezzen.

- Nyissa meg a [Szolgáltatás háló Professional támogatása](http://support.microsoft.com/oas/default.aspx?prid=16146 )jegy.  További tudnivalók [A Microsoft Professional támogatás](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>A szolgáltatás háló különálló csomag letöltése


[Töltse le a különálló csomagot a szolgáltatás háló a Windows Server 2012 R2 vagy újabb verzió](http://go.microsoft.com/fwlink/?LinkId=730690), amelynek Microsoft.Azure.ServiceFabric.WindowsServer neve. &lt;verzió&gt;.zip.


A letöltés csomagban találja a következő fájlokat:

|**Fájlnév**|**Rövid leírása**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|A .cab fájlt, amely tartalmazza a bináris a fürt minden számítógépre telepített.|
|ClusterConfig.Unsecure.DevCluster.json|Fürt konfigurációs mintafájl egy nem védett, a három-csomópont, egyetlen-gép (vagy a virtuális gép) fejlesztési fürt, többek között az egyes csomópontok adatait a fürt beállításokat tartalmazó. |
|ClusterConfig.Unsecure.MultiMachine.json|Fürt konfigurációs mintafájl egy nem védett, több számítógépen (vagy virtuális gép) fürt, többek között az egyes adatait a fürt beállításokat tartalmazó.|
|ClusterConfig.Windows.DevCluster.json|Fürt konfigurációs mintafájl, amely tartalmaz egy biztonságos, három-csomópont, egyetlen-gép (vagy a virtuális gép) fejlesztési fürtre, beleértve minden csomópontot, amely a fürt adatait a beállításait. A fürt védett [Windows identitások](https://msdn.microsoft.com/library/ff649396.aspx)használatával.|
|ClusterConfig.Windows.MultiMachine.json|Fürt konfigurációs mintafájl használata a Windows biztonsági, például a az egyes a számítógépen, amely a biztonságos fürt biztonságos, több számítógépen (vagy a virtuális gép) fürt összes beállításokat tartalmazó. A fürt védett [Windows identitások](https://msdn.microsoft.com/library/ff649396.aspx)használatával.|
|ClusterConfig.x509.DevCluster.json|Fürt konfigurációs mintafájl, amely tartalmaz egy biztonságos, három-csomópont, egyetlen-gép (vagy a virtuális gép) fejlesztési fürtre, beleértve minden csomópont adatait a fürt a beállításait. X509 a fürt védett tanúsítványok.|
|ClusterConfig.x509.MultiMachine.json|Fürt konfigurációs mintafájl, amely tartalmazza a biztonságos, több számítógépen (vagy virtuális gép) fürt, beleértve minden csomópont adatait a biztonságos fürt a beállításokat. X509 a fürt védett tanúsítványok.|
|EULA_ENU.txt|Licencfeltételek a Microsoft Azure Service háló önálló Windows Server csomag felhasználni. Azt is megteheti [a végfelhasználói egy példányának letöltése](http://go.microsoft.com/fwlink/?LinkID=733084) most.|
|Readme.txt|Hivatkozás a kibocsátási megjegyzések és egyszerű telepítési utasításokat. Célszerű az utasításokat a dokumentum egy részét.|
|CreateServiceFabricCluster.ps1|Egy PowerShell-parancsprogramot, amely a fürt ClusterConfig.json beállításainak használatával hoz létre.|
|RemoveServiceFabricCluster.ps1|Egy PowerShell-parancsprogramot, amely a fürtre ClusterConfig.json beállításainak használatával eltávolítja.|
|ThirdPartyNotice.rtf |Harmadik fél szoftvert, a csomagban, értesítés.|
|AddNode.ps1|Egy PowerShell-parancsprogramot-csomópont hozzáadása egy meglévő fürt telepítését.|
|RemoveNode.ps1|Egy PowerShell-parancsprogramot csomópont eltávolíthat egy meglévő fürt telepítését.|
|CleanFabric.ps1|Egy PowerShell-parancsprogramot szolgáltatás háló telepítési ki az aktuális számítógép önálló tisztítására. Előző MSI-telepítés saját társított uninstallers érdemes lehet eltávolítani.|
|TestConfiguration.ps1|Egy PowerShell-parancsprogramot a Cluster.json meghatározott infrastruktúra elemzéséhez használható.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Megtervezése és a fürt üzembe előkészítése
A csoport létrehozása előtt, hajtsa végre az alábbi lépéseket.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Lépés: 1: A fürt infrastruktúra megtervezése
Szolgáltatás háló fürt létre saját gépeken, így eldöntheti, hogy milyen hibákat, amelyet a fürt is tovább áll. Például tegye meg kell külön power vonalak vagy képleteknek az alábbi gépeken futó Internet-kapcsolatok? Ezenkívül fontolja meg ezeket a gép fizikai biztonsága. A gép helyét, és ki őket hozzáférésre van szüksége? Miután elvégezte ezeket a döntéseket, logikailag hozzárendelheti a gép a különböző hibafa tartományok (lásd: lépés: 4). A gyártási fürt megtervezése infrastruktúra bonyolultabb, mint a próba fürtre vonatkozóan.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Lépés: 2: Felkészülés a gép felel meg a vonatkozó követelmények
Adja hozzá a fürthöz kívánt minden gépi előfeltételei:

- A 16 GB RAM legalább ajánlott.
- 40 GB szabad lemezterület legalább ajánlott.
- A 4 core nagyobb Processzor ajánlott.
- A biztonságos hálózathoz vagy az összes gépekhez hálózatok kapcsolat.
- A Windows Server 2012 R2 vagy Windows Server 2012 (kell [KB2858668](https://support.microsoft.com/kb/2858668) telepítve van).
- [.NET-keretrendszer 4.5.1 vagy újabb](https://www.microsoft.com/download/details.aspx?id=40773), teljes telepítés.
- [A Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- A [szolgáltatás RemoteRegistry](https://technet.microsoft.com/library/cc754820) futnia kell a gépen.

A fürt rendszergazda telepítése és beállítása a fürt [rendszergazdai jogosultságokkal](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) kell rendelkeznie a gép mindegyike. A tartományvezérlőnek szolgáltatás háló nem telepíthető.

### <a name="step-3-determine-the-initial-cluster-size"></a>Lépés 3: Fürt kezdeti méretének meghatározása
Egy különálló szolgáltatás háló fürt minden csomópontjának van a szolgáltatás háló futtatókörnyezet rendszerbe, és a fürt tagja. Egy tipikus éles üzemi környezetben van egy csomópont-OS példányonként (fizikai vagy virtuális). A fürt mérete az üzleti igények határozza meg. A minimális fürt méretet három csomópontok (gépek vagy virtuális gépeken futó) azonban kell rendelkeznie.
Az adott számítógépen fejlesztési célokra lehet egynél több csomópontot. Szolgáltatás háló üzemi környezetben, támogatja a virtuális gép vagy a fizikai eső csak egy csomópontot.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Lépés: 4: Hibafa tartományok számának meghatározása és tartományok frissítése
*Hibafa tartomány* (FD) hiba fizikai időegység, és közvetlenül a adatközpontokban fizikai infrastruktúra kapcsolódik. Hibafa tartomány, ahol a hardver-összetevő (számítógépek, a kapcsolók, hálózatok és egyebek), amely egyetlen hiba pont megosztása. Hibafa-tartományok és állványon között 1:1 leképezéssel ugyan, módszerektől beszél, minden egyes állvány tekinthető hibafa tartományt. Amikor a fürt csomópontját, javasoljuk, hogy a csomópontok oszlanak, kell legalább három hibafa tartományok.

FDs ClusterConfig.json ad meg, amikor megadhatja az egyes FD nevét. Szolgáltatás háló támogatja a hierarchikus FDs, így akkor is megfelelően az infrastruktúra topológiát őket a.  Ha például a következő FDs érvényesek:

- "faultDomain": "fd: / Room1/Rack1/gép1"
- "faultDomain": "fd: / FD1"
- "faultDomain": "fd: / Room1/Rack1/PDU1 M1"


A csomópontok logikai egység *tartomány frissítése* (UD). Frissítéskor orchestrated szolgáltatás háló (-alkalmazás frissítése vagy a fürt frissítése) a UD csomópontjait miközben más UDs található csomópontok kérések kiszolgálására elérhető marad a frissítés elvégzéséhez vesznek. A belső vezérlőprogram frissítések elkészíti a gépeken veszi figyelembe UDs, hogy el kell végeznie őket egy gép egyszerre.

Az e fogalmak megfontolni legegyszerűbben a mértékegység váratlan hiba és UDs tervezett karbantartás egységként FDs tekinti.

UDs ClusterConfig.json ad meg, amikor megadhatja az egyes UD nevét. Ha például a következő nevek érvényesek:

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Kék"

[Szolgáltatás háló fürt ismertető](service-fabric-cluster-resource-manager-cluster-description.md)témakörben talál további információt a frissítési tartományok és hibafa tartományok.

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>5 lépés: A szolgáltatás háló különálló csomag letöltése a Windows Server
[A szolgáltatás háló önálló csomag a Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) és bontsa ki a csomagot, amely nem része a fürt telepítési géphez vagy az egyik a gép, amely a fürt része lesz. Előfordulhat, hogy a kicsomagolt mappa átnevezése `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>A csoport létrehozása

Miután, telt és lépésein, készen áll a fürt létrehozásához.

### <a name="step-1-modify-cluster-configuration"></a>Lépés: 1: Fürt konfigurációjának módosítása
A fürt ClusterConfig.json ismerteti. A fájlban a szakaszok a részletekért olvassa [önálló Windows fürt beállításait](service-fabric-cluster-manifest.md).
Nyisson meg egy ClusterConfig.json fájlokat a csomagból, letöltötte, és az alábbi beállításokat módosíthatja:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Kereséskonfigurációs beállítás**|**Leírás**|
|-----------------------|--------------------------|
|**NodeTypes**|Csomópont típus különféle csoportokba választják el a fürt csomópontok teszi lehetővé. A fürtre legalább egy NodeType kell rendelkeznie. Egy csoport összes csomópontok az alábbi közös jellemzőkkel rendelkeznek: <br> **Név** - csomópont típusú neve. <br>**Végpont portok** – ezek különböző elnevezett végpontjait (portok) csomópontot típus társított. Bármely kívánja, az mindaddig, amíg ki nem ütköznek a jegyzék egyéb, és már nem használja-e a számítógép virtuális futó többi alkalmazás portszámot is használhatja. <br> **Elhelyezési tulajdonságai** – ezek ismertetik a csomópont adja meg a elhelyezési kényszerek, a rendszer-szolgáltatásokat vagy a szolgáltatások tulajdonságainak. A tulajdonságok olyan felhasználó által definiált kulcs és az érték összekapcsolásával, amelyek egy adott csomópont további metaadatai biztosítanak. Példák a csomópont tulajdonságai lenne, van-e a csomópontra a merevlemez-meghajtó vagy videokártyát, tengelyek a merevlemezen, magmintákat és más fizikai tulajdonságok számát. <br> **Kapacitás** - csomópont kapacitás meghatározása a nevét, és egy adott erőforrás egy adott csomópontra tartalmazó mennyiségét felhasználási érhető el. A csomópont például meghatározhatják, hogy rendelkezik-e egy mérőszám "MemoryInMb" nevű kapacitás és azt 2048 MB szabad alapértelmezés szerint. Ezek a kapacitásának annak érdekében, hogy az adott mennyiségű erőforrást igénylő szolgáltatások elhelyezése van-e a csomópontok érhető el a szükséges összegek erőforrásokhoz rendelkező futásidőben használják.<br>**IsPrimary** - biztosítása érdekében, hogy csak egy definiált egynél több NodeType ha van beállítva az elsődleges, a értéke *Igaz*, amely olyan, amikor a rendszer szolgáltatások futtatása. Minden más típusnál csomópont az érték *hamis* értékre kell állítani|
|**Csomópontok**|Ezek a részletek, a fürt (csomópont típusa, csomópont nevét, IP-címet, hibafa tartomány és a csomópont frissítési tartomány) tartalma csomópontok minden egyes. A gép, amelyet a fürt hozható létre kell az IP-címe szerepel. <br> Ha az összes csomópont használja ugyanazt a címet, majd egy beépített fürt jön létre, amely tesztelési célú is használhatja. Ne használjon egy beépített fürt gyártási munkaterhelésekből telepítésével.|


### <a name="step-2-run-the-testconfiguration-script"></a>Lépés: 2: TestConfiguration futtatása

A TestConfiguration parancsfájl a infrastruktúra azt vizsgálja, cluster.json, hogy a szükséges engedélyeket állít be, a gép csatlakoztatott egymással, és más attribútumait határozzák meg, hogy a környezetben is sikeresek definiálva. Azt alapjában véve egy mini ajánlott eljárásokat elemző. Ezzel az eszközzel, hogy megbízhatóbb időbeli további ellenőrzések hozzáadása folytatjuk.

Ezt a parancsfájlt, hogy a gép szerint vannak listázva a fürt konfigurációs fájl csomópontok rendszergazdai hozzáféréssel rendelkezik gépi. Ezt a parancsfájlt a gép nem kell lenniük a fürt része.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Lépés 3: Futtassa a létrehozás fürthöz
A fürt konfigurálása a JSON-dokumentumba beszúrt módosított és hozzáadott, csomópont-információkat fürt létrehozását *CreateServiceFabricCluster.ps1* PowerShell-parancsprogramot, futtassa a csomag mappából, és az elérési út átadni a JSON konfigurációs fájl. Ha befejezte, fogadja el a LICENCSZERZŐDÉST.

Ezt a parancsfájlt, hogy a gép szerint vannak listázva a fürt konfigurációs fájl csomópontok rendszergazdai hozzáféréssel rendelkezik gépi. Ezt a parancsfájlt a gép nem kell lenniük a fürt része.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] A telepítési naplók helyileg a CreateServiceFabricCluster PowerShell futtatta a virtuális/gépi érhetők el. Azok a mappában, ha futtatta a PowerShell-parancsot a DeploymentTraces nevű almappában. Szeretné látni, ha a szolgáltatás háló megfelelően telepítették-e egy számítógépre, talál a telepített fájlokat a C:\ProgramData címtárban, és a FabricHost.exe és Fabric.exe folyamatok is láthatja, hogy fut a Feladatkezelőben.

### <a name="step-4-connect-to-the-cluster"></a>Lépés: 4: Csatlakozzon a fürthöz

A biztonságos fürtre szeretne csatlakozni, olvassa el a [szolgáltatás háló biztonságos fürthöz csatlakozni](service-fabric-connect-to-secure-cluster.md)című témakört.

Egy nem biztonságos fürthöz szeretne csatlakozni, futtassa az alábbi PowerShell-parancsot:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Lépés az 5: Jelenítse meg a szolgáltatás háló Explorer

Most a fürt szolgáltatás háló Intézővel akár közvetlenül a gép http://localhost:19080/Explorer/index.html vagy távolról http://< egyik csatlakozhat*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Hozzáadás és eltávolítás csomópontok

Felvehet, és távolítsa el a különálló szolgáltatás háló fürt csomópontok a vállalati igények változásával. Lásd: a [hozzáadni vagy eltávolítani a szolgáltatás háló önálló fürtre csomópontok](service-fabric-cluster-windows-server-add-remove-nodes.md) a lépések részletes leírását.


## <a name="remove-a-cluster"></a>Egy csoport eltávolítása

Fürt eltávolításához a *RemoveServiceFabricCluster.ps1* PowerShell-parancsprogramot, futtassa a csomag mappából, és az elérési út átadni a JSON konfigurációs fájl. Tetszés szerint megadhatja a törlésről a napló helyét.

Ezt a parancsfájlt, hogy a gép szerint vannak listázva a fürt konfigurációs fájl csomópontok rendszergazdai hozzáféréssel rendelkezik gépi. Ezt a parancsfájlt a gép nem kell lenniük a fürt része.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Összegyűjtött telemetriai adatokat, és hogyan akkor lemondása

Alapértelmezés szerint a termék telemetriai a háló szolgáltatás használatát a termék javítható a gyűjti össze. A legjobb gyakorlat Analyzer [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1)elérhetőségének ellenőrzése a telepítés részeként futtató. Ha még nem érhető el, a telepítő nem válaszol, kivéve, ha Ön telemetriai lemondása.

  1. A telemetriai folyamat az alábbi adatokat feltöltése [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) naponta egyszer próbálja. A lehető legjobb feltöltés, és nincs hatással van a fürt funkciókat. A telemetriai csak küldünk a csomópontra a feladatátvételi futtató elsődleges manager. Nincs más csomópontok telemetriai küldjön.

  2. A telemetriai a következőkből áll:

-            A szolgáltatások száma
-            ServiceTypes száma
-            Alkalmazások száma
-            ApplicationUpgrades száma
-            FailoverUnits száma
-            InBuildFailoverUnits száma
-            UnhealthyFailoverUnits száma
-            Kópiák száma
-            InBuildReplicas száma
-            StandByReplicas száma
-            OfflineReplicas száma
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Csomópontok számának
-            IsContextComplete: IGAZ vagy hamis
-            ClusterId: Ez az egy minden fürthöz véletlen globálisan egyedi azonosítója
-            ServiceFabricVersion
-             A virtuális gép vagy a számítógép, ahonnan a telemetriai töltenek fel IP-címe


Telemetriai kikapcsolásához adja hozzá a következő *tulajdonságokat* a fürt config: *enableTelemetry: hamis*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>A csomagban előzetes verzió szolgáltatások

Nincs lehetőség.

>[AZURE.NOTE] Az új [Kiadás verziója a Windows Server önálló fürt (verzió 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), frissíthet a fürt funkciót a későbbi kiadásokban, automatikusan vagy manuálisan. Ez a funkció nem érhető el az előzetes verzióján, mert szüksége lesz a Georgia verziójával fürt létrehozása és az adatokat, és az alkalmazások áttelepítése a betekintő fürt. Figyelje az ezzel kapcsolatos további információra kíváncsi, ezt a beállítást.


## <a name="next-steps"></a>Következő lépések
- [Különálló Windows fürt beállításainak konfigurálása](service-fabric-cluster-manifest.md)
- [Hozzáadása vagy eltávolítása a csomópontok egy különálló szolgáltatás háló fürthöz](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [A Windows operációs rendszert futtató Azure VMs különálló szolgáltatás háló fürt létrehozása](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [A Windows biztonsági használata Windows önálló fürt biztonságos](service-fabric-windows-cluster-windows-security.md)
- [Biztonságos X509 használata Windows önálló fürt tanúsítványok](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
