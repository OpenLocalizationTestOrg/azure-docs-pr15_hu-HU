<properties
    pageTitle="Azure Active Directory identitás védelem észlelni biztonsági |} Microsoft Azure"
    description="Azure Active Directory identitás védelem észlelni biztonsági áttekintése."
    services="active-directory"
    keywords="Azure active directory identitás védelmét, cloud app feltárás, alkalmazásokat, biztonsági, kockázat, kockázat szint, rés, biztonsági házirendek kezelése"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory identitás védelem észlelni biztonsági 

Biztonsági gyengeségek a környezetben, amely a támadók is ki. Azt javasoljuk, ezek a biztonsági testtartását a szervezet hogyan biztonsági cím, és megakadályozhatja, hogy a támadók kihasználása őket.
<br><br>
![biztonsági](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

Az alábbi szakaszok identitás védelem által jelzett biztonsági áttekintést nyújtanak.

## <a name="multi-factor-authentication-registration-not-configured"></a>Többtényezős hitelesítés regisztrációs nincs konfigurálva 

Rés segítségével szabályozhatja a szervezet az Azure többtényezős hitelesítés telepítését. 

Azure többtényezős hitelesítés a felhasználói hitelesítés biztonsági második réteg biztosít. Ez segít védelme hozzáférés az adatokhoz, és az alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben. Egy egyszerű ellenőrzési beállítások cellatartomány útján erős hitelesítéshez kézbesíti – telefonhívás, szöveges üzenet vagy mobilalkalmazásban értesítési vagy ellenőrzési kódot és 3rd fél elfogadható tokenek.

Azt javasoljuk, hogy a felhasználó bejelentkezési bővítmények Azure többtényezős hitelesítés szükséges. Többtényezős hitelesítés játszik szerepet feltételes hozzáférés kockázata-alapú házirendek identitás védelem keresztül érhető el.

További részletekért lásd: [Mi az Azure többtényezős hitelesítést?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Felügyelt felhő alkalmazások

A szervezet nem felügyelt felhő alkalmazások azonosítása rés segítségével.
 
Modern nagyvállalatoknak szolgáltatás részét képező, az informatikai részlegeknek mérete gyakran tudomása összes a felhőben használó alkalmazások a szervezet felhasználói munkájuk elvégzéséhez. Nagyon egyszerűen tekintheti meg, miért rendszergazdák vállalati adatok, lehetséges, az adatok szivárgás és más biztonsági kockázatot ezzel az illetéktelen hozzáférést kétségei vannak tenné. 

Azt javasoljuk, hogy a szervezet üzembe Cloud App felderítése nem felügyelt felhő alkalmazások felderítésére, és ezeket az alkalmazásokat Azure Active Directory kezelése.

További információra kíváncsi olvassa el a [Cloud App feltárás nem felügyelt cloud-alkalmazások megkeresése](active-directory-cloudappdiscovery-whatis.md)című témakört.



##<a name="security-alerts-from-privileged-identity-management"></a>Biztonsági figyelmeztetések a Yammerhez Identitáskezelés

Rés segít Fedezze fel, és a szervezet védett identitások kapcsolatos riasztások megoldása.  

Ahhoz, hogy a felhasználók a Yammerhez műveletek elvégzését, szervezetek kell hozzáférést felhasználók ideiglenes vagy állandó Yammerhez Azure Active Directory, a Azure vagy az Office 365 erőforrások, vagy más szoftver alkalmazások. Minden egyes jogosultsággal rendelkező felhasználóknak növeli a szervezet homonimaszerű webcímmel felületének. Rés segít a szükségtelen Yammerhez hozzáféréssel rendelkező felhasználók azonosításához és csökkentése vagy a kockázatot jelentenek kiküszöbölése érdekében megfelelő lépéseket. 

Azt javasoljuk, hogy szervezete használja az Identitáskezelés Azure Active Directory jogosultsággal rendelkező kezeléséhez, a vezérlőt, és a képernyő kiemelt jogosultságokkal rendelkezik, identitások és erőforrások az Azure Active Directory elérését, valamint más online a Microsoft-szolgáltatásokkal, például az Office 365- vagy Microsoft Intune.

További részletekért olvassa el [Az Identitáskezelés Azure Active Directory Yammerhez](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Lásd még:

 - [Azure Active Directory identitás-védelem](active-directory-identityprotection.md)
