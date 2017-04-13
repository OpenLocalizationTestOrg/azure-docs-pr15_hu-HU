<properties
    pageTitle="Azure Papírhalom szolgáltatás App Technical Preview 1 megkezdése előtt |} Microsoft Azure"
    description="Azure Papírhalom a Web Apps telepítése előtt végrehajtásához szükséges lépések"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Az Azure Papírhalom Web Apps alkalmazások megkezdése előtt

> [AZURE.NOTE] Az alábbi információk csak az Azure Papírhalom TP1 telepítések vonatkozik.

Néhány dolog, telepítse az Azure Papírhalom Web Apps alkalmazások lesz szüksége:

- Egy kész telepítési az [Azure Papírhalom technikai előzetes verzió 1](azure-stack-run-powershell-script.md)
- Elég hely az Azure papírhalom online kisvállalati telepítés Azure Papírhalom rendszerben.  A szükséges terület nagyjából 20GB szabad lemezterület is.
- [Az SQL Server rendszerű kiszolgáló](#SQL-Server).

>[AZURE.NOTE] Az alábbi lépéseket minden a ügyfél virtuális alapján történik.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Azure Papírhalom Web Apps alkalmazások telepítése előtt

Egy erőforrás-szolgáltatót telepíteni, futtatnia kell a PowerShell integrált parancsfájlok Environment(ISE) rendszergazdaként. Emiatt kell cookie-kat és a JavaScript engedélyezése az Internet Explorer profilban jelentkezzen be az Azure Active Directory használt.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Az Internet Explorer fokozott biztonság kikapcsolása

1.  Jelentkezzen be az Azure Papírhalom vásárlási a fogalom az (Ez) rendszergazdaként **AzureStack /**, és nyissa meg **A Kiszolgálókezelő**.
2.  Kapcsolja ki a **Biztonsági beállításai** rendszergazdáknak és a felhasználóknak.
3.  Jelentkezzen be rendszergazdaként ClientVM.AzureStack.local virtuális gépen, és nyissa meg **A Kiszolgálókezelő**.
4.  Kapcsolja ki a **Biztonsági beállításai** rendszergazdáknak és a felhasználóknak.

## <a name="enable-cookies"></a>Cookie-k engedélyezése

1.  Válassza a **Start**> **minden alkalmazás**> **Windows Kellékek**. Kattintson a jobb gombbal **Az Internet Explorer** > **További** > **futtatása rendszergazdaként**.
2.  Ha a program kéri, válassza a **javasolt biztonsági**, és kattintson **az OK**gombra.
3.  Az Internet Explorerben válassza az **eszközök** (a fogaskerék ikon) > **Internetbeállítások** > **adatvédelmi** > **Speciális**.
4.  Válassza a **Speciális**. Győződjön meg arról, hogy mindkét **Elfogadás** jelölőnégyzetek van jelölve, és válassza **az OK gombra** , és **az OK gombra** újra.
5.  Zárja be az Internet Explorerben, és indítsa újra a PowerShell ISE rendszergazdaként.

## <a name="install-the-latest-version-of-azure-powershell"></a>Azure PowerShell legújabb verziójának telepítése

1.  Jelentkezzen be az Azure Papírhalom ez számítógép ****AzureStack/rendszergazdaként.
2.  A távoli asztali kapcsolat használatával jelentkezzen be rendszergazdaként ClientVM.AzureStack.local virtuális gépen.
3.  Nyissa meg a **Vezérlőpultot** , és válassza **a program eltávolítása**. 
4.  Kattintson a jobb gombbal a **Microsoft Azure PowerShell - November 2015** , és válassza az **Eltávolítás**parancsot.
5.  Miután távolítsa el a befejeződik, indítsa újra a ClientVM.AzureStack.local virtuális gépen
6.  Töltse le és telepítse a legújabb [Azure PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>Az SQL Server

Alapértelmezés szerint az Azure Papírhalom Web Apps alkalmazások telepítőt használatára van beállítva, hogy telepítve van a együtt Papírhalom SQL Azure erőforrás-szolgáltató az SQL Server. Ha telepíti az SQL Server erőforrás-szolgáltató (SQL RP) Győződjön meg róla, akkor ajánljuk a figyelmébe az adatbázis rendszergazdai felhasználónevével és jelszavával. Szükséges mindkét Azure Papírhalom Web Apps alkalmazások telepítése során.
Megjegyzés: Is megkapja a telepíthető, és futtassa az SQL Server egy másik kiszolgáló használata lehetőséget. Megadhatja, hogy az elérhető beállítások, a Azure Papírhalom Web Apps alkalmazások telepítő befejezésekor használata SQL Server-példányt.
