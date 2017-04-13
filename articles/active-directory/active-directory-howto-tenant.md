<properties
    pageTitle="Az Azure AD-bérlő beszerzése |} Microsoft Azure"
    description="Hogyan hozhatja ki az Azure Active Directory-bérlői rögzítése és-alkalmazások létrehozásába."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Az Azure Active Directory-bérlői beszerzése

Az Azure Active Directory (Azure Active Directory) a [bérlői](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) egy szervezet képviselőjének.  Célszerű az Azure Active Directory szolgáltatás, amely egy szervezet kap tulajdonában van, ha egy Microsoft felhőalapú szolgáltatásba, például az Azure, a Microsoft Intune vagy az Office 365 számára vannak feliratkozik egy dedikált példányát.  Minden egyes Azure AD-bérlő, és az egyéb Azure AD-bérlők külön.  

A bérlői otthont ad a felhasználóknak a vállalat és őket – a jelszavakat, felhasználói profiladatok, engedélyek, és így tovább információ.  Csoportok, az alkalmazások és az egyéb információkat egy szervezet és az adatvédelmet is tartalmaz.

Azure AD-felhasználóknak, hogy jelentkezzen be az alkalmazás engedélyezéséhez regisztrálnia kell az alkalmazás a saját bérlői webhelyre.  **Teljes mértékben ingyenes**alkalmazás közzétételi Azure AD-ös bérlői.  Erre valójában a legtöbb fejlesztők és hoz létre több bérlők és kísérletezés, fejlesztési, az alkalmazások átmeneti tárolására tesztelés céljából.  Tetszés szerint megadhatja azoknak a szervezeteknek, jelentkezzen be, és az alkalmazás felhasználása licencek vásárlása, ha a kívánt speciális címtár szolgáltatások előnyeit.

Igen hogyan megtekinti az Azure AD-bérlő első kapcsolatban?  A folyamat egy kis különböző Ha lehet, hogy:

- [Meglévő Office 365-előfizetéssel rendelkezik.](#use-an-existing-office-365-subscription)
- [Meglévő Azure verzióra szóló előfizetése társított Microsoft-Account](#use-an-msa-azure-subscription)
- [Szervezeti fiókkal társított meglévő Azure szóló előfizetés](#use-an-organizational-azure-subscription)
- [A fentiek egyike sem rendelkezik, és szeretné indulás az alapoktól](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Meglévő Office 365-előfizetésbe használata
Ha egy meglévő Office 365-előfizetést, már van egy Azure AD-bérlő! Az Office 365-fiókkal jelentkezzen be az [Azure portál](https://portal.azure.com) , és Azure Active Directory használatának megkezdéséhez.

## <a name="use-an-msa-azure-subscription"></a>Egy MSA Azure-előfizetést használata
Ha, korábban már feliratkozott az Azure előfizetés az egyes Microsoft Account, akkor már van egy bérlői!  Az [Azure-portálon](https://portal.azure.com)rendszerbe, automatikusan megnyitja az alapértelmezett bérlőhöz. Tetszés szerint – az aktuális bérlői webhely használni kívánt ingyenes, de érdemes létrehozni egy szervezeti rendszergazdai fiókkal.

Ehhez tegye a következőket.  Azt is megteheti előfordulhat, hogy kívánt új bérlő létrehozása, majd a rendszergazda létrehozása az adott bérlői követő internetcímre.

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com) egyes fiókjához
2.  Nyissa meg a "Azure Active Directory" szakaszt a portál (a bal oldali navigációs sávon, a **További szolgáltatások**listában található)
3.  Meg kell automatikusan bejelentkezni a "könyvtár alapértelmezett", ha nem könyvtárak kattintson a jobb felső sarokban a fióknév válthat.
4.  A **Rövid feladatlista** szakaszban válassza a **felhasználó hozzáadása**lehetőséget.
5.  Felhasználó hozzáadása űrlapon adja meg az alábbiakat:

    - Név: (adja meg a megfelelő érték)
    - Felhasználónév: (adja meg a rendszergazda felhasználónév)
    - Profil: (töltse ki a megfelelő értékeket az utónevet, utolsó nevét, beosztás és szervezeti egység)
    - Szerep: Globális rendszergazdai

6.  Amikor befejezte a felhasználó hozzáadása űrlapon, és az ideiglenes jelszót az új felügyeleti felhasználó kap, mindenképpen jegyezze ezt a jelszót, annak érdekében, hogy a jelszó módosítása ezt az új felhasználót a bejelentkezési lesz szükség szerint. A jelszó közvetlenül az alternatív e-mail üzenetet a felhasználónak is küldhet.
7.  Kattintson az új felhasználó **létrehozása** parancsára.
8.  Ha módosítani szeretné az ideiglenes jelszót, jelentkezzen be a [https://login.microsoftonline.com](https://login.microsoftonline.com) az új felhasználói fiókra, majd kérésekor jelszavának módosítása.


## <a name="use-an-organizational-azure-subscription"></a>Egy szervezeti Azure-előfizetést használata
Ön korábban már feliratkozott az Azure-előfizetéssel a szervezeti fiókkal, ha már van egy bérlői!  Az [Azure-portálon](https://portal.azure.com)keresse meg a bérlő esetén lépjen a "További szolgáltatások" és "Azure Active Directory."  Szabadon használja az aktuális bérlői webhely, tetszés szerint. 


## <a name="start-from-scratch"></a>Indulás az alapoktól
Ne aggódjon, ha a fentiek mindegyike értelmetlen Önnek.  Egyszerűen keresse fel a [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) regisztrálni szeretne az Azure új szervezettel.  Ha befejezte a folyamatot, lehetősége van saját nagyon Azure AD-ös bérlői webhelye a úgy választott tartománynév alatt bejelentkezési be.  Az [Azure-portálon](https://portal.azure.com)megtalálhatja a bérlő "Azure Active Directory" lépés a bal oldali navigációs funkcióját.

Feliratkozna az Azure folyamatának részeként számára kötelező a adja meg a hitelkártya adatait.  Biztonságos - folytathatja, nem ráterheljük alkalmazások közzététele az Azure Active Directory vagy új bérlők hoz létre.
