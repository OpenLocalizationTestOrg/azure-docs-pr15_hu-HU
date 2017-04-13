<properties
    pageTitle="Mi az a Felhőbeli szolgáltatástól modell és csomag |} Microsoft Azure"
    description="Ismerteti a felhőalapú szolgáltatás modell (.csdef, .cscfg) és a csomag (.cspkg) Azure-ban"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Mi az, hogy a Felhőbeli szolgáltatástól modellt, és hogyan tegye készítheti elő?
Egy felhőalapú szolgáltatásba három összetevők, a szolgáltatás definition _(.csdef)_, a szolgáltatás config _(.cscfg)_és a szolgáltatás csomag _(.cspkg)_jön létre. A **ServiceDefinition.csdef** és a **ServiceConfig.cscfg** fájlok XML-alapú, és a felhőalapú szolgáltatást, és hogyan van konfigurálva; szerkezetének ismertetése a modell együttesen neve. A **ServicePackage.cspkg** zip-fájl, amely a **ServiceDefinition.csdef** és egyebek jön létre, az összes szükséges bináris alapú függőség tartalmazza. Azure egy felhőalapú szolgáltatásba a **ServicePackage.cspkg** , mind a **ServiceConfig.cscfg**hoz létre.

Miután a felhőbe szolgáltatást futtató Azure-ban a **ServiceConfig.cscfg** fájl átkonfigurálása, de nem változtatja meg a definícióját.

## <a name="what-would-you-like-to-know-more-about"></a>Mit szeretne többet tudni?

