<properties
   pageTitle="Az egyéni felhasználók alkalmazások közzététele Azure RemoteApp gyűjteményben (előzetes verzió) |} Microsoft Azure"
   description="Megtudhatja, hogyan közzéteheti alkalmazások felhasználónként, hanem függően a csoportok, az Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Az egyéni felhasználók alkalmazások közzététele Azure RemoteApp gyűjteményben (előzetes verzió)

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A cikk ismerteti, hogy miként teheti közzé az egyes felhasználók az alkalmazások Azure RemoteApp gyűjteményben. Az Azure RemoteApp, az új funkciók jelenleg "magánjellegű preview", és csak használható jelölje ki a korai bevezetés kipróbálás céljából.

Az eredetileg a Azure RemoteApp engedélyezett "közzététel" alkalmazások csak egy lehetőség: a rendszergazda tenné közzé a képet az alkalmazások és a webhelycsoport összes felhasználója számára látható lenne.

A leggyakoribb helyzet, hogy sok alkalmazás szerepeltetni egy kép, és egy webhelycsoport üzembe felügyeletéhez kapcsolódó költségek csökkentése érdekében. Oftentimes nem minden alkalmazás szempontjából fontos összes felhasználó – rendszergazdák szeretné az egyes felhasználók alkalmazások közzététele, így azok nem látható a felesleges alkalmazások saját hírcsatorna alkalmazásban.

Ez jelenleg most az Azure RemoteApp – lehetséges korlátozott előzetes szolgáltatásként. Az alábbiakban az új funkciók rövid összefoglalása:

1. Egy webhelycsoport beállítható, hogy két üzemmódok valamelyikében be:
 
  - az eredeti "webhelycsoport mód", ahol a webhelycsoport összes felhasználója láthatja az összes közzétett alkalmazásokat. Ez az alapértelmezett módját.
  - az új "alkalmazás mód", ahol felhasználók csak látni beállított alkalmazásokat kifejezetten hozzájuk rendelt

2. Jelenleg az alkalmazás mód csak engedélyezhető Azure RemoteApp PowerShell-parancsmagok használata.

  - Ha az alkalmazás mód értéke, felhasználói kiosztása a gyűjteményben nem kezelhetők az Azure-portálra. Felhasználói kiosztása rendelkezik PowerShell-parancsmagok kezelhetők.

3. Felhasználók csak fogják látni az alkalmazások közzétenni közvetlenül az őket. Azonban továbbra is lehet a felhasználó számára érhető el, a képen a többi alkalmazást elindításához történő elérésekor választható őket közvetlenül az operációs rendszer lehetséges.
  - Ez a funkció nem található egy biztonságos zárolhatja az alkalmazások; csak korlátozza a láthatósági hírcsatorna alkalmazásban.
  - Ha alkalmazásokból felhasználók elkülönítése kell szüksége lesz külön gyűjtemények használni, amely.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Azure RemoteApp PowerShell-parancsmagok beszerzése

Próbálja ki az új kép funkciók, szüksége lesz Azure PowerShell-parancsmagok használata. Ez jelenleg nem lehet az új alkalmazás közzétételi mód engedélyezése az Azure adatkezelési portál használatával.

Először győződjön meg arról, hogy telepítve van az [Azure PowerShell-modult](../powershell-install-configure.md) .

Ezután indítsa el a rendszergazdai módban PowerShell-konzol, és futtassa a következő parancsmagot:

        Add-AzureAccount

Kérni fogja Azure felhasználónevét és jelszavát. Miután bejelentkezett, lesz az Azure előfizetések Azure RemoteApp parancsmagok futtatása.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>A webhelycsoport melyik mód van ellenőrzése

Futtassa a következő parancsmagot:

        Get-AzureRemoteAppCollection <collectionName>

![Jelölje be a fizetési mód](./media/remoteapp-perapp/araacllelvel.png)

A AclLevel tulajdonságot beállíthatja, hogy az alábbi értékeket:

- Gyűjtemény: az eredeti közzétételi módban. Minden felhasználó látja, hogy minden közzétett alkalmazások.
- Alkalmazás: az új közzétételi módja. A felhasználók csak azokat az alkalmazásokat, közvetlenül az őket közzétett látják.

## <a name="how-to-switch-to-application-publishing-mode"></a>Hogyan válthat az alkalmazás közzétételi mód

Futtassa a következő parancsmagot:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Alkalmazás közzétételi állapot megmaradnak: először minden felhasználó látni fogja az összes az eredeti közzétett alkalmazásokat.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Ki láthatja az adott alkalmazás Felhasználólista hogyan

Futtassa a következő parancsmagot:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Ez megjeleníti, hogy ki láthatja az alkalmazás minden felhasználónak.

Megjegyzés: Megjelenik az alkalmazás aliases (a "alkalmazás alias" a fenti szintaxis neve) futtassa a Get-AzureRemoteAppProgram - Lekérdezés_neve <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Hozzárendelése egy felhasználóhoz, az alkalmazások

Futtassa a következő parancsmagot:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

A felhasználó megjelenik az alkalmazás az Azure RemoteApp ügyfélprogramban, és is csatlakozni tudnak.

## <a name="how-to-remove-an-application-from-a-user"></a>Az alkalmazások eltávolításáról a felhasználó

Futtassa a következő parancsmagot:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Tételével
Nagyra értékeljük a visszajelzését és előnézeti funkció vonatkozó javaslatokat. Kérjük, adja meg a [felmérés](http://www.instant.ly/s/FDdrb) ossza meg velünk véleményét.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Még nem volt a névjegyadatok próbálja meg az megtekintése funkció?
Ha Ön nem részt vesznek az előnézet még, használja a [felmérés](http://www.instant.ly/s/AY83p) hozzáférést kérni.
