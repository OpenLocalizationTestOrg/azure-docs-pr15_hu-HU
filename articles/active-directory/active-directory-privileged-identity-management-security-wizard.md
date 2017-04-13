<properties
   pageTitle="Az Azure Active Directory Yammerhez Identitáskezelés adatvédelmi varázsló"
   description="Az Azure Active Directory Yammerhez Identitáskezelés bővítmény használata első alkalommal jelenik biztonsági varázsló segítségével. Ez a cikk ismerteti a lépéseket a varázslóval."
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

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Az Azure Active Directory jogosultsággal rendelkező Identitáskezelés adatvédelmi varázsló

Ha az első személy futtatásához Azure Yammerhez identitás kezelése (személyes Információkezelő) a szervezet számára, bemutatni varázsló segítségével. A varázsló segít megérteni a biztonsági kockázatot, kiemelt identitások és használatáról a személyes Információkezelő ezeket a kockázatok csökkentésére. Nem kell bármilyen módosítást végez meglévő szerepkör-hozzárendelések a varázslóban, ha később tegye inkább.

## <a name="what-to-expect"></a>Várható fejlemények

Személyes Információkezelő használata a szervezet megkezdése előtt szerepkör-hozzárendelés állandó: a felhasználók mindig vannak ezeket a szerepköröket, ha az azok jelenleg nem szükséges jogosultságaik.  A varázsló első lépésében megjelennek a magas szintű jogosultsággal rendelkező szerepkörének felsorolása és jelenleg hány felhasználóm ezeket a szerepköröket. Ha többet szeretne tudni a felhasználók, ha egy adott szerepkörbe Felhatolás vagy közülük ismeretekkel.

A második lépés a varázsló lépve rendszergazdai szerepkör-hozzárendelés módosítása lehetőséget.  

> [AZURE.WARNING]Fontos, hogy legalább egy globális rendszergazdai, és több jogosultsággal rendelkező szerepkör rendszergazda (nem Microsoft-fiók) szervezeti fiókkal van. Ha csak egy kiemelt szerepkör-rendszergazda, a szervezet nem tudnak kezelése a személyes Információkezelő, ha a fiók törlődik.
> Is tartsa szerepkör-hozzárendelés állandó, ha egy felhasználó rendelkezik Microsoft-fiókkal (fiók használják a jelentkezzen be Microsoft-szolgáltatásokkal, például az Outlook.com és a Skype). Ha az aktiválás, a szerepkör MFA megkövetelésére, a felhasználó zárolva lesz.


Miután módosította, a varázsló nem jelennek meg. A következő alkalommal, amikor Ön vagy egy másik szerepkör jogosultsággal rendelkező rendszergazda használni a személyes Információkezelő, látni fogja a személyes Információkezelő irányítópult.  

- Ha szeretné, hogy hozzáadása vagy a felhasználók eltávolítása a szerepkörök, vagy az állandó jogosult, a további információ: How [to hozzáadása vagy eltávolítása egy felhasználói szerepkör](active-directory-privileged-identity-management-how-to-add-role-to-user.md)-hozzárendelések módosítása.
- Ha szeretné, hogy több felhasználót adhat a hozzáférést a személyes Információkezelő kezeléséhez, további, [hogy miként adhat hozzáférést a személyes Információkezelő kezeléséhez](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
