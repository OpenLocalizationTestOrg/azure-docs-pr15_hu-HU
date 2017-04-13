<properties
    pageTitle="Azure Active Directory jogosultsággal rendelkező Identitáskezelés |} Microsoft Azure"
    description="Ez a témakör ismerteti, hogy mi az Azure Active Directory Yammerhez Identitáskezelés és használatáról a személyes Információkezelő javíthatja a felhőben biztonságát."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure Active Directory jogosultsággal rendelkező Identitáskezelés

Azure Active Directory (AD) az Identitáskezelés jogosultsággal rendelkező kezelheti a vezérlőt, és access figyelheti a szervezeten belül. Ide tartoznak a források az Azure Active Directory és más online a Microsoft-szolgáltatásokkal, például az Office 365- vagy Microsoft Intune való hozzáférést.  

> [AZURE.NOTE] Csak az Azure Active Directory P2 prémium kiadásának a Yammerhez Identitáskezelés érhető el. További tudnivalókért lásd: az [Azure Active Directory kiadásai](active-directory-editions.md).

Szervezetek szeretne biztonságos információk vagy erőforrásokat, hozzáférési jogosultsággal rendelkező személyek számának csökkentése érdekében, mert, amely csökkenti az esélye annak, hogy az első rosszindulatú felhasználó. Felhasználók azonban továbbra is kell Azure, az Office 365-ben és a szoftver alkalmazás a Yammerhez műveletek elvégzését. Szervezetek hozzáférést felhasználók Yammerhez az Azure Active Directory anélkül, hogy mit tesznek azoknak a felhasználóknak az a rendszergazdai engedélyekkel figyelése. Azure Active Directory jogosultsággal rendelkező Identitáskezelés segít a kockázatot feloldásához.  

Azure Active Directory jogosultsággal rendelkező Identitáskezelés segítségével:  

- Mely felhasználók találhatók Azure AD-rendszergazdák
- Igény szerinti, engedélyezze a "csak az idő" a Microsoft Online Services rendszergazdai hozzáférése például az Office 365-ben és az Intune
- Kapcsolatfelvétel észleléséről készült jelentések megtekinté rendszergazdai hozzáférés előzmények és a módosítások rendszergazda hozzárendelések
- Hozzáférési jogosultsággal rendelkező szerepkörbe kapcsolatos értesítéseket kapjon

Azure Active Directory Yammerhez Identitáskezelés kezelhetik a beépített Azure Active Directory szervezeti szerepköreiről, például:  

- Globális rendszergazda
- Számlázási adminisztrátor
- Szolgáltatás-rendszergazda  
- Felhasználó-rendszergazda
- Jelszókezelő

## <a name="just-in-time-administrator-access"></a>Csak az idő rendszergazdai hozzáférés

Régebben felhasználó sikerült hozzárendelése rendszergazdai szerepkör az Azure klasszikus portálja vagy a Windows PowerShell keresztül. Emiatt a felhasználó számára egy **állandó rendszergazda**, a kiosztott szerepkör mindig aktív lesz. Azure Active Directory jogosultsággal rendelkező Identitáskezelés **jogosult felügyeleti**fogalmának. Jogosult rendszergazdák most majd, de nem minden nap a Yammerhez hozzáférést igénylő felhasználók kell lennie. A szerepkör nem aktív, amíg a felhasználó, hozzáférésre van szüksége, akkor az aktiválási folyamat befejezéséhez, és időt hibaszám aktív rendszergazda válik.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Kiemelt Identitáskezelés engedélyezése a címtárban

