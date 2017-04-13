<properties
   pageTitle="Azure AD Connect: Kép funkciójával |} Microsoft Azure"
   description="Ez a témakör ismerteti a több részlet szolgáltatást, amelyek az Azure AD Connect előzetes verzióban."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Többet szeretne tudni az előnézeti lehetőségei
Ez a témakör ismerteti a preview szolgáltatások használatához.

## <a name="group-writeback"></a>Csoport visszaírást
A választható funkció a csoport visszaírást beállítás lehetővé teszi, való visszaírást **Office 365-csoportok** erdőhöz Exchange telepítve van programmal. Ez a egy csoportot, amely a program mindig lezárt fájlrendszer a felhőben. Ha helyszíni Exchange van, akkor is írhat vissza ezekbe a csoportokba a helyszíni így felhasználóit a helyszíni Exchange-postaládához is küldhet és fogadhat e-mailben a ezekbe a csoportokba.

További információt az Office 365-csoportok és használatuk megtalálható [Itt](http://aka.ms/O365g).

Ebben a csoportban jelenik meg, terjesztési csoportként a helyszíni Active Directory tartományi szolgáltatások. A helyszíni Exchange server Exchange 2013 összesítő frissítés 8 (a Skype 2015 vállalati március kiadott) vagy az Exchange 2016 új csoport elemtípushoz felismerje kell lennie.

**Jegyzetek az előzetes verzióban**

- A cím könyv attribútum jelenleg nem kitölti az előzetes verzióban. Ez az attribútum nélkül a csoport érintettek nem láthatják a globális címlistában. Az adott jellemző feltöltéséhez legegyszerűbben az Exchange PowerShell-parancsmag használata `update-recipient`.
- Csak az Exchange sémával erdők érvényes célok csoportok számára is. Ha nem Exchange észlelt, csoport visszaírást nem lehet az engedélyezése.
- Csak egyetlen erdőn Exchange-szervezet telepítések jelenleg támogatott. Ha egynél több Exchange szervezet helyszíni, majd szüksége lesz egy helyszíni GALSync megoldás jelenjen meg a többi erdőkben csoportokhoz.
- A csoport visszaírást funkció jelenleg nem kezeli az biztonsági csoportoknak vagy terjesztési csoportokat.

>[AZURE.NOTE] Azure Active Directory prémium verzióra való előfizetés szükség a csoport visszaírást.

## <a name="user-writeback"></a>Felhasználói visszaírást
> [AZURE.IMPORTANT] A felhasználó visszaírást megtekintése funkció el lett távolítva az Azure AD Connect augusztus 2015 módosítás. Ha azt engedélyezte a, majd le kell tiltania ezt a szolgáltatást.

## <a name="next-steps"></a>Következő lépések
Az [Azure AD Connect az egyéni telepítés](./connect/active-directory-aadconnect-get-started-custom.md)folytatásához.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
