<properties
    pageTitle="Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-es találkozik |} Microsoft Azure"
    description="Ebből a cikkből megtudhatja, hogyan rendszergazdák beállíthatják csoportházirend ahhoz, hogy legyen a tartományhoz a vállalati hálózathoz eszközök."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény

Tartomány illesztés, a hagyományos módon szervezetek kapcsolt eszközök munka az utolsó 15 év és sok másra. Engedélyezve van a felhasználóknak, hogy jelentkezzen be az eszközök használatával a Windows Server Active Directory (Active Directory) munkahelyi vagy iskolai fiók és engedélyezett informatikai ezekre az eszközökre teljesen kezeléséhez. Szervezet általában támaszkodhat Képkezelő módszereket, amelyekkel a felhasználók rendelkezést eszközök, és általában System Center Configuration Manager (SCCM) vagy a Csoportházirend kezelése használatával őket.

Tartomány illesztés, a Windows 10, a következő előnyökkel lesz, eszközök és Azure Active Directory (Azure Active Directory) összekapcsolása után:

- Egyszeri bejelentkezés (SSO) Azure AD-erőforrások bárhonnan
- Hozzáférés a vállalati a Windows áruházból, munkahelyi vagy iskolai fiók (nincs Microsoft-fiók szükséges) használatával
- Enterprise-kompatibilis barangoló a felhasználói beállításokat minden eszközön a munkahelyi vagy iskolai fiók (nincs Microsoft-fiók szükséges)
- Erős hitelesítés és kényelmes való bejelentkezés a Microsoft Passport és a Windows Helló munkahelyi vagy iskolai fiók
- Képes elérésének korlátozására csak eszközök, amelyek megfelelnek a szervezeti eszköz csoportházirend-beállításai

## <a name="prerequisites"></a>Előfeltételek

Tartomány illesztés továbbra is hasznos lehet. Jó helyen jár, hogy az Azure Active Directory előnyei SSO juthat, barangoló munkahelyi vagy iskolai fiók és az access beállításai a munkahelyi vagy iskolai fiókkal a Windows áruházból, szüksége lesz a következő:

- Azure Active Directory-előfizetés
- Ha ki szeretné terjeszteni a helyszíni könyvtár az Azure Active Directory Azure AD Connect
- Házirendet, amely nem állított be a tartományhoz eszközök csatlakozhat az Azure Active Directory
- A Windows 10 build (build 10551 vagy újabb verziót) eszközök

Ha engedélyezni szeretné a Microsoft Passport munkahelyi telefonszámán és a Windows Helló, is szüksége lesz az alábbiakat:

- Nyilvános kulcs infrastruktúrájának esetében a felhasználó tanúsítványok kiállítási.
- System Center Configuration Manager 1509 Technical Preview verziót. További tudnivalókért olvassa el a [Microsoft System Center konfigurációs Manager Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) és a [System Center Configuration Manager csapatának blogja](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)című témakört. Ez szükséges felhasználói tanúsítványok Microsoft Passport kulcsok alapján telepítése.

A nyilvános kulcsú infrastruktúra telepítési megkövetelésének alternatívájaként a következőket végezheti el:

- A Windows Server 2016 Active Directory tartományi szolgáltatások néhány tartomány vezérlők van.

Feltételes hozzáférés engedélyezése, nincs további telepítések eszközökhöz tartományhoz elérésének csoportházirend-beállításai hozhat létre. Az eszközön a megfelelőségi alapján hozzáférés-vezérlés kezelésére, a következőkre lesz szüksége:

- System Center Configuration Manager verzió 1509 Technical Preview Passport felhasználási területei

## <a name="deployment-instructions"></a>Telepítési utasításokat.



### <a name="step-1-deploy-azure-active-directory-connect"></a>Lépés: 1: Azure Active Directory telepítése kapcsolódni

