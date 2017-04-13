<properties
   pageTitle="Logika alkalmazások példák és -esetek |} Microsoft Azure"
   description="Közös összefüggés alkalmazások példák, és megtudhatja, hogy miként tudnak megvalósítani tipikus esetei"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Logika alkalmazások példák és tipikus esetei

A dokumentum részletek esetei és példát tartalmaz az segít megérteni néhány módszert, akkor segítségével logika alkalmazások üzleti folyamatok automatizálása. 

## <a name="custom-triggers-and-actions"></a>Egyéni eseményindítók és műveletek

Többféleképpen is elindíthat egy másik alkalmazásból összefüggés-alkalmazást. Az alábbiakban néhány hétköznapi példát:

- [Hozzon létre egy egyéni eseményindító vagy művelet](app-service-logic-create-api-app.md)
- [A hosszú ideig futó műveletek](app-service-logic-create-api-app.md)
- [HTTP kérelem eseményindító (KÜLDÉS)](app-service-logic-http-endpoint.md)
- [Webhook eseményindítók és műveletek](app-service-logic-create-api-app.md)
- [Lekérdezési indítók](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Felhasználási területei

- [Kérés szinkron válasz](app-service-logic-http-endpoint.md)
- [Válasz az SMS kérése](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Hibakezelő és naplózás

- [Kivétel és hibakezelésre](app-service-logic-exception-handling.md)
- [Azure-riasztások és diagnosztika konfigurálása](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Felhasználási területei

- [Használati eset: Hiba és kivétel kezelése](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Üzembe helyezése és kezelése

- [Hozzon létre egy automatikus telepítési](app-service-logic-create-deploy-template.md)
- [Építse fel és telepítse az Visual Studio logika alkalmazások](app-service-logic-deploy-from-vs.md)
- [Logika alkalmazásainak figyelése](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Tartalomtípusok, átalakítások és átalakítások

[Munkafolyamat definition language](http://aka.ms/logicappsdocs) logika alkalmazások számos függvény lehetővé teszi a alakíthatja, és a különböző tartalomtípusok használata tartalmazza.  Ezeken a motor mindent összes megőrzéséhez tartalomtípusok a munkafolyamaton keresztül adatfolyamok, azt is.

- [Tartalom-típusok kezelése](app-service-logic-content-type.md) például alkalmazás/json, az alkalmazás/xml és az egyszerű és szöveg
- [Munkafolyamat-definíciók létrehozása](app-service-logic-author-definitions.md)
- [Munkafolyamat-definition language hivatkozása](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Kötegekben és hurok

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Amíg](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integrálása az Azure függvények

- [Azure függvények-integráció](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Felhasználási területei

- [Szolgáltatás Bus kiindulópontként Azure függvény](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP, a többi és SOAP

 - [SOAP hívása](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Azt fog tartani felvétele példák és -esetek ezt a dokumentumot. Az alábbi szakaszt a megjegyzések használatával tudathatja velünk, hogy milyen példák vagy esetek szeretné, hogy az itt látható.