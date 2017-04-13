<properties
    pageTitle="A Windows Azure Active Directory tartományhoz eszközökhöz automatikus regisztrációs beállítása |} Microsoft Azure"
    description="Állítsa be a tartományhoz tartozó Windows eszközök automatikusan és csendes regisztrálhatja az Azure Active Directory címtárral."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Automatikus regisztráció Windows Azure Active Directory eszközökhöz tartományhoz tartozó beállítása

[Azure Active Directory eszköz alapuló feltételes hozzáférés](active-directory-conditional-access.md)használatához Windows tartományhoz csatlakoztatott számítógép rendelkeznie kell regisztrált az Azure Active Directory (Azure Active Directory). Ebben a cikkben megtudhatja, mit kell tennie beállítása a Windows Azure AD a szervezet tartományhoz eszközökhöz nyilvántartása.

Az Azure Active Directory feltételes access programmal is biztosít előnyei:

-   Az Azure Active Directory-alkalmazások munkahelyi vagy iskolai fiók segítségével felület továbbfejlesztett egyszeri bejelentkezés (SSO)
-   Vállalati barangoló beállítások minden eszközön
-   A Windows áruházból elérését a vállalati verzió
-   Erősebb hitelesítési és kényelmes jelentkezzen be Windows Helló

> [AZURE.NOTE] A Windows 10 November frissítésének néhányat a fokozott felhasználói élmény az Azure Active Directory kínál, de a Windows 10 évforduló frissítés teljes mértékben támogatja a feltételes access eszköz-alapú. Feltételes access kapcsolatos további tudnivalókért olvassa el a [feltételes Azure Active Directory eszköz-alapú hozzáférés](active-directory-conditional-access.md)című témakört. A munkahelyi és a felhasználók hogyan a Windows 10-es eszköz regisztrálja Azure AD a Windows 10-es eszközök kapcsolatos további tudnivalókért lásd [a Windows 10, a nagyvállalati: eszközök használata munkahelyi](active-directory-azureadjoin-windows10-devices-overview.md).

Rögzítheti az egyes Windows rendszerben, például az alábbi verziókat korábbi verziói:

-   Windows 8.1
-   Windows 7 rendszerben

Ha a Windows Server számítógéppel, asztali, regisztrálhatja a következő platformokon:

- A Windows Server 2016-ban
- A Windows Server 2012 R2
- A Windows Server 2012
- A Windows Server 2008 R2 rendszerben


## <a name="prerequisites"></a>Előfeltételek

A tartományhoz eszközök segítségével Azure AD automatikus regisztrációs fő kötelező, hogy naprakész verziójának Azure Active kapcsolódás könyvtárban (Azure AD Connect).

Attól függően, hogy helyi frissítés vagy egy expressz vagy egyéni telepítési használt, és hogyan telepítette az Azure AD Connect az alábbi előfeltételek előfordulhat, hogy konfigurálva vannak-e automatikusan:

-   A **szolgáltatás csatlakozási pontot a helyszíni Active Directory**. Az Azure Active Directory bérlői adatok azokon a számítógépeken feltárás, hogy regisztráljon az Azure Active Directory.

-   **Active Directory összevonási szolgáltatások (AD FS) kiállítási átalakítása szabályokat**. A regisztráció (szövetséges konfigurációk vonatkozik) a számítógép-hitelesítést.

Ha egyes eszközök esetén a szervezetnél nem a Windows 10 tartományhoz csatlakoztatott eszközök, győződjön meg arról, hogy az alábbi műveletek végezhetők:

- Állított be egy házirendet az Azure Active Directory, hogy a felhasználók eszközalapú regisztrálhatnak
- Többtényezős hitelesítés, az Active Directory összevonási szolgáltatások érvényes alternatívájaként a integrált Windows-hitelesítés (IWA) beállítása



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>A az Azure AD-bérlő feltáráshoz szolgáltatás csatlakozási pontot beállítása

A szolgáltatás csatlakozási pontot objektum kell a konfigurációban helyi partíciót a számítógép tartományhoz hol tartomány erdő elnevezési. A szolgáltatás csatlakozási pontot hol számítógépek regisztrálni a Azure AD-bérlő feltárás információkat tartalmazza. A több erdőre az Active Directory-konfigurációja a szolgáltatás csatlakozási pontot kell találhatók, minden számítógép tartományhoz van.

