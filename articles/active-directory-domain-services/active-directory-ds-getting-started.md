<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Hozzon létre a AAD Adatközpont rendszergazdák csoport |} Microsoft Azure"
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
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások – első lépések

Az alábbiakban ismertetjük az Azure Active Directory tartományi szolgáltatások, a Azure AD-bérlő engedélyezéséhez szükséges feladatokat.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Feladat 1: A "AAD Adatközpont rendszergazdák" csoport létrehozása
Az első tevékenység felügyeleti csoport létrehozása az Azure Active Directory-ös bérlőben. A speciális felügyeleti csoport neve **AAD Adatközpont rendszergazdák**. Ennek a csoportnak a tagjai rendszergazdai jogosultsággal vannak, amelyek a tartományhoz tartozó az Azure Active Directory tartományi szolgáltatások felügyelt tartománynak gépeken. A tartományhoz gépeken ebben a csoportban a "Rendszergazdák" csoport lett hozzáadva. Emellett a csoport tagjai segítségével távoli asztali tartományhoz gépek távolról csatlakozni.  

> [AZURE.NOTE] Tartományi rendszergazdája vagy vállalati rendszergazdai jogosultságok készült Azure Active Directory tartományi szolgáltatások, a felügyelt tartomány nem rendelkezik. Felügyelt tartományok ezek a jogosultságok szolgáltatás vannak fenntartva, és nem elérhetők a bérlő felhasználóihoz. Azonban néhány jogosultsággal rendelkező műveletek elvégzéséhez a konfigurációs ebben a feladatban létrehozott speciális Rendszergazdacsoportba is használhatja. Ezeket a műveleteket olyan számítógépek csatlakoztat a tartomány csoportjába tartozó rendszergazdák tartományhoz gépeken konfigurálása csoport házirend stb.

A konfigurációs feladat létrehozása a felügyeleti csoportba, és egy vagy több felhasználó a címtárban hozzáadása a csoporthoz. Végezze el a felügyeleti csoport létrehozása az Azure Active Directory tartományi szolgáltatások a következő lépéseket:

1. Nyissa meg az **Azure klasszikus portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Jelölje ki az **Active Directory** -csomópontot, a bal oldali ablaktáblában.

3. Jelölje ki az Azure Active Directory bérlői webhely (könyvtár), amelynek az Azure Active Directory tartományi szolgáltatások engedélyezése szeretné. Egy tartomány minden Azure Active Directory csak hozhat létre.

    ![Jelölje ki az Azure Active Directory](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kattintson a **csoportok** fülre.

5. Csoport hozzáadása a Azure AD-bérlő, a munkaablak, a lap alján a **Csoport hozzáadása** gombra.

    ![Csoport gomb felvétele](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Hozzon létre egy **AAD Adatközpont rendszergazdák**nevű csoportot. **CSOPORTJÁNAK típusának** beállítása **biztonsági**.

    > [AZURE.WARNING] Engedélyezze a hozzáférést az Azure Active Directory tartományi szolgáltatások belül kezeli a tartomány, hozzon létre egy csoportot a azonos nevű.

    ![Rendszergazda csoport létrehozása](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Adja hozzá egy leírást a csoporthoz, így mások megértéséhez, hogy az ebben a csoportban használja rendszergazdai jogosultságokat Azure Active Directory tartományi szolgáltatások belül.

8. A csoport létrehozását követően kattintson annak a csoportnak szeretné megjeleníteni a tulajdonságokat ennek a csoportnak a nevét. Felhasználók felvétele az ennek a csoportnak a tagjai, kattintson az alsó panelen kattintson a **Tagok hozzáadása** gombra.

    ![Csoport tagjainak gomb felvétele](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. **Tagok hozzáadása** párbeszédpanelen jelölje be a felhasználók, akik kell a csoport tagjai, és jelölje be a jelölőnégyzetet, ha be szeretné fejezni.

    ![Felhasználók felvétele a rendszergazda csoport](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Feladat 2: Létrehozása, és válassza az Azure virtuális hálózat
A következő konfigurációs feladata a [létrehozásához, vagy válasszon egy Azure virtuális hálózat](active-directory-ds-getting-started-vnet.md).
