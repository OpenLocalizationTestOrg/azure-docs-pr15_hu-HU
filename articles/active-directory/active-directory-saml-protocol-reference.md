<properties
    pageTitle="Azure Active Directory SAML Protocol hivatkozás |} Microsoft Azure"
    description="Ez a cikk áttekintést nyújt a Single Sign-On, és egyetlen Sign-Out SAML profilok az Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Hogyan használja az Azure Active Directory a SAML protokollt

Azure Active Directory (Azure Active Directory) használ, a SAML 2.0-s jegyzőkönyv tudják az alkalmazások ahhoz, hogy egy egyszeri bejelentkezéses megoldást a felhasználók. Azure AD [Az egyszeri bejelentkezés](active-directory-single-sign-on-protocol-reference.md) , és [egyetlen kijelentkezési](active-directory-single-sign-out-protocol-reference.md) SAML profiljai bemutatják, hogyan SAML Előfeltételek, protokollok és kötések használhatók az identitás-szolgáltató szolgáltatásban.

SAML protokoll szükséges a identitásszolgáltató (Azure Active Directory) és a hozzáférés-szolgáltató (az alkalmazás) az exchange maguk információt.

Az alkalmazások az Azure Active Directory regisztrálásakor az alkalmazás fejlesztője Azure Active Directory összevonási kapcsolatos információ regisztrálja. Ide tartoznak a **URI átirányítás** és a **Metaadat-alapú URI** az alkalmazás.

Azure AD a felhőalapú szolgáltatás a **Metaadat-alapú URI** beolvassa az aláírási billentyűt, és jelentkezzen ki a felhőbeli szolgáltatástól URI-használja. Ha az alkalmazás nem támogatja a metaadat-alapú URI, a Fejlesztőeszközök kell lépnie a Microsoft terméktámogatási a Kijelentkezés URI megadására, és bejelentkezés billentyűt.

Azure Active Directory bérlői-specifikus és közös (bérlői független) egyetlen és egyszeri bejelentkezéses kijelentkezési végpontok közzététele. Az alábbi URL-címmel rendelkező helyek képviseli--még nem csak egy azonosítók – így láthatja el a kívánt végpontot, olvassa el a metaadatok.

 - A bérlői webhely-specifikus végpont a következő helyen található `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  A <TenantDomainName> helyőrző egy bejegyzett tartománynév vagy a az Azure AD-bérlő TenantID globálisan egyedi azonosítója. Például az összevonási a contoso.com címen működő bérlőn metaadatai címen: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- A bérlői beállítástól független végponton található a `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. A végpontjának címe **közös** jelenik meg, helyett egy bérlői tartománynevet, vagy azonosítót használva.

A tesz közzé, Azure Active Directory összevonási metaadat-alapú dokumentumok tudni olvassa el a [Metaadat-alapú összevonás](active-directory-federation-metadata.md)című témakört.
