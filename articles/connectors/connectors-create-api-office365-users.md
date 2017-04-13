<properties
    pageTitle="Logika alkalmazások hozzáadása az Office 365-felhasználóknak összekötő |} Microsoft Azure"
    description="Az Office 365-felhasználóknak összekötő REST API-paraméterekkel áttekintése"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Első lépések az Office 365-felhasználóknak összekötő

Csatlakozás az Office 365-felhasználók profilokat, a keresés felhasználói és egyebek. 

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik.

Az Office 365-felhasználóknak a következőkre van lehetősége:

- Hozza létre az üzleti folyamat kap, az Office 365-felhasználóknak adatok alapján. 
- Közvetlen beosztottai, kérjen használata műveleteket első vezető felhasználói profil és az egyéb. Az alábbi műveletek kap választ, és végezze el a kimenet rendelkezésre álló további lehetőségeket. Például első személy közvetlen beosztottai, majd ezt az információt készítése és SQL Azure-adatbázis frissítése. 

Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az Office 365-felhasználóknak összekötő érhető el az alábbi műveletek tartalmaz. Vannak olyan nincs indítók.

| Eseményindítók | Műveletek|
| --- | --- |
|Nincs lehetőség | <ul><li>Kezelő beszerzése</li><li>Saját profil beszerzése</li><li>A felhasználó közvetlen beosztottai beszerzése</li><li>Felhasználói profil beszerzése</li><li>Felhasználók keresése</li></ul>|

Összekötők JSON és az XML formátumú támogatja az adatokat. 


## <a name="create-a-connection-to-office-365-users"></a>Az Office 365-felhasználóknak kapcsolat létrehozása

Ez az összekötő logika alkalmazás beállításakor kell bejelentkezés az Office 365-felhasználók fiókjához, és a fiókhoz való csatlakozáshoz összefüggés-alkalmazás letiltása.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Miután létrehozta a kapcsolatot, írja be az Office 365-felhasználóknak tulajdonságait, például a felhasználói azonosítójával. Ez a témakör a **REST API-hivatkozás** e-tulajdonságokat ismerteti.

>[AZURE.TIP] A ugyanazt az Office 365-felhasználóknak a kapcsolatot az egyéb összefüggés-alkalmazásokban is használhatja.


## <a name="office-365-users-rest-api-reference"></a>Az Office 365 felhasználói REST API-hivatkozás
Verziójára vonatkozik: 1.0.

### <a name="get-my-profile"></a>Saját profil beszerzése 
Olvassa be az aktuális felhasználói profilját.  
```GET: /users/me``` 

Nincsenek a hívás paraméterek.

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-user-profile"></a>Felhasználói profil beszerzése 
Egy adott felhasználói profil veszi.  
```GET: /users/{userId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Felhasználóazonosító|karakterlánc|igen|elérési út|nincs lehetőség|Egyszerű nevét vagy e-mail felhasználóazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-manager"></a>Kezelő beszerzése 
Felhasználói profil beolvassa a az adott felhasználó-kezelő.  
```GET: /users/{userId}/manager``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Felhasználóazonosító|karakterlánc|igen|elérési út|nincs lehetőség|Egyszerű nevét vagy e-mail felhasználóazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|



### <a name="get-direct-reports"></a>A felhasználó közvetlen beosztottai beszerzése 
Ismerkedés a felhasználó közvetlen beosztottai.  
```GET: /users/{userId}/directReports``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Felhasználóazonosító|karakterlánc|igen|elérési út|nincs lehetőség|Egyszerű nevét vagy e-mail felhasználóazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|



### <a name="search-for-users"></a>Felhasználók keresése 
Olvassa be a felhasználói profilok találatok.  
```GET: /users``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|searchTerm|karakterlánc|nem|lekérdezés|nincs lehetőség|Keresendő karakterlánc (vonatkozik: Keresztnév, a megadott név, a Vezetéknév, a levelek, a mail becenév és a felhasználó egyszerű felhasználónév megjelenítése)|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|



## <a name="object-definitions"></a>Objektum definíciók

#### <a name="user-user-model-class"></a>Felhasználó: Felhasználói modell osztályához

|Tulajdonság neve | Adattípus |Szükséges
|---|---|---|
|DisplayName|karakterlánc|nem|
|GivenName|karakterlánc|nem|
|Vezetéknév|karakterlánc|nem|
|Levelek|karakterlánc|nem|
|Mailnickname-beli|karakterlánc|nem|
|TelephoneNumber|karakterlánc|nem|
|AccountEnabled|logikai érték|nem|
|Azonosító|karakterlánc|igen
|UserPrincipalName|karakterlánc|nem|
|Szervezeti egység|karakterlánc|nem|
|Munkakör|karakterlánc|nem|
|mobilePhone|karakterlánc|nem|


## <a name="next-steps"></a>Következő lépések

[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

Térjen vissza az [API-khoz listában](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
