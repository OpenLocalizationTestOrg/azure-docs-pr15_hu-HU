<properties
   pageTitle="Szolgáltatás háló alkalmazás és a szolgáltatás biztonsági házirendek |} Microsoft Azure"
   description="Megtudhatja, hogy miként egy szolgáltatás háló alkalmazás futtatásához a rendszer és a helyi biztonsági fiókok, beleértve a SetupEntry pontot, ahol egy alkalmazást kell néhány jogosultsággal rendelkező művelet végrehajtása, megkezdése előtt"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Az alkalmazás biztonsági házirendek beállítása
Azure Service háló lehetővé teszi a biztonságos alkalmazások fut a fürt a különböző felhasználói fiókok. Szolgáltatás háló is titkosítja a felhasználói fiókkal, például a fájlok, mappák és a tanúsítványok telepítési idején az alkalmazások által használt erőforrások. Így futó alkalmazást, akár egy megosztott környezetben szolgáltatott, biztonságos egymástól. 

Alapértelmezés szerint a szolgáltatás háló alkalmazások futhatnak a fiókot, amely a Fabric.exe folyamat fut a. Szolgáltatás háló is lehetővé teszi a helyi felhasználói fiók vagy a helyi rendszerfiók, az alkalmazás jegyzék meghatározott alkalmazások futtatásához. A támogatott helyi rendszer fióktípusok **helyi felhasználó**, **hálózati**, **LocalService**és **LocalSystem**.

 A Windows Server az az Adatközpont használatával a különálló installer szolgáltatás háló futtatásakor az Active Directory (AD) tartományi fiókok is használhatja.

Felhasználócsoportok definiálhatók és létre úgy, hogy egy vagy több felhasználó bővíthető minden csoport együtt kell kezelni. Ez akkor hasznos, ha másik szolgáltatáscsaládba belépési pontról több felhasználó, és bizonyos közös jogosultság, amelyek a rendelkezésre álló csoport szintre van szükségük.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>A házirend-szolgáltatás SetupEntryPoint konfigurálása

A [alkalmazásmodell](service-fabric-application-model.md) ismertetett módon a **SetupEntryPoint** található a Yammerhez a hitelesítő adatait (Ez általában a *hálózati* szolgáltatásfiók) szolgáltatás háló mint bármely más belépési pontról előtt futó. **Belépési** által megadott végrehajtható az általában a hosszabb ideig futó szolgáltatás szolgáltató, így egy külön beállítási megnyitásának helye problémákat elkerülhető, hogy a szolgáltatás fogadó végrehajtható futtatása magas jogosultságokkal rendelkező hosszabb ideig. Miután sikeresen kijelentkezik a **SetupEntryPoint** futtatása **belépési** által meghatározott végrehajtható fájl Az eredményül kapott folyamat felügyelt és újraindítása után kezdődő újra **SetupEntryPoint**, ha minden eddiginél ér véget, vagy összeomlik.

Egy egyszerű szolgáltatás nyilvánvalóan példa, hogy a SetupEntryPoint a fő belépési a szolgáltatás a következő:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>A helyi fiók használatával házirend beállítása

Miután beállította a szolgáltatást, hogy van egy beállítási megnyitásának helye, módosíthatja a biztonsági engedélyeket, hogy az alkalmazás jegyzék alatt fusson. A followin példa megtudhatja, hogy hogyan állíthatja be a szolgáltatást a felhasználói rendszergazdai jogokkal futtatása.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Első lépésként hozzon létre egy **rendszerbiztonsági** szakaszt a felhasználó neve, például SetupAdminUser. Ez azt jelzi, hogy a felhasználót a rendszer a Rendszergazdák csoport tagjának.

Ezután a **ServiceManifestImport** csoportban állítsa be egy házirendet, amely a tőketörlesztés mértékét alkalmazása **SetupEntryPoint**. Ez azt jelenti az szolgáltatás háló, hogy a **MySetup.bat** fájl futtatásakor kell RunAs rendszergazdai jogosultságokkal. Tekintve, hogy van *nem* alkalmazza a házirendet a fő belépési pontjához való, csoportban a rendszer **hálózati** szolgáltatásfiók fut **MyServiceHost.exe** lévő kódot. Ez az alapértelmezett fiókot, amely az összes szolgáltatás belépési pontról futtatása történik.

Most már helyezzen el a fájlt MySetup.bat tesztelje a rendszergazdai jogosultságokkal a Visual Studio projekthez. A Visual Studióban kattintson a jobb gombbal a szolgáltatás projekten, és vegyen fel egy MySetup.bat nevű új fájlt.

Ezután győződjön meg arról, hogy a szolgáltatás csomag a MySetup.bat fájl található. Alapértelmezés szerint nincs. Jelölje ki a fájlt, kattintson a jobb gombbal a helyi menü eléréséhez, és válassza a **Tulajdonságok parancsot**. A Tulajdonságok párbeszédpanelen győződjön meg róla, hogy **másolja a vágólapra a kimeneti könyvtár** értéke **Másolás: Ha újabb**. Lásd: az alábbi képernyőfelvételen.

