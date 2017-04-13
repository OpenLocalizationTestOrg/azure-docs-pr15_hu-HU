<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Hibaelhárítási útmutatójának |} Microsoft Azure"
    description="Útmutató a Microsoft Azure Active Directory tartományi szolgáltatások – hibaelhárítás"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure Active Directory tartományi szolgáltatások – hibaelhárítási útmutatója
Ebben a cikkben hibaelhárítási tippek a állíthat be és Azure Active Directory (AD) tartományi szolgáltatások felügyelete előfordulhatnak problémák.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Nem engedélyezi az Azure Active Directory Azure Active Directory tartományi szolgáltatások
Ez a szakasz segítséget nyújt a hibák elhárítása, ha a címtárában az Azure Active Directory tartományi szolgáltatások engedélyezése sikertelen, vagy állítottuk vissza a "Letiltva" kap-e be.

Válassza ki a hibaelhárítási lépések, amelyek megfelelnek a fel, amelyekkel hibaüzenet.

|**Hibaüzenet jelenik meg**|**Megoldás**|
|---|:---|
|*A név contoso100.com már használja a hálózaton. Adja meg, amely nem használja.*|[A virtuális hálózaton tartomány névütközés](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Tartományi szolgáltatások nem engedélyezhető a Azure AD-ös bérlői. A szolgáltatás nem rendelkezik a megfelelő engedélyekkel az alkalmazás neve "Azure Active Directory tartományi szolgáltatások szinkronizálás". Az alkalmazás neve "Azure Active Directory tartományi szolgáltatások szinkronizálás" törlése, és próbálkozzon újra a számítógépet a Azure AD-bérlő tartományi szolgáltatások engedélyezése.*|[Tartományi szolgáltatások nincs megfelelő engedélye az Azure Active Directory tartományi szolgáltatások szinkronizálási alkalmazás](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Tartományi szolgáltatások nem engedélyezhető a Azure AD-ös bérlői. A tartományi szolgáltatások-alkalmazás a Azure AD-bérlő nincsenek tartományi szolgáltatások engedélyezése a szükséges engedélyekkel. Törölje az alkalmazás azonosítója d87dcbc6-a371-462e-88e3-28ad15ec4e64 az alkalmazást, és próbálkozzon újra a számítógépet engedélyezheti a Azure AD-bérlő tartomány szolgáltatásokat.*|[A tartomány Services alkalmazás nincs megfelelően konfigurálva a bérlői webhelyén](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Tartományi szolgáltatások nem engedélyezhető a Azure AD-ös bérlői. A Microsoft Azure Active Directory-alkalmazás le van tiltva a Azure AD-bérlő. Engedélyezze az alkalmazás azonosítója 00000002-0000-0000-c000-000000000000 az alkalmazást, és próbálkozzon újra a számítógépet engedélyezheti a Azure AD-bérlő tartomány szolgáltatásokat.*|[A Microsoft Graph-alkalmazás le van tiltva a Azure AD-bérlő](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Tartomány névütközés
**Hibaüzenet jelenik meg:**

*A név contoso100.com már használja a hálózaton. Adja meg, amely nem használja.*

**Remediation:**

Győződjön meg arról, hogy az azonos nevű tartományban érhető el, hogy virtuális hálózaton meglévő tartomány nem rendelkezik. Tegyük fel például, már elérhető "contoso.com" nevű a a kijelölt virtuális hálózat tartományt. Később megpróbálja engedélyezni az Azure Active Directory tartományi szolgáltatások felügyelt tartomány azonos nevű tartomány (például contoso.com), hogy virtuális hálózaton. Hibát tapasztal meg az Azure Active Directory tartományi szolgáltatások.

Ez a hiba oka az, hogy a tartomány nevét a virtuális hálózat az ütközések feloldása. Ebben az esetben az Azure Active Directory tartományi szolgáltatások felügyelt tartomány beállításának kell használnia egy másik nevet. Másik megoldás, vonja kiépítése a meglévő tartomány, és ezután folytassa Azure Active Directory tartományi szolgáltatások engedélyezése.


### <a name="inadequate-permissions"></a>Megfelelő engedélyei
**Hibaüzenet jelenik meg:**

*Tartományi szolgáltatások nem engedélyezhető a Azure AD-ös bérlői. A szolgáltatás nem rendelkezik a megfelelő engedélyekkel az alkalmazás neve "Azure Active Directory tartományi szolgáltatások szinkronizálás". Az alkalmazás neve "Azure Active Directory tartományi szolgáltatások szinkronizálás" törlése, és próbálkozzon újra a számítógépet a Azure AD-bérlő tartományi szolgáltatások engedélyezése.*

**Remediation:**

Ellenőrizze, hogy van-e az alkalmazások az Azure Active directory a "Azure Active Directory tartományi szolgáltatások szinkronizálás" nevű. Ha az alkalmazás létezik, törölje azt, majd ismételt engedélyezése Azure Active Directory tartományi szolgáltatások.

A következő lépésekkel ellenőrizheti az alkalmazás jelenlétét és törölheti azt, ha az alkalmazás létezik:

  1. Nyissa meg az **Azure klasszikus portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Jelölje ki az **Active Directory** -csomópontot, a bal oldali ablaktáblában.
  3. Jelölje ki az Azure Active Directory bérlői webhely (könyvtár), amelynek az Azure Active Directory tartományi szolgáltatások engedélyezése szeretné.
  4. Nyissa meg az **alkalmazások** fülre.
  5. Válassza a legördülő menü a **vállalaton tulajdonosa alkalmazások** lehetőséget.
  6. Ellenőrizze, hogy nevű **Azure Active Directory tartományi szolgáltatások szinkronizálási**alkalmazás. Ha az alkalmazás létezik, folytassa a törölheti azt.
  7. Miután törölte az alkalmazást, próbálja meg ismét engedélyezni az Azure Active Directory tartományi szolgáltatások.


### <a name="invalid-configuration"></a>Érvénytelen konfiguráció
**Hibaüzenet jelenik meg:**

*Tartományi szolgáltatások nem engedélyezhető a Azure AD-ös bérlői. A tartományi szolgáltatások-alkalmazás a Azure AD-bérlő nincsenek tartományi szolgáltatások engedélyezése a szükséges engedélyekkel. Törölje az alkalmazás azonosítója d87dcbc6-a371-462e-88e3-28ad15ec4e64 az alkalmazást, és próbálkozzon újra a számítógépet engedélyezheti a Azure AD-bérlő tartomány szolgáltatásokat.*

**Remediation:**

Ellenőrizze, hogy esetén egy "AzureActiveDirectoryDomainControllerServices" (ha az alkalmazás azonosítója, amelyet d87dcbc6-a371-462e-88e3-28ad15ec4e64) nevű alkalmazást az Azure Active directory-ban. Ha az alkalmazás létezik, kell törölheti és ismételt engedélyezése Azure Active Directory tartományi szolgáltatások.

Az alábbi PowerShell parancsprogramot segítségével keresse meg az alkalmazást, és törölheti azt.

> [AZURE.NOTE] A parancsfájl **Azure Active Directory a 2-es verziójú PowerShell** -parancsmagok. Egy teljes listát az összes rendelkezésre álló parancsmagok és a modul letöltésére olvassa el a [AzureAD PowerShell hivatkozás dokumentációt](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>A Microsoft Graph letiltása
**Hibaüzenet jelenik meg:**

Tartományi szolgáltatások nem engedélyezhető a Azure AD-ös bérlői. A Microsoft Azure Active Directory-alkalmazás le van tiltva a Azure AD-bérlő. Engedélyezze az alkalmazás azonosítója 00000002-0000-0000-c000-000000000000 az alkalmazást, és próbálkozzon újra a számítógépet engedélyezheti a Azure AD-bérlő tartomány szolgáltatásokat.

**Remediation:**

Ellenőrizze, hogy ha le van tiltva a azonosítójú 00000002-0000-0000-c000-000000000000 kérelmet. Ennek az alkalmazásnak a Microsoft Azure Active Directory-alkalmazás, és Graph API hozzáférést biztosít a Azure AD-bérlő. Azure Active Directory tartományi szolgáltatások kell szinkronizálni a Azure AD-bérlő a felügyelt tartományára aktívak az alkalmazás.

Ez a hiba megoldásához alkalmazás engedélyezése, és próbálkozzon újra a számítógépet a Azure AD-bérlő tartományi szolgáltatások engedélyezése.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>A felhasználók nem tudnak bejelentkezni az Azure Active Directory tartományi szolgáltatások felügyelt tartomány
Ha a Azure AD-bérlő az egy vagy több felhasználó nem tud bejelentkezni az újonnan létrehozott felügyelt tartomány, hajtsa végre az alábbi hibaelhárítási lépéseket:

- **Bejelentkezés (UPN) formátumban:** Bejelentkezéskor a (UPN) formátumban (például 'joeuser@contoso.com') az SAMAccountName formátum (CONTOSO\joeuser) helyett. Az SAMAccountName előfordulhat, hogy automatikusan jönnek létre felhasználók amelynek UPN előtag túl hosszú, vagy egy másik felhasználó felügyelt tartományban megegyezik. Az egyszerű Felhasználónévi formátum garantálja az Azure AD-bérlő belül egyedinek kell lennie.

> [AZURE.NOTE] Azt javasoljuk, hogy jelentkezzen be az Azure Active Directory tartományi szolgáltatások felügyelt tartományt a (UPN) formátumban.

- Ellenőrizze, hogy a [Jelszó-szinkronizálás engedélyezve](active-directory-ds-getting-started-password-sync.md) megfelelően a az első lépések útmutató című témakörben ismertetett lépéseket.

- **Külső partnerek:** Győződjön meg arról, hogy a szóban forgó felhasználói fiók nem jelenik meg az Azure Active Directory bérlői webhely külső fiók. Külső partnerek közé tartozik a Microsoft-fiókok (például 'joe@live.com') vagy külső felhasználói fiókok Azure Active directory. Azure Active Directory tartományi szolgáltatások nincs ilyen felhasználói fiók hitelesítő adatait, mivel ezek a felhasználók nem tud bejelentkezni a felügyelt tartományban.

- **Fiókok szinkronizált:** Ha a szóban forgó felhasználói fiókokat helyszíni Directoryból vannak szinkronizálva, ellenőrizze az alábbiakat:
    - Telepítve van, vagy frissülnek a [legújabb ajánlott kiadását Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - [A teljes szinkronizálást](active-directory-ds-getting-started-password-sync.md)az Azure AD Connect állította be.

    - Attól függően, hogy a címtárában méretét esetlegesen felhasználói fiókok időre lenne szükség, és a hitelesítő adatok elérhetővé szeretne tenni az Azure Active Directory tartományi szolgáltatások tiltva. Győződjön meg róla, mielőtt elég ideig hitelesítés (attól függően, hogy a címtár - napjára néhány órát, vagy két nagy könyvtárak méretének) ismétlése várakozás.

    - Ha ellenőrzése a fenti lépések után a probléma továbbra is fennáll, indítsa újra a Microsoft Azure AD-szinkronizálás szolgáltatás. A szinkronizálási számítógépről elindítani a parancssort, és hajtsa végre az alábbi parancsokat:
      1. "A Microsoft Azure Active Directory-szinkronizálás" nettó leállítása
      2. nettó indítása "A Microsoft Azure Active Directory-szinkronizálás"

- **Felhőben fiókok**: Ha a szóban forgó felhasználói fiók felhőben felhasználói fiókhoz, gondoskodjon arról, hogy a felhasználó módosította a jelszavát, miután engedélyezte az Azure Active Directory tartományi szolgáltatások. Ebben a lépésben hatására a hitelesítő adatok tiltva az Azure Active Directory tartományi szolgáltatások jöjjön szükséges.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>A felhasználók a Azure AD-bérlő eltávolítja a program nem távolítja el a felügyelt tartomány
Azure Active Directory megakadályozza, hogy a felhasználó objektumok véletlen törlését. Ha a Azure AD-bérlő töröl egy felhasználói fiókot, a megfelelő felhasználói objektum kerül a Lomtárba. A törlési művelet a rendszer szinkronizálja a felügyelt tartomány, esetén a megfelelő felhasználói fiók letiltott megjelölni. Ez a funkció segít a törlés, a felhasználói fiók később visszavonása vagy visszaállítása.

Eltávolítása a felhasználói fiók teljes körű felügyelt tartománya, törölje a felhasználó végleges a Azure AD-bérlő. Az Eltávolítás-MsolUser PowerShell-parancsmag a használata a "-RemoveFromRecycleBin" jelölőnégyzete, a [MSDN-cikk](https://msdn.microsoft.com/library/azure/dn194132.aspx)útmutatását.


## <a name="contact-us"></a>Kapcsolatfelvétel
Lépjen kapcsolatba az Azure Active Directory tartományi szolgáltatások termékfejlesztők híreit a [visszajelzés megosztása és a támogatási] (aktív-címtár-ds-ügyfél-us.md).
