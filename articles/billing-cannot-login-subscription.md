<properties
    pageTitle="Nem tudok bejelentkezni a Azure előfizetés |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure előfizetés login kapcsolatos néhány gyakori problémák megoldása."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Nem tudok bejelentkezni Azure előfizetésem kezelése

Ez a cikk végigvezeti Önt a legáltalánosabb módszer a bejelentkezési problémák megoldására egy része.

## <a name="page-hangs-in-the-loading-status"></a>Lap lefagy a betöltésének állapota

Ha lefagy a internet böngészőben a lapot, próbálkozzon az alábbi lépéseket minden egyes mindaddig, amíg az [Azure portál](https://portal.azure.com)is elérhető.

-   Frissítse a lapot.
-   Az Internet más böngészőt használ.
-   Ha a Microsoft Internet Explorer használata esetén keresse meg a Azure portált az InPrivate-böngészési mód használatával. 

    VÁLASZOK PARANCSRA.  Kattintson az **eszközök** ![eszközök gomb](./media/billing-cannot-login-subscription/Toolsbutton.png) > **biztonsági** > **InPrivate-böngészési**.

    B  Keresse meg a [Azure-portálra](https://portal.azure.com), és jelentkezzen be a portálra.

## <a name="error-message-no-subscriptions-found"></a>"Nem található előfizetés" hibaüzenet jelenik meg

Ha a fiókja nem rendelkezik a megfelelő engedélyekkel, megjelenhet egy **nem található előfizetés** hibaüzenet. Csak a fiók rendszergazda a [Fiók központban](https://account.windowsazure.com/)érheti el a szolgáltatás-rendszergazdák (Nyelvű) vagy a további rendszergazdák (CA).

**1 alkalmazási helyzetek: Hiba üzenet jelenik meg az [Azure portál](https://portal.azure.com)**

A probléma megoldásához, [a közös rendszergazdája és a tulajdonosok szerepkör hozzáadása](billing-add-change-azure-subscription-administrator.md) a fiókhoz.

**2 alkalmazási helyzetek: Hiba üzenet jelenik meg az [Azure-fiók központ](https://account.windowsazure.com/Subscriptions)**

Ellenőrizze, hogy a fiók, mellyel a fiók rendszergazda. Annak ellenőrzéséhez, aki a fiók-rendszergazda, kövesse az alábbi lépéseket:

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2.  A központi lapon válassza azt az **előfizetést**.
3.  Jelölje ki az ellenőrizni kívánt előfizetést, és válassza a **Beállítások**gombra.
4.  Válassza a **Tulajdonságok parancsot**. A fiók rendszergazda az előfizetés jelenik meg a **Fiók rendszergazda** mezőbe.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Automatikusan jelentkezett be, egy másik felhasználó

Ez a hiba akkor fordulhat elő, ha egynél több felhasználói fiókot használata a böngészőben.

A probléma megoldásához próbálkozzon az alábbi lehetőségek közül:

-   A gyorsítótár kiürítése, és törölje a cookie-k. Kattintson az Internet Explorerben az **eszközök** ![eszközök gomb](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internetbeállítások** > **törlése**. Győződjön meg arról, hogy az ideiglenes fájlok, a cookie-kat, a jelszó és a böngészési előzmények a jelölőnégyzet be van jelölve, és kattintson az törlése gombra.

-   Azokat a személyes beállításokat, amelyek végzett vissza az Internet Explorer beállításainak visszaállítása Kattintson az **eszközök** ![eszközök gomb](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internetbeállítások** > **Speciális** > jelölje be a **személyes beállítások törlése** > **alaphelyzetbe állítása**.

-   Keresse meg az InPrivate-böngészési módban Azure portált. Kattintson az **eszközök** ![eszközök gomb](./media/billing-cannot-login-subscription/Toolsbutton.png) > **biztonsági** > **InPrivate-böngészési**.

## <a name="need-help-contact-support"></a>Segítségre van szüksége? Forduljon az ügyfélszolgálathoz. 

Ha továbbra is segítségre van szüksége, [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a probléma megoldódott gyorsan. 