<properties
    pageTitle="Azure AD Connect szinkronizálása: ismertetése deklaráció kiépítési kifejezések |} Microsoft Azure"
    description="A deklaráció kiépítési kifejezések ismerteti."
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
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect szinkronizálása: deklaráció kiépítési kifejezések ismertetése
Azure AD Connect szinkronizálási épül deklaráció kiépítési először a Forefront identitáskezelő 2010 alkalmazásban. Lehetővé teszi a teljes identitás integrációs nincs szükség a lefordított kódírás logikájának megvalósítása.

Az alapvető deklaráció kiépítési része flow attribútumos kifejezés nyelve. A használt nyelvet része a Microsoft®®-Visual Basic for Applications (VBA). Nyelvét használják a Microsoft Office és a felhasználók VBScript élményt fogja is ismerni. A deklaráció kiépítési nyelvén csak függvényeivel, és nem strukturált nyelven. Nincsenek módszerek vagy kimutatások. Express programból folyamat való inkább beágyazott függvények.

További részletekért olvassa el [a Visual Basic for alkalmazások nyelvi hivatkozás az Office 2013 Üdvözöljük](https://msdn.microsoft.com/library/gg264383.aspx).

Az attribútumok erősen írta-e be. Függvény csak a megfelelő típusú attribútumok fogad el. Érdemes emellett kis-és nagybetűk. Függvény neve és a attribútum neveket is kell rendelkeznie a megfelelő géphez, vagy hiba történt.

## <a name="language-definitions-and-identifiers"></a>Nyelvi definíciók és azonosítók

- Függvények rendelkező lapjának a neve követnie argumentumai szögletes zárójelek között: függvénynevet (argumentumot 1, N argumentum).
- Attribútumok azonosítjuk szögletes zárójelek között: [attribútumnév]
- Paraméterek azonosítják százalékjel: ParameterName %
- A karakterláncot tartalmazó állandók idézőjelek közé körül: például "Contoso" (Megjegyzés: kell használnia az írógép-idézőjelet "", és nem nyomdaira "")
- Numerikus értékek idézőjeleket kifejezett és várhatóan decimális. A hexadecimális elé & h Ha például a 98052 & HFF
- A logikai értékeket tartalmazó állandók kell megadni: igaz, hamis.
- Beépített állandók és literálok kell megadni, csak az névvel: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Függvények
Számos függvény deklaráció kiépítési használja ahhoz, hogy attribútum értékeket átalakító lehetőséget. Ezek a függvények egymásba, egy függvény az eredmény a másik függvény átadott.

`Function1(Function2(Function3()))`

A függvények teljes listáját a [függvényeinek részletes ismertetése](active-directory-aadconnectsync-functions-reference.md)találhatók.

### <a name="parameters"></a>Paraméterek
Paraméter van megadva, összekötő vagy a rendszergazda PowerShell használatával. Paraméterek általában érték, amely operációs rendszerek különbözőek, például a tartománynevet, a felhasználó található. Ezek a paraméterek attribútum flow használható.

Az Active Directory-összekötő a következő paraméterek előírt szinkronizálás a bejövő szabályok:

| Paraméterek neve | Megjegyzés |
| --- | --- |
| Domain.Netbios | A tartomány jelenleg importált, például FABRIKAMSALES NetBIOS formázása |
| Domain.FQDN | A tartomány jelenleg importált, például sales.fabrikam.com FQDN formátumban |
| Domain.LDAP | A tartomány jelenleg importálnak, például az Adatközpont LDAP-formátum = értékesítés, Adatközpont fabrikam Adatközpont = = hu |
| Forest.Netbios | A erdő neve jelenleg importált, például FABRIKAMCORP NetBIOS formátuma |
| Forest.FQDN | Teljesen minősített tartománynév formátumban kell a erdő jelenleg importált, például a fabrikam.com |
| Forest.LDAP | LDAP formátumban kell a erdő jelenleg importálnak, például az Adatközpont fabrikam Adatközpont = = hu |

A rendszer a következő paraméter, amellyel a az összekötő futó az azonosító beszerzése biztosít:  
`Connector.ID`

Íme egy példa a tartomány, ahol a felhasználó található netbios nevű metaverse attribútum tartomány feltöltő:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operátorok
Az alábbi operátorok használhatók:

- **Összehasonlító**: <, < =, <>, >, > =
- **Matematikai**: +, -, \*, -
- **Karakterlánc**: és (ÖSSZEFŰZ)
- **Logikai**: & & (és). (vagy)
- **Kiértékelési sorrend**:)

Operátorok kiértékelése balról jobbra és értékelési prioritású. Ez azt jelenti, hogy a \* (mezősokszorozó) nem kiértékelt előtt - (kivonás). 2\*(5 + 3) ugyanaz, mint 2 nem\*5 + 3. A zárójelek () kiértékelési sorrendjének módosítása, amikor a balról jobbra kiértékelési sorrend nem megfelelő használják.

## <a name="multi-valued-attributes"></a>Többértékű attribútum
A függvények is egyetlen értékű és többértékű attribútum is működnek. Többértékű attribútum a függvény felett a minden érték működik, és a minden érték a függvényben vonatkozik.

Példa:  
`Trim([proxyAddresses])`Végezze el a proxyAddress attribútumot minden érték Trim.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`A minden érték egy @-sign, cserélje le a tartományt az @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Keresse meg a SIP-cím, és távolítsa el azt az értékeket.

## <a name="next-steps"></a>Következő lépések

- További információ a konfigurációs modellt [Deklaráció kiépítési ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Lásd: hogyan deklaráció használt kimenő kész [az alapértelmezett beállítások ismertetése](active-directory-aadconnectsync-understanding-default-configuration.md)a kiépítési.
- Megtudhatja, hogy miként deklaráció kiépítési [hogyan lehet módosítani szeretné az alapértelmezett beállítás](active-directory-aadconnectsync-change-the-configuration.md)használatával gyakorlati módosítása.

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

**Hivatkozás témakörök**

- [Azure AD Connect szinkronizálása: függvények hivatkozás](active-directory-aadconnectsync-functions-reference.md)
