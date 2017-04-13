<properties
    pageTitle="Azure Active Directory identitás védelem értesítések |} Microsoft Azure"
    description="Megtudhatja, hogyan értesítések támogatja-e a vizsgálat tevékenységeket."
    services="active-directory"
    keywords="Azure active directory identitás védelmét, cloud app feltárás, alkalmazásokat, biztonsági, kockázat, kockázat szint, rés, biztonsági házirendek kezelése"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory identitás védelem értesítések 


Azure Active Directory-identitás védelem kétféle hozzáadva egyszerűbben kezelheti a felhasználó kockázatok és a kockázat események automatikus értesítő e-mailek küldése:

- Felhasználói sérül figyelmeztető e-mail

- Heti kivonat e-mailben

## <a name="user-compromised-alert-email"></a>Felhasználói sérül figyelmeztető e-mail

Felhasználó biztonságos e-mail figyelmeztetést jön létre, amikor az Azure Active Directory-identitás-védelem azonosítja a fiók, mint sérül. Az e-mailt a felhasználóknak, az Identitáskezelés védelem irányítópult kockázat jelentéshez megjelölt hivatkozást tartalmaz. Javasoljuk, hogy azonnal kivizsgáljuk szóló értesítések sérül.


## <a name="weekly-digest-email"></a>Heti kivonat e-mailben

Heti kivonat az e-mailt az új kockázati esemény összefoglalását tartalmazza.<br>
Ezek a következők:

- A kockázat felhasználók
- A gyanús tevékenységek
- Észlelt biztonsági
- Az identitás-védelem kapcsolódó jelentéseket mutató hivatkozások


<br>
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

A heti kivonat e-mailt küld válthat.
<br><br>
![Felhasználói kockázatok](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**A kapcsolódó konfigurációs párbeszédpanel megnyitása**:

1. Az **Azure Active Directory-identitás védelem** a lap kattintson a **Beállítások**gombra.
<br><br>
![Felhasználói kockázati házirend](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. Az **Általános** csoportban kattintson az **értesítések**.
<br><br>
![Felhasználói kockázat házirend](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Lásd még:

- [Azure Active Directory identitás-védelem](active-directory-identityprotection.md) 

