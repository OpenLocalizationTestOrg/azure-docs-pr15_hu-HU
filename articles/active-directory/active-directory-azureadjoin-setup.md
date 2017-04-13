<properties
    pageTitle="A felhasználók az Azure Active Directory Bekapcsolódás beállítását |} Microsoft Azure"
    description="Ebből a cikkből megtudhatja, hogyan rendszergazdák hozhatnak Azure Active Directory csatlakozás helyszíni könyvtár és eszköz regisztrációs."
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
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Azure Active Directory Bekapcsolódás beállítása a szervezetben

Azure Active Directory Bekapcsolódás (Azure Active Directory csatlakozni) beállítása előtt kell szinkronizálni a felhasználók a felhőben helyszíni címtárában felfelé, vagy felügyelt fiókok létrehozása az Azure Active Directory manuálisan.

A helyszíni felhasználóit az Azure Active Directory szinkronizálásával kapcsolatos részletes útmutatást a [integrálása az Azure Active Directory címtárral a helyszíni identitások](active-directory-aadconnect.md)vonatkozik.


Manuális létrehozásához, és a felhasználók kezelése az Azure Active Directory, olvassa el a [felhasználók kezelése az Azure Active Directory](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Regisztráció eszköz beállítása
1. Jelentkezzen be rendszergazdaként az Azure-portálra.
2. A bal oldali ablaktáblában jelölje ki az **Active Directory**.
3. A **címtár** lapon jelölje ki a címtárban.
4. Kattintson a **beállítás** fülre.
5. Nyissa meg az **eszközök** csoportban.
6. Az **eszközök** lapon állítsa be a következőket:  
   * **Maximális száma az eszközök FELHASZNÁLÓNKÉNTI**: válassza ki a felhasználó lehetnek az Azure Active Directory eszközök maximális száma.  Ha egy felhasználó eléri a kvóta, ezek nem lesznek tudja hozzáadni a további eszközök, amíg legalább egy, a meglévő eszközök törlődik.
   * **MEGKÖVETELÉSE TÖBBTÉNYEZŐS AUTH való csatlakozás eszközök**: beállítása, hogy felhasználók meg kell adnia egy második hitelesítési tényező be szeretne kapcsolódni az eszköze Azure ad-e. Azure többtényezős hitelesítés a további tudnivalókért lásd: [Ismerkedés az Azure többtényezős hitelesítés a felhőben](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Felhasználók előfordulhat, hogy az AZURE Active Directory ILLESZTÉS eszközök**: jelölje be a felhasználók és csoportok, amely összekapcsolja a eszközök és Azure Active Directory számára engedélyezett.
   * **További RENDSZERGAZDÁK tovább AZURE Active Directory TARTOMÁNYHOZ eszközök**: Azure Active Directory Premium vagy a vállalati mobilitás csomagja (EMS), megadhatja, melyik felhasználó megkapja az eszköz helyi rendszergazdai jogosultságokat. Globális rendszergazdák és a eszköz tulajdonosok helyi rendszergazdai jogosultságok kapnak alapértelmezés szerint.

<center>![Állítsa be az eszközt regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Beállítása után Azure Active Directory Bekapcsolódás a felhasználók számára, hogy csatlakozhat Azure AD a vállalati vagy személyes eszközökön keresztül.

Az alábbiakban a három helyzetleírás ahhoz, hogy a felhasználók beállítása az Azure Active Directory Bekapcsolódás használhatja:

- A felhasználók közvetlenül az Azure Active Directory bekapcsolódhatnak a vállalat által birtokolt eszközt.
- Illesztés tartomány felhasználókat a helyszíni Active Directory vállalat által birtokolt eszköz és majd Azure Active Directory terjed ki az eszközt.
- Felhasználók hozzáadása munkahelyi vagy iskolai fiók Windows személyes eszközön

## <a name="additional-information"></a>További információk
* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
