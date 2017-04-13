<properties
    pageTitle="Azure AD Connect szinkronizálási szolgáltatások és konfigurációs |} Microsoft Azure"
    description="Azure AD Connect szinkronizálási szolgáltatás szolgáltatás mellett funkcióit ismerteti."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect szinkronizálási szolgáltatások

A szinkronizálási funkcióját Azure AD Connect két részből áll:

- A helyszíni összetevő neve **Azure AD Connect szinkronizálási**, más néven **szinkronizálási motor**.
- A szolgáltatás tartózkodó az Azure Active Directory néven is ismert **Azure AD Connect szinkronizálási szolgáltatás**

Ebből a témakörből megtudhatja, hogyan az **Azure AD Connect szinkronizálási szolgáltatás** az alábbi szolgáltatások működnek, és hogyan adhatja őket a Windows PowerShell használatával.

Az [Azure Active Directory modul Windows Powershellhez](http://aka.ms/aadposh)megtörténik az ezeket a beállításokat. Letöltheti és telepítheti külön-külön az Azure AD Connect. Az ebben a témakörben ismertetett parancsmag bevezetett [2016 március kiadás (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Ha nem rendelkezik az ebben a témakörben ismertetett parancsmag, vagy nem állít elő ugyanazt az eredményt, végezze el, hogy a legújabb verzió futtatásakor.

A konfiguráció az Azure Active directory megtekintéséhez futtatása `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures eredménye](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

E beállítások közül számos csak módosítható Azure AD Connect.

Az alábbi beállítások beállítható `Set-MsolDirSyncFeature`:

DirSyncFeature | Megjegyzés
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Lehetővé teszi, hogy attribútum kell karanténba helyezett, ha egy másik objektum, hanem a teljes objektum adatkapcsolat exportálás során másolatot.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Lehetővé teszi, hogy az elsődleges SMTP-cím mellett userPrincipalName illesztéshez objektumok.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Lehetővé teszi, hogy a szinkronizálási motor frissítheti a userPrincipalName attribútum felügyelt/licenccel rendelkező felhasználók (nem szövetséges).

Miután engedélyezte a funkciót, nem lehet letiltani újra.

>[AZURE.NOTE] 2016 augusztus 24-ból a *Duplikálás attribútum tűrőképessége* szolgáltatás alapértelmezés szerint engedélyezve van az új Azure Active Directory könyvtárak. Ez a funkció is lehet közzétételének és a dátum előtt létrehozott könyvtárak engedélyezve van. E-mailben értesítést kap, amikor a címtárában első ezzel a funkcióval.

Az alábbi beállítások szerint Azure AD Connect úgy vannak beállítva, és nem módosíthatják a `Set-MsolDirSyncFeature`:

DirSyncFeature | Megjegyzés
--- | ---
DeviceWriteback | [Azure AD Connect: Eszköz visszaírást engedélyezése](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect szinkronizálása: címtár-bővítmények](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Az Azure AD Connect szinkronizálás jelszó-szinkronizálás végrehajtása](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Kép: Csoport visszaírást](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Jelenleg nem támogatott.

## <a name="duplicate-attribute-resiliency"></a>Ismétlődő attribútum tűrőképessége
Az ismétlődő UPN objektumok helyett kiépítése hibás / proxyAddresses, az ismétlődő attribútum van "karanténba helyezett" és az ideiglenes érték van-e hozzárendelve. Amikor a rendszer az ütközést, ideiglenes (UPN) a megfelelő értéket automatikusan módosul. Ez a jelenség külön-külön engedélyezhető az UPN és proxyAddress. További információra kíváncsi olvassa el a [identitás szinkronizálási és ismétlődő attribútum tűrőképessége](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)című témakört.

## <a name="userprincipalname-soft-match"></a>UserPrincipalName finom egyezés
Ha ezt a szolgáltatást, finom-egyezés engedélyezve van (UPN) mellett az [elsődleges SMTP-címet](https://support.microsoft.com/kb/2641663), amelyet mindig legyen engedélyezve van. Finom-egyezés meglévő felhő felhasználók megfelelően az Azure Active Directory a helyszíni felhasználói számára használható.

Ha módosítani szeretné egyezés a helyszíni Active Directory-fiókok a felhőben létrehozott fiókkal rendelkező nem használ az Exchange Online, majd ez a funkció akkor hasznos lehet. Ebben az esetben általában nincs okának az SMTP-attribútum beállítása a felhőben.

Ez a funkció áll a alapértelmezés szerint az újonnan létrehozott Azure AD-könyvtárak. Ha ezt a szolgáltatást, a futtatásával láthatja:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Ha ez a szolgáltatás nincs engedélyezve a az Azure AD-hez, majd engedélyezheti újra futtatásával:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Szinkronizálhatják a frissítéseket a userPrincipalName
Régebben a UserPrincipalName attribútum a szinkronizálási szolgáltatás használatát a helyszíni frissítések le van tiltva, kivéve, ha két feltétel teljesül:

- A felhasználó (nem szövetséges) kezeli.
- A felhasználó nem rendelt licenc.

További részletekért olvassa el [az Office 365, az Azure-vagy az Intune felhasználónevek nem egyeznek a helyszíni egyszerű felhasználónév vagy egy alternatív bejelentkezési azonosító](https://support.microsoft.com/kb/2523192).

Ez a funkció engedélyezi a szinkronizálási motor frissítheti a userPrincipalName, ha megváltozott a helyszíni és jelszó-szinkronizálás használata. Ha összevonási használja, ez a funkció nem támogatott.

Ez a funkció áll a alapértelmezés szerint az újonnan létrehozott Azure AD-könyvtárak. Ha ezt a szolgáltatást, a futtatásával láthatja:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Ha ez a szolgáltatás nincs engedélyezve a az Azure AD-hez, majd engedélyezheti újra futtatásával:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

A funkció engedélyezése után már meglévő userPrincipalName értékek marad-van. A következő módosítás a userPrincipalName attribútum a helyszíni a normál delta szinkronizálás a felhasználókra gyakorolt frissül (UPN).  

## <a name="see-also"></a>Lásd még:

- [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
