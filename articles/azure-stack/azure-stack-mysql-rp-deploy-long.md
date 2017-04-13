<properties
    pageTitle="A MySQL erőforrás szolgáltatót Azure Papírhalom telepítése |} Microsoft Azure"
    description="Azure Papírhalom szeretne telepíteni, a MySQL-erőforrás szolgáltató részletes lépéseket."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>A MySQL-erőforrás szolgáltató Azure Papírhalom webalkalmazás használata a terjesztése

> [AZURE.NOTE] Az alábbi információk csak az Azure Papírhalom TP1 telepítések vonatkozik.

Ez a cikk a lépések részletes a MySQL-erőforrás szolgáltató beállításának meg fogalmat (Ez) Azure Papírhalom igazolása így Azure Papírhalom, beleértve a kódmentes WordPress webhelyek MySQL használata a [MySQL-adatbázisokhoz](azure-stack-mysql-rp-deploy-short.md) elkezdheti készült [Azure Web Apps alkalmazások](azure-stack-webapps-deploy.md)használatát.

## <a name="set-up-steps-before-you-deploy"></a>Állítsa be a lépéseket, mielőtt beállítaná

Mielőtt beállítaná az erőforrás-szolgáltató, akkor kell:

- A .NET 3.5-ös, egy alapértelmezett Windows Server kép
- Az Internet Explorer (IE) fokozott biztonság kikapcsolása
- Azure PowerShell legújabb verziójának telepítése

### <a name="create-an-image-of-windows-server-including-net-35"></a>Hozzon létre a Windows Server, beleértve a .NET 3.5-ös képe

Kihagyhatja ezt a lépést, ha letöltötte az Azure Papírhalom bittel 2/23/2016 után, mert az alapértelmezett alap Windows Server 2012 R2 kép tartalmaz 3.5-ös .NET keretrendszer letöltéséhez és újabb verzióiban.

Ha 2/23/2016 előtt töltötte le, létre kell hoznia egy Windows Server 2012 R2 adatközponthoz virtuális .NET 3.5-ös képpel és beállítása a Platform kép adattárban az alapértelmezett kép.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>IE kikapcsolása fokozott biztonság és engedélyezése cookie-k használata

Egy erőforrás-szolgáltató üzembe helyezéséhez futtatja a PowerShell integrált parancsfájlok környezet (ISE) rendszergazdaként, így a cookie-kat és a JavaScript engedélyezése az Internet Explorer profilban használatával jelentkezzen be az Azure Active Directory-rendszergazda, és a felhasználó bejelentkezési bővítmények kell.

**Internet Explorer kikapcsolása fokozott biztonság:**

1. Jelentkezzen be az Azure Papírhalom vásárlási a fogalom az (Ez) számítógépre rendszergazdaként AzureStack /, és nyissa meg a Kiszolgálókezelő.

2. Kapcsolja ki az **Internet Explorer fokozott biztonság beállításai** rendszergazdáknak és a felhasználóknak.

3. Jelentkezzen be rendszergazdaként a **ClientVM.AzureStack.local** virtuális gép, és nyissa meg a Kiszolgálókezelő.

4. Kapcsolja ki az **Internet Explorer fokozott biztonság beállításai** rendszergazdáknak és a felhasználóknak.

**Cookie-k engedélyezése:**

1. A Windows kezdőképernyőjét kattintson a **minden alkalmazás**gombra, kattintson a **Windows Kellékek menüpontra**, kattintson a jobb gombbal **Az Internet Explorer**, mutasson a **További**, és válassza a **Futtatás rendszergazdaként**.

2. Ha a rendszer kéri, jelölje be a **javasolt biztonsági**, és kattintson **az OK**gombra.

3. Az Internet Explorerben, kattintson a **Tools (fogaskerék) ikonra** , a &gt; **Internetbeállítások** &gt; **Adatvédelem** fülre.

4. Kattintson a **Speciális kategóriára**, győződjön meg arról, hogy mindkét **Elfogadás** gomb kijelölve, kattintson az **OK gombra**, és kattintson ismét az **OK gombra** .

5. Zárja be az Internet Explorerben, és indítsa újra a PowerShell ISE rendszergazdaként.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Azure PowerShell-Azure Papírhalom kompatibilis kiadásának telepítéséhez

1. Távolítsa el az ügyfél virtuális bármelyik meglévő Azure PowerShell.

2. Jelentkezzen be rendszergazdaként AzureStack/az Azure Papírhalom ez számítógépen.

3. Távoli asztali változatában, jelentkezzen be a **ClientVM.AzureStack.local** virtuális gép rendszergazdaként.

4. Nyissa meg a Vezérlőpultot, kattintson **a program eltávolítása** &gt; kattintson az **Azure PowerShell** &gt; kattintson az **Eltávolítás**gombra.