Az [Azure portálon](https://portal.azure.com/)is Azure Active Directory Yammerhez Identitáskezelés használatbavétele.

>[AZURE.NOTE] Szervezeti fiókkal globális rendszergazdának kell lennie (például @yourdomain.com), nem Microsoft-fiókkal (például @outlook.com), ahhoz, hogy egy könyvtár az Azure Active Directory jogosultsággal rendelkező Identitáskezelés.

1. Jelentkezzen be az [Azure portált](https://portal.azure.com/) a könyvtár globális rendszergazdaként.
2. Ha a szervezete egynél több címtár, jelölje ki a felhasználónév az Azure portál jobb felső sarkában. Jelölje ki a címtárban, ahol Azure Active Directory jogosultsággal rendelkező Identitáskezelés fogja használni.
3. Jelölje ki a **További szolgáltatások** , és a szűrő mezőben lévő értéket használja szeretne keresni az **Azure Active Directory Yammerhez Identitáskezelés**.
4. Jelölje be a **PIN-kód irányítópult** , és kattintson a **Létrehozás**gombra. Ekkor megnyílik a Yammerhez Identitáskezelés alkalmazást.

Ha az első személy használata az Azure Active Directory Yammerhez Identitáskezelés címtárában, majd a [Biztonság varázsló](active-directory-privileged-identity-management-security-wizard.md) végigvezeti a hozzárendelés eredeti felület. Ezt követően automatikusan lesz a első **biztonsági rendszergazdája** és a **rendszergazda jogosultsággal rendelkező szerepkör** a címtárban.

Csak a Yammerhez szerepkör rendszergazdája kezelheti az access más rendszergazdák számára. Azt is megteheti, hogy [más felhasználók számára lehetőséget adni a személyes Információkezelő kezeléséhez](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Kiemelt Identitáskezelés irányítópult

Azure Active Directory jogosultsággal rendelkező identitáskezelő biztosít, amellyel fontos információkat, például egy irányítópult:

- Értesítések, amely felhívja a lehetőségek, biztonság növelése érdekében
- A Yammerhez szerepkörökhöz rendelt felhasználók száma  
- Jogosult és állandó rendszergazdák száma
- Áttekinti a folyamatos hozzáféréshez.

![Személyes Információkezelő irányítópult - képernyőképe][2]

## <a name="privileged-role-management"></a>Kiemelt szerepkörkezelés

Azure Active Directory Yammerhez Identitáskezelés kezelheti a rendszergazdák hozzáadásával és eltávolításával állandó vagy jogosult rendszergazdák minden szerepkörhöz.

![Személyes Információkezelő hozzáadása és eltávolítása rendszergazdák - képernyőképe][3]

## <a name="configure-the-role-activation-settings"></a>A szerepkör aktiválási beállításainak konfigurálása

[Szerepkör beállításainak](active-directory-privileged-identity-management-how-to-change-default-settings.md) használatával az jogosult szerepkör aktiválási tulajdonságait, például állíthatja be:

- A szerepkör aktiválási időszak hosszának
- A szerepkör aktiválási értesítés
- Az adatokat egy felhasználónak kell adnia a szerepkör aktiválási folyamata során  

![Képernyőkép a személyes Információkezelő - rendszergazda aktiválás - beállítások][4]

Ne feledje, hogy a kép, a gombok **Többtényezős** hitelesítéshez le vannak tiltva. Igen, az egyes kiemelt jogosultságokkal rendelkezik szerepkörök, azt fokozása védelem MFA van szükség.

## <a name="role-activation"></a>Szerepkör az aktiválás  

A [szerepkörbe aktiválása](active-directory-privileged-identity-management-how-to-activate-role.md)jogosult rendszergazda kéri egy idő kötött "aktiválási" a szerep. Az aktiválás kérhet Azure Active Directory Yammerhez Identitáskezelés használata az **Aktiválás: a szerepkör** lehetőséget.

Egy szerepkör aktiválásához kívánó rendszergazdának inicializálni az Azure Active Directory Yammerhez Identitáskezelés az Azure-portálon.

Szerepkör aktiválás egy testre szabható. A személyes Információkezelő beállításainak meghatározhatja az aktiválás, és mit hossza a rendszergazdának ahhoz, hogy aktiválja a szerepkör információkat.

![Személyes Információkezelő rendszergazda kérelem szerepkör aktiválás – kép][5]

## <a name="review-role-activity"></a>Tekintse át a szerepkör tevékenység

Kétféleképpen milyen az alkalmazottak és a rendszergazdák használják a Yammerhez szerepkörök nyomon követéséhez. Az első lehetőséget [Naplózási előzmények](active-directory-privileged-identity-management-how-to-use-audit-log.md)használ. A naplózási előzményeket naplózza a változások követése a Yammerhez szerepkör-hozzárendelések és szerepkör aktiválási előzmények.

![Személyes Információkezelő aktiválási előzmények - képernyőképe][6]

A második, hogy normál [access áttekinti](active-directory-privileged-identity-management-how-to-start-security-review.md)beállítása. Ezeket az access véleményezések által elvégzett, és rendelt véleményezők (például a csapat-kezelő), vagy az alkalmazottak áttekintheti magukat. Ez a legegyszerűbben úgy, hogy figyelheti a ki is hozzáférést igényel, és ki nem tartalmaz.


## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
