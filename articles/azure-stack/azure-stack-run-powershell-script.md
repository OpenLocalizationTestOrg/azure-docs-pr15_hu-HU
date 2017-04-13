<properties
    pageTitle="Azure Papírhalom ez telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként készítheti elő a az Azure Papírhalom ez, és futtassa a PowerShell telepítése Azure Papírhalom ez."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Azure Papírhalom ez terjesztése
Az Azure Papírhalom ez üzembe helyezéséhez először kell [a telepítési gép előkészítése](#prepare-the-deployment-machine) , majd [futtassa a PowerShell telepítési parancsfájlt](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Töltse le és bontsa ki a Microsoft Azure Papírhalom ez TP2

Mielőtt nekikezdene, ellenőrizze, hogy Ön 85 GB területet.

1. Az összesítés ~ 20 GB-os következő 12 fájlokat tartalmazó zip-fájl letöltése Azure Papírhalom ez TP2 áll:
    - 1 MicrosoftAzureStackPOC.EXE
2. Tekintse át a licencszerződés mozgási sebességének és információkat a önálló készülék varázsló, és kattintson a **Tovább gombra**.
3. Tekintse át az adatvédelmi nyilatkozat képernyője és az információkat a önálló készülék varázsló, és kattintson a **Tovább**gombra.
4. Jelölje be a cél, a fájlok olvassa, kattintson a **Tovább**gombra.
    - Az alapértelmezett érték: <drive letter>:\<aktuális mappa > \Microsoft Azure Papírhalom Ez
5. Tekintse át a rendeltetési hely képernyő és információkat a önálló készülék varázsló, és kattintson a **bontsa ki** a CloudBuilder.vhdx (~44.5 GB) és a ThirdPartyLicenses.rtf fájlok kibontásához.

> [AZURE.NOTE] Miután kinyerte a fájlokat, törölheti a zip-fájl, hogy tárterületet a gépen. Vagy áthelyezheti egy másik helyre, hogy ha kell meg újratelepítése zip-fájl nem kell újra letölteni a zip-fájlok.

## <a name="prepare-the-deployment-machine"></a>A telepítési gép előkészítése

1. Győződjön meg arról, hogy fizikailag csatlakoztathatja a telepítési géphez, illetve hozzáférést fizikai console (például KVM). A telepítési gép lépésben 9 újraindítása után szüksége lesz az ilyen hozzáférést.

2. Győződjön meg arról, hogy a telepítési számítógép megfelel-e a [minimális követelmények](azure-stack-deploy.md). A [Telepítési Azure Papírhalom technikai előzetes verzió 2-ellenőrzőt](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) segítségével erősítse meg az igényeknek megfelelően alakíthatja.

3. Jelentkezzen be, a helyi rendszergazdáját, hogy ez számítógépre.

4. Másolja a CloudBuilder.vhdx fájlt C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Ha úgy dönt, hogy nem használja az ajánlott parancsprogramot a ez gazdaszámítógép (5 – 7 lépés lépéseket) előkészítésére, nem kell megadni egy licenc billentyűt az aktiválás lapon. A Windows Server 2016 kép próbaverziót része, és egy licenc kulcs beírása okoz lejárati figyelmeztető üzenet.

5. Ez a gépre futtassa az Azure Papírhalom TP2 támogatási-fájlok letöltéséről az alábbi PowerShell parancsprogramot:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    A parancsfájl letölti a az Azure Papírhalom TP2 támogató fájlokat a $LocalPath paraméter által megadott mappába.
    
6. Nyisson meg egy magasabb szintű PowerShell konzolt, és módosítsa a címtárban, amelybe másolta a fájlokat.
    - 11 MicrosoftAzureStackPOC-N.BIN (ahol N az 1-11-es)
7. Kattintson a jobb gombbal a MicrosoftAzureStackPOC.EXE > futtatása rendszergazdaként.

8. Futtassa a PrepareBootFromVHD.ps1. Ezt a parancsfájlt, és a felügyelet nélküli fájlok mellett a Szerkesztés megadott egyéb támogatási parancsfájlok érhetők el.
    Létezik a PowerShell-parancsprogramot öt paraméterei:
    - CloudBuilderDiskPath (kötelező) – a CloudBuilder.vhdx az állomáson elérési útvonalát.
    - (Nem kötelező) – DriverPath elemre koppintva további illesztőprogramokat a állomás a virtuális HD.
    - ApplyUnattend (nem kötelező) – az operációs rendszer konfigurációja automatizálhatja a Váltás a paraméter megadása Adja meg, ha a felhasználó biztosítania kell a AdminPassword konfigurálása az operációs rendszer rendszerindításkor (feltéve tartozó fájl unattend_NoKVM.xml igényel).
    Ha Ön nem használja ezt a paramétert, az általános unattend.xml fájl használják, további testreszabási nélkül. Meg kell KVM teljes testreszabási azt újraindítása után.
    - (Nem kötelező) – AdminPassword csak akkor használható, ha a ApplyUnattend paraméter értéke, hat karaktert legalább van szükség.
    - (Nem kötelező) VHDLanguage – Itt adhatja meg a virtuális nyelvét, "hu-hu" alapértelmezett értéke.
    A parancsfájl ismertetését, és tartalmazza a példa a használatra, bár a leggyakoribb használatát:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Ha a pontos parancsot futtatja, meg kell adnia a parancssorban az AdminPassword.

9. Amikor befejeződött a parancsfájlt, meg kell erősítenie az újraindítás. Ha más felhasználók-e jelentkezve, ez a parancs sikertelen lesz. Ha nem sikerül a parancs, a következő parancsot:`Restart-Computer -force` 

10. A HOST újraindítja az operációs rendszer: a CloudBuilder.vhdx, ahol a telepítő továbbra is.

> [AZURE.IMPORTANT] Azure Papírhalom férnie az internethez, vagy közvetlenül egy áttetsző proxyn keresztül. A TP2 ez telepítés pontosan egy hálózati kártya támogat a hálózathoz. Ha több, győződjön meg arról, hogy csak egy engedélyezve van (és összes többi le vannak tiltva) a következő szakaszban a telepítési parancsfájlt futtatása előtt.

## <a name="run-the-powershell-deployment-script"></a>A PowerShell telepítési parancsfájlt futtatása

1. Jelentkezzen be, a helyi rendszergazdáját, hogy ez számítógépre. Használja az előző lépésben megadott hitelesítő adatokat.

2. Nyisson meg egy magasabb szintű PowerShell konzolt.

3. A PowerShell, ez a parancs futtatásával:`cd C:\CloudDeployment\Configuration`

4. A központi telepítés parancsot:`.\InstallAzureStackPOC.ps1`

5. **Írja be a jelszót** a parancssorba írja be a jelszót, és erősítse meg. Az összes virtuális gépeken futó jelszót. Mindenképpen jegyezze meg.

6. Írja be az Azure Active Directory fiók hitelesítő adatait. Ennek a felhasználónak kell lennie a globális rendszergazdák a címtár-ös bérlői.

7. A telepítési folyamatot vehet igénybe néhány órát, amelynek során a rendszer automatikusan újraindítás egyszer.

    > [AZURE.IMPORTANT] Ha a telepítő haladásuk figyelemmel kíséréséhez, mint azurestack\AzureStackAdmin jelentkezzen be. Ha bejelentkezik a helyi rendszergazdaként után a számítógép csatlakozik a tartomány, nem látható a telepítés előrehaladását. Futtassa újra a telepítés, ne inkább rendszergazdaként jelentkezzen be azurestack\AzureStackAdmin futó érvényesítéséhez.

    A telepítési létrejött, a PowerShell konzol jeleníti meg: **Kész: "Telepítés" művelet**.

    Ha a telepítés meghiúsul, próbálja meg [újra futtathatja](azure-stack-rerun-deploy.md). Vagy meg [újratelepítése](azure-stack-redeploy.md) is azt az alapoktól.

### <a name="deployment-script-examples"></a>Telepítési parancsfájlt példák

Ha a AAD személyazonosságát csak egy adott AAD könyvtár társított:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Ha nagyobb, mint egy AAD Directory társított AAD személyazonosságát:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Ha a környezeti nincs DHCP engedélyezve, tartalmaznia kell a következő további paraméterek (például meghatározott használatát) felett a lehetőségek közül:

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Választható paraméterek InstallAzureStackPOC.ps1

| Paraméter | Kötelező és választható | Leírás |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Nem kötelező. | Állítja be az Azure Active Directory-felhasználónevet és jelszót. Ezeket a Azure hitelesítő adatokat a szervezeti Azonosítóval vagy a Microsoft-Account lehet. Szeretné használni a Microsoft Account hitelesítő adatait, hagyja ki ezt a paramétert a parancsmagot a. Az Azure-hitelesítés előugró ablakban a paraméter kimarad kéri a telepítés során. Ez a telepítés során a hitelesítési és frissítési tokenek hoz létre. |
| AADDirectoryTenantName | Szükséges | A bérlői könyvtár akkor állítja be. Használja ezt a paramétert egy adott könyvtárat szeretne megadni, ahol az AAD fiók kezelése több könyvtárak engedéllyel rendelkezik-e. Teljes név formátuma egy AAD címtár-bérlői webhelyet <directoryName>. onmicrosoft.com. |
| AdminPassword | Szükséges | A helyi rendszergazdafiók és más felhasználói fiókok állítja be az összes a virtuális gépeken létrehozott ez telepítés részeként. Erre a jelszóra egyeznie kell az aktuális helyi rendszergazdai jelszót az állomáson. |
| AzureEnvironment | Nem kötelező. | Jelölje ki a kívánt regisztrálhatja a Azure Papírhalom telepítési Azure környezetben. A választható lehetőségek *Nyilvános Azure*és az *Azure - kínai*, *Azure - Amerikai Egyesült Államok kormányzati*. |
| EnvironmentDNS | Nem kötelező. | A DNS-kiszolgáló létrejön az Azure Papírhalom telepítés részeként. Ahhoz, hogy a megoldás a időbélyeg kívüli nevek belül számítógépek, adja meg a meglévő infrastruktúra DNS-kiszolgálójába. A bélyegző a DNS-kiszolgáló ismeretlen név felbontás kérések ennek a kiszolgálónak továbbítja. |
| NatIPv4Address | Kötelező DHCP hálózati Címfordítást ikon | Az m/m-BGPNAT01 akkor állítja be a statikus IP-címet. Csak akkor használja ezt a paramétert, ha a DHCP nem rendelhet felhasználókhoz egy érvényes IP-címet az Internet eléréséhez. |
| NatIPv4DefaultGateway | Kötelező DHCP hálózati Címfordítást ikon | Állítja be az alapértelmezett átjáró m/m-BGPNAT01 a statikus IP-címet is használható. Csak akkor használja ezt a paramétert, ha a DHCP nem rendelhet felhasználókhoz egy érvényes IP-címet az Internet eléréséhez.  |
| NatIPv4Subnet | Kötelező DHCP hálózati Címfordítást ikon | IP-alhálózat előtag DHCP hálózati Címfordítást támogatási fölé. Csak akkor használja ezt a paramétert, ha a DHCP nem rendelhet felhasználókhoz egy érvényes IP-címet az Internet eléréséhez.  |
| PublicVLan | Nem kötelező. | Beállítja a virtuális azonosítóját. Csak akkor használja ezt a paramétert, ha a host és m/m-BGPNAT01 konfigurálnia kell a virtuális azonosító elérheti a fizikai hálózati (és az Internet). Ha például`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Ismétlése | Nem kötelező. | Ez a jelző segítségével futtassa újra a telepítés.  Az összes előző beviteli használják. Adatok ismételt beírására korábban megadott mivel több egyedi értékek telepítéshez használt és keletkezett nem támogatott. |
| TimeServer | Nem kötelező. | Ha meg kell adnia egy adott időkiszolgáló, használja ezt a paramétert. |

## <a name="next-steps"></a>Következő lépések

[Azure Papírhalom csatlakoztatása](azure-stack-connect-azure-stack.md)
