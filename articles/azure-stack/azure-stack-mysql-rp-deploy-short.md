<properties
    pageTitle="MySQL-adatbázis használata a Azure Papírhalom PaaS |} Microsoft Azure"
    description="Ismerje meg a MySQL-erőforrás szolgáltató telepíthető, és adja meg a MySQL Azure Papírhalom a szolgáltatás a rövid lépéseket."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>A Azure Papírhalom PaaS MySQL-adatbázis használata

> [AZURE.NOTE] Az alábbi információk csak az Azure Papírhalom TP1 telepítések vonatkozik.

Azure Papírhalom a MySQL erőforrás szolgáltató telepítheti. Után az erőforrás-szolgáltató telepít, létrehozhat MySQL-kiszolgáló és az adatbázisok telepítési-sablonok Azure erőforrás-kezelő eszközzel, és adja meg a MySQL-adatbázis szolgáltatásként. MySQL-adatbázisok, amelyek megegyeznek a webhelyeken, sok webhely platformokon támogatja. Telepíti az erőforrás-szolgáltató, vagy létrehozhat WordPress webhelyek a webalkalmazások Azure platform a megegyezik egy szolgáltatást (PaaS) bővítmény az Azure Papírhalom.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>A rövid lépéseket követve az erőforrás-szolgáltató terjesztése
Ha már ismert Azure Papírhalom, az alábbi lépésekkel. Ha ezután további információra kíváncsi, kövesse az egyes szakasz hivatkozásait, vagy egyenesen az [Azure Papírhalom ez MySQL adatbázis erőforrás szolgáltató kártya Deploy](azure-stack-mysql-rp-deploy-long.md).

1.  Győződjön meg arról, hogy [az összes beállítási lépések elvégzése](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) az erőforrás-szolgáltató telepítése előtt:

    - Az alap Windows Server képen 3.5-ös .NET-keretrendszer már be van állítva. (Ha február, 23, 2016, miután letöltötte az Azure Papírhalom bittel kihagyhatja ezt a lépést.)
    - [Ez az Azure Papírhalom kompatibilis Azure PowerShell egy kiadását telepítve van](http://aka.ms/azStackPsh).
    - Az Internet Explorer biztonsági beállításait a ClientVM [engedélyezve van az Internet Explorer fokozott biztonság ki van kapcsolva, és a cookie-k](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Töltse le a MySQL-erőforrás szolgáltató bináris fájlt](http://aka.ms/masmysqlrp) , és bontsa ki, meg fogalmat (Ez) igazolása az Azure egymást fedő a ClientVM.

3. [Bootstrap.cmd és a parancsfájlok futtatásához](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Parancsfájlok csoportja, amely két fő lapok csoportosítva megnyílik a a PowerShell integrált parancsfájlok környezet (ISE). Futtassa a betöltött parancsfájlok sorrendben balról jobbra az egyes lapokon.

    1. Parancsfájlok futtatása **Előkészítés** lapján balról jobbra:

        - Az erőforrás-szolgáltató és Azure erőforrás-kezelő közötti biztonságos kommunikációt helyettesítő tanúsítvány létrehozása.
        - Fogadja el a MySQL-EULA szerződést, és töltse le a MySQL-bináris.
        - Töltse fel a tanúsítványok és minden más eltérések Azure Papírhalom tárterület-fiókkal.
        - Tár-csomagok közzététele, hogy a MySQL-erőforrások a gyűjtemény keresztül telepítheti.

        > [AZURE.IMPORTANT] A parancsfájlok bármelyikét lefagyhat ok az Azure Active Directory-ös bérlői webhelyen elküldése után a biztonsági beállítások előfordulhat, hogy nem blokkolja-e egy DLL futtatásához a telepítéshez szükséges. A probléma megoldásához az erőforrás-szolgáltató mappába a Microsoft.AzureStack.Deployment.Telemetry.Dll keres, kattintson jobb gombbal, válassza a **Tulajdonságok parancsot**, és kattintson az **Általános** lapon jelölje be a **Tiltás feloldása** .

    2. Parancsfájlok futtatása a **központi telepítés** lapon balról jobbra:

        - [Deploy (virtuális) virtuális gép](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , hogy az erőforrás-szolgáltató, MySQL-kiszolgáló és az adatbázisok, hogy a program a elindítását egyaránt fog tárolni. Ez a parancsfájl a paraméter módosítania kell az egyes értékek a parancsfájl futtatása előtt JSON fájlok hivatkozik.
        - [Regisztrálhatja a helyi DNS-rekord](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) , amely az erőforrás-szolgáltató virtuális rendeli.
        - [Az erőforrás-szolgáltató regisztrálni](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) a helyi Azure erőforrás-kezelő használatával.

        > [AZURE.IMPORTANT] Az összes parancsfájl feltételezik, hogy az alap operációs rendszer kép megfelel-e a vonatkozó követelmények (.NET 3.5-ös telepítette, JavaScript-fájlok és a ClientVM és Azure PowerShell telepítve van a legújabb engedélyezett cookie-k). Ha hiba, amikor a parancsfájlt, győződjön meg róla, hogy teljesülnek-e a előfeltételek.

5. [Az új MySQL-erőforrás szolgáltató tesztelése](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment)telepítse az Azure Papírhalom portálról MySQL-adatbázishoz. Kattintson a **Létrehozás** gombra &gt; **egyéni** &gt; **MySQL-kiszolgáló és az adatbázis**.

Ez a MySQL-erőforrás szolgáltató be- és körülbelül 25 perc alatt fut szerezheti be.
