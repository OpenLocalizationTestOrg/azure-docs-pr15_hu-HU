<properties
    pageTitle="Azure Active Directory naplózási API hivatkozás |} Microsoft Azure"
    description="Ismerkedés az Azure Active Directory-ellenőrzési API-val"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory naplózási API-hivatkozás

Ez a témakör az Azure Active Directory API jelentéskészítés súgótémakörök gyűjteménye része.  
Azure Active Directory jelentéskészítés biztosít az API-val, amely lehetővé teszi a kódot vagy a kapcsolódó eszközök naplózási adatok eléréséhez.
Ez a témakör hatóköre a **naplózási API-val**kapcsolatos hivatkozás információkkal.

Lásd:

- További tájékoztatást a [naplókat](active-directory-reporting-azure-portal.md#audit-logs)
- [Az Azure Active Directory jelentéskészítés API – első lépések](active-directory-reporting-api-getting-started.md) a jelentéskészítési API kapcsolatban további tudnivalókat.

A kérdésekre, problémák vagy visszajelzést, kérjük, forduljon [AAD: segítséget](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Ki férhet hozzá az adatokat?

- A biztonsági rendszergazdának vagy a biztonsági olvasó szerepkör-felhasználók

- A globális rendszergazdák

- Minden olyan alkalmazás, amely tartalmazza az API hozzáférés engedélyezése (alkalmazás engedélyezési lehet beállítása csak a globális rendszergazdai engedélye alapján)



## <a name="prerequisites"></a>Előfeltételek

Ez a jelentés az jelentéskészítés API-k eléréséhez kell rendelkeznie:

- Az [Azure Active Directory ingyenes vagy jobb edition](active-directory-editions.md)

- Kész a [Előfeltételek API jelentéskészítés az Azure AD eléréséhez](active-directory-reporting-api-prerequisites.md). 
 

##<a name="accessing-the-api"></a>Az API elérése

Ez az API programozás útján vagy a [Diagram Explorer](https://graphexplorer2.cloudapp.net) keresztül vagy elérheti, például a PowerShell használatával. Ahhoz, hogy helyesen adatainak értelmezéséhez OData szűrése szintaxisának AAD Graph többi hívások PowerShell, kell használnia a backtick (más néven: fordított ékezet) a $ karakter "escape" karakter. A backtick karakter szolgál [PowerShell-féle escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé teszi a $ karakter egy konstans értelmezési, és elkerülése érdekében a zavaró azt PowerShell változó neveként PowerShell (ie: $filter).

Ez a témakör elsősorban a Graph Explorer. Egy PowerShell példa című témakörben a [PowerShell-parancsprogramot](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>API-végpontok


Ez az API a következő URI érheti el:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Nincs korlát az Azure Active Directory naplózási API-t (használatával OData sortávolságot) által visszaadott rekordok száma nem.
Jelentésadatok adatmegőrzési vonatkozó korlátok tanulmányozza [Jelentéskészítés az adatmegőrzési házirendek](active-directory-reporting-retention.md).

A hívás kötegekben adja vissza az adatokat. Minden köteg legfeljebb 1000 rekordokat tartalmaz.  
A következő köteg a rekordok, használja a következő hivatkozásra. A visszaadott rekordok első halmazára nyerheti ki az skiptoken információkat. A Kihagyás jogkivonat végén található az eredményt állítja be.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Támogatott szűrők

A rekordok, amelyek egy API által visszaadott számát lefelé szűkítheti hívja fel a szűrés formában.  
Bejelentkezési API-val kapcsolatos adatok, a következő szűrők használhatók:

- **$top =\<vissza kell rekordok száma\> ** - visszaadott rekordok számának korlátozása. Ez a művelet drága. Ne használja a szűrő, ha objektumok ezer vissza szeretné.     
- **$filter =\<a szűrő utasítás\> ** - az Önt érdeklő rekordok típusának megadásához támogatott szűrése mezők alapján



## <a name="supported-filter-fields-and-operators"></a>Támogatott szűrőmezők és műveleti jelek

Adja meg az Önt érdeklő rekordokat, hogy készíthet, amely tartalmazhat egyik vagy a következő szűrő mezők kombinációja szűrő kimutatást:

- határozza meg [activityDate](#activitydate) -, dátum vagy dátumtartomány
- [activityType](#activitytype) - egy tevékenység típusa határozza meg.
- [tevékenység](#activity) - karakterláncként határozza meg a tevékenység  
- [szereplő/neve](#actorname) - határozza meg, hogy a szereplő a szereplő név űrlap
- [szereplő/objektumazonosító](#actorobjectid) – határozza meg, hogy a szereplő a szereplő azonosító formájában   
- [szereplő/upn](#actorupn) - határozza meg, hogy a szereplő űrlap a szereplő elve felhasználónév (UDN) 
- [cél/neve](#targetname) - határozza meg, hogy a cél a szereplő név űrlap
- [cél/objektumazonosító](#targetobjectid) – határozza meg, hogy a célhely űrlap a célalkalmazás azonosítója  
- [cél/upn](#targetupn) - határozza meg, hogy a szereplő űrlap a szereplő elve felhasználónév (UDN)   




----------

### <a name="activitydate"></a>activityDate

**Támogatott operátorokat**: eq, ge, le, gt, lt

**Példa**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Megjegyzések**:

dátum és idő UTC formátumban kell lennie.

----------

### <a name="activitytype"></a>activityType

**Támogatott operátorokat**: eq

**Példa**:

    $filter=activityType eq 'User'  

**Megjegyzések**:

kis-és nagybetűk

----------

### <a name="activity"></a>tevékenység

**Támogatott operátorokat**: eq, tartalmaz, startsWith

**Példa**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Megjegyzések**:

kis-és nagybetűk

----------

### <a name="actorname"></a>szereplő/neve

**Támogatott operátorok**: eq, tartalmaz, startsWith

**Példa**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Megjegyzések**:

nagybetűk

    

----------
### <a name="actorobjectid"></a>szereplő/objektumazonosító

**Támogatott operátorokat**: eq

**Példa**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>cél/neve

**Támogatott operátorokat**: eq, tartalmaz, startsWith

**Példa**:

    $filter=targets/any(t: t/name eq 'some name')   

**Megjegyzések**:

Nagybetűk

----------

### <a name="targetupn"></a>cél / (UPN)

**Támogatott operátorok**: eq, startsWith

**Példa**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Megjegyzések**:

- Nagybetűk
- Be kell állítania a teljes névtér, amikor Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity lekérdezése

----------

### <a name="targetobjectid"></a>cél/objektumazonosító

**Támogatott operátorok**: eq

**Példa**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>szereplő / (UPN)

**Támogatott operátorok**: eq, startsWith

**Példa**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Megjegyzések**:

- Nagybetűk 
- Be kell állítania a teljes névtér, amikor Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity lekérdezése

----------




## <a name="next-steps"></a>Következő lépések

- Példák a szűrt rendszer tevékenységekhez szeretné? Nézze meg az [Azure Active Directory naplózási API minták](active-directory-reporting-api-audit-samples.md).

- Szeretne többet megtudni az Azure AD-API jelentéskészítés? [Első lépések az Azure Active Directory jelentéskészítés API](active-directory-reporting-api-getting-started.md)című témakör tartalmaz.