A szolgáltatás csatlakozási pontot ezekre a helyekre, az Active Directory meg:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Az Active Directory tartományi neve *example.com*a elnevezési környezet konfigurációja egy akinek nem CN = Configuration, Adatközpont például Adatközpont = = hu.

Az alábbi Windows PowerShell-parancsprogramot használatával az Azure Active Directory bérlői objektum és feltárás értékek ellenőrizni. (A példában a konfigurációs elnevezési környezetében cserélje a konfigurációs elnevezési helyi.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

A `$scp.Keywords` kimeneti az Azure Active Directory-bérlői információkat jeleníti meg:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Ha a szolgáltatás csatlakozási pontot nem létezik, hozza létre az alábbi PowerShell parancsprogramot az Azure AD Connect kiszolgálón futó:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Futtatásakor `$aadAdminCred = Get-Credential`, a formázás *user@example.com* esetében a felhasználó nevét a **Get-hitelesítő adatok** párbeszédpanelen.  
> A Initialize-ADSyncDomainJoinedComputerSync parancsmag futtatásakor [*összekötő fiók neve*] cserélje a tartományi fiók, amely az Active Directory-összekötő fiók van használatban.  
> A parancsmaggal használja a Powershellhez Active Directory modul egy tartományvezérlőnek az Active Directory webszolgáltatásokhoz támaszkodik. Az Active Directory webes szolgáltatások Windows Server 2008 R2 és újabb verziók tartomány vezérlők használata támogatott. Tartomány-vezérlők Windows Server 2008 és korábbi verzióiban a System.DirectoryServices API PowerShell használatával hozzon létre a szolgáltatás csatlakozási pontot, és rendelje a kulcsszavak értékeket.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Active Directory összevonási szolgáltatások szabályok létrehozása a azonnali eszköz regisztrációs szövetséges szervezetekben

A szövetséges Azure-ban AD konfigurálás eszközök a Active Directory összevonási szolgáltatások (vagy a helyszíni összevonási kiszolgáló) használja arra az Azure Active Directory hitelesítést végezni. Ezután regisztrálja ellen Azure Active Directory eszköz regisztrációs (Azure Active Directory eszköz regisztrációs) szolgáltatást.

> [AZURE.NOTE] Nem szövetséges konfigurációban felhasználói jelszót tiltva a rendszer szinkronizálja az Azure Active Directory, és a Windows 10 és Windows Server 2016 tartományhoz tartozó számítógép hitelesítést az Azure Active Directory eszköz regisztrációs szolgáltatás. Egy felhasználó hitelesíti használatával a hitelesítő adatait, amelyek a felhasználói írja be a helyszíni fiókok, és amelyek van továbbítani Azure Active Directory Azure AD Connect keresztül. A Windows 10 és Windows Server 2016 számítógépek nem szövetséges konfiguráció esetén a szervezet a eszköz-alapú hitelesítésszolgáltató megadására szolgáló lehetőség közül választhat. További információ a szakasz töltse le a Windows Installer csomagok Ez a cikk a Windows 10 számítógépek témakörben találhat.

A Windows 10 és Windows Server 2016 számítógépek Azure AD Connect társít az eszköz objektum az Azure Active Directory a helyi számítógép-objektumot. A következő Jogcímalapú hitelesítés Azure Active Directory eszköz regisztrációs szolgáltatása számára a regisztrálás és eszköz-objektum létrehozása során kell létezik:

- http://schemas.microsoft.com/ws/2012/01/AccountType, amely azonosítja a tartományhoz tartozó számítógép fő hitelesítő DJ értéket tartalmazza.
- http://schemas.microsoft.com/Identity/Claims/onpremobjectguid a helyszíni számítógépfiók **objectGUID** attribútum értéket tartalmazza.
- http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarysid a számítógép elsődleges biztonsági azonosítója (biztonsági AZONOSÍTÓK), amely megfelel a **objectSid** attribútum érték a helyszíni számítógépfiók tartalmazza.
- http://schemas.microsoft.com/ws/2008/06/Identity/Claims/issuerid Azure AD meg az Active Directory összevonási szolgáltatások vagy a a helyszíni biztonsági jogkivonat szolgáltatás (STS) kibocsátott jogkivonathoz használó értéket tartalmazza. Az Active Directory több erdőre konfigurációban fontos. Ebben a konfigurációban számítógépeken előfordulhat, hogy azok kívül azt, amely a csatlakozik, az Active Directory összevonási szolgáltatások Azure AD hozzá, vagy a helyszíni STS erdőhöz. Az Active Directory összevonási szolgáltatások, írja be az értéket http://< a*tartománynév*>/adfs/szolgáltatások/adatvédelmi /, ahol <*tartománynév*> annak az Azure Active Directory érvényesített tartomány nevét.

Segítségével hozhat létre szabályok manuálisan, az Active Directory összevonási szolgáltatások, az alábbi PowerShell parancsprogramot, amely a kiszolgáló csatlakozik munkamenetben. Cserélje ki az első sor érvényesített szervezeti tartománynevét az Azure Active Directory.

> [AZURE.NOTE] Ha nem használja az Active Directory összevonási szolgáltatások összevonási kiszolgáló helyszíni, útmutatás a forgalmazó az ezt a problémát a következő követelések szabályok létrehozásához.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] A környezet helyszíni erdőn esetén, használja a csak az első három szabályt. Ha a számítógépek amelyek egynél az Azure Active Directory szinkronizálása egy másik tartományban találhatók, vagy a használja az üzenetekhez, a szinkronizálási beállításokat az alternatív nevek, fel kell venni a hátralévő szabályokat.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] A Windows 10 és Windows Server 2016 tartományhoz csatlakoztatott számítógép használatával IWA aktív WS – adatvédelmi zárólap AD FS által üzemeltetett hitelesítést végezni. Győződjön meg róla, hogy a végpont aktív. A webes alkalmazás Proxy használatakor ügyeljen arra, hogy a végpont proxyn keresztül van közzétéve. Az endpoint adfs/szolgáltatások/adatvédelmi/13/windowstransport. Ellenőrizze, hogy nem aktív, az Active Directory összevonási szolgáltatások kezelőkonzol használja a **szolgáltatás** > **Végpontok pontra**. Ha nem használja az Active Directory összevonási szolgáltatások összevonási kiszolgáló helyszíni, útmutatás a szállítójával győződjön meg arról, hogy a megfelelő végpont aktív.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Active Directory összevonási szolgáltatások eszköz regisztráció hitelesítés beállítása

