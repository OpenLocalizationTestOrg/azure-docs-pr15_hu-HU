<properties
    pageTitle="Azure AD Connect: Portok |} Microsoft Azure"
    description="Ez a lap nyitva az Azure AD Connect szükséges portokon technikai útmutató lap"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hibrid identitás szükséges portok és protokollok
A következő kombináció technikai hivatkozás adja meg a szükséges portok és protokollok hibrid identitás megoldást végrehajtásához szükséges adatokat. Használja az alábbi ábrán, és a megfelelő táblázatból.

![Mi az Azure AD Connect](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Táblázat 1 - Azure Active Directory csatlakozás és a helyszíni Active Directory
Az alábbi táblázat ismerteti az portok és protokollok szükséges az Azure AD Connect kiszolgáló közötti kommunikáció és a helyszíni Active Directory.

Protocol (protokoll) | Portok | Leírás
--------- | --------- |---------
DNS|53 (TCP/UDP)| Kattintson a célerdőben keresések DNS.
Kerberos|88 (TCP/UDP)| Kerberos-hitelesítés, az Active Directory erdőhöz.
MS-RPC |135 (TCP/UDP)| Használja a kiindulási konfiguráció az Azure AD Connect varázsló során, amikor azt az Active Directory erdő köti.
LDAP|389 (TCP/UDP)| Adatok importálása az Active Directory használható. Adatok titkosítva van a Kerberos bejelentkezési és zár.
LDAP/SSL|636 (TCP/UDP)| Adatok importálása az Active Directory használható. Az adatátvitel bejelentkezve, és a titkosított. Csak akkor használható, ha a használja az SSL.
TÁVOLI ELJÁRÁSHÍVÁS |49152 – 65535 (véletlen magas RPC Port)(TCP/UDP)| Használja a kezdeti Azure AD Connect-konfiguráció során, amikor azt az Active Directory-forests köti. [KB929851](https://support.microsoft.com/kb/929851) [KB832017](https://support.microsoft.com/kb/832017)és [KB224196](https://support.microsoft.com/kb/224196) talál további információt.

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Csatlakozás táblázat 2 - Azure Active Directory és Azure Active Directory
Az alábbi táblázat összefoglalja az portok és protokollok az Azure AD Connect-kiszolgáló és Azure Active Directory közötti kommunikációhoz szükséges.

Protocol (protokoll) |Portok |Leírás
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Töltse le a CRL-ek (tanúsítvány-visszavonási listák) SSL-tanúsítványok ellenőrzésére szolgál.
HTTPS|443(TCP/UDP)| Szinkronizálása az Azure Active Directory szolgál.

Listáját a URL-EK és IP-cím meg kell nyitnia a tűzfalon, lásd: [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Csatlakozás táblázat 3 – az Azure Active Directory és az összevonási kiszolgálók/WAP
Az alábbi táblázat összefoglalja az portok és protokollok az Azure AD Connect és összevonási/WAP kiszolgáló közötti kommunikáció szükséges.  

Protocol (protokoll) |Portok |Leírás
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Töltse le a CRL-ek (tanúsítvány-visszavonási listák) SSL-tanúsítványok ellenőrzésére szolgál.
HTTPS|443(TCP/UDP)| Szinkronizálása az Azure Active Directory szolgál.
WinRM|5985| WinRM figyelő

## <a name="table-4---wap-and-federation-servers"></a>Táblázat-4 - WAP és az összevonási kiszolgálók
Az alábbi táblázat összefoglalja az portok és protokollok az összevonási kiszolgálók és WAP kiszolgálók közötti kommunikációhoz szükséges.

Protocol (protokoll) |Portok |Leírás
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Hitelesítéshez használt.

## <a name="table-5---wap-and-users"></a>5 – WAP és a felhasználók táblában
Az alábbi táblázat összefoglalja az portok és protokollok szükséges a felhasználók és a WAP kiszolgálók közötti kommunikációt.

Protocol (protokoll) |Portok |Leírás
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Eszköz hitelesítéshez használt.
TCP|49443 (TCP)| Hitelesítési tanúsítványt használja.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Táblázat 6a és 6b - Azure Active Directory csatlakozás állapot ügynök az (AD FS /-szinkronizálás) és Azure Active Directory
Az alábbi táblázat ismerteti a végpontok portok és protokollok Azure Active Directory csatlakozás állapot ügynökök és Azure Active Directory közötti kommunikációhoz szükséges

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Táblázat 6a - portok és protokollok Azure Active Directory csatlakozás állapot Agent az (AD FS /-szinkronizálás) és Azure Active Directory
Az alábbi táblázat összefoglalja az alábbi kimenő portok és protokollok, az Azure Active Directory csatlakozás állapot ügynökök és Azure Active Directory közötti kommunikációhoz szükséges.  

Protocol (protokoll) |Portok  |Leírás
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Kimenő
Azure Service Bus|5671 (TCP/UDP)| Kimenő

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6B - végpontok Azure Active Directory csatlakozás állapotjelző Agent az (AD FS /-szinkronizálás) és Azure Active Directory
Végpontok listáját című [követelmények az Azure Active Directory csatlakozás állapotjelző ügynök](active-directory-aadconnect-health-agent-install.md#requirements).
