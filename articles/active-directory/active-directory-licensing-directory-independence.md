<properties
   pageTitle="Hozzáadása és kezelése több Azure Active Directory könyvtárak |} Microsoft Azure"
   description="Útmutatók és a gyakorlati tanácsok a hozzáadása és kezelése az Azure Active Directory könyvtárak, mailbe arról, hogy egy teljesen független erőforrásként könyvtárak"
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

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Hozzáadása és több Azure Active Directory könyvtárak kezelése

Az Azure Active Directory (Azure Active Directory), minden egyes címtár egy teljesen független erőforrás: egy partner teljes szolgáltatáskészletet nyújtó, és logikailag független más könyvtárak, hogy Ön kezeli. Könyvtárak között nincs a szülő – gyermek kapcsolat áll fenn. Könyvtárak között e függetlenséget erőforrás függetlenség, a felügyeleti függetlenség és a szinkronizálás függetlenség tartalmazza.

##<a name="resource-independence"></a>Erőforrás függetlenség

Hoz létre, vagy egy adott könyvtár erőforrás törlése, ha minden erőforrás egy másik címtárban, a külső felhasználók leírása alább részleges kivétellel még nincs hatással. Egy adott könyvtár az egyéni tartomány "contoso.com" használja, ha azt bármilyen más Directoryval nem használhatók.

##<a name="administrative-independence"></a>Felügyeleti függetlenség

Ha könyvtár "Contoso" a nem rendszergazdai felhasználó majd "Próba" próba könyvtár létrehozása:
- Alapértelmezés szerint a felhasználó, aki létrehoz egy mappát, hogy új címtárban külső felhasználóként hozzá, és a globális rendszergazdai szerepkört, hogy a címtárban.
- A rendszergazdák könyvtár "Contoso" nincs közvetlen rendszergazdai jogosultságok Directoryhoz "Próba" kivéve, ha a rendszergazda "Próba" kifejezetten részesíti őket ezek a jogosultságok. "Contoso" rendszergazdái is címtár "Próba" való hozzáférés szabályozása, ha az azok szabályozhatja, hogy a felhasználói fiókot, amely létrehozott "Próba."
- Ha például módosítja (hozzáadása vagy eltávolítása) rendszergazdai szerepkör egy adott könyvtár egy felhasználóhoz, a módosítás nem befolyásolja bármely rendszergazdai szerepkör, amelyek a felhasználói lehet egy másik címtárban.

##<a name="synchronization-independence"></a>Szinkronizálás függetlenség

Beállíthatja, hogy egymástól függetlenül minden Azure Active Directory könyvtár akár egyetlen példányából szinkronizált adatok beolvasása:
  - A címtár-szinkronizáló (DirSync) eszközben, az adatok szinkronizálása az Active Directory erdőn.
  - Az Azure Active Directory Connector-Forefront identitáskezelő, az adatok szinkronizálása az egy vagy több helyszíni erdők és/vagy a nem Azure Active Directory-adatforrások.

##<a name="add-an-azure-ad-directory"></a>Az Azure Active directory hozzáadása

Az Azure Active directory hozzáadása az Azure klasszikus portálon, jelölje ki az Azure Active Directory-bővítmény a bal oldalon, és koppintson a **Hozzáadás**gombra.

> [AZURE.NOTE]   Azure anyagainak eltérően a könyvtárak nem jelennek az Azure előfizetéssel gyermek forrásai. Ha előbb lemondja vagy lehetővé teszi az Azure előfizetése lejár, továbbra is elérheti az Azure PowerShell, az Azure Graph API vagy más felületek használata az Office 365 felügyeleti központ például directory adatok. Egy másik előfizetés is társíthat a címtárban.

Egy széles áttekintése az Azure Active Directory licencelési hibák és a gyakorlati tanácsok [Mi az Azure Active Directory licencelési?](active-directory-licensing-what-is.md).
