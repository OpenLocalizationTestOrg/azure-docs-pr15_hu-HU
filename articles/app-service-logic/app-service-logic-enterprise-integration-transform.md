<properties 
    pageTitle="Nagyvállalati integrációs csomag áttekintése |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="Ahhoz, hogy a Microsoft Azure alkalmazás szolgáltatással üzleti folyamatok és integrációs forgatókönyvek te104004683et nagyvállalati integrációs csomag" 
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

# <a name="enterprise-integration-with-xml-transforms"></a>XML-átalakítók vállalati integrációja

## <a name="overview"></a>– Áttekintés
A nagyvállalati integrációs átalakítás összekötő adatokat az egyik formátumról formátumúvá alakítja át egy másik. Előfordulhat például, egy bejövő üzenet, amely tartalmazza az aktuális dátum YearMonthDay formátumban. A dátum MonthDayYear formátumban kell formáznia vagy átalakítás is használhatja.

## <a name="what-does-a-transform-do"></a>Mit jelent egy átalakítás?
Átalakítás, amely más néven egy térképen, egy adatforrás XML-séma (a bemeneti) és a cél XML-séma (kibocsátás) áll. Különböző beépített függvények használatával segítségével, módosítására, és szabályozhatja az adatokat, például a karakterlánc-műveletek, feltételes hozzárendelések, aritmetikai kifejezések, dátum-idő formatters, és még hurok tartalmazza.

## <a name="how-to-create-a-transform"></a>Hogyan lehet létrehozni vagy átalakítás?
Átalakítás/megfeleltetése a Visual Studio [Nagyvállalati integrációs SDK](https://aka.ms/vsmapsandschemas)használatával hozhat létre. Amikor elkészült, létrehozása és tesztelése a átalakítás, töltse fel az átalakítás integrációs fiókjába. 

## <a name="how-to-use-a-transform"></a>Átalakító használata
Az átalakítás feltöltése integrációs fiókjába, után használhatja összefüggés-alkalmazás létrehozása céljából. A logikai alkalmazás fog futni a átalakítások valahányszor a logika alkalmazás induljanak (és nem kell átalakítani kívánt beviteli tartalom).

**Az alábbiakban átalakító szükséges lépéseket**:

### <a name="prerequisites"></a>Előfeltételek 
Kattintson az előnézetben meg kell:  

-  [Az Azure függvények tároló létrehozása] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Az Azure függvények tároló létrehozása")  
-  [Hozzáadás az Azure függvények tárolóhoz függvény] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Ez a sablon alapján webhook C# azure függvény használata a logika alkalmazások integrációs forgatókönyvek átalakítás funkciókhoz hoz létre")    
-  Integráció fiók létrehozása és a térkép elhelyezése  

>[AZURE.TIP] Jegyezze le a nevét az Azure függvény és a függvények Azure tároló, szüksége lesz rájuk a következő lépésben.  

Most, hogy a Előfeltételek a által készített érdeklő, pedig a összefüggés-alkalmazás létrehozása:  

1. Hozzon létre egy logika alkalmazást, és [csatolja a integrációs fiókjába](./app-service-logic-enterprise-integration-accounts.md "ismerje meg, hogy egy logikai alkalmazásba integrációs fiók, amelyre a hivatkozás") , amely tartalmazza a térkép.
2. A **kérelem – Ha a HTTP-kérelem érkezik** eseményindító felvétele a logika alkalmazásba  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Az első parancsot választva **művelet hozzáadása** **Átalakítása XML** beavatkozását hozzáadása   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Adja meg a word *átalakító* a Keresés mezőbe annak érdekében, hogy a szűrő, amely a használni kívánt a műveletek  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Jelölje ki az **XML átalakítási** műveletet   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Jelölje ki a használni kívánt függvényt tartalmazó **FÜGGVÉNY tároló** . Ez az a ezeket a lépéseket a korábban létrehozott Azure függvények tároló nevét.
7. Jelölje ki a használni kívánt **FÜGGVÉNYT** . Ez az a korábban létrehozott Azure függvény neve.
8. Adja hozzá az XML- **tartalom** lesz a átalakító. Megjegyzés: a használható jelenhet meg a HTTP-összehívást a **tartalom**található XML-adatokat. Ebben a példában válassza ki a HTTP-kérés, amelyen a logika alkalmazás törzsében.
9. Jelölje ki a nevét a **térkép** tudja elvégezni az átalakítás kívánt. A térkép kell a integrációs-fiókjában. Egy korábbi lépésben, már rendelkezésére bocsátotta a logika alkalmazás hozzáférést a térképet tartalmazó integrációs-fiókjába.
10. Mentse a munkáját  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Ezen a ponton végzett állítsa be a térképre. Egy valós alkalmazásban előfordulhat, hogy tárolni szeretné az átalakított adatokat egy üzleti alkalmazásban, például a SalesForce. Könnyen Salesforce az átalakítás kimenetként szolgáló műveletet. 

Most már tesztelheti az átalakítás kérést, így a HTTP-végpontot.  

## <a name="features-and-use-cases"></a>Funkciók és használati eset

- Lehet, hogy az átalakítás létrehozott térképen egyszerű, például másolása egy neve és címe a dokumentum egy másik. Vagy a ki-be a térkép Operations összetettebb átalakítások hozhat létre.  
- Több térkép műveletek és funkciók érhetők el könnyen, beleértve a karakterláncok, dátum-idő függvények és így tovább.  
- A sémák közötti közvetlen adatok másolatának műveleteket hajthat végre. Ez a hozzárendelést a SDK szerepel, az a egyszerűen, a rajz egy sort, amely az elemeket a forrás sémában kapcsolódik a mint a cél sémában.  
- Térkép létrehozása, megtekintheti a térképet, grafikus ábrázolása megjelenítése a kapcsolatok és a hivatkozások hoz létre, amely.
- A próba-leképezés funkcióval minta XML-üzenet írása. Egy egyszerű kattintással tesztelje a térkép hozott létre, és a létrehozott kimenet jelenik meg.  
- Töltse fel a meglévő térképek  
- Az XML formátum támogatása tartalmazza.


## <a name="learn-more"></a>tudj meg többet
- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag")  
- [További információ: térképek] (./app-service-logic-enterprise-integration-maps.md "Megtudhatja, hogy nagyvállalati integrációs megfeleltetések")  
 