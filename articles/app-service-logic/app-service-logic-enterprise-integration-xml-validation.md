<properties 
    pageTitle="XML-adatok érvényesítése a nagyvállalati integrációs Pack áttekintése |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="További tudnivalók az érvényesítési nagyvállalati integrációs csomag és logikai hálózatok-alkalmazások működése" 
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

# <a name="enterprise-integration-with-xml-validation"></a>XML-adatok érvényesítése vállalati integrációja

## <a name="overview"></a>– Áttekintés
Gyakran B2B esetekben megállapodást a partnerek ellenőriznie kell ahhoz, hogy megkezdhesse a az adatok feldolgozásának azok az exchange egymással üzenetek érvényesek. Az a nagyvállalati integrációs csomag az XML-adatok érvényesítése összekötő használhat, ha egy előre definiált séma alapján dokumentumok érvényesítéséhez.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Hogyan kell a dokumentumokat az XML-adatok érvényesítése összekötő ellenőrzése
1. Hozzon létre egy logika alkalmazást, és [csatolja a integrációs fiókjába](./app-service-logic-enterprise-integration-accounts.md "ismerje meg, hogy egy logikai alkalmazásba integrációs fiók, amelyre a hivatkozás") , amely tartalmazza a használni kívánt XML-adatok érvényesítése a séma.
2. A **kérelem – Ha a HTTP-kérelem érkezik** eseményindító felvétele a logika alkalmazásba  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Az **XML-adatok érvényesítése** beavatkozását első parancsot választva **adja hozzá a művelet** hozzáadása  
4. *Xml* annak érdekében, hogy az összes műveletet, amely a használni kívánt szűrés a Keresés mezőbe írja be 
5. Jelölje be az **XML-adatok érvényesítése**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Jelölje ki a **tartalom** szövegdobozt  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Jelölje be a szervezet címke ellenőrizni kell a tartalom.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Jelölje ki a **SÉMA neve** listában, és válassza a fenti beviteli *tartalom* érvényesítéséhez használni kívánt séma     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Mentse a munkáját  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Ezen a ponton befejezte az érvényesítési összekötő beállítását. Egy teljesítménynövekedés alkalmazásban előfordulhat, hogy tárolni szeretné az ellenőrzött adatok egy üzleti alkalmazásban, például a SalesForce. Egyszerűen felvehet egy művelet Salesforce az érvényesítési kimenetként. 

Most már tesztelheti az érvényesítési műveletet kérést, így a HTTP-végpontot.  

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag")   