* További tudnivalók a [ServiceDefinition.csdef](#csdef) és [ServiceConfig.cscfg](#cscfg) fájlokat szeretnék.
* Tudni, hogy már tudja, adja meg [a példák](#next-steps) Mi lehet az konfigurálása.
* Hozzon létre a [ServicePackage.cspkg](#cspkg)szeretném.
* Visual Studio használok, és szeretném azt...
    * [Hozzon létre egy új, felhőalapú szolgáltatásba][vs_create]
    * [Egy meglévő felhőalapú szolgáltatást átkonfigurálása][vs_reconfigure]
    * [Egy felhőalapú szolgáltatás project terjesztése][vs_deploy]
    * [Távoli asztali be egy felhőalapú szolgáltatás példány][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
A **ServiceDefinition.csdef** fájl adja meg a beállításokat, amelyekkel megadhatja, hogy egy felhőalapú szolgáltatás Azure által használt. Az [Azure Service Definition séma (.csdef fájlt)](https://msdn.microsoft.com/library/azure/ee758711.aspx) a megengedett formátum nyújt a szolgáltatás-definíciós fájl. A következő példa bemutatja a beállításokat, amelyekkel a webes és dolgozó szerepkörök adható meg:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

A [szolgáltatás Definition séma] [] az XML-sémát, itt használt jobban megértéséhez megtekinthesse, azonban az alábbiakban gyors magyarázata a elemeivel:

**Webhelyek**  
A webhelyek vagy a webhely-alkalmazásokhoz, amelyek a IIS7 definícióinak tartalmazza.

**InputEndpoints**  
Lépjen kapcsolatba a felhőbeli szolgáltatástól felhasznált végpontok tartalmazza.

**InternalEndpoints**  
A kommunikáció a többi szerepkör-példányok által használt végpontokat definícióinak tartalmazza.

**ConfigurationSettings**  
A szerepkör a szolgáltatásokhoz beállítás-definíciók tartalmazza.

**Tanúsítványok**  
Tartalmazza a tanúsítványok, a szerepkör szükséges. Az előző kód példa bemutatja a tanúsítvány, amellyel Azure csatlakozás beállításait.

**LocalResources**  
Erőforrások helyi tárolás tartalmazza. Helyi tárolás erőforrás a virtuális gép, amelyben egy szerepkört egy példánya fut a fájlrendszerben fenntartott könyvtárában.

**Import**  
Az importált modulokat tartalmazza. Az előző példa modulokat távoli asztali kapcsolat és Azure csatlakozás jeleníti meg.

**Indítási**  
A szerepkör indításakor futtatott feladatokat tartalmazza. A feladatok .cmd vagy végrehajtható fájl határozzák meg.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
A felhőalapú szolgáltatás beállításainak beállításait az értékeket a **ServiceConfiguration.cscfg** fájlban határozza meg. Megadhatja az egyes szerepkör, ezt a fájlt a telepíteni kívánt példányok száma. A beállítások, a szolgáltatás-definíciós fájl definiált értékei a szolgáltatás konfigurációs fájl hozzáadódnak. A thumbprints bármely a felhőbeli szolgáltatástól társított adatkezelési tanúsítványok is bekerülnek a fájlt. Az [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx) szolgáltatás konfigurációs fájl nyújt a megengedett formátumát.

A szolgáltatás konfigurációs fájl nem része az alkalmazással, de egy külön fájlba Azure feltöltött és konfigurálása a felhőalapú szolgáltatást használják. Új szolgáltatás konfigurációs fájl feltöltheti a felhőalapú szolgáltatás újbóli nélkül. A felhőbeli szolgáltatástól konfigurációs értékeinek megváltoztathatja a felhőbeli szolgáltatástól futása közben. A következő példa bemutatja a webes és dolgozó szerepkörökhöz definiálható beállításokat:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

A hivatkozott pontosabb megértéséhez az XML-sémát, itt használt [Szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/azure/ee758710.aspx) , azonban az alábbiakban egy rövid leírását az elemeket:

**Példányok**  
Konfigurálja a szerep példánya fut számát. Megakadályozhatja, hogy a felhőalapú szolgáltatás frissítéskor esetleg nem érhető el valamit, ajánlási központi telepítését a webes elérésű szerepkörök egynél több példánya. Ezzel az eljárással meg vannak tapadó az [Azure kiszámítania szolgáltatási szerződés SZOLGÁLTATÁSISZINT-](http://azure.microsoft.com/support/legal/sla/), amely biztosítja 99.95 % külső kapcsolódási internetes szerepkörök, ha két vagy több szerepkör-példányok telepítik egy szolgáltatás útmutatását.

**ConfigurationSettings**  
A szerepkör a futó példányok beállításainak konfigurálása A nevét a `<Setting>` elemek egyeznie kell a beállítást definíciók a szolgáltatás-definíciós fájl.

**Tanúsítványok**  
Adja meg a szolgáltatás által használt tanúsítványokat. Az előző példa bemutatja, hogyan meghatározása a tanúsítvány a távelérési modulhoz. Az érték az *ujjlenyomatot* attribútum kell állíthatók be a tanúsítvány az ujjlenyomatot használni.

<p/>

 >[AZURE.NOTE] A tanúsítvány az ujjlenyomatot lehet hozzáadni a konfigurációs fájl egyszerű szövegszerkesztőben használatával, vagy az értéket is hozzáadhat a **tanúsítványok** lapján a szerepkör a Visual Studióban **Tulajdonságok** lapon.



## <a name="defining-ports-for-role-instances"></a>Szerepkör-példányok portok meghatározása
Azure lehetővé teszi, hogy csak egy bejegyzés pont webes szerepkörbe. Ez azt jelenti, hogy minden forgalom történik-e egy IP-címet. Beállíthatja, hogy a webhely megosztása olyan portot irányítsa át az értekezlet-összehívást a megfelelő helyre a host fejléc konfigurálásával. Az alkalmazások az IP-címet a jól ismert portokat meghallgatásához is beállítható.

A következő példában az adatokat egy webes szerepkör webhely és a webes alkalmazással. A webhely 80-as port alapértelmezett bejegyzés helyként van beállítva, és a webalkalmazások kéréseket fogadására "mail.mysite.cloudapp.net" nevezik alternatív host parancsát vannak beállítva.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Szerepkör beállításainak módosítása
Az Azure, a szolgáltatás nem elérhető vétele nélkül futtatása közben, frissítheti a felhőalapú szolgáltatás beállításait. Konfigurációs adatok módosításához vagy egy új konfigurációs fájl feltöltése vagy helyen a konfigurációs fájl szerkesztése elemre, és alkalmazza a futó szolgáltatásban. A szolgáltatás konfigurálását is lehet a következő módosításokat végzett:

- **Az értékek konfigurációs beállítások módosítása**  
Ha a konfiguráció beállítása a módosításokat, egy szerepkör-példány választhatja a módosítás érvénybe léptetéséhez közben a példány online állapotban, vagy a példány biztonságosan Lomtár, és közben a példány a módosítás érvénybe léptetéséhez offline állapotban van.

- **Szerepkör-példányok szolgáltatás topológiájának módosítása**  
Futó példányok, kivéve, ha egy példány eltávolítása topológia módosítások nem vonatkoznak. Az összes többi példányok általában nem kell lennie a rendszer; jó helyen jár megadhatja szerepkör-példányok topológiájának változást válaszul Lomtár.

- **A tanúsítvány ujjlenyomat módosítása**  
Tanúsítvány csak akkor frissíthető, ha egy szerepkör-példány offline állapotban. Ha egy tanúsítványt hozzáadott, törölni vagy módosítható, amíg egy szerepkör-példány online állapotban, Azure biztonságosan megnyitja a példány offline frissítse a tanúsítványt, és ismét online állapotba a módosítás befejeződése után.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Konfigurációs módosítások szolgáltatás futtatókörnyezet eseményekkel kezelése
Az [Azure futtatókörnyezet tár](https://msdn.microsoft.com/library/azure/mt419365.aspx) magában foglalja a [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) névtér, amely biztosítja az osztályok az Azure környezetben szerepkörbe egy példánya fut kódból személlyel. A [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) osztály határozza meg az alábbi események előtt és után a konfiguráció módosítása kiváltó:

- **Esemény [módosítása](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)**  
Ez akkor fordul elő, a konfigurációs módosítást a szerepkör a névjegyadatok kell elvégezni a szerepkör-példányok lefelé, ha szükséges, biztosítva egy adott példányhoz alkalmazása előtt.
- **[Módosított](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) esemény**  
Egy adott példányhoz a szerepkör a konfigurációs módosítást alkalmazása után fordul elő.

> [AZURE.NOTE] Tanúsítvány módosítások mindig offline állapotba helyezéséhez szerepkörbe példányát, mert azok ne emelje RoleEnvironment.Changing vagy RoleEnvironment.Changed eseményeket.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Azure-ban egy felhőalapú szolgáltatásba, alkalmazások telepítése, az alkalmazás a megfelelő formátumban először meg kell csomagot. A **CSPack** parancssori eszköz (telepített az [Azure SDK](https://azure.microsoft.com/downloads/)) hozhat létre a csomagfájlt Visual Studio alternatívájaként használható.

**CSPack** használja a szolgáltatás-definíciós fájl és a szolgáltatás konfigurációs fájl tartalmának meghatározása a csomag tartalma. **CSPack** hoz létre, amely szerint az [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy)Azure is feltölthet alkalmazás csomagfájlt (.cspkg). A csomag neve alapértelmezés szerint `[ServiceDefinitionFileName].cspkg`, de megadhat egy másik nevet használatával a `/out` **CSPack**lehetőséget.

**CSPack** általában felső található  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (a windows), hogy telepítve van a SDK csomagjában talál a **Microsoft Azure parancssor** helyi futtatásával érhető el.  
>  
>CSPack.exe futtatta önmagában dokumentációjában az összes lehetséges kapcsolók és parancsok megjelenítéséhez.

<p />

>[AZURE.TIP]
A felhőalapú szolgáltatás helyileg futtatása a **Microsoft Azure kiszámítania irányító**, adja meg a ezt a lehetőséget a bináris fájlokat, ahonnan azok futtatható a számítási irányító címtár elrendezés alkalmazása másolja **/copyonly** beállítást.

### <a name="example-command-to-package-a-cloud-service"></a>Példa egy felhőalapú szolgáltatásba csomag parancs
Az alábbi példa létrehoz egy alkalmazáscsomag egy webes szerepkör információkat tartalmazó. A parancs adja meg a szolgáltatás-definíciós fájl szeretne használni, a hol találhatók a bináris fájlokat, könyvtár és a csomag fájl nevét.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Ha az alkalmazás egy webes szerepkör és dolgozó szerepkört is tartalmaz, a következő parancsot használja:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Ha a változók határozzák meg az alábbi képlettel történik:

| Változó                  | Érték |
| ------------------------- | ----- |
| \[Könyvtárnév\]         | A alkönyvtár könyvtárában a legfelső szintű projekt, amely a Azure projekt .csdef fájlt tartalmazza.|
| \[ServiceDefinition\]     | A szolgáltatás-definíciós fájl neve. Ez a fájl neve alapértelmezés szerint ServiceDefinition.csdef.  |
| \[Kimenetifajlneve\]        | A létrehozott csomag fájl neve. Általában ennek értéke az alkalmazás nevére. Ha nincs fájlnév megadva, az alkalmazáscsomag jön létre \[ApplicationName\].cspkg.|
| \[RoleName\]              | A szerepkör a szolgáltatás-definíciós fájl meghatározott neve.|
| \[RoleBinariesDirectory] | A helye a bináris fájlokat a szerep.|
| \[Virtuális_elérési_út\]           | A fizikai könyvtárak minden virtuális elérési definiált szolgáltatás meghatározása helyek szakaszában.|
| \[Fizikai_elérési_út\]          | A fizikai könyvtárak minden virtuális elérési az a webhely csomópontot a szolgáltatás definíció definiált a tartalmát.|
| \[RoleAssemblyName\]      | A szerepkör a bináris fájl neve.| 


## <a name="next-steps"></a>Következő lépések

E vagyok létrehozása egy felhőalapú szolgáltatás csomagot, és szeretném azt...

* [Távoli asztal beállítása egy felhőalapú szolgáltatás előfordulásra vonatkozóan][remotedesktop]
* [Egy felhőalapú szolgáltatás project terjesztése][deploy]

Visual Studio használok, és szeretném azt...

* [Hozzon létre egy új, felhőalapú szolgáltatásba][vs_create]
* [Egy meglévő felhőalapú szolgáltatást átkonfigurálása][vs_reconfigure]
* [Egy felhőalapú szolgáltatás project terjesztése][vs_deploy]
* [Távoli asztal beállítása egy felhőalapú szolgáltatás előfordulásra vonatkozóan][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