5. [A legújabb Azure Powershellt, amely támogatja az Azure Papírhalom töltse le](http://aka.ms/azstackpsh) és telepítse az eszközt.

    Miután telepítette a PowerShell, ez az ellenőrzés PowerShell-parancsprogramot, győződjön meg arról, hogy tud csatlakozni az Azure Papírhalom példányához (bejelentkezési weblapról jelenjen) futtathatja.

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Az erőforrás-szolgáltató telepítési PowerShell betöltő

1. Csatlakozás az Azure Papírhalom Ez távoli asztali clientVm.AzureStack.Local és azurestack rendszergazdaként jelentkezzen be\\azurestackuser.

2. [Töltse le a MySQL-RP bináris](http://aka.ms/masmysqlrp) fájl- és csomagolja ki ez D:\\MySQLRP.

3. Futtassa a D:\\MySQLRP\\Bootstrap.cmd fájl rendszergazdaként (azurestack\administrator).

    Ezzel megnyitja a Bootstrap.ps1 fájl PowerShell ISE.

4. Betöltése a windows PowerShell ISE befejeztével kattintson a "Lejátszás" gombra, vagy nyomja le az F5 billentyűt.

    Két fő lapok fogja tölteni, minden egyes tartalmazó a parancsprogramokat és a fájlokat a MySQL-erőforrás szolgáltatót telepíteni kell.

## <a name="prepare-prerequisites"></a>Felkészülés a vonatkozó követelmények

Kattintson a **Előfeltételek Készítsünk** lapra:

- Szükséges tanúsítványok létrehozása
- Töltse le a MySQL-bináris az Azure jegyzettömbhöz
- Töltse fel a eltérések Azure Papírhalom egy tároló fiókkal
- Gyűjtemény elemek közzététele

### <a name="create-the-required-certificates"></a>A szükséges tanúsítványok létrehozása
A **New-SslCert.ps1** parancsprogram hozzáadása a \_. A D: való AzureStack.local.pfx az SSL-tanúsítvány\\MySQLRP\\Előfeltételek\\BlobStorage\\tároló mappát. A tanúsítvány védelemmel látja el, az erőforrás-szolgáltató és a helyi példányt az Azure erőforrás-kezelő közötti kommunikációt.

1. **Előfeltételek Készítsünk** fő lapján kattintson a **New-SslCert.ps1** fülre, és indítsa el.

2. A megjelenő kérdésnél írja be a titkos kulcs, és **Jegyezze fel a jelszó**védő PFX jelszóval. Újabb mobiltelefon szükséges.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>Töltse le a MySQL-bináris az Azure jegyzettömbhöz

1. Kattintson a **Letöltés-MySqlServer.ps1** fülre, és indítsa el.
2. Amikor a rendszer kéri, kattintson az **Igen gombra** a megerősítése párbeszédpanelen fogadja el a LICENCSZERZŐDÉST.

    Ez a parancs két zip-fájlok hozzáadása a D:\MySql\Prerequisites\BlobStorage\Container mappát.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Az összes eltérések feltöltése egy Azure Papírhalom tároló fiókkal

1. Kattintson a **Feltöltés-Microsoft.MySql-RP.ps1** fülre, és indítsa el.

2. A Windows PowerShell hitelesítő adatok kérelem párbeszédpanelen írja be az Azure Papírhalom szolgáltatás rendszergazdai hitelesítő adatait.

3. Amikor a rendszer kéri az Azure Active Directory bérlői Azonosítóját, írja be az Azure Active Directory bérlői tartománynevét: például microsoftazurestack.onmicrosoft.com.

    Előugró ablak a hitelesítő adatokat kér.

    > [AZURE.TIP] Az előugró nem jelenik meg, ha, vagy nem kapcsolta ki IE fokozott biztonság JavaScript engedélyezése a számítógép és a felhasználó, vagy még nem elfogadott a cookie-kat IE. Lásd: a [lépéseket, mielőtt beállítaná állíthat be](#set-up-steps-before-you-deploy).

4. Írja be az Azure Papírhalom szolgáltatás rendszergazdai hitelesítő adataival, és kattintson a **Bejelentkezés**gombra.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Újabb erőforrás elrejtésével gyűjtemény elemek közzététele

Kattintson a **Közzététel-GalleryPackages.ps1** fülre, és indítsa el. A parancsfájl két piactér elemek ad az Azure Papírhalom ez portál piactér piactér elemként adatbázis-erőforrások üzembe használható.

## <a name="deploy-the-mysql-resource-provider-vm"></a>A MySQL-erőforrás-szolgáltató virtuális terjesztése

Most, hogy az Azure Papírhalom ez szükséges tanúsítványok és piactér elemek készített, telepítheti egy szolgáltató SQL Server erőforrás. Kattintson az **üzembe MySQL-szolgáltató** lapra:

   - Adja meg a telepítési folyamatot hivatkozó JSON fájlban értékeket
   - Az erőforrás-szolgáltató terjesztése
   - A helyi DNS frissítése
   - Regisztráljon az SQL Server erőforrás szolgáltató kártya

### <a name="provide-values-in-the-json-file"></a>Adja meg az értékeket a JSON-fájlban

Kattintson a **Microsoft.MySqlprovider.Parameters.JSON**. Ezzel a fájllal rendelkezik az erőforrás-kezelő Azure-sablon az Azure jegyzettömbhöz megfelelően üzembe kell, hogy paramétereket.

1. Töltse ki a JSON-fájl **üres** paraméterei:

    - Győződjön meg arról, hogy Ön megadja a **adminusername** és **adminpassword** a MySQL-erőforrás szolgáltató virtuális.

    - Ellenőrizze, hogy az [Előkészítés prequisites](#prepare-prerequisites) lépésben egy megjegyzés az elvégzett **SetupPfxPassword** paraméterhez adja meg a jelszót.

    - Győződjön meg arról, adja meg a **basicAuthUserName** és **basicAuthPassword** paramétereket. **Jegyezze fel ezeket az értékeket.** Meg kell őket, hogy később az erőforrás-szolgáltató regisztrálni.

2. Kattintson a **Mentés**gombra.

### <a name="deploy-the-resource-provider"></a>Az erőforrás-szolgáltató terjesztése

1. Kattintson a **központi telepítés-Microsoft.Mysql-provider.PS1** fülre, és futtassa a.
2. Az Azure Active Directory, amikor a rendszer kéri, írja be a bérlői webhely neve.
3. Az előugró ablakban elküldése az Azure Papírhalom szolgáltatás rendszergazdai hitelesítő adataival.

A teljes példányban eltarthat néhány nagyon területekre Azure Papírhalom POCs 15 és 45 percet között.

### <a name="update-the-local-dns"></a>A helyi DNS frissítése

1. Kattintson a **Register-Microsoft.MySQL-fqdn.ps1** fülre, és futtassa a.
2. Amikor a rendszer kéri az Azure Active Directory bérlői azonosítója, a bemeneti az Azure Active Directory bérlői webhely teljes tartománynevét: például **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Regisztráljon az SQL-RP erőforrás-szolgáltató

1. Kattintson a **Register-Microsoft.My-provider.ps1** fülre, és futtassa a.

2. Hitelesítő adatokat kér, használja a **basicAuthUserName** és **basicAuthPassword** paraméterként jegyezni.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Ellenőrizze a példányban az Azure Papírhalom portál használatával

1. Jelentkezzen ki a ClientVM, és rendszergazdaként jelentkezzen be ismét **AzureStack\User**.

2. **Azure Papírhalom ez portálon** kattintson az asztalon, és jelentkezzen be a portálra a szolgáltatás rendszergazdaként.

3. Győződjön meg arról, hogy a telepítés sikeres volt. Kattintson a **Tallózás gombra** &gt; **Erőforráscsoport**, kattintson a használt erőforráscsoport (az alapértelmezés **MySQLRP**), és győződjön meg arról, hogy a lap (felső fele) essentials részét felolvassa **sikeres telepítési**.


4. Győződjön meg arról, hogy a regisztráció sikeres volt. Kattintson a **Tallózás** &gt; **erőforrás szolgáltatók**, és keresse meg a **MySQL-helyi**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>A központi telepítés tesztelése az első MySQL-adatbázis létrehozása

1. Jelentkezzen be az Azure Papírhalom ez portálra szolgáltatás rendszergazdaként.

2. Kattintson a **+** gomb &gt; **egyéni** &gt; **MySQL-kiszolgáló és az adatbázisokat**.

3. Töltse ki az űrlapot az adatbázis adataival.

    **Jegyezze fel a beírt "kiszolgáló neve".** A kapcsolatok karakterláncot, az adatbázis tartalmazza a "kiszolgálónév" részeként a felhasználónevet: például ** "user@ <ServerName>"**. Meg kell az adatbázishoz való csatlakozáskor a felhasználónév az ilyen formátumú szövegbeviteli: például ha telepíti a MySQL-webhelyen az Azure webhely erőforrás-szolgáltató használata


## <a name="next-steps"></a>Következő lépések

Próbáljon meg más [PaaS szolgáltatások](azure-stack-tools-paas-services.md) , például az [erőforrás-szolgáltató SQL Server](azure-stack-sql-rp-deploy-short.md) és a [Web Apps alkalmazások erőforrás szolgáltató](azure-stack-webapps-deploy.md).