Győződjön meg arról, hogy IWA többtényezős hitelesítés eszköz regisztrációhoz az AD FS érvényes alternatívájaként van-e beállítva. Ehhez egy szabályt, amely a hitelesítési módszer áthaladt alakítás kiállítási van szükség.

1. Az Active Directory összevonási szolgáltatások kezelése konzolban, nyissa meg **az AD FS** > **Megbízhatósági kapcsolatok** > **Fél megbízik használna**.

2. Kattintson a jobb gombbal a Microsoft Office 365-ben identitás Platform megbízó fél meghatalmazási objektumot, és válassza a **Szerkesztés állítást szabályok**.

3.  A **Kiállítási átalakítása szabályok** lapon jelölje be a **Szabály hozzáadása**.

4.  A **szabályhoz állítás** sablont listában válassza a **küldése követelések egyéni szabály alkalmazásával**.

5.  Kattintson a **Tovább gombra**.

6.  Lévő **igényt szabály neve** mezőbe írja be a **Hitelesítési módszer állítást szabály**.

7.  A **szabályhoz állítás** mezőben adja meg ezt a parancsot:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Az összevonási kiszolgálón írja be a PowerShell-parancsot:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> az Azure Active Directory megbízó felek adatvédelmi objektum megbízó fél objektum neve. Az objektum általában nevű *Microsoft Office 365-ben identitás Platform*.



## <a name="deployment-and-rollout"></a>Telepítési és exportálása

Ha a tartományhoz tartozó számítógép felel meg az Előfeltételek, készen állnak a regisztrálása az Azure Active Directory.

A következő alkalommal az eszköz újraindítása, vagy ha a felhasználó bejelentkezik a Windows Azure AD automatikus regisztrálhatja a Windows 10 évforduló frissítés és a Windows Server 2016 tartományhoz tartozó számítógép. Azure AD, amikor az eszköz újraindítása, a tartomány join művelet után regisztrálja új számítógépekre, amelyek a tartományhoz.

> [AZURE.NOTE] A Windows 10 tartományhoz csatlakoztatott számítógép automatikusan regisztrálása Azure Active Directory, csak akkor, ha a bevezetés csoportházirend-objektum van-e beállítva.

Csoportházirend-objektum segítségével szabályozhatja a Windows 10 és Windows Server 2016 tartományhoz csatlakoztatott számítógép automatikus bejegyzése bevezetésének. A Windows 10 tartományhoz csatlakoztatott számítógép automatikus regisztrációs bevezetésére, választott számítógépre telepítheti a Windows Installer-csomag.