![Visual Studio CopyToOutput az SetupEntryPoint parancsfájl][image1]

Most a MySetup.bat fájlra, és adja hozzá a következő parancsok végrehajtása:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Ezután össze, és a megoldást üzembe helyez a helyi fejlesztési fürtre.  Miután a szolgáltatás elindult, ahogy azt szolgáltatás háló Explorer, láthatja, hogy egy kétféleképpen sikeres volt-e a MySetup.bat. Nyissa meg a egy PowerShell a parancssor parancsot, és írja be:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Jegyezze fel a csomópontot, ahol a szolgáltatást telepítették, és a szolgáltatás háló Explorer lépések például csomópont 2 nevét. Ezután keresse meg az alkalmazás példány munka mappát **TestVariable**értékének megjelenítő out.txt-fájlt. A példa: Ha ezt a szolgáltatást telepítették csomópont 2, akkor az elérési út is nyissa meg a **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>A helyi rendszer fiókokkal házirend beállítása
Gyakran célszerű futtassa az Indítás parancsfájlt, ahogy azt megelőző Rendszergazdák fiók helyett egy helyi rendszerfiók. A RunAs futtatása rendszergazdaként házirend általában nem működik jól, mivel a számítógépek felhasználói hozzáférés Felügyeletének alapértelmezés szerint engedélyezve van. Ezekben az esetekben **helyi rendszerfiók alatt a SetupEntryPoint futtatásához az ajánlási helyett egy helyi felhasználó rendszergazdák csoportba felvett**. A következő példa bemutatja a SetupEntryPoint helyi rendszerfiók alatt futtatása beállítást.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Indítsa el az egy SetupEntryPoint PowerShell-parancsait
A **SetupEntryPoint** helyétől PowerShell futtatásához futtathatja **PowerShell.exe** egy köteg fájlban, amely egy PowerShell-fájl mutat. Első lépésként vegyen fel egy PowerShell-fájlt a szolgáltatási projekt, például **MySetup.ps1**. Ne feledje, hogy a tulajdonság *újabb másol* , hogy a szolgáltatás csomag is szerepel a fájlt. A következő példa bemutatja egy köteg mintafájl MySetup.ps1, a rendszer környezeti változó **TestVariable**nevű naptárrácsban nevű PowerShell fájl elindítására.


MySetup.bat PowerShell-fájl elindítására.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

A PowerShell-fájlt adja hozzá az alábbiak szerint a rendszer környezet-változó beállítása:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Megjegyzés:** Alapértelmezés szerint a parancsfájl futtatásakor azt vizsgálja meg az alkalmazások mappa neve a fájlok **használata** . Ebben az esetben ha MySetup.bat fut szeretnénk erre kattintva keresse meg a MySetup.ps1 ugyanabban a mappában, amely az alkalmazás **kód csomag** mappájában. Ha módosítani szeretné ezt a mappát, a munkakönyvtár beállítása a következő látható módon.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Helyi hibakereséshez konzol átirányítással
Időnként célszerű hibakeresés céljából futtató console kimenetét megjelenítéséhez. Ehhez állíthat be egy konzol átirányítása házirendet, amely a kimeneti fájl ír. A kimeneti fájl írja be a **naplófájl** neve a csomóponton hol az alkalmazás telepítése és futtatása (lásd: a hol találhatók meg az előző példában) alkalmazás mappát.

**Megjegyzés: Soha ne** az konzol átirányítási házirend használata egy gyártási rendszerbe, mivel ez hatással lehet a alkalmazás feladatátvételi alkalmazásban. **Csak** ezzel helyi fejlesztés és hibakeresés céljából.  

A következő példa bemutatja a konzol átirányítása FileRetentionCount értéket tartalmazó beállítás.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Most módosítja a MySetup.ps1 fájl írni egy **halvány** parancsot, ha ez lesz írni a kimeneti fájl hibakeresés céljából.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Miután megtalálta a hibát a parancsfájlt, azonnal távolítsa el a konzol átirányítása házirend**

## <a name="configure-policy-for-service-code-packages"></a>A szolgáltatás kód csomagokhoz házirend beállítása 
Az előző lépésben hogyan RunAs szabály alkalmazása SetupEntryPoint bemutattuk. Nézzük meg kissé mélyebb ismerteti, hogyan hozhat létre, amelyek másként szolgáltatás házirendek alkalmazhatók különböző rendszerbiztonsági.

