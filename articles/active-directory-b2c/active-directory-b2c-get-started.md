<properties
    pageTitle="Azure Active Directory B2C: Létrehozása az Azure Active Directory B2C bérlői |} Microsoft Azure"
    description="A témakör az Azure Active Directory B2C bérlői létrehozásának lépéseiről"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Létrehozása az Azure Active Directory B2C bérlői

Microsoft Azure Active Directory (Azure Active Directory) B2C használatának megkezdéséhez kövesse a cikkben ismertetett három lépését.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Lépés: 1: Az Azure előfizetéssel regisztrálhat

Ha már van egy Azure-előfizetést, ugorja át ezt a lépést. Ha nem, az [Azure előfizetés](../active-directory/sign-up-organization.md) regisztrálhat, és Azure Active Directory B2C való hozzáférés.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Lépés: 2: Létrehozása az Azure Active Directory B2C bérlői

Új Azure Active Directory B2C bérlői webhely létrehozásához kövesse az alábbi lépéseket. Jelenleg B2C funkciók nem kapcsolható be a meglévő bérlők.

1. Jelentkezzen be az [Azure klasszikus portál](https://manage.windowsazure.com/) az előfizetés-rendszergazdaként. Ez a ugyanazt a munkahelyi vagy iskolai fiók vagy a Microsoft-fiókot regisztrálni szeretne az Azure használt.
2. Kattintson az **Új** > **alkalmazás szolgáltatások** > **az Active Directory** > **címtár** > **egyéni létrehozása**.

    ![Képernyőkép: a kezdési bérlő létrehozása](./media/active-directory-b2c-get-started/new-directory.png)

3. Válassza ki a **nevét**, **Tartománynevet** és **ország vagy régió** az bérlői webhelyen.
4. Jelölje be a beállítást, amely szerint **Ez a B2C könyvtárában**.
5. Kattintson a jelölőnégyzet be van jelölve a művelet végrehajtásához.

    ![Képernyőkép: a jelölőnégyzet be van jelölve a B2C könyvtár létrehozása](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. A bérlő ekkor létrejön, és az Active Directory-bővítmény meg fog jelenni. A bérlő a globális rendszergazdának is történik. Szükség szerint más globális rendszergazda is hozzáadhat.

    > [AZURE.IMPORTANT]
    Ha szeretné B2C bérlő használni egy gyártási alkalmazást, olvassa el a [termelési -](active-directory-b2c-reference-tenant-type.md)skála előzetes B2C bérlők összehasonlítása. Megjegyzendő, hogy vannak ismert hibák egy meglévő B2C bérlői törlése és hozza létre újból a azonos nevű tartományban. Meg kell B2C bérlő létrehozása, egy másik tartománynevet.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Lépés 3: Nyissa meg a B2C szolgáltatások lap az Azure portálon

1. Nyissa meg az Active Directory-bővítmény a bal oldali navigációs sávon.
2. Keresse meg a bérlő csoportban a **címtár** fülre, és kattintson rá.
3. Kattintson a **beállítás** fülre.
4. A **B2C felügyelete** csoport **kezelése B2C beállításai** hivatkozásra.

    ![Képernyőkép: a B2C címtár konfigurációja](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Az Azure portálon, kijelölt a B2C szolgáltatások lap megjelenítő megnyílik egy új böngészőlapot vagy -ablakot.

    > [AZURE.IMPORTANT]
    Az Azure portál elérhető legyen a bérlő legfeljebb 2-3 percig is eltarthat. Újra próbálkozik ezeket a lépéseket, egy kis időt kijavítja ezt követően. Ha nem, forduljon a támogatási szolgálathoz.

6. Rögzítés a könnyebb hozzáférés a Startboard való lap. (A PIN-kód eszköz van-e jelölve a szolgáltatások lap jobb felső sarkában piros.)

    ![Képernyőkép: a B2C szolgáltatások lap](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Felhasználók és csoportok, önkiszolgáló jelszó-visszaállítás konfigurációs és a vállalat esetleges lehetőségei a bérlő az [Azure klasszikus portálon](https://manage.windowsazure.com/)kezelheti.

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Egyszerű hozzáférést a B2C szolgáltatások lap az Azure portálon

Javítja a feltárást, hogy hozzáadtunk egy helyi szeretne a B2C szolgáltatások lap az Azure portálon.

1. Jelentkezzen be az Azure-portálra a a B2C bérlő globális rendszergazdaként. Ha már bejelentkezett különböző bérlői webhelyre, váltson a bérlők (a jobb felső sarokban lévő).
2. Kattintson a bal oldali navigációs sávon kattintson a **Tallózás gombra** .
3. Kattintson az **Azure Active Directory B2C** a B2C szolgáltatások lap eléréséhez.

    ![Képernyőkép: a Tallózás B2C szolgáltatások lap](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Következő lépések

Ismerje meg az alkalmazás regisztrálhatja az Azure Active Directory B2C, és a rövid útmutató alkalmazás összeállítása olvasásával [Azure Active Directory B2C: az alkalmazás regisztrálhatja](active-directory-b2c-app-registration.md).
