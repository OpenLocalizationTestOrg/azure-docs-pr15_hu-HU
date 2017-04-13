<properties
    pageTitle="Az Azure Active Directory, a telepítés során egy új eszköz beállítása |} Microsoft Azure"
    description="Ez a témakör bemutatja, hogyan felhasználók hozhatnak Azure Active Directory Bekapcsolódás első futtatása során."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Az Azure Active Directory, a telepítés során egy új eszköz beállítása

A Windows 10, a felhasználók is csatlakozni tudnak a eszközök Azure Active Directory (Azure Active Directory) az első indításkori (FRX) a. Ez a segítségével a szervezetek terjesztése légmentes fóliacsomagolású eszközök: azok az alkalmazottak vagy a diákok, illetve a őket a saját (CYOD) kiválasztása.
Ha a Windows 10 Professional vagy a Windows 10 Enterprise kiadásai telepítve van egy eszközt, a felület alapértelmezés szerint a telepítés során a vállalat által birtokolt eszközök esetén.

## <a name="to-join-a-device-to-azure-ad"></a>Ha be szeretne kapcsolódni egy eszközt az Azure Active Directory


1. Ha bekapcsolja az új eszköz, majd a telepítés megkezdéséhez, meg kell jelennie a **Készen áll az első** üzenet. Kövesse a megjelenő utasításokat követve állítsa be az eszközt.
2. Indítsa el a területi és nyelvi beállítások testre szabásával. Kattintson az elfogadás a Microsoft szoftverlicenc-szerződést.
![A terület testreszabása](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Jelölje ki a hálózathoz kapcsolódik az internethez használni szeretne.
4. Válassza ki, hogy egy személyes vagy vállalat által birtokolt eszköz használata. Ha vállalat által birtokolt, kattintson **az eszköz kapcsolódik a szervezeten**. Ez a parancs elindítja az Azure Active Directory Bekapcsolódás élmény. Az alábbiakban a képernyőn, hogy akkor, ha a Windows 10 Professional használ.
<center>
![Ki a tulajdonosa a képernyő](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Adja meg, hogy a szervezet által megadott hitelesítő adatait.
<center>
![Bejelentkezési képernyője](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Miután megadta, hogy a felhasználó nevét, a megfelelő bérlői Azure Active Directory található. Ha a szövetséges tartományban, átirányítja a helyszíni biztonságos jogkivonat szolgáltatás (STS) kiszolgálóhoz – például az Active Directory összevonási szolgáltatások (AD FS).
7. Ha egy felhasználó nem szövetséges tartományokból, írja be hitelesítő adatait, közvetlenül a Azure Active Directory által üzemeltetett lapon. Ha a vállalat arculati elemek lett beállítva, használni is talál a szervezet emblémáját és támogatják a szöveget.
8.  Többtényezős hitelesítés bonyolulttá a rendszer kéri. Ez a kérdés az informatikai rendszergazdák által módosítható.
9.  Azure Active Directory ellenőrzi, hogy szükség van-e a felhasználó eszköze az regisztrációs a mobileszköz-kezelés.
10. A Windows az eszköz a szervezet regisztrál az Azure Active Directory és igényléséhez a mobileszköz-kezelés, ha szükséges.
11. Ha a felügyelt felhasználó, a Windows megnyitja az asztali folyamatát automatikus bejelentkezés.
12. Ha a szövetséges felhasználó, akkor irányítja át a Windows bejelentkezési képernyője a hitelesítő adatok.

> [AZURE.NOTE] Nem támogatott a tartományhoz egy helyszíni Windows Server Active Directory Windows out kész asként. Ezért ha be szeretne kapcsolódni egy számítógép tartományhoz, akkor válassza a **helyi fiókkal a Windows beállítása** hivatkozásra inkább. Ezután bekapcsolódhat a tartományt a beállításait a számítógépen előtt elvégzettként már.

## <a name="additional-information"></a>További információk
* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Jelszavak – Microsoft Passport nélkül hitelesítő identitások](active-directory-azureadjoin-passport.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
