<properties 
    pageTitle="Nagyvállalati integrációs Pack B2B megoldások |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="További tudnivalók: adatok, a nagyvállalati integrációs csomag B2B funkcióival fogadása" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>További tudnivalók: adatok, a nagyvállalati integrációs csomag B2B funkcióival fogadása#

## <a name="overview"></a>– Áttekintés ##

Ehhez a dokumentumhoz a logika alkalmazások nagyvállalati integrációs csomag része. Olvassa el a további tudnivalók [a nagyvállalati integrációs csomag lehetőségeit](./app-service-logic-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Előfeltételek ##

A AS2 és X12 szüksége lesz a nagyvállalati integrációs fiók műveletek

[Nagyvállalati integrációs fiók létrehozása](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>A logikai alkalmazások B2B összekötők használata ##

Integráció fiók létrehozta, és megjelenik a partnerek, és azt rendelkezést készen áll a összefüggés-alkalmazás létrehozása után, üzleti az üzleti (B2B) munkafolyamat hajtja végre.

Az e walkthru látni fogja a AS2 és X12 használatával műveletek adatokat kapja a kereskedelmi partnertől, üzleti az üzleti összefüggés-alkalmazás létrehozása céljából.

1. Hozzon létre új logika alkalmazás és a [hivatkozás integrációs fiókjára](./app-service-logic-enterprise-integration-accounts.md).  
2. A **kérelem – Ha a HTTP-kérelem érkezik** eseményindító felvétele a logika alkalmazásba  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Az első parancsot választva **művelet hozzáadása** **Dekódolását AS2** beavatkozását hozzáadása  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Adja meg a word **as2** annak érdekében, hogy az összes műveletet, amely a használni kívánt szűrés a Keresés mezőbe  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Jelölje ki a **AS2 - dekódolását AS2 üzenet** műveletet  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Ahogy azt, adja hozzá a **szervezet** tart, mint a bemeneti. Ebben a példában válassza ki a HTTP-kérés, amelyen a logika alkalmazás törzsében. Megadhat egy kifejezést a**FEJLÉCEK** mezőben a fejlécek bemeneti azt is megteheti:

    @triggerOutputs()['headers']

8. A **fejlécek** AS2 szükséges hozzá. Ezek a HTTP-kérés fejlécek tekinthetők meg. Ebben a példában válassza ki a fejléceket a HTTP-kérés, amelyen a logika alkalmazást.
9. Most már vehet dekódolási X12 üzenet művelet újra **művelet hozzáadása**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Adja meg a word **x12** annak érdekében, hogy az összes műveletet, amely a használni kívánt szűrés a Keresés mezőbe  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Jelölje ki a **X12-dekódolását X12 üzenet** fel szeretne venni a logika alkalmazás művelet  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Most már kell adja meg a bemeneti ezt a műveletet, amely a eredménye a fenti AS2 művelet lesz. Az aktuális üzenet tartalmának JSON-objektum, és van kódolva base64. Ezért kell adjon meg egy kifejezést, mint a bemeneti úgy adja meg az alábbi kifejezés a **X12 sík fájl MESSAGE TO DEKÓDOLÁSI** beviteli mezőben  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Ebben a lépésben a X12 adatok kereskedelmi partneren, és a kimeneti lesz a JSON-objektum elemszámát fog dekódolását. Annak érdekében, hogy a partner, hogy az adatok átvételének engedélyezése elküldheti vissza az AS2 üzenet törlés értesítés (MDN) a HTTP válasz műveletet tartalmazó visszajelzés  
14. A **Válasz** művelet vehet **művelet hozzáadása**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Adja meg a word **Válasz** annak érdekében, hogy az összes műveletet, amely a használni kívánt szűrés a Keresés mezőbe  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Jelölje ki a **Válasz** műveletet, így az bekerül  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Állítsa be a választ **szervezet** mezőt a MDN hozzáférhet a **dekódolási X12 üzenet** művelet eredményét a következő kifejezés használatával  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Mentse a munkáját  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

Ezen a ponton végzett beállítja a B2B összefüggés. Egy valós alkalmazásban, előfordulhat, hogy tárolni szeretné a dekódolt X12 adatok üzleti alkalmazások és -adatok tárban. Egyszerűen hozzáadhatja a további műveletek ehhez, vagy írja be a saját üzleti alkalmazások csatlakozhat, és ezek API-k használata a logika alkalmazásban egyéni API-khoz.

## <a name="features-and-use-cases"></a>Funkciók és használati eset ##

- A AS2 X12 dekódolását és kódolását műveletek lehetővé teszi az adatok fogadására és adatok küldése a kereskedelmi iparági szabvány szerinti összefüggés alkalmazásokkal: szabványos protokollt használó partnerek  
- Segítségével AS2 és X12 vagy anélkül egymást adatmegosztás kereskedelmi partnerek szükség szerint
- A B2B műveletek megkönnyítik létrehozása a partnerek és rendelkezést integrációs fiók és felhasználása őket egy logikai alkalmazásban  
- A logika alkalmazás a többi tevékenységekkel időtartamának küldhet és fogadhat adatokat, és az egyéb alkalmazások és szolgáltatások, például a SalesForce  

## <a name="learn-more"></a>tudj meg többet ##

[További tudnivalók a nagyvállalati integrációs csomag](./app-service-logic-enterprise-integration-overview.md)  