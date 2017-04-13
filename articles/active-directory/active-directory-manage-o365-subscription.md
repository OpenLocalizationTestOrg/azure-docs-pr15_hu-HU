<properties
   pageTitle="Az Office 365-előfizetés Azure-ban a címtárban kezelése |} Microsoft Azure"
   description="Egy Office 365-előfizetés az Azure Active Directory és az Azure klasszikus portál directory kezelése"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Az Office 365-előfizetés Azure-ban a címtárban kezelése

Ez a cikk ismerteti, hogyan kezelheti az Office 365-előfizetést, az Azure klasszikus portál használatával létrehozott könyvtárában. A szolgáltatás rendszergazdája vagy a közös rendszergazda az Azure előfizetés való bejelentkezéshez az Azure klasszikus portál kell lennie. Ha még nincs egy Azure-előfizetést, regisztrálhat egy [ingyenes 30 napos próbaverzióját](https://azure.microsoft.com/trial/get-started-active-directory/) ma, és megoldást üzembe helyez a első felhő az 5 perc alatt használja ezt a hivatkozást. Ne felejtse el használni a jelentkezzen be az Office 365-höz használt munkahelyi vagy iskolai fiókjával.

Miután az Azure előfizetés, jelentkezzen be az Azure klasszikus portálra, és Azure szolgáltatások elérése. Kattintson az ugyanabban a könyvtárban hitelesíti az Office 365-felhasználók kezelése az Active Directory-bővítményre.

Ha már van egy Azure-előfizetést, eljárással lehet felügyelni a címtárhoz további is nem túl bonyolult feladat. Például Lukács Tibor előfordulhat, hogy telepítve van az Office 365-előfizetéssel a Contoso.com. Az Azure előfizetéssel, ő előfizetett a Microsoft-fiók használatával is rendelkezik msmith@hotmail.com. Ebben az esetben ő két könyvtárak kezeli.

  Előfizetés |  Az Office 365-ben  |  Azure
  -------------- | ------------- | -------------------------------
  Megjelenítendő név |  A contoso  |     Alapértelmezett Azure Active Directory (Azure Active Directory) könyvtár
  Tartománynév  |  a contoso.com  | msmithhotmail.onmicrosoft.com

Azt szeretné, hogy a felhasználói identitások a Contoso címtárban kezelésére, miközben ő be van jelentkezve a Microsoft-fiókkal Azure, hogy ő engedélyezheti az Azure Active Directory-szolgáltatások, például többtényezős hitelesítést. Az alábbi ábra mutatja be a folyamat nyújthatnak segítséget:

![Diagram két független könyvtárak kezelése](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

Ebben az esetben a két könyvtárak egymástól független.

## <a name="to-manage-two-independent-directories"></a>Két független könyvtárak kezelése
Ahhoz, hogy mindkét könyvtárak kezelésére, miközben ő be van jelentkezve, Azure Lukács Tibor msmith@hotmail.com, őt az alábbi lépéseket kell elvégeznie:

> [AZURE.NOTE]
> Is kell elvégezni ezeket a lépéseket, csak akkor, ha a felhasználó Microsoft-fiókkal van aláírva. Ha a felhasználó jelentkezett-e be a munkahelyi vagy iskolai fiókkal, a vezérlőt, amellyel a **meglévő könyvtár használata** nem érhető el. A munkahelyi vagy iskolai fiókkal is hitelesítse csak az otthoni címtár (Ez azt jelenti, hogy a könyvtár hol tárolja a munkahelyi vagy iskolai fiókjával, és hogy a vállalati vagy iskolai tulajdonosa).

1.  Jelentkezzen be az [Azure klasszikus portál](https://manage.windowsazure.com) mint msmith@hotmail.com.
2.  Kattintson az **Új** > **alkalmazás szolgáltatások** > **Az Active Directory** > **címtár** > **Egyéni létrehozása**.
3.  Kattintson a meglévő könyvtár használata, és jelölje be az **aláírandó most már készen is állok** jelölőnégyzetet.
4.  A Contoso.onmicrosoft.com globális rendszergazdaként jelentkezzen be az Azure klasszikus portálra (például msmith@contoso.com).
5.  Amikor a rendszer megkérdezi, hogy **a Contoso könyvtár használata Azure?**, kattintson a **Tovább**gombra.
6.  Kattintson a **Kijelentkezés gombra**.
7.  Jelentkezzen be az Azure klasszikus portálra mint msmith@hotmail.com. A Contoso és az alapértelmezett könyvtárat jelennek meg az Active Directory-bővítmény.

Az alábbi lépések elvégzése után az msmith@hotmail.com egy globális rendszergazdai a Contoso címtárban.

## <a name="to-administer-resources-as-the-global-admin"></a>Erőforrások felügyelheti a globális rendszergazdájaként
Most vegyük tegyük fel, gipsz, Jakab, hogy a webhelyekre és az adatbázis-erőforrások az Azure előfizetés társított felügyelete kell msmith@hotmail.com. Mielőtt kikkel teheti meg, a Lukács Tibor kell további lépések elvégzéséhez:

1.  Jelentkezzen be a szolgáltatás-rendszergazdai fiókkal az Azure előfizetés [Azure klasszikus portal](https://manage.windowsazure.com) (ebben a példában msmith@hotmail.com).
2.  Nem ruházhatja át az előfizetés a Contoso könyvtár: kattintson a **Beállítások** > **előfizetések** > jelölje ki azt az előfizetést > **Könyvtár szerkesztése** > jelölje ki a **Contoso (Contoso.com)**. Az átadás, bármilyen munkahelyi vagy iskolai részeként a fiókot, amely az előfizetéshez, további rendszergazdák törlődnek.
3.  Gipsz, Jakab hozzáadása az előfizetés közös rendszergazdaként: kattintson a **Beállítások** > **rendszergazdák** > jelölje ki azt az előfizetést > **Hozzáadás** > típusa **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Következő lépések
Előfizetések és könyvtárak közötti kapcsolatra kapcsolatos további tudnivalókért olvassa el a [hogyan előfizetés társítva könyvtárában](active-directory-how-subscriptions-associated-directory.md)című témakört.