> [AZURE.NOTE] Bevezetés vezérlő csoportházirend is elindítja a regisztráció Windows 8.1 tartományhoz csatlakoztatott számítógép. A házirend-rögzítése a Windows 8.1 tartományhoz csatlakoztatott számítógép is használhatja. Vagy Windows-verzió, köztük a Windows 7 vagy Windows Server-verziók kombinálja esetén rögzítheti a Windows 10 és Windows Server 2016 számítógépek Windows Installer-csomag használatával.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>A automatikus regisztrációs bevezetésének szabályozhatja a csoportházirend-objektum létrehozása

Ha szabályozni szeretné a számítógép tartományhoz az Azure AD automatikus regisztrációs bevezetésének, a rögzíteni kívánt számítógépekre telepítheti a **tartományhoz tartozó számítógép eszköz regisztrálni** csoportházirend. Ha például telepítheti a házirend szervezeti egység vagy egy olyan biztonsági csoportot.

A házirend beállításához kövesse az alábbi lépéseket:

1. Nyissa meg a Kiszolgálókezelő, és ezután válassza az **eszközök** > **A Csoportházirend kezelése**.

2. Nyissa meg a tartomány csomópontot, amely megfelel a tartományt, amelyhez automatikus regisztráció Windows 10-es vagy Windows Server 2016 számítógép aktiválása.

3. Kattintson a jobb gombbal a **Csoportházirend-objektumok**, és válassza az **Új**gombra.

4. Írja be a csoportházirend-objektum nevét. Ha például *Azure AD automatikus regisztráció*. Kattintson **az OK gombra**.

4. Kattintson a jobb gombbal az új csoportházirend-objektumot, és válassza a **Szerkesztés**gombra.

5. Nyissa meg a **Számítógép konfigurációja** > **házirendek** > **Felügyeleti sablonok** > **Windows-összetevők** > **eszköz regisztrációs**. Kattintson a jobb gombbal a **Register tartomány eszközök számítógépek csatlakozott**, és válassza a **Szerkesztés**gombra.

    > [AZURE.NOTE] A csoportházirend-sablonjával átnevezése a Csoportházirend kezelése konzolt korábbi verzióival. Ha a konzol egy korábbi verzióját használja, válassza a **Számítógép konfigurációja** > **házirendek** > **Felügyeleti sablonok** > **Windows-összetevők** > **Munkahelye illesztés** > **automatikusan munkahelye illesztés ügyfélszámítógépek**.

6. Jelölje ki a **engedélyezve**, és válassza az **Alkalmaz**.

7. Kattintson **az OK gombra**.

8. A csoportházirend-objektum csatolása a kívánt helyre. Ha például hozzákapcsolhatja azt egy adott szervezeti egységre. Akkor is hivatkozással tehette azt Azure AD automatikus regisztrálása számítógépen meghatározott biztonsági csoportját. A tartomány csoportházirend-objektum csatolása a házirendet a tartományhoz a Windows 10-es és a Windows Server 2016 számítógépeknek a szervezet beállításához.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>A Windows 10 számítógépek Windows Installer csomagok letöltése  

Regisztrálja a tartományhoz tartozó futtató Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2, töltse le és telepítse a Windows Installer-csomag (MSI) fájlok:

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

A csomag telepítéséhez például System Center Configuration Manager szoftver terjesztési rendszert használ. A csomag támogatja a normál Csendes telepítési beállítások a *Csendes* paraméterrel. System Center Configuration Manager 2016 további előnyökkel jár felajánlja a korábbi verzióinak, például kész regisztrációk megkeresését. További tudnivalókért lásd: [System Center 2016-ban](https://www.microsoft.com/en-us/cloud-platform/system-center).

A telepítő ütemezett feladat a rendszer a felhasználó környezetében futó hoz létre. A tevékenység induljanak, amikor a felhasználó bejelentkezik a Windows. A tevékenység IWA keresztül hitelesítését követően a felhasználói hitelesítő adatokkal Azure AD meg automatikusan regisztrálja az eszközön. Az ütemezett feladat megtekintéséhez nyissa meg **a Microsoft** > **Munkahelye csatlakozni**, és keresse meg a Feladatütemező könyvtár.



## <a name="next-steps"></a>Következő lépések

- [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access.md)
