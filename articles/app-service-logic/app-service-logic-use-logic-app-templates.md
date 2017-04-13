<properties
 pageTitle="Logika alkalmazássablonok |} Microsoft Azure"
 description="Előre létrehozott logika alkalmazássablonok használata az első lépésekhez"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Logika alkalmazássablonok

## <a name="what-are-logic-app-templates"></a>Mik azok a logika alkalmazássablonok

Egy logikai alkalmazássablon egy beépített logika alkalmazást, amelyek segítségével gyorsan elsajátíthatja a saját munkafolyamat. 

Ezek a sablonok megtudhatja, hogy az összefüggés-alkalmazások használata beépíthető különböző mintázatok nagyszerűen állnak. Használhatja ezeket a sablonokat,-, vagy módosíthatja őket az igényektől igazítása.

## <a name="overview-of-available-templates"></a>A rendelkezésre álló sablonok – áttekintés

Vannak olyan sok rendelkezésre álló sablonok a logika alkalmazás platform aktuálisan közzétett. Néhány példa kategóriák, valamint a velük összekötői típusú felsorolása olvasható.

### <a name="enterprise-cloud-templates"></a>Vállalati felhő sablonok
Dynamics CRM, Salesforce, mezőbe, Azure Blob és más vállalati felhő igényeinek összekötők együttműködő sablonok. Néhány példa, mit lehet tenni a következő Microsoft tartalmazza, az érdeklődők rendszerezése és a vállalati fájl adatok biztonsági másolatának.

### <a name="enterprise-integration-pack-templates"></a>Nagyvállalati integrációs csomag sablonok
VETER konfigurációit (érvényesítése, kibontása, átalakítás, kiegészítése, átirányítása) folyamatok, egy X12 szerkesztése dokumentum AS2 fölé, és az XML-, átalakítás, valamint X12 és AS2 üzenet kezelési érkeznek.

### <a name="protocol-pattern-templates"></a>Protocol (protokoll) mintát sablonok
Ezek a sablonok logika alkalmazások tartalmazó protocol mintázatok kérés-válasz például HTTP, valamint a integrációs fölé FTP és SFTP állnak. Hogyan ezek léteznek vagy alapjaként összetettebb protokoll mintázatok hozhat létre.  

### <a name="personal-productivity-templates"></a>Személyes hatékonyság sablonok
Személyes termelékenység javítása érdekében mintázatok napi emlékeztetők beállítása, fontos munkatételek alakítani feladatlisták és le egy felhasználó jóváhagyási lépés hosszadalmas feladatok automatizálása sablonokat tartalmazza.

### <a name="consumer-cloud-templates"></a>Fogyasztói felhő sablonok
Egyszerű integráció a közösségi hálózat szolgáltatások, például a Twitteren, a tartalékidő és a levelezés, végül alkalmas kezdeményezések marketing közösségi hálózat erősítő sablonok. Ezek is zavaros másol, például sablonokat, amelyek segíthetnek a hatékonyság növelése töltött időt mentenie a hagyományos ismétlődő tevékenységeket. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>A sablon használatával összefüggés-alkalmazás létrehozása 

Első lépésként logika alkalmazás sablon használatával a logika alkalmazás Tervező ismertetőt találhat. A Tervező beviszi egy meglévő logika alkalmazásban megnyitva, ha a logikai alkalmazás automatikusan betölti a Tervező nézetben. Jó helyen jár Ha új logika alkalmazás készít, látni a képernyőn az alábbi.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

A képernyőn választhat indítson egy üres logika alkalmazás vagy beépített sablon. A sablonok közül választhat, további információkkal szolgálnak. Ebben a példában az *Új fájl létrehozásakor a Dropbox, másolja a vágólapra a onedrive-ra* sablon használjuk.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Ha úgy dönt, hogy a sablon használata, csak jelölje ki az *ezzel a sablonnal* gombra. Meg kell adnia való bejelentkezéshez a partnereket a mely összekötők a sablon segítségével. Vagy ha korábban már létrehozta a kapcsolatot a következő összekötők, válassza alább látható módon a továbbra is.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

A kapcsolat létrehozása, és *továbbra is*kiválasztása után a logika alkalmazás Tervező nézetben nyílik meg.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

A fenti példában akárcsak sok sablonokkal egyes tulajdonság kötelező mezői előfordulhat, hogy kitöltendő belül az összekötők; azonban néhány továbbra is előfordulhat, egy érték nem megfelelően az az érték alkalmazások terjesztése előtt. Ha megpróbálja üzembe megadása a hiányzó mezőket részét nélkül, amelyek értesítik, a megjelenő hibaüzenet.

Ha ki szeretne térni a sablon megjelenítő, jelölje ki a *sablonok* gombra a felső navigációs sávon. Váltás a sablon megjelenítő, elvesznek a nem mentett végrehajtási. Előtt sablon megjelenítő az eddig, megjelenik egy figyelmeztető üzenet értesítő e.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Hogyan kell egy sablonból létrehozott logika alkalmazások terjesztése

Miután végzett a kívánt módosításokat, és a sablon betöltése, jelölje be a Mentés gombra a képernyő bal felső sarkában. Menti, és a teszi közzé a logika alkalmazást.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

További információt a mezőkódokkal adjon hozzá egy meglévő logika alkalmazás sablonba további lépéseket, és ellenőrizze az általános szerkesztése című cikkben olvashat a [összefüggés-alkalmazás létrehozása](app-service-logic-create-a-logic-app.md).