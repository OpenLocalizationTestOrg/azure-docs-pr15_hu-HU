<properties
    pageTitle="Azure Active Directory B2C: Önkiszolgáló jelszó alaphelyzetbe állítása |} Microsoft Azure"
    description="A témakör bemutatja, hogyan lehet önkiszolgáló jelszó alaphelyzetbe állítása az Azure Active Directory B2C a fogyasztói beállítása"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Önkiszolgáló jelszó alaphelyzetbe állítása az a fogyasztói beállítása

Az önkiszolgáló jelszó alaphelyzetbe állítása funkciót a fogyasztói (aki regisztráltak helyi fiókok) alaphelyzetbe önállóan jelszavukat. Ez jelentősen csökkenti a a terméktámogatási munkatársaknak a terhet különösen akkor, ha az alkalmazás fogyasztói rendszeresen használ, több millió rendelkezik. Jelenleg csak támogatjuk igazolt e-mail cím használatával helyreállítási szolgál. Hozzáadunk további helyreállítási módszerek (igazolt telefonszám, biztonsági kérdések stb.) a jövőben.

> [AZURE.NOTE]
Ez a cikk vonatkozik az önkiszolgáló jelszó alaphelyzetbe állítása az bejelentkezési házirend környezetben használja. Ha teljesen testre szabható van szüksége a jelszó házirendek az alkalmazás meghívott, [ismertető](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy)témakört.

Alapértelmezés szerint a címtárában nem lesz meg az önkiszolgáló jelszó alaphelyzetbe állítása be van kapcsolva. Kapcsolni kövesse az alábbi lépéseket:

1. Jelentkezzen be az [Azure klasszikus portál](https://manage.windowsazure.com/) az előfizetés-rendszergazdaként. Ez a ugyanazt a munkahelyi vagy iskolai fiók vagy a Microsoft-fiókot, amellyel a könyvtár létrehozása.
2. Nyissa meg az Active Directory-bővítmény a bal oldali navigációs sávon.
3. Keresse meg a címtárában csoportban a **címtár** fülre, és kattintson rá.
4. Kattintson a **beállítás** fülre.
5. Görgessen lefelé a **felhasználó jelszavának alaphelyzetbe állítása házirend** szakaszig, és állítsa be az **Igen**értékre a **jelszó jogosult felhasználók visszaállítása** lehetőséget. Figyelje meg, hogy a **Másodlagos E-mail címet** beállítás be van jelölve; kiléphet belőle, mivel az.

    ![Önkiszolgáló jelszó-visszaállítás](./media/active-directory-b2c-reference-sspr/sspr.png)

6. A lap alján a **Mentés** gombra. Elkészült!

Teszteléséhez bármely bejelentkezési házirendet, amely a helyi fiókok megegyezik egy identitásszolgáltató a "Futtatása" funkciót használja. A helyi fiók Bejelentkezés lapon (ahol, adja meg az e-mail címét és jelszavát, vagy felhasználónév és jelszó), kattintson a **nem tud hozzáférni a fiókjához?** a fogyasztói felület megerősítéséhez.

> [AZURE.NOTE]
Az önkiszolgáló jelszó alaphelyzetbe állítása oldalak testre szabható a [vállalat esetleges funkció](../active-directory/active-directory-add-company-branding.md)használatával.
