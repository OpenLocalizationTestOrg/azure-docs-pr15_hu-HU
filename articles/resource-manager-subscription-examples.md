<properties
   pageTitle="Esetek és példát tartalmaz az előfizetés irányítási |} Microsoft Azure"
   description="Gyakori alkalmazási területek az Azure előfizetés irányítási megvalósításáról példákat."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Példák a Azure vállalati scaffold végrehajtása

Ez a témakör hogyan egy vállalati segítségével miként állíthat a javaslatok az [Azure vállalati scaffold](resource-manager-subscription-governance.md)példákat. Gyakorlati tanácsok a gyakori alkalmazási területek bemutatják egy kontraktor nevű kitalált vállalat használja. 

## <a name="background"></a>Háttér

A Contoso egy világszerte biztosító ellátási lánc megoldások mindent csomagolt modellhez "Szoftver szolgáltatásként" modellből vevők helyszíni rendszerbe.  Végig a földgömb értékes fejlesztésének központok indiai, az Amerikai Egyesült Államokban és Kanadában a szoftver azok fejlesztése. 

A vállalat külső részét meg van osztva több független részlegek olyan jelentősen üzleti kezelheti a termékeket. Minden egyes részleg saját fejlesztők, termékmenedzserek és építészek tartalmaz. 

A vállalati technológia szolgáltatások végző részleg központi informatikai lehetőséget nyújt, és több adatközpontokban, amelyeknél az részlegek állomás kérelmeiket kezeli. A adatközpontokban kezelése, valamint a Trendérték szervezet tartalmaz, illetve központi együttműködési (például a levelezést és a webhelyek) és a telefonos/hálózati szolgáltatások kezelése. Azok is kisebb részlegek, akik nem rendelkeznek a működési oktatói ügyfélkapcsolati munkaterhelésekből kezelése. 

Ez a témakör a következő personas használatosak:

- Dave a Trendérték Azure-rendszergazda.
- Anna Contoso igazgató a fejlesztés a ellátási lánc részleg. 

A Contoso kell egy a vállalati verzió és a felhasználói elérésű alkalmazást összeállítása. Az alkalmazások futtatásához a Azure határozott. Dave beolvassa a [elfogadott előfizetés irányítási](resource-manager-subscription-governance.md) témakört, és ettől kezdve készen áll a javaslatok végrehajtásához. 

## <a name="scenario-1-line-of-business-application"></a>Alkalmazási helyzetek 1: a vállalati verzió alkalmazás

A Contoso állítja össze a forrás kód rendszer (BitBucket) világszerte fejlesztők való használatra.  Az alkalmazás infrastruktúra szolgáltatásként (IaaS) tárolására, és így webkiszolgálón és az adatbázis-kiszolgálóval áll. A fejlesztők elérhető fejlesztői környezetükben kiszolgálók, de nincs szükség, azokat a kiszolgálókat Azure-ban való hozzáférés. A Contoso Trendérték kívánja lehetővé teszi a alkalmazás tulajdonosa és a csoportwebhelyek alkalmazása kezelése. Az alkalmazás csak akkor közben Contoso a vállalati hálózaton. Dave állíthatja be az előfizetést, ehhez az alkalmazáshoz szükséges. Az előfizetés is helyet más Fejlesztőeszközök kapcsolatos szoftver a jövőben.  

### <a name="naming-standards--resource-groups"></a>Elnevezési szabályai- és erőforrás-csoportok

Dave hoz létre, amelyek a közös végig a részlegek Fejlesztőeszközök támogatja-előfizetést. Ő létre kell hoznia a és előfizetési resource csoportok (az az alkalmazás és a hálózatok) érthető neveket. Ő hozza létre a következő előfizetés- és erőforrás-csoportok:

| Elem | név | Leírás |
| ---- | ---- | ----------- |
| Előfizetés | A Contoso Trendérték DeveloperTools előállítása | A Fejlesztőeszközök közös támogatja |
| Erőforráscsoport | rgBitBucket | Tartalmazza a webes alkalmazáskiszolgáló és az adatbázis-kiszolgáló |
| Erőforráscsoport | rgCoreNetworks | Tartalmazza a virtuális hálózatok és az átjáró hely közötti kapcsolat |


### <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

Miután létrehozott egy előfizetés, Dave szeretné, hogy a megfelelő csoportok és a tulajdonosok alkalmazás hozzáférhet az erőforrások biztosítására. Dave ismer fel, hogy az egyes csapatok eltérő követelményei vannak. Ő segítségével a csoportok a Contoso helyszíni Active Directory (AD) Azure Active Directory szinkronizált, és a megfelelő szintű a csoportoknak a hozzáférést biztosít. 

