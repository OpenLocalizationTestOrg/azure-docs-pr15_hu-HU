<properties
   pageTitle="Az Azure Active Directory felügyeleti egységek kezelése"
   description="Az engedélyek az Azure Active Directory finomabb delegálása felügyeleti egységek használatával"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Az Azure Active Directory - Public Preview felügyeleti egységek kezelése

Ez a cikk ismerteti felügyeleti egységek – új Azure Active Directory tároló erőforrások a rendszergazdai engedélyekkel delegálása felett a felhasználók és a felhasználók csak egy részhalmazát alkalmazása házirendek részhalmazának használható. Az Azure Active Directory a felügyeleti egységek engedélyezése a központi rendszergazdák meghatalmazotti engedélyek területi rendszergazdák számára, vagy alapszinten házirend beállítása.

Ez akkor hasznos, a független részlegek, például egy nagy Egyetem, sok önálló iskolák (iskolája üzleti, műszaki iskolai és így tovább) egymástól független épülnek fel rendelkező szervezetek. Az ilyen részlegek van a saját informatikai rendszergazdáknak, akik a hozzáférés vezérléséhez, a felhasználók kezelése, és a kifejezetten az illető osztást házirendek beállítása. Tudja központi rendszergazdák szeretne adni a megosztott rendszergazdák engedélyek, a felhasználók az adott részlegek fölé. Pontosabban, ebben a példában használ, egy központi rendszergazdája, például hozzon létre egy felügyeleti egységet adott iskolába (üzleti iskolai), és töltse ki az üzleti iskolai felhasználói. Kattintson egy központi rendszergazdai vehet fel a vállalati iskolai informatikai szakemberek hatókörű szerepkör, ez azt jelenti, adja meg az informatikai szakemberek az üzleti iskolai rendszergazdai engedélyekkel csak az iskola felügyeleti részleg fölé.

> [AZURE.IMPORTANT]
> Létrehozhat és felügyeleti mértékegységet használni, csak akkor, ha engedélyezi a Azure Active Directory prémium verzióban. További tudnivalókért lásd: [Ismerkedés az Azure Active Directory prémium](active-directory-get-started-premium.md).

A központi rendszergazdai szempontjából egy felügyeleti mértékegysége létrehozott, és az erőforrások töltve directory-objektum. **Ebben a kiadásban ezek az erőforrások csak azok a felhasználók is lehet.** Miután elkészült, és töltve, a felügyeleti egység használható keresési tartomány a rendelt engedélyek korlátozása csak a felügyeleti egység található erőforrások fölé.

## <a name="managing-administrative-units"></a>Felügyeleti egységek kezelése

A előzetes verzióval létrehozása és kezelése a rendszergazdai mennyiség, az Azure Active Directory modul Windows Powershellhez parancsmag használatával.

További információt a szoftverkövetelményekkel és az Azure Active Directory modul telepítése, illetve az Azure Active Directory modul parancsmag felügyeleti egységek szintaxis, paraméter leírása és a példákban kezelésére szolgáló információkat olvassa el a [kezelése a Windows PowerShell használatá Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Következő lépések
[Azure Active Directory kiadások](active-directory-editions.md)
