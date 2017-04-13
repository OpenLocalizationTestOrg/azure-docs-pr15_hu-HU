<properties
    pageTitle="Felhasználó felvétele az Azure RemoteApp webhelycsoport |} Microsoft Azure"
    description="Megtudhatja, hogy miként vehet fel felhasználókat az Azure RemoteApp gyűjtemény"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>A felhasználók hozzáadása az Azure RemoteApp gyűjtemény

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ahhoz, hogy a felhasználók és -alkalmazások használata a Azure RemoteApp, hogy hozzáférést őket a webhelycsoport. Egyszerű része: a **Felhasználói hozzáférés** lapon adja meg a fiók adatait a felhasználó számára, és kattintson a pipára.

Fiókkal kapcsolatos információk van szüksége? A webhelycsoport típusú, elkészült (felhő vagy hibrid), és hogy használ az Office 365 ProPlus, hogy a webhelycsoportban, amely függ.

## <a name="supported-user-identities"></a>Támogatott felhasználói identitások

A webhelycsoport különböző típusú (hibrid összehasonlítása a felhőben) támogatja az access-alkalmazások használata a különböző felhasználói identitások.  

RemoteApp hibrid gyűjteménye szeretne az Active Directory tartományi infrastruktúra beállítása helyiségek és az Azure Active Directory-ös bérlői webhelye címtár-integrációs (és másik lehetőségként egyszeri bejelentkezés). Ezenkívül kell néhány az Active Directory-objektum létrehozása a helyszíni címtárban.  

RemoteApp felhő gyűjteménye bármelyik felhasználó, amelynek az Azure Active Directory támogatja az azonosítási is kell adni a felhasználói hozzáférés történő távoli alkalmazás formájában használva, amelyet fel szeretne venni a Microsoft Accounts.  Az alábbi táblázatban látható.

Az Office 365-felhasználók Azure Active Directory-felhasználók. Ha rendelkeznek az Azure Active Directory hibrid, címtár szinkronizált fiókok, azok is felhasználói hozzáférési jogosultsággal RemoteApp hibrid környezetben.   

Az alábbi táblázat rövid összefoglalása, amelynek identitás támogatott, a webhelycsoport és az Active Directory-követelményei vannak is használhatja.

|Felhasználói fiókok |Felhőalapú   |Hibrid|
|--------------|--------|------|
|Microsoft-fiók|     igen|    nem|
|Azure Active Directory (Azure AD)| | |
|Azure Active Directory felhő csak    |igen    |nem |
|A jelszó-szinkronizálás ADsync  |igen    |igen    |
|Jelszó-szinkronizálás nélkül ADsync|  igen |nem |
|Az AD FS ADsync  |igen    |igen    |
|[Azure 3rd külső Identitásszolgáltatók támogatott](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (például a Ping) |igen    |igen|
|Többtényezős hitelesítés    |igen    |igen    |

Nézze meg a konfigurálása az Active Directory RemoteApp [További információt](remoteapp-ad.md) .


> [AZURE.NOTE] Az Azure Active Directory-felhasználók kell lennie a bérlőjének, amely az előfizetéshez van társítva. (Megtekintése és módosítása az előfizetés a **Beállítások** lapon a portálon. [Módosítás az Azure Active Directory-bérlői RemoteApp által használt](remoteapp-changetenant.md) további információt talál.)

## <a name="office-365-proplus-user-account-information"></a>Az Office 365 ProPlus felhasználói fiókok adatait
Ha a webhelycsoport *vagy* az Office 365 ProPlus sablonként képe esetén, ha létrehozott egy egyéni képe az Office 365 használó, csak engedélyezettek, hogy az előfizetése az alapértelmezett tartományt az Office 365-előfizetése van Azure Active Directory-felhasználók felvétele az. További információt talál [az Office 365 használata Azure RemoteApp](remoteapp-o365.md) .
