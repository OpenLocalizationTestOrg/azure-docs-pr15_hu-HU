<properties
   pageTitle="Hogyan többtényezős hitelesítést igényel |} Microsoft Azure"
   description="Az Azure Active Directory Yammerhez Identitáskezelés kiterjesztéssel Yammerhez identitások megtudhatja, hogy miként (MFA) többtényezős hitelesítést igényel."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Hogyan lehet az Azure Active Directory Yammerhez Identitáskezelés MFA megkövetelésére

Azt javasoljuk, hogy a rendszergazdák minden szükséges többtényezős hitelesítés (MFA). Így csökkenthető a kockázat miatt a biztonságos jelszó-támadások.

Kérheti, hogy felhasználók egy MFA beavatkozás igazolására szolgáló eljárás végre, ha bejelentkezni. A blogbejegyzés [az Office 365 és az Azure MFA MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) hasonlítja össze, mit tartalmaz az Office és az Azure előfizetések található a Microsoft Azure többtényezős hitelesítés felületek funkcióival.

Hogy felhasználók egy MFA beavatkozás igazolására szolgáló eljárás végre, azok aktiválása az Azure Active Directory személyes Információkezelő szerepkörbe is szükséges. Módja, ha a felhasználó nem elvégezni egy MFA beavatkozás igazolására szolgáló eljárás, hogy bejelentkezés után, kéri ehhez által a személyes Információkezelő.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Azure Active Directory jogosultsággal rendelkező Identitáskezelés MFA megkövetelése

A személyes Információkezelő a Yammerhez szerepkör rendszergazdájaként identitások felügyeletét látja el, amikor figyelmeztetések, amelyek MFA javasoljuk jogosultsággal rendelkező partnerek számára látható. Kattintson a személyes Információkezelő irányítópulton a biztonsági figyelmeztetés, és egy új lap nyílik meg, hogy kell MFA a rendszergazdai fiókok listáját.  MFA kérheti több szerepkörök kiválasztása, majd a **javítás** gombra, vagy kattintson az egyes szerepkörök melletti három pontra, és kattintson a **javítás** gombra.

> [AZURE.IMPORTANT] Jobb oldali most Azure MFA csak akkor működik munkahelyi vagy iskolai fiók esetén nem Microsoft-fiókok (általában egy személyes fiók használatával jelentkezzen be Microsoft-szolgáltatásokkal, például Skype, Xbox, Outlook.com, stb.). Emiatt bárki egy Microsoft-fiókkal nem lehet jogosult rendszergazda, mert nem használják a MFA aktiválása a szerepköröket. Ha ezek a felhasználók továbbra is a Microsoft-fiókkal munkaterhelésekből kezelése van szükség, szintű őket állandó rendszergazdák most.

Továbbá módosíthatja egy adott szerepkört MFA kötelező gombjával a személyes Információkezelő irányítópult szerepkörök területen. Kattintson a **Beállítások** a szerepkörkezelés lap, és kiválasztja az **engedélyezése** többtényezős hitelesítés.

## <a name="how-azure-ad-pim-validates-mfa"></a>Miként ellenőrzi az Azure Active Directory személyes Információkezelő a MFA

Kétféleképpen lehet érvényesítése MFA, amikor a felhasználó aktiválja a szerepkör a.

A legegyszerűbb, hogy a felhasználók, akik a Yammerhez szerepkör aktiválása az Azure MFA támaszkodhat. Ehhez először jelölje be azoknak a felhasználóknak licence, ha szükséges, és Azure többtényezős regisztrálta. Ezzel kapcsolatban további információt az [Ismerkedés az Azure többtényezős hitelesítés a felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)van. Akkor ajánlott, de nem szükséges, úgy beállítani, hogy Azure AD-megkövetelésére MFA a felhasználóknak, ha bejelentkezni. Ennek oka az, a MFA ellenőrzések Azure Active Directory személyes Információkezelő magát szerint történik.

Azt is megteheti Ha a felhasználókat a helyszíni hitelesíteni a felelős MFA identitásszolgáltató lehet. Például ha beállította az Active Directory összevonási szolgáltatások Azure Active Directory, a [biztonságossá tétele felhő erőforrások Azure többtényezős hitelesítés és AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) megnyitása előtt intelligens kártya-alapú hitelesítést igényel útmutatást tartalmaz konfigurálása az Active Directory összevonási szolgáltatások Azure Active Directory jogcímalapú küldhet. Ha a felhasználó megpróbál egy szerepkör aktiválásához, Azure Active Directory személyes Információkezelő akkor fogad el, hogy MFA már érvényesítése megtörtént a felhasználó kap a megadandó claims után.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
