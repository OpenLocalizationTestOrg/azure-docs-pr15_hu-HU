<properties
    pageTitle="Azure MFA felhő viewben kiszolgáló |} Microsoft Azure"
    description="Válassza ki a többtényezős hitelesítés biztonság megoldást, azzal, hogy milyen am i biztonságos próbál meg jobbra és a hol találhatók a felhasználóim található.  Válassza a felhőben, MFA-kiszolgáló és AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Az Azure többtényezős hitelesítés megoldás kiválasztása

Mivel több változatok Azure többtényezős hitelesítés (MFA), megállapítása, hogy melyik verziót használni a megfelelő egy néhány kérdésre választ kell adnia azt.  A fenti kérdések a következők:

-   [Mi megkísérlem az biztonságos](#what-am-i-trying-to-secure)
-   [Hol találhatók a felhasználóknak](#where-are-the-users-located)
- [Milyen szolgáltatások van szükségem?](#what-featured-do-i-need)

Az alábbi szakaszok annak megállapítása, hogy ezek a válaszok egyes ad útmutatást.

## <a name="what-am-i-trying-to-secure"></a>Mi megkísérlem az biztonságos?

Annak megállapításához, a megfelelő két lépésből álló ellenőrzési megoldást, először azt választ kell adnia a kérdés, melyek a második hitelesítési módszer, biztonságos próbál meg.  Olyan alkalmazás, amely az Azure-ban található ez?  Vagy távelérési rendszert?  Annak megállapítása, hogy milyen azt próbál biztonságos, amelyet a kérdés, ahol többtényezős hitelesítés engedélyezni kell fogadásához azt.  


Mi próbál biztonságos| Többtényezős hitelesítés a felhőben|Többtényezős hitelesítés kiszolgáló
------------- | :-------------: | :-------------: |
Első fél Microsoft-alkalmazások|● |● |
Az alkalmazás gyűjteményben szoftver-alkalmazások|● |● |
Azure Active Directory-alkalmazás proxyn keresztül közzétett IIS-alkalmazások|● |● |
Azure Active Directory-alkalmazás proxyn keresztül nem közzétett IIS-alkalmazások | |● |
Távoli hozzáférés például VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Hol találhatók a felhasználóknak

Ezután rögzíthetők a hol találhatók a felhasználók segítségével határozza meg a megfelelő megoldást szeretne használni, az a felhő vagy a helyszíni MFA-kiszolgálóval.



Felhasználók helye| Többtényezős hitelesítés a felhőben|Többtényezős hitelesítés kiszolgáló
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure Active Directory és a helyszíni Active Directory összevonási használata az Active Directory összevonási szolgáltatások|● |● |
Azure Active Directory és a helyszíni Active Directory DirSync, Azure AD-szinkronizálás Azure AD Connect - nincs jelszó-szinkronizálás használata|● |● |
Azure Active Directory és a helyszíni Active Directory DirSync, Azure AD-szinkronizálás Azure AD Connect - használata jelszó-szinkronizálás|● | |
A helyszíni Active Directory| |● |

## <a name="what-features-do-i-need"></a>Milyen szolgáltatások van szükségem?

Az alábbi táblázat a többtényezős hitelesítést a felhőben, és a többtényezős hitelesítést kiszolgálóval elérhető szolgáltatásainak összehasonlítása.

 | Többtényezős hitelesítés a felhőben | Többtényezős hitelesítés kiszolgáló
------------- | :-------------: | :-------------: |
Mobilalkalmazás értesítést, a második tényező | ● | ● |
Mobilalkalmazás ellenőrzőkódot, mint a második tényező | ● | ●
Telefonhívás, mint a második tényező | ● | ●
Egyirányú SMS, a második tényező | ● | ●
Kétirányú SMS, mint a második tényező |  | ●
Hardveres tokenek, a második tényező |  | ●
Alkalmazás jelszavak MFA nem támogató ügyfélalkalmazások | ● |  
Rendszergazdai szabályozható hitelesítési módszerek | ● | ●
PIN-kód mód |  | ●
Csalás értesítés | ● | ●
MFA-jelentések | ● | ●
Egyszeri figyelmen kívül hagyása |  | ●
Egyéni Üdvözlések esetében a telefonhívások | ● | ●
Testre szabható Hívóazonosító esetében a telefonhívások | ● | ●
Megbízható IP-címei | ● | ●
Ne feledje, hogy MFA megbízható eszközök  | ● |  
Feltételes hozzáférés | ● | ●
Gyorsítótár |  | ●

Most, hogy szeretné-e használni a felhőben többtényezős hitelesítés vagy a MFA kiszolgáló helyszíni határozott, hogy miként vehetik használatba beállításával és Azure többtényezős hitelesítés használatával. **Jelölje ki az igényektől jelző ikon!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
