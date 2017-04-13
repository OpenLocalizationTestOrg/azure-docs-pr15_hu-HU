<properties
    pageTitle="Első lépések az Azure többtényezős hitelesítés szolgáltató |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy Azure többtényezős hitelesítés szolgáltatóval."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Ismerkedés az Azure többtényezős hitelesítés szolgáltatónál
Alapértelmezés szerint az Azure Active Directory és az Office 365-felhasználóknak globális rendszergazdái kétlépcsős hitelesítési érhető el. Jó helyen jár Ha ki szeretne [Speciális szolgáltatások](multi-factor-authentication-whats-next.md) előnyeit majd meg kell vásárolni az Azure többtényezős hitelesítés (MFA).

> [AZURE.NOTE]  Az Azure többtényezős hitelesítés szolgáltatóját használatos Azure MFA teljes verziója által biztosított szolgáltatások előnyeit. A felhasználók akik **nem rendelkeznek licenccel Azure MFA, az Azure Active Directory prémium, vagy a EMS keresztül**.  Azure MFA, Azure Active Directory prémium és EMS tartalmazzák az alapértelmezés szerint Azure MFA teljes verzióját.  Ha licenceket, majd szükségtelen egy Azure többtényezős hitelesítés szolgáltatóval.

Az Azure többtényezős hitelesítés szolgáltatóját a SDK letöltése van szükség.

> [AZURE.IMPORTANT]  A SDK letöltéséhez létrehozása az Azure többtényezős hitelesítés szolgáltató esetén Azure MFA, AAD Premium vagy EMS licenceket.  Ha már rendelkezik licenccel és létrehozása az Azure többtényezős hitelesítés szolgáltatóját erre a célra, feltétlenül hozza létre a szolgáltatót a **Engedélyezett felhasználónkénti** modellel. Ezután a szolgáltató az Azure MFA, Azure Active Directory Premium vagy EMS licencek tartalmazó könyvtár mutató hivatkozás  Ezzel biztosíthatja, hogy akkor is csak számlát kapni több, mint a saját licencek száma a SDK használatával egyedi felhasználó.


## <a name="to-create-a-multi-factor-auth-provider"></a>Többtényezős hitelesítés szolgáltató létrehozása

Az Azure többtényezős hitelesítés szolgáltatóját létrehozásához kövesse az alábbi lépéseket.

1. Jelentkezzen be rendszergazdaként az [Azure klasszikus portálon](https://manage.windowsazure.com) .
2. A bal oldalon válassza **Az Active Directory**.
3. A képernyő tetején, az Active Directory lapon jelölje ki a **Többtényezős hitelesítésszolgáltatók**.
![Egy MFA-szolgáltatóval létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. A képernyő alján kattintson az **Új**gombra.
![Egy MFA-szolgáltatóval létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. A szolgáltatások alkalmazás csoportban jelölje be a **Többtényezős hitelesítés szolgáltató**
![egy MFA-szolgáltatóval létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Jelölje ki az **első létrehozása**.
![Egy MFA-szolgáltatóval létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Töltse ki a következő mezőket, és válassza a **Create**.
    1. **Név** – a többtényezős hitelesítés szolgáltató neve.
    2. **Használat modellje** meg szeretné-e az egyéni felhasználók engedélyezése vagy egy ellenőrzési fizet. Válassza a két lehetőség egyikét:
        - Egy hitelesítés – hitelesítési használati költségek vásárlási modell. Jellemzően az esetek, amelyek a fogyasztói elérésű alkalmazások Azure többtényezős hitelesítés.
        - Enabled felhasználónként – vásárlási modellben használva, amelyet használati költségek engedélyezett felhasználói. Jellemzően az alkalmazásokat, például az Office 365-ben alkalmazott hozzáférésének. Néhány felhasználó már licencelt Azure többtényezős, válassza ezt a lehetőséget.
    2. A **könyvtár** – a többtényezős hitelesítést szolgáltató társított az Azure Active Directory-bérlői. Tartsa szem előtt az alábbiakat:
        - Többtényezős hitelesítés szolgáltató létrehozása Azure Active Directory címtárhoz szükségtelen. Hagyja a mezőt üresen, ha az Azure többtényezős hitelesítést kiszolgáló vagy SDK csak szeretné.
        - A többtényezős hitelesítés szolgáltató a kihasználhatja a speciális funkciójára Azure Active Directoryval társított kell lennie.
        - Azure AD Connect, AAD Sync vagy DirSync csak annak követelménye, ha a helyszíni Active Directory-környezet szinkronizálása a Azure Active Directoryval.  Ha csak egy Azure AD-címtár, amely nem szinkronizálja a rendszer, majd ez nem kötelező.
![Egy MFA-szolgáltatóval létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Gombra létrehozása, a többtényezős hitelesítést szolgáltató létrejön és megjelenik egy üzenet arról: **többtényezős hitelesítést szolgáltató sikeresen létre**. Kattintson az **OK gombra**.
![Egy MFA-szolgáltatóval létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
