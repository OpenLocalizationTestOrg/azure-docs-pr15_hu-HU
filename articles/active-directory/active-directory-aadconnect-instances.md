<properties
    pageTitle="Azure AD Connect: Szinkronizálása szolgáltatás példányainak |} Microsoft Azure"
    description="Ezen az oldalon az Azure Active Directory-példány megfontolandó különleges szempontok dokumentumok."
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

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Megfontolandó különleges szempontok példányok
Azure AD Connect leggyakrabban használatos az Azure Active Directory világszerte használata és az Office 365-ben. De is vannak más esetek, és ezek a URL-címek és más speciális szempontok eltérő követelményeik vannak.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud-német
A [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) egy német adatok meghatalmazott üzemeltetett szuverén felhő.

Ez a felhőben jelenleg előzetes verzióban. A szokásos megteheti úgy is, például forgatókönyveket számos ellenőrzése a tartományok, végre kell hajtania az operátor. Kérjük, forduljon a helyi Microsoft képviselőjével a követelmények további információt olvashat a nyomtatási képen részt.

Nyissa meg a proxykiszolgáló URL-EK |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Tanúsítvány-visszavonási listák |

Amikor bejelentkezik a az Azure Active directory onmicrosoft.de tartomány fiókkal kell használnia.

A funkciók jelenleg nem szerepel a Microsoft Cloud németországi:

- Azure Active Directory csatlakozás állapot nem érhető el.
- Az automatikus frissítések nem érhető el.
- Jelszó visszaírást nem érhető el.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure kormányzati felhő
A [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) az a felhő, az Amerikai Egyesült Államok kormányzati.

A felhő el lett támogatják DirSync korábbi verzióit. Az összeállítás 1.1.180 az Azure AD Connect a következő létrehozása a felhőben használata támogatott. A generációs csak USA-beli alapján végpontok használ, és nyissa meg a proxykiszolgálón az URL-címek listája van.

Nyissa meg a proxykiszolgáló URL-EK |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Tanúsítvány-visszavonási listák |

Azure AD Connect nem tudja automatikusan észleli, hogy az Azure Active directory a kormányzati felhőben található. Ehelyett kell az alábbi műveletek Azure AD Connect telepítésekor.

1. Az Azure AD Connect telepítés megkezdéséhez.
2. Az első oldalon, ahol van, fogadja el a LICENCSZERZŐDÉST feltételezett megjelenik, amint ne hajtsa végre, de az operációs rendszert futtató telepítővarázslóban hagyja.
3. Indítsa el a regedit parancsot, és beállításkulcs módosítása `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` értékre `2`.
4. Lépjen vissza az Azure AD Connect telepítési varázsló segítségével fogadja el a LICENCSZERZŐDÉST, és továbbra is. A telepítés során feltétlenül használni a **konfigurációs egyéni** telepítési elérési utat (és nem Express telepítésével). Kattintson a telepítést, a szokásos módon.

A funkciók jelenleg nem szerepel a Microsoft Azure kormányzati felhőbe:

- Azure Active Directory csatlakozás állapot nem érhető el.
- Az automatikus frissítések nem érhető el.
- Jelszó visszaírást nem érhető el.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
