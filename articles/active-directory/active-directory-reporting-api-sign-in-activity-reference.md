<properties
    pageTitle="Azure Active Directory bejelentkezési jelentés a tevékenységek API hivatkozás |} Microsoft Azure"
    description="Hivatkozás az Azure Active Directory bejelentkezési tevékenység jelentés API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory bejelentkezési jelentés a tevékenységek API-hivatkozás


Ez a témakör az Azure Active Directory API jelentéskészítés súgótémakörök gyűjteménye része.  
Azure Active Directory jelentéskészítés biztosít az API-val, amely lehetővé teszi a kódot vagy a kapcsolódó eszközök bejelentkezési tevékenység jelentésadatok eléréséhez.
Ez a témakör hatóköre, hogy a hivatkozás információt a **Bejelentkezés tevékenység jelentés API**biztosítson.

Lásd:

- További tájékoztatást [bejelentkezési tevékenységek](active-directory-reporting-azure-portal.md#sign-in-activities)
- [Az Azure Active Directory jelentéskészítés API – első lépések](active-directory-reporting-api-getting-started.md) a jelentéskészítési API kapcsolatban további tudnivalókat.

A kérdésekre, problémák vagy visszajelzést, kérjük, forduljon [AAD: segítséget](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Ki férhet hozzá az API-adatok?

- A biztonsági rendszergazdának vagy a biztonsági olvasó szerepkör-felhasználók

- A globális rendszergazdák

- Minden olyan alkalmazás, amely tartalmazza az API hozzáférés engedélyezése (alkalmazás engedélyezési lehet beállítása csak a globális rendszergazdai engedélye alapján)



## <a name="prerequisites"></a>Előfeltételek

Ez a jelentés az jelentéskészítési API-k eléréséhez, meg kell rendelkeznie:

- Az [Azure Active Directory prémium P1 vagy P2 edition](active-directory-editions.md)

- Az [API jelentéskészítés az Azure AD eléréséhez Előfeltételek](active-directory-reporting-api-prerequisites.md)kitölteni. 


##<a name="accessing-the-api"></a>Az API elérése

Ez az API programozás útján vagy a [Diagram Explorer](https://graphexplorer2.cloudapp.net) keresztül vagy elérheti, például a PowerShell használatával. Ahhoz, hogy a helyes értelmezése OData szűrő szintaxisának AAD Graph többi hívások PowerShell, kell használnia a backtick (más néven: fordított ékezet) a $ karakter "escape" karakter. A backtick karakter szolgál [PowerShell-féle escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé teszi a $ karakter egy konstans értelmezését, és elkerülése érdekében a zavaró azt PowerShell változó neveként PowerShell (ie: $filter).

Ez a témakör elsősorban a Graph Explorer. Egy PowerShell példa című témakörben a [PowerShell-parancsprogramot](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>API-végpontok

Ez az API alap URI-nak érheti el:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Az adatok mennyisége miatt ez az API-os érvényes egymillió rekordokat adja vissza. 

A hívás kötegekben adja vissza az adatokat. Minden egyes köteg legfeljebb 1000 rekordokat tartalmaz.  
A következő köteg a rekordok, használja a következő hivatkozásra. A visszaadott rekordok első halmazára nyerheti ki az [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) információkat. A Kihagyás jogkivonat végén található az eredményt állítja be.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Támogatott szűrők

Akkor is szűkítéséhez a rekordok, amelyek egy API által visszaadott számát hívja fel a szűrés formában.  
Bejelentkezési API-val kapcsolatos adatok, a következő szűrők használhatók:

- **$top =\<vissza kell rekordok száma\> ** - visszaadott rekordok számának korlátozása. Ez a művelet drága. Ha az objektumok ezer vissza szeretné a szűrő nem kell használni.  
- **$filter =\<a szűrő utasítás\> ** - adja meg, alapján támogatott szűrőmezők, az Önt érdeklő rekordok



## <a name="supported-filter-fields-and-operators"></a>Támogatott szűrőmezők és műveleti jelek

Adja meg az Önt érdeklő rekordokat, hogy készíthet, amely tartalmazhat egyik vagy a következő szűrő mezők kombinációja szűrő kimutatást:

- határozza meg [signinDateTime](#signindatetime) -, dátum vagy dátumtartomány

- [felhasználóazonosító](#userid) - definiálja egy adott felhasználói alapú a felhasználói azonosítójával.

- a [userPrincipalName](#userprincipalname) - definiálja egy adott felhasználói alapján a felhasználó egyszerű felhasználónév (UDN)

- [appId](#appid) - definiálja egy adott alkalmazás alapú az alkalmazás azonosítója

- [appDisplayName](#appdisplayname) - definiálja egy adott alkalmazás az alkalmazás megjelenítendő név alapján

- [loginStatus](#loginStatus) – meghatározza a bejelentkezések állapotát (sikeres és sikertelen)


> [AZURE.NOTE] Graph Intézővel esetén a kis-és nagybetűk használata a minden levél, a Szűrő mezőket.


A visszaadott adatok hatókörének szűkítésére, készíthet a támogatott szűrők és szűrőmezők kombinációi. Az alábbi kimutatás például július 1 2016 és 6-a között 2016 július között felső 10 rekordot adja eredményül:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Támogatott operátorok**: eq, ge, le, gt, lt

**Példa**:

Egy adott napon használatával

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Dátumtartomány használata    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Megjegyzések**:

A dátum és idő paraméter UTC formátumban kell lennie. 


----------

### <a name="userid"></a>Felhasználóazonosító

**Támogatott operátorok**: eq

**Példa**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Megjegyzések**:

Felhasználóazonosító értéke szöveges értékként



----------

### <a name="userprincipalname"></a>userPrincipalName

**Támogatott operátorok**: eq

**Példa**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Megjegyzések**:

A userPrincipalName szöveges értékként értéke

----------

### <a name="appid"></a>appId

**Támogatott operátorok**: eq

**Példa**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Megjegyzések**:

AppId értéke szöveges értékként

----------


### <a name="appdisplayname"></a>appDisplayName

**Támogatott operátorok**: eq

**Példa**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Megjegyzések**:

AppDisplayName értéke szöveges értékként

----------

### <a name="loginstatus"></a>loginStatus

**Támogatott operátorok**: eq

**Példa**:

    $filter=loginStatus+eq+'1'  


**Megjegyzések**:

Két lehetőség a loginStatus az: 0 - sikeres, 1 - hiba

----------



## <a name="next-steps"></a>Következő lépések

- Példák a szűrt bejelentkezési tevékenységekhez szeretné? Nézze meg az [Azure Active Directory bejelentkezési tevékenység jelentés API minták](active-directory-reporting-api-sign-in-activity-samples.md).

- Szeretne többet megtudni az Azure AD-API jelentéskészítés? [Első lépések az Azure Active Directory jelentési API](active-directory-reporting-api-getting-started.md)című témakör tartalmaz.