
<properties
    pageTitle="Speciális szabályok létrehozása a attribútumok segítségével |} Microsoft Azure"
    description="Hogyan – a hangpostáján kifejezés szabály operátorok és a Paraméterek csoport, többek között támogatott speciális szabályok létrehozásához."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>Speciális szabályok létrehozása a attribútumok segítségével

Az Azure klasszikus portál lehetőséget nyújt a ahhoz, hogy összetettebb attribútum alapuló dinamikus tagságot az Azure Active Directory (Azure Active Directory) csoport speciális szabályok létrehozásához.  

Bármely felhasználó módosítás attribútumok, a rendszer átmenetileg a megjelenítéséhez, ha az attribútum módosítása annak a felhasználónak szeretné elindítani bármelyik csoportot könyvtárában található összes dinamikus csoport szabály hozzáadása vagy eltávolítása. Ha egy felhasználó megfelel egy szabály csoport, hozzáadásuk tagként a csoporthoz. Ha már nem megfelelnek a szabály csoport tagjának, hogy törlődjenek egy tagként csoport.

## <a name="to-create-the-advanced-rule"></a>A speciális szabály létrehozása

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és nyissa meg a szervezet címtárára.

2. Kattintson a **csoportok** fülre, és nyissa meg a szerkeszteni kívánt csoportot.

3. A **beállítás** lapon jelölje ki, válassza a **Speciális szabály** lehetőséget, és írja be a szövegmezőbe a speciális szabályt.

## <a name="constructing-the-body-of-an-advanced-rule"></a>A szervezet egy speciális szabály megépítése.

A speciális szabályt, amely a csoportok dinamikus tagságot készíthet értéke lényegében bináris kifejezés, amely három részből áll, és IGAZ vagy hamis eredmény egy eredményez. A három részből állnak:

- Bal oldali paraméter
- Bináris operátor
- Jobb oldali állandó

Egy teljes speciális szabály néz ki: (leftParameter binaryOperator "RightConstant"), ahol a nyitó és záró zárójelet szükség a teljes bináris kifejezés, dupla idézőjelek szükségesek jobb állandó és szintaxisa a következő a bal oldali paraméter user.property. Speciális szabály is tartalmazhatnak egynél több bináris kifejezések pontosvesszővel a - és, -, illetve és – nem logikai operátorokat.
A következő példák megfelelően kialakított speciális szabály:

- (user.department - eq "Értékesítés") – vagy (user.department - eq "Marketing")
- (user.department - eq "Értékesítés") – és – nem (user.jobTitle-tartalmazza az "SDE")

Kifejezés szabály operátorok és használható paraméterek teljes listáját olvassa el az alábbi szakaszokban című témakört.

A speciális szabály törzsében teljes hossza nem haladhatja meg a 2048 karaktert.

