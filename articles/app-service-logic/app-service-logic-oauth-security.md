<properties
    pageTitle="OAUTH biztonsági szoftver összekötők és az API-alkalmazások |} Azure"
    description="További információ: OAUTH biztonsági az összekötők és Azure App szolgáltatásban; API-alkalmazások architektúra microservices; szoftver"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Tudnivalók a szoftver összekötők OAUTH biztonsági

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

A szoftvert, mint például Facebook, a Twitteren, a DropBox és így tovább kötelezővé tétele a felhasználóknál az OAUTH protokollal hitelesítse magát egy szolgáltatást (szoftver) összekötők számos.  A szoftver összekötők logika alkalmazásokból használatakor kínálunk, amely a kattintás helyére "Engedélyezése" a logika alkalmazások tervezőben egyszerűsített felhasználói felület. Ha Ön **engedélyezése**, rendszer kéri, hogy jelentkezzen be a (Ha még nem), és adja meg a felhasználó hozzájárul ahhoz csatlakozni az Ön nevében a szoftver szolgáltatást. Miután adja meg a felhasználó hozzájárul ahhoz, és engedélyezheti, az összefüggés-alkalmazások majd érheti el az alábbi szoftver szolgáltatások.

## <a name="create-your-own-saas-app"></a>A saját szoftver-alkalmazás létrehozása
A egyszerűsített élmény oka lehetséges azt a korábban létrehozott, és az alkalmazás regisztrált szoftver szolgáltatásokra.  Bizonyos esetekben célszerű regisztrálhat, és használja a saját alkalmazást.  Szükség, például amikor az egyéni alkalmazásokban a következő szoftver összekötők használni kívánt. Ebben a példában a DropBox-összekötő, de a folyamat OAUTH támaszkodó összekötők megegyezik.

Akár környezetében logika alkalmazások a saját alkalmazások helyett használja az alapértelmezett alkalmazás, amely elvégezheti a szükséges is használhatja. Ha nem sikerül kapcsolódni a "Engedélyezése" gomb, próbálja meg a saját alkalmazások létrehozását. Az alábbi lista tartalmazza ezeket a lépéseket, a Twitteren összekötő:

1. Nyissa meg a Twitteren összekötő az Azure előzetes portálon. Nyissa meg a **Tallózás** > **API-alkalmazásokat**. Jelölje ki a Twitteren összekötő:  
    ![][1]

2. Válassza a **Beállítások** > **hitelesítés**:  
    ![][2]

3. Másolja az **Átirányítás URI** értéket.  
    ![][3]

4. Nyissa meg a [Twitter](http://apps.twitter.com) , és **Hozzon létre egy új alkalmazást**. A **Visszahívási URL-cím** tulajdonságot illessze be az **Átirányítás URI** értéket másolja át a Twitteren összekötő: ![][4]  
5. A Twitter-alkalmazás létrehozásakor, jelölje be a **billentyű és a hozzáférési jogkivonat**. Másolja a következő értékeket.
6. Illessze be a Twitteren összekötő hitelesítési beállítások ezeket az értékeket az **Ügyfél-azonosító** , és az **Ügyfél titkos** tulajdonságok:   
    ![][5]  
7. Mentse az összekötő beállításait.  

Most már láthatja az összekötő logika alkalmazásokban használandó. Ez az összekötő logika alkalmazásokból használata esetén az alapértelmezett alkalmazás helyett használja az alkalmazás.  

> [AZURE.NOTE] Ha az alkalmazás korábban engedélyezték, előfordulhat zavartalanul az alkalmazást.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
