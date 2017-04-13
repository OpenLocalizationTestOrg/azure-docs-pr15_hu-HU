<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Szolgáltatásai |} Microsoft Azure"
    description="Az Azure Active Directory tartományi szolgáltatások funkciók"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások

## <a name="features"></a>Szolgáltatások
Azure Active Directory tartományi szolgáltatások felügyelt tartományok az alábbi funkciók érhetők el.

- **Egyszerű telepítési élmény:** Azure Active Directory tartományi szolgáltatások használatával mindössze néhány kattintással az Azure Active Directory-bérlői webhelyen engedélyezheti. A Azure AD-bérlő e cloud-bérlő vagy a helyszíni címtárában szinkronizálva, függetlenül a felügyelt tartomány kiépítése gyorsan.

- **Tartomány illesztés támogatása:** A felügyelt tartománya érhető el az Azure virtuális hálózaton tartomány illesztés számítógépek könnyen. A tartomány illesztés felület, a Windows-ügyfél és kiszolgálói operációs rendszerek works zökkenőmentes Azure Active Directory tartományi szolgáltatások által kiszolgált tartományok szemben. Automatikus tartomány csatlakozás ilyen tartományok elleni szerszámok is használhatja.

- **Egy tartomány példány per Azure Active directory:** A minden Azure AD-hez egyetlen Active Directory-tartománya hozhat létre.

- **Tartományok létrehozása az egyéni nevek:** Azure Active Directory tartományi szolgáltatások használó egyéni nevek (például contoso100.com) tartományok hozhat létre. Bármelyik igazolt vagy nem ellenőrzött tartománynevek is használhatja. Tetszés szerint is létrehozhat egy tartományt a beépített tartomány utótaggal (azaz "*. onmicrosoft.com") az Azure Active directory által kínált.

- **Integrálódik az Azure Active Directory:** Nem kell konfigurálni vagy kezelése az Azure Active Directory tartományi szolgáltatások való replikáció. Felhasználói fiókok, csoporttagság és a felhasználó hitelesítő adatait (jelszó) az Azure Active Directoryból automatikusan elérhető az Azure Active Directory tartományi szolgáltatások. Új felhasználók, csoportok és a Azure AD-bérlő vagy a helyszíni címtárában attribútumok módosítását automatikusan szinkronizálja az Azure Active Directory tartományi szolgáltatások.

- **NTLM és Kerberos-hitelesítés:** NTLM és Kerberos-hitelesítés támogatása telepítheti a Windows-hitelesítés támaszkodó alkalmazásokat.

- **a vállalati hitelesítő adatok jelszavak használata:** A felhasználóknak a Azure AD-bérlő jelszavak Azure Active Directory tartományi szolgáltatások használata. Felhasználók azok a vállalati hitelesítő adatokkal tudnak tartomány illesztés gépek, interaktív, illetve távoli asztali kapcsolaton keresztül jelentkezzen be, és szemben kezelt tartomány hitelesítést végezni.

- **LDAP kötés & LDAP olvassa el a támogatási:** LDAP kötések hitelesítést végezni az Azure Active Directory tartományi szolgáltatások által kiszolgált tartományban lévő felhasználók támaszkodó alkalmazások is használhatja. Ezenkívül használó alkalmazások LDAP olvasási műveletek lekérdezés felhasználói/számítógép attribútumait a címtárból megbízhatja Azure Active Directory tartományi szolgáltatások szemben.

- **Biztonságos LDAP (LDAPS):** Lehetősége van engedélyezni a címtárhoz access keresztül biztonságos LDAP (LDAPS). Biztonságos LDAP az access alapértelmezés szerint a virtuális hálózaton belül érhető el. Azonban tetszés szerint is engedélyezheti biztonságos LDAP hozzáférést az interneten keresztül.

- **Csoportházirend:** Egyetlen beépített csoportházirend-objektum minden a felhasználók és a számítógép-et is meg lehessen őrizni a megfelelés a tárolók biztonsági házirendek szükséges felhasználói fiókokat és a tartományhoz tartozó számítógép.

- **DNS kezelése:** A "AAD Adatközpont rendszergazdák" csoport tagjainak kezelheti DNS a felügyelt tartomány használata a DNS-felügyelet már jól ismert eszközök, például a DNS felügyeleti beépülő modult.

- **Létrehozása egyéni szervezeti egységeket:** A "AAD Adatközpont rendszergazdák" csoport tagjainak hozhat létre egyéni szervezeti a felügyelt tartományban. Ezek a felhasználók vannak teljes rendszergazdai jogosultsággal fölé egyéni szervezeti, így azok is hozzáadása és eltávolítása szolgáltatás fiókok, a számítógépek, a csoportok belül e egyéni szervezeti stb.

- **Több Azure régióban elérhető:** Lásd: a [régió szerint Azure szolgáltatások](https://azure.microsoft.com/regions/#services/) lap tudnivalók a Azure régiók, amelyben Azure Active Directory tartományi szolgáltatások érhető el.

- **Magas elérhetősége:** Azure Active Directory tartományi szolgáltatások olyan magas elérhetőségét a tartomány. Ez a funkció biztosítása magasabb üzemidő és hibákra címtárfrissítések kínál. Beépített rendszerállapot figyelése ajánlatok az automatikusan kezdeményezett remediation a hibák be új előfordulását le szeretné cserélni a sikertelen példányok és a tartomány folyamatos szolgáltatásának biztosítására frissítésjelző szerint.

- **Már jól ismert eszközök használata:** Windows Server Active Directory kezelése ismerős eszközök, például az Active Directory felügyeleti központ vagy az Active Directory PowerShell használatával felügyelheti felügyelt tartományok.
