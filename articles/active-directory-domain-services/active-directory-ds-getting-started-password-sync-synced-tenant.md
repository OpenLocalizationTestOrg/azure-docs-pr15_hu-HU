<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Jelszó-szinkronizálás engedélyezése |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások – első lépések"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások jelszó-szinkronizálás engedélyezése
Megelőző tevékenységek, az Azure Active Directory tartományi szolgáltatások az Azure Active Directory-bérlői webhelyen engedélyezett. A következő feladatra, hogy az Azure Active Directory tartományi szolgáltatások jelszó-szinkronizálás engedélyezése. Miután hitelesítőadat-szinkronizálás be van állítva, felhasználók is jelentkezzen be a felügyelt tartomány a vállalati hitelesítő adataival.

Lépései eltérőek alapján, hogy a szervezet van-e a felhőben Azure Active Directory bérlői vagy értéke szinkronizál a rendszer a helyszíni címtárában Azure AD Connect használatával.

<br>

> [AZURE.SELECTOR]
- [Felhőben Azure AD-bérlői](active-directory-ds-getting-started-password-sync.md)
- [Azure AD-bérlő szinkronizált](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Feladat 5: Engedélyezi a jelszó-szinkronizálás AAD tartományi szolgáltatásokhoz való egy szinkronizált Azure AD-bérlő
A szinkronizált Azure AD-bérlő szinkronizálása az Azure AD Connect használata a szervezet helyszíni Directoryval van beállítva. Azure AD Connect nem szinkronizálja a NTLM és a hitelesítő adatok Kerberos tiltva az Azure Active Directory alapértelmezés szerint. Azure Active Directory tartományi szolgáltatások használatához szükséges konfigurálása szinkronizálása NTLM és Kerberos-hitelesítés szükséges hitelesítő adatok tiltva Azure AD Connect. Az alábbi lépéseket a szükséges hitelesítő adatok tiltva az Azure Active Directory-bérlőhöz szinkronizálás engedélyezése.


### <a name="install-or-update-azure-ad-connect"></a>Vagy Azure AD Connect frissítés telepítése
Telepítse a legújabb ajánlott megjelenési időpontjához Azure AD Connect tartományhoz csatlakoztatott számítógép. Ha egy meglévő példányának Azure AD Connect beállítási, frissítenie kell, hogy az Azure AD Connect legújabb verzióját használja. Ismert problémák/hibákról, előfordulhat, hogy már rögzített elkerülése érdekében győződjön meg róla, mindig használata az Azure AD Connect legújabb verzióját.

**[Letöltés: Azure AD csatlakoztatása](http://www.microsoft.com/download/details.aspx?id=47594)**

Ajánlott verzió: **1.1.281.0** – 2016 szeptember 7 közzé.

  > [AZURE.WARNING] Telepíteni kell az Azure AD Connect (NTLM és Kerberos-hitelesítés szükséges), a régi jelszót hitelesítő adatok szinkronizálása az Azure Active Directory-bérlőhöz lehetővé teszi, hogy a legújabb ajánlott kiadásába. Ez a funkció nem érhető el a korábbi verziókban Azure AD Connect vagy a régi DirSync eszközzel.

Azure AD Connect telepítési útmutatója érhetők el a következő cikkben – [az Azure AD Connect – első lépések](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>NTLM és Kerberos hitelesítő adatok tiltva az Azure Active Directory-szinkronizálás engedélyezése
Hajtsa végre az alábbi PowerShell parancsprogramot egyes AD erdő, teljes jelszó-szinkronizálás, és lehetővé teszi az összes a helyszíni felhasználói hitelesítő adatok tiltva a Azure AD-bérlő szinkronizálását. Ez a parancsfájl lehetővé teszi, hogy a hitelesítő adatok tiltva a Azure AD-bérlő szinkronizálódnak NTLM/Kerberos-hitelesítés szükséges.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

A címtár méretétől függően (szám, a felhasználók, csoportok stb), hitelesítő adatok tiltva az Azure Active Directory szinkronizáló időt vesz igénybe. A jelszavak lesz használható az Azure Active Directory tartományi szolgáltatások felügyelt tartományban után a hitelesítő adatok tiltva az Azure Active Directory szinkronizálódtak hamarosan.


<br>

## <a name="related-content"></a>Kapcsolódó tartalom

- [AAD tartományi szolgáltatásokhoz való a felhőben Azure jelszó-szinkronizálás engedélyezése az Active directory](active-directory-ds-getting-started-password-sync.md)

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)

- [A Windows virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-windows-vm.md)

- [Piros kalap vállalati Linux virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
