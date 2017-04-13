<properties
   pageTitle="Közzététel-WebApplicationWebSite (Windows PowerShell-parancsprogramot) |} Microsoft Azure"
   description="Megtudhatja, hogy miként webes projekt közzététele egy Azure webhelyre. Ha nem léteznek a parancsfájl a szükséges erőforrások az Azure előfizetés hoz létre."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Közzététel-WebApplicationWebSite (Windows PowerShell-parancsprogramot)

##<a name="syntax"></a>Szintaxis

Webes projektet egy Azure webhelyet tesz közzé. Ha nem léteznek a parancsfájl a szükséges erőforrások az Azure előfizetés hoz létre.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguráció

A részletek, a telepítés leíró JSON-konfigurációs fájl elérési útja.

|Paraméter|Alapérték|
|---|---|
|Aliasok|nincs lehetőség|
|Szükség?|Igaz|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

## <a name="subscriptionname"></a>SubscriptionName

Az Azure szeretne létrehozni a webhelyét, az előfizetés neve.

|Paraméter|Alapérték|
|---|---|
|Aliasok|nincs lehetőség|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

## <a name="webdeploypackage"></a>WebDeployPackage

A webes telepítőcsomag közzététele a webhelyen elérési útja. A csomag Visual Studio webhely közzététele varázslóval hozhat létre. További tudnivalókért lásd: [első lépések a Azure Cloud Services és az ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Paraméter|Alapérték|
|---|---|
|Aliasok|nincs lehetőség|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Felhasználónév és jelszó az Azure SQL-adatbázis.

|Paraméter|Alapérték|
|---|---|
|Aliasok|nincs lehetőség|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|nincs lehetőség|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

IGAZ, ha nyomtatása a forgatókönyv a kimeneti adatfolyam üzenetek.

|Paraméter|Alapérték|
|---|---|
|Aliasok|nincs lehetőség|
|Szükség?|hamis|
|Pozíció|névvel ellátott|
|Alapérték|hamis|
|Fogadja el a beviteli folyamat?|hamis|
|Fogadja el a helyettesítő karakterek?|hamis|

## <a name="remarks"></a>Megjegyzések:

Egy teljes magyarázat, hogyan használhatja a parancsfájl létrehozása fejlesztők és a próba-környezetben, [Használja a Windows PowerShell-parancsfájlokat történő közzététel a fejlesztők és a próba-környezetben](vs-azure-tools-publishing-using-powershell-scripts.md)talál.

A JSON konfigurációs fájl adja meg, hogy mi az, hogy telepíthető részleteit. A projekt, például a nevét és felhasználónevét, a webhely létrehozásakor megadott információkat tartalmazza. Is az adatbázis kiépítése, ha van ilyen. A következő kód egy példa JSON fájl szemlélteti:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Módosíthatja a JSON konfigurációs fájl módosításához telepítve van. Egy webhely szakaszában szükség, de az adatbázis szakasz nem kötelező.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért olvassa el a [Közzététel-WebApplicationVM (Windows PowerShell-parancsprogramot)](vs-azure-tools-publish-webapplicationvm.md) című témakört.