Azure AD Connect lehetővé teszi, hogy a helyszíni számítógépek kiépítése eszköz objektumként a felhőben. Azure AD Connect telepítéséhez, olvassa el a "Telepítés Azure AD Connect" [integrálása az Azure Active Directory címtárral a helyszíni identitások](active-directory-aadconnect.md#install-azure-ad-connect)című témakörben.

 - Ha követte az [egyéni telepítési az Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (nem a gyors telepítés), majd kövesse **a helyszíni Active Directory pont szolgáltatás kapcsolat létrehozása**, később az ezt a lépést.
 - Ha a szövetséges konfiguráció az Azure Active Directory, mielőtt telepítené az Azure AD Connect (Ha például telepítette az Active Directory összevonási szolgáltatások (AD FS) előtt), majd kövesse a **konfigurálása az Active Directory összevonási szolgáltatások állítást szabályok** , később ezt a lépést.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Szolgáltatás csatlakozási pontot létrehozása a helyszíni Active Directory-ban

A tartományhoz eszközök automatikus regisztrációs az Azure eszköz regisztrációs szolgáltatással idején Azure AD-bérlői információk felfedezése a szolgáltatás csatlakozási pontot fogja használni.

Az Azure AD Connect kiszolgálón futtassa az alábbi PowerShell-parancsait:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Amikor a parancsmagot $aadAdminCred Get-hitelesítő adatok, = a formátum használata *user@example.com* esetében a felhasználó nevét a hitelesítőadat megjelenésére Get-hitelesítő előugró ablakban megjelenő.

A parancsmaggal Initialize-ADSyncDomainJoinedComputerSync... fut, [*összekötő fiók neve*] cserélje a tartományi fiók, az Active Directory connector-fiókjához használt.

#### <a name="configure-ad-fs-claim-rules"></a>Active Directory összevonási szolgáltatások állítást szabályok konfigurálása
Konfigurálása az Active Directory összevonási szolgáltatások állítást szabályok lehetővé teszi, hogy a számítógép Azure eszköz regisztrációs szolgáltatással pillanatnyi regisztrációs azáltal, hogy számítógépek Kerberos/NTLM-e az Active Directory összevonási szolgáltatások használatával hitelesítést végezni. Ebben a lépésben nélkül számítógépek fog kapni az Azure Active Directory késleltetett módon (fizetnie Azure AD Connect szinkronizálási alkalommal).

>[AZURE.NOTE]
Ha nincs telepítve az Active Directory összevonási szolgáltatások, mint az összevonási kiszolgáló helyszíni, kövesse a szállító állítást szabályok létrehozásához.

Az AD FS-kiszolgáló (vagy az AD FS-kiszolgáló csatlakozik munkamenet) a következő parancsokat PowerShell:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10-es számítógépek hitelesíteni fogja aktív WS – adatvédelmi zárólap AD FS által üzemeltetett integrált Windows-hitelesítés használatával. Győződjön meg arról, hogy engedélyezve van-e a végponttal. Ha a webes hitelesítési proxykiszolgáló használata esetén is győződjön meg arról, hogy a végpont proxyn keresztül van közzétéve. Ehhez az adfs/szolgáltatások/adatvédelmi/13/windowstransport ellenőrzése. Jelenítsen meg, mivel engedélyezve van a **szolgáltatás**az Active Directory összevonási szolgáltatások management console > **Végpontok pontra**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Lépés: 2: Automatikus eszköz regisztrációs csoportházirenden keresztül konfigurálása az Active Directoryban

Az Active Directory csoportházirend segítségével az Azure AD automatikus regisztrálhatja a Windows 10-es tartományhoz eszközök konfigurálása.

> [AZURE.NOTE]
> Legújabb hogy miként állíthatja be az automatikus eszköz regisztrációs témaköröket olvassa el, [hogy miként állíthatja be az automatikus regisztráció Windows tartomány csatlakozott az Azure Active Directory eszközökhöz](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> A csoportházirend-sablonjával átnevezése a Windows 10-ben. Ha a Csoportházirend-eszköz futtatja a Windows 10-es számítógépén, mint a házirend jelennek meg: <br>
> **Regisztrálja tartományhoz csatlakoztatott számítógép eszközök**<br>
> A házirend az alábbi helyen található:<br>
> ***Számítógép sablonok/Windows-konfigurációja/házirendek/felügyeleti összetevők/eszköz regisztráció***


## <a name="additional-information"></a>További információk
* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
