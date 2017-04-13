<properties 
pageTitle="Felhőalapú szolgáltatások szerepkör config XPath csal lap |} Microsoft Azure" 
description="A különböző XPath-beállításokat is használhatja a felhőalapú szolgáltatás szerepkör config elérhetővé teheti beállítások környezeti változó." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>XPath-környezet változó elérhetővé teheti szerepkör konfigurációs beállítások

A felhőalapú szolgáltatás dolgozó vagy a webes szerepkör szolgáltatás definíciós fájl elérhetővé tenni futtatókörnyezet konfigurációs értékek környezet változók. A következő XPath értékek támogatottak (amelyek megfelelnek az API-értékeket).

XPath mintaértékek is elérhetők a [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) tár keresztül. 

## <a name="app-running-in-emulator"></a>Alkalmazás irányító fut

Azt jelzi, hogy az alkalmazás a irányító fut-e.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Kód  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Telepítési azonosító

A telepítési azonosító példány veszi.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Kód  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Szerepkör-azonosító 

Olvassa be a példány aktuális szerepkör Azonosítóját.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kód  | var id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Tartomány frissítése

A frissítés tartomány-példány veszi.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kód  | var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Hibafa-tartomány

Olvassa be a példány hibafa tartományát.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kód  | var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Szerepkör neve

Olvassa be a példányok szerepkör nevét.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kód  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Konfigurációs beállítás

Olvassa be a megadott kereséskonfigurációs beállítás értékét.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Kód  | var beállítás = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Helyi tároló elérési út

Olvassa be a példány helyi tároló elérési útját.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Kód  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Helyi tárhely

Olvassa be a példány helyi tároló méretét.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Kód  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Végpont Protocol (protokoll) 

Olvassa be a példány végpont Protocol (protokoll).

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kód  | var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protocol (protokoll); |

## <a name="endpoint-ip"></a>Végpont IP

A megadott végpont IP-cím kap.

| Típus | Példa |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kód  | var cím = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Végpont port 

Olvassa be a végpont port példány.

| Típus  | Példa |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kód  | a port var = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Példa

Íme egy példa létrehoz egy indítási tevékenység nevű környezeti változó dolgozó szerepet `TestIsEmulated` beállítása a [ @emulated xpath érték](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) fájlt.

Hozzon létre egy [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) csomagot.

Engedélyezze a [távoli asztali](cloud-services-role-enable-remote-desktop.md) szerepkörhöz.
