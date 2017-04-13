<properties
    pageTitle="2.0-s verzió alkalmazás regisztrációs |} Microsoft Azure"
    description="Hogyan lehet rögzíteni az alkalmazás a Microsofttal bejelentkezési engedélyezése és a 2.0-s verzió végpont használata Microsoft-szolgáltatások elérése"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Hogyan lehet rögzíteni az alkalmazást a 2.0-s verzió végponttal

Az alkalmazás, amely elfogadja a MSA- és Azure AD össze bejelentkezés, először kell alkalmazás regisztrálása Microsoft.  Most, akkor használhatja az összes meglévő alkalmazás, előfordulhat, hogy az Azure Active Directory vagy MSA - kell egy teljesen újból létre.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végponton kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="visit-the-microsoft-app-registration-portal"></a>Keresse fel a Microsoft alkalmazás regisztrációs portál
Első dolog, amit először - [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nyissa meg.  Az új alkalmazás regisztrációs a portál kezelésére szolgáló a Microsoft-alkalmazásokat.

Jelentkezzen be a személyes vagy a munkahelyi vagy iskolai Microsoft-fiókjával.  Ha nem rendelkezik, vagy új személyes fiókot regisztrálni. Jóváhagyást, azt nem ideig, mire – Itt azt kell várnia.

Kész? Akkor célszerű most megtekintik Microsoft alkalmazásai, ez az valószínűleg üres.  Vegyük módosíthatja, hogy.

**Az alkalmazás hozzáadása**parancsra, és adja meg egy nevet.  A portál rendel az alkalmazás egy globálisan egyedi azonosítója, amely a a kódot kell megadnia.  Ha az alkalmazás a kiszolgálóoldali összetevőt tartalmaz, hogy szüksége van hozzáférési jogkivonat hívó API-khoz (számításba: Office, a Azure vagy a saját webes API-val), bizonyára tudni szeretné, hogy az **Alkalmazás titkos** itt is látható létrehozása.
<!-- TODO: Link for app secrets -->

Ezután adja meg a platform, amelyekkel az alkalmazást.

- Webalapú alkalmazásokat a **URI átirányítás** , ahol bejelentkezési üzenetek elküldésének szükséges.
- Mobilalkalmazások másolja le az alapértelmezett átirányítási uri automatikusan jött létre.

Tetszés szerint testre szabhatja a bejelentkezési lapja a profil szakasz kinézetét és hangulatát.  Ellenőrizze, hogy, mielőtt a **Mentés** gombra.

> [AZURE.NOTE] Amikor az alkalmazás [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)hoz létre, az alkalmazás regisztrálva lesz a fiók használatával jelentkezzen be a portálon otthoni bérlőhöz.  Ez azt jelenti, hogy nem regisztrálhatja az alkalmazások az Azure Active Directory bérlőhöz a személyes Microsoft-fiók használatával.  Ha explicit módon szeretné rögzíteni, az alkalmazás az adott bérlői webhelyet, jelentkezzen be az adott bérlői eredetileg létrehozott fiók.

## <a name="build-a-quick-start-app"></a>A rövid útmutató az alkalmazás összeállítása
Most, hogy egy Microsoft-alkalmazást, akkor a 2.0-s verzió – rövid útmutató az oktatóanyagok egyik végezhetik el.  Íme néhány javaslatok:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
