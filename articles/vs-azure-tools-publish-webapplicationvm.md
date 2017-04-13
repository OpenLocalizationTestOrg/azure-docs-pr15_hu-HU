<properties
   pageTitle="Közzététel WebApplicationVM |} Microsoft Azure"
   description="Megtudhatja, hogy miként üzembe egy webalkalmazás virtuális géphez. Ha nem léteznek a parancsfájl a szükséges erőforrások az Azure előfizetés hoz létre."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Közzététel-WebApplicationVM (Windows PowerShell-parancsprogramot)

Üzembe helyezése a webalkalmazás virtuális géphez. Ha nem léteznek a parancsfájl a szükséges erőforrások az Azure előfizetés hoz létre.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguráció

A részletek, a telepítés leíró JSON-konfigurációs fájl elérési útja.

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|Igaz|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

### <a name="subscriptionname"></a>SubscriptionName

Szeretné a virtuális gép létrehozása az Azure előfizetés neve.

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|Az első előfizetés az előfizetés fájlt használja|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

### <a name="webdeploypackage"></a>WebDeployPackage

A virtuális gép közzététele a webhelyen telepítőcsomag elérési útja. A csomag Visual Studio webhely közzététele varázslóval hozhat létre. Lásd: [Útmutató: a webes telepítőcsomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

### <a name="allowuntrusted"></a>AllowUntrusted

IGAZ, ha engedélyezi, amely nem egy megbízható legfelső szintű hitelesítésszolgáltató alá tanúsítványokat használatát.

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|hamis|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

### <a name="vmpassword"></a>VMPassword

A virtuális gép fiók hitelesítő adatait. Példa: - VMPassword @{Name = "felügyeleti"; Jelszó = "jelszó"}

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Az Azure SQL-adatbázis hitelesítő adatait. Példa: - DatabaseServerPassword @{Name = "felügyeleti"; Jelszó = "jelszó"}

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

IGAZ, ha nyomtatása a forgatókönyv a kimeneti adatfolyam üzenetek.

|Aliasok|nincs lehetőség|
|---|---|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|hamis|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

## <a name="remarks"></a>Megjegyzések:

Egy teljes magyarázat, hogyan használhatja a parancsfájl létrehozása fejlesztők és a próba-környezetben, [Használja a Windows PowerShell-parancsfájlokat történő közzététel a fejlesztők és a próba-környezetben](vs-azure-tools-publishing-using-powershell-scripts.md)talál.

A JSON konfigurációs fájl adja meg, hogy mi az, hogy telepíthető részleteit. A projekt, például a nevet, affinitás csoport, virtuális kép és a virtuális gép méretének létrehozásakor megadott információkat tartalmazza. Azt is magában foglalja a virtuális gépen az adatbázisok létrehozására, a végpontok, ha vannak ilyenek, valamint a webes telepítési paramétereket. A következő kód egy példa JSON fájl szemlélteti:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

A JSON konfigurációs fájl módosíthatja, milyen már kiépítve szerkesztheti. Szükség egy virtuális gép és egy felhőalapú szolgáltatásba, de az adatbázis szakasz nem kötelező.