### <a name="create-local-user-groups"></a>Helyi felhasználói csoportok létrehozása
Felhasználócsoportok definiálhatók és létre, amely lehetővé teszi a felvenni kívánt csoport egy vagy több felhasználó. Ez akkor különösen hasznos, ha másik szolgáltatáscsaládba belépési pontról több felhasználó, és bizonyos közös jogosultság, amelyek a rendelkezésre álló csoport szintre van szükségük. A következő példa bemutatja a **LocalAdminGroup** nevű rendszergazdai jogosultságokkal rendelkező helyi csoportot. Két felhasználó Customer1 és Customer2, a helyi csoport tagjai történik.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Hozzon létre helyi felhasználók
Létrehozhat egy helyi felhasználó az alkalmazáson belül szolgáltatás biztonságos használható. Ha egy **helyi felhasználó** fióktípus az alkalmazás jegyzék rendszerbiztonsági szakaszában van megadva, szolgáltatás háló helyi felhasználói fiókok gépek, ahol az alkalmazás van telepítve hoz létre. Alapértelmezés szerint az ezekben a fiókokban nem rendelkezik az alkalmazás jegyzék (például az alábbi példa a Customer3) megadott azonos néven. Ehelyett dinamikusan jönnek létre, és jelszavai véletlen.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>A szolgáltatás kód csomagok házirendek hozzárendelése
Az egy **ServiceManifestImport** **RunAsPolicy** szakaszban adja meg a fiók használva futtatható a kód csomag rendszerbiztonsági szakaszából. A felhasználói fiókok rendszerbiztonsági szakaszában a szolgáltatás jegyzék kód csomagok is társít. A telepítő vagy a fő belépési pontról adhatja meg, vagy megadhat `All` alkalmazása is. A következő példa bemutatja a különböző házirendek alkalmazva:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Ha **EntryPointType** nincs megadva, az alapértelmezett értéke EntryPointType = "Fő". Meghatározó **SetupEntryPoint** akkor különösen akkor hasznos, ha azt szeretné, bizonyos magas jogosultság beállítási művelet a rendszerfiókkal futtatására. A tényleges szolgáltatás kód futtatását is lehetővé teszi, a jogosultság alsó fiókkal.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Alapértelmezett szabály alkalmazása az összes szolgáltatás kód csomagok
A **DefaultRunAsPolicy** szakaszban adja meg az összes kód csomagokhoz, amelyeken nincs telepítve az egy adott **RunAsPolicy** definiált alapértelmezett felhasználói fiók szolgál. Ha a legtöbb szolgáltatás jegyzék az alkalmazások által használt a megadott kódot csomagot kell az adott felhasználó alatt fusson, az alkalmazás csak határozhatja meg, hogy felhasználói fiókkal alapértelmezett RunAs házirend. A következő példa megadja, hogy ha kód csomag nincs megadva **RunAsPolicy** , a kód csomag fusson a rendszerbiztonsági szakaszban megadott **MyDefaultAccount** .

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Az Active Directory tartományi csoport vagy felhasználó használatával
A Windows Server a különálló telepítővel telepített szolgáltatást háló, futtathatja a szolgáltatást a hitelesítő adatok területén az Active Directory (AD) felhasználói vagy biztonságicsoport-fiókot. Megjegyzés: Ez az Active Directory-e helyszíni Ön tartományában lévő és nem az Azure Active Directory (AAD). Tartomány-felhasználó vagy csoport használatával érheti el a tartomány (például fájlmegosztások) más erőforrások engedéllyel rendelkező engedélyeket.

A következő példa bemutatja az Active Directory felhasználója nevű *tesztfelhasználó nevet* a tanúsítvány *MyCert*nevezik titkosítva tartomány jelszavukat. Használhatja a `Invoke-ServiceFabricEncryptText` hozhat létre a titkos titkosítás szöveget Powershell-parancsot. Nézze meg ez a cikk [szolgáltatás háló alkalmazásokban titkos kulcsok kezeléséről](service-fabric-application-secret-management.md) további információt. A titkos kulcs a titkosított jelszót a tanúsítvány be kell vetni a helyi számítógép-sávon módszer (a az erőforrás-kezelő keresztül: az Azure). Ezután szolgáltatás háló a gépre üzembe helyezése a szolgáltatás csomag, esetén a titkos visszafejtése és a felhasználó nevét, valamint hitelesítést végezni, az Active Directory, a hitelesítő adatokat a futtatására.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>SecurityAccessPolicy hozzárendelése a http- és HTTPS-végpont
Ha RunAs szabály alkalmazása a szolgáltatás, és a szolgáltatás jegyzék deklarálása végpont erőforrásokat a HTTP protokollt, meg kell adnia egy **SecurityAccessPolicy** annak érdekében, hogy ezeket a végpontokat rendelt portokat megfelelően hozzáférési, amely a szolgáltatás fut a RunAs felhasználói fiók szerepel. Egyéb esetben **http.sys** nem rendelkezik hozzáféréssel a szolgáltatás, és az ügyfél el a hívások hibák. A followin példa a Customer3 fiók neve **ServiceEndpointName**, biztosítva, hogy a teljes hozzáférési joggal zárólap vonatkozik.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

A HTTPS-végpont akkor is, hogy a tanúsítványt, és térjen vissza az ügyfél nevét. Ehhez **EndpointBindingPolicy**, használja a tanúsítvány, az alkalmazás jegyzék tanúsítványok részében definiált.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>A teljes alkalmazás nyilvánvalóan példa
A következő alkalmazás jegyzék számos különböző beállítás látható:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

* [A alkalmazásmodell ismertetése](service-fabric-application-model.md)
* [Adja meg a szolgáltatás jegyzék erőforrások](service-fabric-service-manifest-resources.md)
* [Alkalmazások telepítése](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
