<properties
    pageTitle="Azure AD Connect a Microsoft Cloud-német"
    description="Azure AD Connect lesz a helyszíni könyvtárak integrálása az Azure Active Directory. Ez lehetővé teszi, hogy meg kell adnia egy közös identitás Azure Active Directory integrálódik az Office 365, az Azure és a szoftver-alkalmazásokhoz."
    keywords="Bevezetés az Azure AD Connect Azure AD Connect áttekintését, a Mi az Azure AD Connect, telepítse az active directory, Németország, fekete-erdői"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect a Microsoft Cloud Németország - Public Preview

## <a name="introduction"></a>– Bevezetés
Azure AD Connect közötti a helyszíni Active Directory és Azure Active Directory-szinkronizálás biztosít.
Jelenleg az jelenik meg a [Microsoft Cloud Németország](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) számos kell végrehajtani, az operátor. Microsoft Cloud Németország használatakor tartsa szem előtt a következőket:


- Az alábbi URL-címeit meg kell nyitni a proxykiszolgáló sikeresen előforduló szinkronizálási meg:
    - *. microsoftonline.de
    - *. windows.net
    - + Tanúsítvány-visszavonási listák

- Amikor bejelentkezik az Azure Active directory, a fiók onmicrosoft.de a tartományban kell használnia.
- Az alábbi szolgáltatások nem érhetők el:
    - Azure AD Connect állapota
    - Az automatikus frissítések
    - Jelszó visszaírást

## <a name="download"></a>Letöltés
Azure AD Connect letölthető az Azure AD Connect lap belül a portálon.  Az alábbi útmutatást segítségével keresse meg az Azure AD Connect lap.

### <a name="the-azure-ad-connect-blade"></a>Az Azure AD Connect lap

Miután az Azure-portálra van bejelentkezve, akkor tegye a következőket:

1. Nyissa meg a Tallózás gombra
2.  Jelölje ki az Azure Active Directory
3.  Válassza az Azure AD Connect

Meg kell jelennie a következőket:

![Azure AD Connect lap](media\active-directory-aadconnect-germany\germany1.png)

 
Az alábbi táblázat ismerteti a szolgáltatásokat, a lap látható.


Cím|Leírás|
----- | ----- |
SZINKRONIZÁLÁS ÁLLAPOTA|Nézzük meg, hogy szinkronizálási engedélyezhetik és tilthatják.|
UTOLSÓ SZINKRONIZÁLÁS|A legutóbbi szinkronizálási sikeres befejezését.|
SZÖVETSÉGES TARTOMÁNYBAN|A szövetséges tartományban már konfigurálva vannak számát mutatja.|


## <a name="installation"></a>Telepítés
Telepítse az Azure AD Connect, használhatja a dokumentációt [Itt](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Speciális funkciójára és további információk
További információt és útmutatást egyéni beállításokkal vagy speciális beállításait kezdje [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).  Ezen az oldalon itt információkkal és hivatkozásokkal vonatkozó útmutatást.
