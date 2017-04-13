<properties
    pageTitle="Egyéni beállításokat az alkalmazás-szolgáltatási környezetben"
    description="Egyéni beállítások alkalmazás szolgáltatási környezetben"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Egyéni beállítások alkalmazás szolgáltatási környezetben

## <a name="overview"></a>– Áttekintés ##
Alkalmazás környezetek elkülönítik egyetlen ügyfélnek, mert nincsenek kizárólag alkalmazás szolgáltatási környezetben alkalmazható egyes beállításokat. Ez a cikk a dokumentumok a különböző adott testre szabott funkciók elérhető App szolgáltatási környezetben.

Ha nem rendelkezik az alkalmazás-szolgáltatási környezetben, megtudhatja, [hogy miként hozhat létre az alkalmazás-szolgáltatási környezetben](app-service-web-how-to-create-an-app-service-environment.md).

Alkalmazás-szolgáltatási környezetben testreszabások tömb használatával az új **clusterSettings** attribútum tárolhat. Az attribútum *hostingEnvironments* Azure erőforrás-kezelő személy "Tulajdonságok" szótár található.

A következő rövidített erőforrás-kezelő sablon kódtöredékének **clusterSettings** attribútum jeleníti meg:


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

A **clusterSettings** attribútum beépíthetők erőforrás-kezelő sablon frissítéséhez az alkalmazás-szolgáltatási környezetben.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Azure erőforrás Explorert használ szolgáltatási környezetben alkalmazás frissítése
Azt is megteheti frissítheti az alkalmazás-szolgáltatási környezetben fájlkezelőn keresztül [Azure erőforrás](https://resources.azure.com).  

1. Az erőforrás Explorerben válassza az alkalmazás szolgáltatási környezetben csomópontot (**előfizetések** > **resourceGroups** > **szolgáltatók** > **Micrososft.Web** > **hostingEnvironments**). Válassza ki a frissíteni kívánt adott alkalmazás szolgáltatási környezetben.

2. A jobb oldali ablaktáblán kattintson az **Olvasási/írási** interaktív az erőforrás Explorer Szerkesztés engedélyezése a felső eszköztáron.  

3. Kattintson az erőforrás-kezelő sablon szerkeszthetővé teheti a kék **szerkesztése** gombra.

4. Görgessen le a jobb oldali ablaktábla alján. A **clusterSettings** attribútum legalján, ahol adja meg vagy módosítsa az értéket.

5. Írja be (vagy illessze be) a **clusterSettings** attribútumban kívánt konfigurációs értékek tömbjét.  

6. Kattintson a zöld **ELHELYEZNI** gomb, amely a jobb oldali véglegesítse a változás az alkalmazás-szolgáltatási környezetben való tetején található.

Azonban módosítása céljából, szorozva az alkalmazás szolgáltatási környezetben, a változtatások érvénybe léptetéséhez első végű számával nagyjából 30 percig tart.
Például ha egy alkalmazás szolgáltatási környezetben négy első vége van, megnyílik a konfigurációs frissítés befejezéséhez nagyjából két óra. A konfigurációs módosítást van folyamatban közzétételének, míg más méretezési műveletek vagy konfigurációs módosítást művelet történhet az alkalmazás-szolgáltatási környezetben.

## <a name="disable-tls-10"></a>TLS 1.0 letiltása ##
Ismétlődő kérdést az ügyfelek, különösen olyan ügyfeleknek, akik PCI megfelelőségi foglalkoznak eseményeket, és megtudhatja, hogy miként kifejezetten tiltsa le a saját alkalmazások TLS 1.0.

A következő **clusterSettings** bejegyzés keresztül TLS 1.0 is tiltható le:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>TLS titkosítás csomagja sorrendjének módosítása ##
Az ügyfelek egy másik kérdést, ha módosíthatják a kiszolgáló által egyeztetett titkosítási listája ezt a **clusterSettings** módosításával alább látható módon lehet elérni. A rendelkezésre álló titkosítás csomagok listáját [Ez MSDN-cikk] lehet beolvasni (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Ha nem a megfelelő értékek, amelyek SChannel nem érti titkosítás csomagja vannak beállítva, a kiszolgáló minden TLS kommunikációs esetleg nem működik-e. Ebben az esetben meg kell a *FrontEndSSLCipherSuiteOrder* bejegyzés eltávolítása **clusterSettings** és elküldése biztosítson az alapértelmezett titkosítás csomagja beállításokat a frissített erőforrás-kezelő sablon.  Körültekintően ezt a funkciót használja.

## <a name="get-started"></a>Első lépések
A sablon Azure quickstart útmutató erőforrás-kezelő webhely sablon az alap definition hozhat létre [Az alkalmazás-szolgáltatási környezetben](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/)tartalmazza.


<!-- LINKS -->

<!-- IMAGES -->
