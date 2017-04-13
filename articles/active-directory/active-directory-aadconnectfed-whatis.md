<properties
    pageTitle="Azure AD Connect és összevonási |} Microsoft Azure"
    description="Ezen az oldalon az a központi hely, az Active Directory összevonási szolgáltatások műveletekkel Azure AD Connect használatával kapcsolatos összes dokumentáció"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect és összevonás

Azure AD Connect lehetővé teszi a szövetséges konfigurálása a helyszíni Active Directory összevonási szolgáltatások és Azure Active Directory. Az összevonási bejelentkezési engedélyezheti a felhasználóknak, hogy jelentkezzen be az Azure AD-alapú szolgáltatások helyszíni jelszavukat, és kattintson a vállalati hálózathoz, anélkül, hogy adja meg újra a jelszavukat. Az Active Directory összevonási szolgáltatások összevonási beállítás lehetővé teszi üzembe egy új, vagy adjon meg egy meglévő Active Directory összevonási szolgáltatások Windows Server 2012 R2 farmban.

Ez a témakör az összevonási bővebben az otthoni kapcsolatos funkciók az Azure AD Connect és listák hivatkozások kapcsolódó összes egyéb témaköröket. Azure AD Connect hivatkozásokat olvassa el a helyszíni identitások integrálása az Azure Active Directory című témakört.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - összevonási témakörök

| A témakör | Mire vonatkozik, és mikor olvasása |
|:------|:-----------|
| **Azure AD Connect felhasználónak bejelentkezés beállításai** ||
| [Felhasználói bejelentkezési beállításainak ismertetése](active-directory-aadconnect-user-signin.md) | Felhasználói bejelentkezési beállítások, és hogyan befolyásolják az Azure bejelentkezési felhasználói felület |
| **Active Directory összevonási szolgáltatások telepítési Azure AD Connect használatával**||
| [Előzetes követelmények](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Azure AD Connect keresztül sikeres Active Directory összevonási szolgáltatások telepítés előtti követelmények|
| [Active Directory összevonási szolgáltatások farm konfigurálása](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Azure AD Connect használatával új AD FS-farmok telepítése |
| **Active Directory összevonási szolgáltatások konfigurációjának módosítása** | |
| [Az adatvédelmi javítása](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Hogy miként javíthatja ki az aktuális adatvédelmi között a helyszíni Active Directory összevonási szolgáltatások és az Office 365 / Azure |
| [Új AD FS-kiszolgáló hozzáadása](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | További Active Directory összevonási szolgáltatások kiszolgálón bejegyzés kezdeti telepítés AD FS-farmba kibontása |
| [Új Active Directory összevonási szolgáltatások WAP kiszolgáló hozzáadása](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | További WAP kiszolgáló bejegyzés kezdeti telepítés AD FS-farmba kibontása |
| [Új szövetséges tartomány felvétele](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Az összevonáshoz Azure AD a további tartomány felvétele |
|**Bejegyzés telepítési feladatok**||
| [Egyéni vállalati embléma hozzáadása / ábra](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| A bejelentkezés módja módosítása, amely megjelenik a bejelentkezési lapon az AD FS az egyéni embléma megadásával |
| [Bejelentkezési leírásának megadása](active-directory-aadconnect-federation-management.md#add-sign-in-description) | A bejelentkezési lapon az AD FS bejelentkezési leírás módosítása | 
| [Módosítás az AD FS formál szabályok](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Módosítása / igénylése szabályok felvétele az Active Directory összevonási szolgáltatások megfelelő Azure AD Connect szinkronizálási konfiguráció |


## <a name="additional-resources"></a>További források

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
* [Azure Active Directory összevonási szolgáltatások bevezetésének](active-directory-aadconnect-azure-adfs.md)
* [Magas elérhetősége határokon földrajzi AD FS telepítési Azure forgalom kezelő Azure-ban](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