> [AZURE.NOTE]
>Karakterlánc és regex műveletek kis-és nagybetűk. Null ellenőrzések $null használatával állandó, mint például, user.department - eq $null is elvégezheti.
Árajánlatok tartalmazó karakterláncok "segítségével kell escape" karakter például user.department - eq \`"Értékesítés".

## <a name="supported-expression-rule-operators"></a>Támogatott kifejezés szabály operátorok
Az alábbi táblázat felsorolja a támogatott kifejezés szabály operátorok és a speciális szabály törzsében használandó szintaxisáról:

| Műveleti jel        | Szintaxis         |
|-----------------|----------------|
| Nem egyenlő      | -ne            |
| Egyenlő          | -eq            |
| Nem kezdődik | -notStartsWith |
| Karakterekkel kezdődik.     | -startsWith    |
| Nem tartalmazza.    | -notContains   |
| Tartalmaz        | -tartalmaz      |
| Nem egyezik       | -notMatch      |
| Hol.van függvénnyel           | -megfelelően         |


## <a name="query-error-remediation"></a>Lekérdezés hiba remediation
Az alábbi táblázat a lehetséges hibák és javítása őket, ha következnek be

| Lekérdezés elemzési hiba     | Hiba használatát       | Javított használati             |
|-----------------------|-------------------|-----------------------------|
| Hiba: Attribútum nem támogatott.                                      | (user.invalidProperty - eq "Érték")       | (user.department - eq "érték")<br/>Tulajdonság meg kell egyeznie a [Tulajdonságok listában támogatott](#supported-properties)egyet.                          |
| Hiba: Operátor nem támogatott attribútum.                       | (user.accountEnabled-IGAZ logikai értéket tartalmazza)                                                                               | (user.accountEnabled - eq igaz)<br/>Tulajdonság írja be a logikai érték. A támogatott operátorok (-eq vagy - ne) logikai típusú, a fenti listában.                                                                                                                                   |
| Hiba: A lekérdezés összeállítása hiba.                                      | (user.department - eq "Értékesítés")- és (user.department - eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Értékesítés")- és (user.department - eq "Marketing")<br/>Logikai operátorok meg kell egyeznie a fenti támogatott tulajdonság legördülő menüből. (user.userPrincipalName-megfelelően ".*@domain.ext")or(user.userPrincipalName -megfelelően "@domain.ext$")Error a reguláris kifejezésekkel. |
| Hiba: Bináris kifejezést nem a megfelelő formátumban van.                     | (user.department – eq "Értékesítés") (user.department - eq "Értékesítés") (user.department-eq "Értékesítés")                             | (user.accountEnabled - eq igaz)- és (user.userPrincipalName-tartalmaz"alias@domain")<br/>Lekérdezés több hibát tartalmaz. Zárójel nem a megfelelő helyre.                                                                                                                            |
| Hiba: Ismeretlen hiba történt dinamikus tagságot beállítását. | (user.accountEnabled - eq "igaz" AND user.userPrincipalName-tartalmaz"alias@domain")                               | (user.accountEnabled - eq igaz)- és (user.userPrincipalName-tartalmaz"alias@domain")<br/>Lekérdezés több hibát tartalmaz. Zárójel nem a megfelelő helyre.                                                                                                                            |

## <a name="supported-properties"></a>Támogatott tulajdonságai
A speciális szabály is használhatja az összes felhasználó tulajdonságainak a következők:

### <a name="properties-of-type-boolean"></a>A Tulajdonságok parancsot írja be a logikai érték

Engedélyezett operátorok

* -eq


* -ne


| Tulajdonságok     | Megengedett érték  | Használat                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | IGAZ, hamis      | user.accountEnabled - eq igaz)  |
| dirSyncEnabled | IGAZ, hamis null | (user.dirSyncEnabled - eq igaz) |

### <a name="properties-of-type-string"></a>Karakterlánc típusú tulajdonságai

Engedélyezett operátorok

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -tartalmaz


* -notContains


* -megfelelően


* -notMatch

| Tulajdonságok                 | Megengedett érték                                                                                        | Használat                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| a város                       | Karakterlánc érték vagy $null                                                                           | (user.city - eq "érték")                                   |
| ország                    | Karakterlánc érték vagy $null                                                                            | (user.country - eq "érték")                                |
| szervezeti egység                 | Karakterlánc érték vagy $null                                                                          | (user.department - eq "érték")                             |
| displayName                | Minden olyan karakterlánc                                                                                 | (user.displayName - eq "érték")                            |
| facsimileTelephoneNumber   | Karakterlánc érték vagy $null                                                                           | (user.facsimileTelephoneNumber - eq "érték")               |
| givenName                  | Karakterlánc érték vagy $null                                                                           | (user.givenName - eq "érték")                              |
| Munkakör                   | Karakterlánc érték vagy $null                                                                           | (user.jobTitle - eq "érték")                               |
| levelek                       | Karakterlánc érték vagy $null (a felhasználó SMTP-címe)                                                  | (user.mail - eq "érték")                                   |
| mailnickname-beli               | Minden olyan karakterlánc (a felhasználó e-mail alias)                                                            | (user.mailNickName - eq "érték")                           |
| mobil                     | Karakterlánc érték vagy $null                                                                           | (user.mobile - eq "érték")                                 |
| Objektumazonosító                   | A user objektumban globálisan egyedi azonosítója                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Nincs DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Karakterlánc érték vagy $null                                                                            | (user.physicalDeliveryOfficeName - eq "érték")             |
| Irányítószám                 | Karakterlánc érték vagy $null                                                                            | (user.postalCode - eq "érték")                             |
| preferredLanguage          | ISO 639-1-kód                                                                                        | (user.preferredLanguage - eq "hu-hu")                      |
| sipProxyAddress            | Karakterlánc érték vagy $null                                                                            | (user.sipProxyAddress - eq "érték")                        |
| állam                      | Karakterlánc érték vagy $null                                                                            | (user.state - eq "érték")                                  |
| streetAddress              | Karakterlánc érték vagy $null                                                                            | (user.streetAddress - eq "érték")                          |
| Vezetéknév                    | Karakterlánc érték vagy $null                                                                            | (user.surname - eq "érték")                                |
| telephoneNumber            | Karakterlánc érték vagy $null                                                                            | (user.telephoneNumber - eq "érték")                        |
| usageLocation              | Két betűkkel országhívószám                                                                           | (user.usageLocation - eq "USA")                             |
| userPrincipalName          | Minden olyan karakterlánc                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | tag Vendég $null                                                                                    | (user.userType - eq "Tag")                              |

### <a name="properties-of-type-string-collection"></a>Írja be a karakterlánc webhelycsoport tulajdonságai

Engedélyezett operátorok

* -tartalmaz


* -notContains

| Poperties      | Megengedett érték                        | Használat                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Minden olyan karakterlánc                      | (user.otherMails-tartalmaz"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-tartalmaz "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Bővítmény attribútumok és egyéni attribútumok
Dinamikus tagsági szabály kiterjesztés attribútumok és egyéni attribútumok használhatók.

Bővítmény attribútumok a rendszer szinkronizálja a helyszíni környezetbe ablak kiszolgáló AD, és kövesse a "ExtensionAttributeX", ahol az X értéke 1-15 formátumát.
Példa egy szabályt, amely kiterjesztés tulajdonság használja a következő lesz

(user.extensionAttribute15 - eq "értékesítés")

Egyéni attribútumok a rendszer szinkronizálja a helyszíni környezetbe a Windows Server Active Directory vagy egy csatlakoztatott szoftver alkalmazásból, és a formátumának "user.extension_[GUID]\__ [attribútum]", hol [globálisan egyedi azonosítója] az alkalmazáshoz létrehozott az attribútum AAD AAD az egyedi azonosító és [attribútum] megegyezik az attribútum neve jött létre.
Példa egy szabályt, amely használja egy egyéni attribútum

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Az egyéni attribútumnév lekérdezésével egy felhasználó a címtárban található attribútum Graph Intézővel és keresése az attribútum nevét.

## <a name="direct-reports-rule"></a>A felhasználó közvetlen beosztottai szabály
Egy felhasználó a kezelő attribútum alapján csoportnak a tagjai most kitöltheti.

**"Manager" csoportként csoport konfigurálása**

1. Az Azure klasszikus portálon kattintson **Az Active Directory**, és kattintson a szervezet neve.

2. Kattintson a **csoportok** fülre, és nyissa meg a szerkeszteni kívánt csoportot.

3. Kattintson a **beállítás** fülre, és válassza a **Speciális SZABÁLYT**.

4. Írja be a szabály a következő szintaxissal:

    Közvetlen jelentések esetében *a felhasználó közvetlen beosztottai {obectID_of_manager}*. Példa egy érvényes szabályt közvetlen beosztottai

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    "62e19b97-8b3d-4d4a-a106-4ce66896a863" esetén a vezető objektumazonosító. Az objektum azonosító az Azure Active Directory felhasználó lapon a felhasználó, aki a **profil lapon** találhatók.

3. Ez a szabály mentésekor a tartományhoz a csoport tagjai, amelyek megfelelnek a szabály minden felhasználónak kell. Eltarthat néhány percig, amíg a csoport kezdetben kitöltéséhez.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Szabályok létrehozása a eszköz objektumok attribútumainak használatával

Is létrehozhat egy szabályt, amely a csoport tagsági eszköz objektumok kijelölése. A következő attribútumok eszköz használható:

| Tulajdonságok              | Megengedett érték                  | Használat                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| displayName             | minden olyan karakterlánc                | (device.displayName - eq "Papp Iphone")                       |
| deviceOSType            | minden olyan karakterlánc                | (device.deviceOSType - eq "IOS")                             |
| deviceOSVersion         | minden olyan karakterlánc                | (eszközt. OSVersion - eq "9.1")                                |
| isDirSynced             | IGAZ, hamis null                 | (device.isDirSynced - eq "igaz")                             |
| isManaged               | IGAZ, hamis null                 | (device.isManaged - eq "hamis")                              |
| isCompliant             | IGAZ, hamis null                 | (device.isCompliant - eq "igaz")                             |
| deviceCategory          | minden olyan karakterlánc                | (device.deviceCategory - eq "")                              |
| deviceManufacturer      | minden olyan karakterlánc                | (device.deviceManufacturer - eq "Microsoft")                 |
| deviceModel             | minden olyan karakterlánc                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | minden olyan karakterlánc                | (device.deviceOwnership - eq "")                             |
| Tartománynév              | minden olyan karakterlánc                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | minden olyan karakterlánc                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | IGAZ, hamis null                 | (device.deviceOSType - eq "igaz")                            |
| managementType          | minden olyan karakterlánc                | (device.managementType - eq "")                              |
| szervezeti_egység      | minden olyan karakterlánc                | (device.organizationalUnit - eq "")                          |
| deviceId                | egy érvényes deviceId                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> Eszköz szabályok használatával a "egyszerű szabály" tartalmazó legördülő listára az Azure klasszikus portálon nem hozható létre.


## <a name="additional-information"></a>További információk
Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [Hibaelhárítási dinamikus tagságot-csoportok](active-directory-accessmanagement-troubleshooting.md)

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)

* [Azure Active Directory-parancsmagjai csoport beállításainak konfigurálása](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