Dave rendeli az előfizetés a következő szerepkörök: 

| Szerepkör | Hozzárendelt | Leírás |
| ---- | ----------- | ----------- |
| [Tulajdonos](./active-directory/role-based-access-built-in-roles.md#owner) | Azonosító felügyelt a Contoso Active Directory | Az azonosító vezérli csupán az idő (igény szerinti) access Contoso Identitáskezelés eszközzel, és biztosíthatja, hogy teljesen naplózza az előfizetés tulajdonosa access. |
| [Biztonsági kezelője](./active-directory/role-based-access-built-in-roles.md#security-manager) | Biztonság és a kockázatok kezelése részleg | A szerepkör lehetővé teszi, hogy a felhasználók számára, tekintse meg az Azure biztonság otthon és az erőforrások állapotát. |
| [Hálózati közös munka](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Hálózatkezelő csapat | A szerepkör lehetővé teszi, hogy a Contoso hálózatkezelő csapat kezelheti a webhely-webhely VPN és virtuális hálózathoz. |
| *Egyéni szerepkör* | Alkalmazás-tulajdonos | Dave létrehoz egy szerepkört, amely megadja az erőforrások erőforráscsoport belüli módosítását. További tudnivalókért lásd: [Azure RBAC egyéni szerepkörök](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Házirendek

Az alábbi követelményeknek, az előfizetés erőforrások kezelésére szolgáló Dave foglalja magában:

- Mivel a Fejlesztőeszközök támogatja a fejlesztők világszerte, ő nem kívánja megakadályozni erőforrások létrehozni bármelyik tartományban lévő felhasználók. Jó helyen jár úgy kell, hogy hol találhatók az erőforrások létrehozott. 
- A költségek érintett áll. Emiatt azt szeretné, hogy meg, hogy alkalmazás tulajdonosok szükségtelenül drága virtuális gépeken futó létrehozása.  
- Ez az alkalmazás a sok részlegek a fejlesztők szolgál, mert azt szeretné, hogy az egyes erőforrások címkézéséhez tulajdonosával üzleti egység és az alkalmazás. A címkék használatával Trendérték a megfelelő csapatok is bill.

Létrehoz az alábbi [erőforrás-kezelő házirendek](resource-manager-policy.md): 

| A mező | Hatás | Leírás |
| ----- | ------ | ----------- |
| hely | naplózás | Az erőforrások bármelyik tartományban lévő kibocsátása naplózása |
| típus | elutasítása | A G-sorozat virtuális gépeken futó létrehozása elutasítása |
| címkék | elutasítása | Alkalmazás tulajdonosa címke megkövetelése |
| címkék | elutasítása | Költség központ kód megkövetelése |
| címkék | Hozzáfűzés | **Részleghez** neve és a címke érték **Trendérték** hozzáfűzése összes erőforrás |


### <a name="resource-tags"></a>Erőforrás-címkék

Dave megérti, hogy ő adott számlán a azonosítja a költséghely BitBucket végrehajtásához kell szerepelnie. Ezenkívül Dave iránt érdeklődik, hogy a Trendérték tulajdonában álló erőforrások. 

Automatikus hozzáadása a következő [címkéket](resource-group-using-tags.md) az erőforrás-csoportok és erőforrások. 
 
| Címkenév | Címke Érték |
| -------- | --------- |
| ApplicationOwner | Ez az alkalmazás kezelő személy neve. |
| CostCenter | A csoportot, amelyhez a rendszer fizet, a Azure felhasználás a költséghely. |
| Részleghez | **Trendérték** (a az előfizetéshez tartozó részleg) |

### <a name="core-network"></a>Alapvető hálózati

A Contoso Trendérték információk biztonsági és a kockázatok kezelése csoport áttekinti a Emília javasolt terv áthelyezése az Azure alkalmazást. Győződjön meg arról, hogy a az alkalmazás nem érhető el az interneten szeretnének.  Dave tartalmaz, amely a jövőben átkerülnek Azure alkalmazások fejlesztői. Az alkalmazások nyilvános felületek szükség.  Felel meg ezeknek a követelményeknek, ő rendelkezik külső és belső virtuális hálózatokat, és a hozzáférés korlátozására hálózati biztonsági csoport.

Létrehoz az alábbi forrásokat:

| Erőforrás típusa | név | Leírás |
| ------------- | ---- | ----------- |
| Virtuális hálózati | vnInternal | Az BitBucket alkalmazással használja, és a Contoso a vállalati hálózathoz csatlakozik készült ExpressRoute keresztül.  Alhálózat (sbBitBucket) az egy adott IP-címterület alkalmazást tartalmaz. |
| Virtuális hálózati | vnExternal | Nyilvános elérésű végpontok igénylő jövőbeli alkalmazások számára érhető el. |
| Hálózati biztonsági csoport | nsgBitBucket | Ellenőrzi, hogy a terhelést a homonimaszerű webcímmel felületének kis méretre van állítva azáltal, hogy csak a 443-as port alhálózat ahol az a alkalmazás él (sbBitBucket) kapcsolatok. |

### <a name="resource-locks"></a>Erőforrás zárolás: 

Dave ismer fel, hogy a vállalati hálózathoz Contoso virtuális belső hálózatához kapcsolatot az egyes wayward parancsfájlt vagy véletlen törlését fogja védeni. 

Létrehoz az alábbi [erőforrás-zárolás](resource-group-lock-resources.md):

| Zárolás típusa | Erőforrás | Leírás |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Megakadályozza, hogy a felhasználók törléséről a virtuális hálózati vagy alhálózat, de nem akadályozza meg a felvett új alhálózat. |

### <a name="azure-automation"></a>Azure automatizálási 

Dave el ehhez az alkalmazáshoz automatizálása tartalmaz. Bár ő készült Azure automatizálási fiók, ő nem kezdetben használják. 

### <a name="azure-security-center"></a>Azure biztonság otthon 

A Contoso informatikai Szolgáltatáskezelés egyszerűen azonosíthatók, és a kockázatok kezelésére kell. Azok szeretné érteni, hogy milyen hibák előfordulhat, hogy létezik.  

Ezeknek a követelményeknek teljesítéséhez Dave lehetővé teszi, hogy az [Azure biztonság otthon](./security-center/security-center-intro.md), és a biztonsági kezelője szerepkör hozzáférést biztosít. 

## <a name="scenario-2-customer-facing-app"></a>2 alkalmazási helyzetek: ügyfél elérésű alkalmazás

Az üzleti vezetés ellátási lánc részleg a különféle lehetőségek használatával fokozhatja a Contoso vevőkkel részvételét hűséges kártyával azonosította. Anna csoport létre kell hoznia az alkalmazást, és úgy dönt, hogy Azure növeli az azt jelenti, hogy szükséges a vállalati verziós értekezlet. Anna működik-e a Trendérték fejlesztése, és az alkalmazás működő két előfizetések konfigurálása Dave.

### <a name="azure-subscriptions"></a>Azure előfizetések 

Dave az Azure vállalati portál jelentkezik be, és láthatja, hogy a ellátási lánc részleg már létezik.  Azonban ehhez a projekthez kerül ellátási lánc csapattag Azure-ban az első fejlesztési projekt Dave felismer Anna fejlesztőcsapatához új fiókot kell.  Ő létrehozza a "R & D" fiókot a csapat számára, és az access rendel Anna. Anna jelentkezik be a fiók portálon keresztül, és két előfizetések létrehozása: egy tartsa lenyomva az ujját a fejlesztés kiszolgálók, és egy gyártási kiszolgálókon tartása a.  A korábban létrehozott elnevezési szabályai kikkel követi, az alábbi előfizetésekhez létrehozása: 

| Előfizetés használata | név |
| ---------------- | ---- |
| Fejlesztési | SupplyChain ResearchDevelopment LoyaltyCard fejlesztési |
| Gyártási | SupplyChain műveletek LoyaltyCard előállítása |

### <a name="policies"></a>Házirendek

Az alkalmazás megvitatása és azonosítása, hogy az alkalmazás csak szolgál a Észak-amerikai régióban ügyfelek Dave és Anna.  Anna és a csoport létrehozása az alkalmazás a Azure-féle alkalmazás szolgáltatási környezetben és Azure SQL használatának megtervezése. Hozzon létre virtuális gépeken futó fejlesztés során szükségük.  Anna szeretne gondoskodhat arról, hogy a fejlesztők az erőforrások feltárása és a problémák megvizsgálja az exponenciális Simítási adatok nélkül van szükségük. 

**Fejlesztési előfizetés**őket a következő házirend létrehozása:

| A mező | Hatás | Leírás |
| ----- | ------ | ----------- |
| hely | naplózás | Az erőforrások bármelyik tartományban lévő kibocsátása naplózása: |

Nem korlátozza a felhasználó létrehozhat egy fejlesztés alatt álló termékváltozat típusát, és nincs szükségük címkék erőforráscsoport vagy erőforrásokat.

A **gyártási előfizetés**hoznának létre az alábbi házirendeket alkalmazhatja:

| A mező | Hatás | Leírás |
| ----- | ------ | ----------- |
| hely | elutasítása | Tiltsa le az Amerikai Egyesült Államok adatközpontokban kívüli erőforrások létrehozását. |
| címkék | elutasítása | Alkalmazás tulajdonosa címke megkövetelése |
| címkék | elutasítása | Részleg címke szükség. | 
| címkék | Hozzáfűzés | Címke hozzáfűzése minden erőforráscsoport munkakörnyezetben jelző. |

Ezek nem korlátozza a felhasználó gyártási hozhat létre termékváltozat típusú.

### <a name="resource-tags"></a>Erőforrás-címkék 

Dave megérti, hogy ő azonosítja a megfelelő üzleti csoportok a következőhöz: tulajdonjogot és számlázási információk kell szerepelnie. Ő határozza meg, hogy az erőforrás-csoportok és erőforrások erőforrás címkék. 
 
Címkenév | Címke Érték |
| -------- | --------- |
| ApplicationOwner | Ez az alkalmazás kezelő személy neve. |
| Szervezeti egység | A csoportot, amelyhez a rendszer fizet, a Azure felhasználás a költséghely. |
| EnvironmentType | **Gyártási** (Annak ellenére, hogy az előfizetés tartalmazza a **gyártási** a nevében, beleértve a címke lehetővé teszi, hogy könnyen azonosító erőforrásokat a portálon vagy a számlán megtekintve.) |

### <a name="core-networks"></a>Alapvető hálózatok

A Contoso Trendérték információk biztonsági és a kockázatok kezelése csoport áttekinti a Emília javasolt terv áthelyezése az Azure alkalmazást. Győződjön meg arról, hogy hűséges kártya alkalmazása megfelelően elszigetelt és DMZ-hálózatban védett szeretnének.  Ez a követelmény teljesítéséhez Dave és Anna hozzon létre egy külső virtuális hálózatot, és a Contoso vállalati hálózatról hűséges kártya alkalmazása elkülönítése hálózati biztonsági csoport.  

**Fejlesztési előfizetés**hoznának létre:

| Erőforrás típusa | név | Leírás |
| ------------- | ---- | ----------- |
| Virtuális hálózati | vnInternal | A Contoso hűséges kártya fejlesztői környezet szolgál, és készült ExpressRoute keresztül Contoso vállalati hálózathoz csatlakozik. |

A **gyártási előfizetés**hoznának létre:

| Erőforrás típusa | név | Leírás |
| ------------- | ---- | ----------- |
| Virtuális hálózati | vnExternal | Tárolja az hűséges kártya alkalmazást, és nem közvetlenül csatlakozik Contoso készült ExpressRoute. Kód tolódik keresztül a forráskód rendszer közvetlenül a PaaS szolgáltatásokat. |
| Hálózati biztonsági csoport | nsgBitBucket | Ellenőrzi, hogy a terhelést a homonimaszerű webcímmel felületének kis méretre van állítva azáltal, hogy csak a kötött kommunikáció a TCP 443-as.  A Contoso is vizsgálja további védelmet a webes alkalmazás tűzfalat használ. |  

### <a name="resource-locks"></a>Erőforrás zárolás: 

Dave Anna kölcsönözzön és úgy dönt, hogy az erőforrás zárolások hozzáadása a fő forrásokhoz, hogy egy errant kód leküldéses során véletlen törlését a környezetben. 

Az alábbiak lock hoznának létre:

| Zárolás típusa | Erőforrás | Leírás |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Szeretné akadályozni a virtuális hálózati vagy alhálózat törlésére. A zárolás nem akadályozza meg a felvett új alhálózat. |

### <a name="azure-automation"></a>Azure automatizálási 

Anna és saját fejlesztőcsapatához van teljes körű runbooks kezelése ehhez az alkalmazáshoz a környezetet. Az alkalmazás és a többi DevOps feladatok csomópontok hozzáadása vagy törlése a runbooks lehetővé teszi. 

Ezek a runbooks használatához lehetővé teszik [az automatizálási](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure biztonság otthon 

A Contoso informatikai Szolgáltatáskezelés egyszerűen azonosíthatók, és a kockázatok kezelésére kell. Azok szeretné érteni, hogy milyen hibák előfordulhat, hogy létezik.  

Ezeknek a követelményeknek teljesítéséhez Dave lehetővé teszi, hogy Azure biztonság otthon. Ő biztosítja, hogy az Azure biztonság otthon figyeli az erőforrások és a DevOps és a biztonsági csoportok hozzáférést biztosít. 

## <a name="next-steps"></a>Következő lépések

- Erőforrás-kezelő sablonok létrehozásával kapcsolatos további tudnivalókért olvassa el a [Gyakorlati tanácsok az erőforrás-kezelő Azure-sablonok létrehozása](resource-manager-template-best-practices.md)című témakört.

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) járult ezt a témakört.*
