<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Felügyeleti útmutató |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások felügyelt tartományok létrehozása szervezeti egység (szervezeti egység)"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Szervezeti egység (szervezeti egység) létrehozása az Azure Active Directory tartományi szolgáltatások felügyelt tartományban
Azure Active Directory tartományi szolgáltatások felügyelt tartományok "AADDC számítógépek" és "AADDC felhasználók" nevű rendre két beépített tárolók tartalmazzák. A "AADDC számítógépek" tároló számítógép-objektumok, amelyek a felügyelt tartományban található összes számítógépre tartalmaz. A "Felhasználók AADDC" tároló felhasználók és csoportok szerepel a Azure AD-bérlő. Időnként szükség lehet a felügyelt tartományban munkaterhelésekből üzembe szolgáltatás-fiókok létrehozása. Erre a célra hozzon létre egy egyéni szervezeti egység (OU) a felügyelt tartományban, és a szervezeti egységre belül szolgáltatás-fiókok létrehozása. Ez a cikk bemutatja, hogyan hozhat létre szervezeti Egységgel a felügyelt tartományban.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Active Directory felügyeleti eszközök telepítése Távfelügyelet tartományhoz virtuális gépen
Azure Active Directory tartományi szolgáltatások felügyelt tartományok távolról a már jól ismert az Active Directory felügyeleti eszközök, például az Active Directory felügyeleti központ (ADAC) vagy az Active Directory PowerShell használatával is kezelhetők. Bérlői rendszergazdák nincs jogosultsága, tartomány vezérlők felügyelt tartományban távoli asztali keresztül csatlakozhat. A felügyelt tartomány kezeli, telepítse az Active Directory felügyeleti eszközök szolgáltatás a felügyelt tartományhoz virtuális gépen. Olvassa el az utasításokat [egy felügyelt Azure Active Directory tartományi szolgáltatások tartományt felügyelete](active-directory-ds-admin-guide-administer-domain.md) című témakört.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>A felügyelt tartomány szervezeti egység létrehozása
Most, hogy az Active Directory felügyeleti eszközök telepítve van a tartomány virtuális gép csatlakozott, azt ezeket az eszközöket, hogy hozzon létre egy szervezeti egység felügyelt tartományban. Hajtsa végre az alábbi lépéseket:

> [AZURE.NOTE] Csak a "AAD Adatközpont rendszergazdák" csoport tagjai egyéni szervezeti létrehozásához szükséges jogosultságokkal rendelkezik. Győződjön meg arról, hogy a felhasználók, akik a csoporthoz tartozik, hajtsa végre az alábbi lépéseket.

1. A kezdőképernyőn kattintson a **Felügyeleti eszközök**elemre. Meg kell jelennie virtuális a gépre van telepítve az Active Directory felügyeleti eszközei.

    ![Telepítve van a kiszolgáló felügyeleti eszközei](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Kattintson **az Active Directory felügyeleti központ**elemre.

    ![Active Directory felügyeleti központ](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. A tartomány megtekintéséhez kattintson a bal oldali ablaktáblában (például contoso100.com) a tartomány nevét.

    ![ADAC - nézet tartomány](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. A **feladatok** jobb oldali ablaktáblában kattintson az **Új** csoportban a tartomány neve csomópontot. Ez a példa azt kattintson az **Új** gombra a **feladatok** jobb oldali ablaktáblában kattintson a "contoso100(local)" csomópontot.

    ![ADAC – új szervezeti egység](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Meg kell jelennie a szervezeti egység létrehozása lehetőséget. Kattintson a **Szervezeti egység** indítsa el a **Szervezeti egység létrehozása** párbeszédpanel.

6. **Szervezeti egység létrehozása** párbeszédpanelen adja meg egy **nevet** az új szervezeti egység. Adjon meg egy rövid leírást a szervezeti egység. A **Felügyelt** mező a szervezeti egység is megadhat. Az egyéni szervezeti létrehozásához kattintson az **OK gombra**.

    ![ADAC – OU párbeszédpanel létrehozása](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. Az újonnan létrehozott szervezeti egység ekkor megjelenik az Active Directory felügyeleti központ (ADAC) a.

    ![ADAC – OU létrehozott](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Újonnan létrehozott szervezeti engedélyek és biztonsági funkciói
Alapértelmezés szerint a felhasználó (a "AAD Adatközpont rendszergazdák" csoport tagja), az egyéni szervezeti létrehozó rendszergazdai jogosultsággal (teljes hozzáférés) a szervezeti fölé. A felhasználó majd lépjen tovább, és jogosultságokat más felhasználóknak vagy a "AAD Adatközpont" rendszergazdacsoportjának tetszés szerint. Az alábbi képernyőképen a felhasználó naplójában 'bob@domainservicespreview.onmicrosoft.com' ki hozta létre, hogy az új "MyCustomOU" szervezeti egység adják teljes hozzáféréssel.

 ![ADAC – új OU biztonsági](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Jegyzetek egyéni szervezeti felügyelete
Most, hogy a létrehozott egyéni szervezeti, lépjen tovább, és a szervezeti felhasználók, csoportok, számítógépek és szolgáltatás fiókok létrehozása. Nem aktiválhatók felhasználóknak vagy csoportoknak a felhasználóktól"AAD Adatközpont" OU egyéni szervezeti.

> [AZURE.WARNING] Felhasználói fiókok, csoportok, szolgáltatás fiókok és a létrehozott egyéni szervezeti a számítógép-objektumok a az Azure Active Directory-ös bérlőben nem érhetők el. Az objektumok más szóval, ne jelenjen meg az Azure Active Directory Graph API használatával vagy az Azure Active Directory felhasználói felület. Az objektumok csak érhetők el az Azure Active Directory tartományi szolgáltatások felügyelt tartományban.


## <a name="related-content"></a>Kapcsolódó tartalom

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory felügyeleti központ: Első lépések](https://technet.microsoft.com/library/dd560651.aspx)

- [Szolgáltatási fiókjaival részletes útmutató](https://technet.microsoft.com/library/dd548356.aspx)
