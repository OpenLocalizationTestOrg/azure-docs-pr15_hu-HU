<properties
    pageTitle="Ismerje meg, hogyan kódolását vagy sík fájlok, a nagyvállalati integrációs csomag és logikai hálózatok alkalmazások használatával dekódolását |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure"
    description="Nagyvállalati integrációs csomag és logikai hálózatok alkalmazások funkcióját használni kódolását vagy dekódolását egyszerű fájlok"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Enterprise-integráció a strukturálatlan fájl

## <a name="overview"></a>– Áttekintés

Előfordulhat, hogy szeretne kódolni XML-tartalmat, egy üzleti partnernek üzleti vállalati verzió (B2B) használatával az elküldés előtt. Alkalmazásban a logika a logika alkalmazások funkció az Azure alkalmazás szolgáltatás által végzett a strukturálatlan fájl összekötő kódolást ehhez is használhatja. A logikai alkalmazásban létrehozott elérheti az XML tartalom számos különböző forrásokból, beleértve a HTTP-kérés eseménykód, egy másik alkalmazásból, vagy akár a sok [összekötők](../connectors/apis-list.md)közül. Logika-alkalmazásokkal kapcsolatos további tudnivalókért lásd: a(./app-service-logic-what-are-logic-apps.md "További tudnivalók a logika alkalmazások") [logika alkalmazások dokumentáció].  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Az összekötő kódolás strukturálatlan fájl létrehozása

Kövesse ezeket a lépéseket követve kódolást összekötőre, az logika alkalmazást a strukturálatlan fájl.

1. Logika alkalmazás- és [Összekapcsoláshoz integrációs fiókjára](./app-service-logic-enterprise-integration-accounts.md "ismerje meg, hogy egy logikai alkalmazásba integrációs fiók, amelyre a hivatkozás")létrehozása Ehhez a fiókhoz tartalmazza a használni kívánt XML-adatok kódolását sémában.  
2. **Kérés – Ha a HTTP-kérés** az eseményindító hozzáadása az logika alkalmazást.  
![Jelölje ki az eseményindító képernyőképe](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Adja hozzá a strukturálatlan fájl kódolása a művelet, az alábbi képlettel történik:

    egy. Jelölje ki a **plusz** jelre.

    b. Jelölje ki a **művelet hozzáadása** hivatkozásra (jelenik meg, miután kijelölte a pluszjelre).

    c billentyűkombinációt. A Keresés mezőbe írja be *strukturálatlan* szűréséhez használni kívánt egy a műveletek.

    d. Válassza a **Strukturálatlan fájl kódolást** lehetőséget a listából.   
![Képernyőkép a strukturálatlan fájl kódolást beállítás](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. **Egyszerű, strukturálatlan fájl kódolást** párbeszédpanelen jelölje ki a **tartalom** szövegdobozt.  
![Képernyőkép: tartalom szövegdoboz](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Jelölje be a szervezet címkét, amelyet szeretne kódolni a tartalmat. A szervezet címke fog feltöltése a tartalom mezőben.     
![Képernyőkép a szervezet címke](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Jelölje ki a **Séma neve** legördülő listában, és válassza a a séma kódolása a bemeneti tartalom használni kívánt.    
![Képernyőkép a séma neve listában](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Mentse a munkáját.   
![Képernyőkép: mentés ikon](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Ezen a ponton befejezte a strukturálatlan fájlhoz kódolási összekötő beállítását. Egy valós alkalmazásban előfordulhat, hogy tárolni szeretné a kódolt adatok a vállalati verzió alkalmazásban, például a Salesforce. Vagy elküldheti, hogy egy kereskedési kódolt adatok partner. Egyszerűen hozzáadhatja a művelet küldése kódolási végrehajtani a kimenet Salesforce, illetve a kereskedelmi partner, a többi összekötők megadott közül bármelyik használatával.

Most már tesztelheti az összekötő kérelmének végez a HTTP-végpontot, és többek között az XML-struktúrából a összehívás törzsében.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Hogyan hozhat létre az összekötő dekódolását strukturálatlan fájlhoz

>[AZURE.NOTE] Ezek a lépések elvégzéséhez integrációs fiókba már feltöltött sémafájl van szükség.

1. **Kérés – Ha a HTTP-kérés** az eseményindító hozzáadása az logika alkalmazást.  
![Jelölje ki az eseményindító képernyőképe](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Adja hozzá a strukturálatlan fájl dekódolását a művelet, az alábbi képlettel történik:

    egy. Jelölje ki a **plusz** jelre.

    b. Jelölje ki a **művelet hozzáadása** hivatkozásra (jelenik meg, miután kijelölte a pluszjelre).

    c billentyűkombinációt. A Keresés mezőbe írja be *strukturálatlan* szűréséhez használni kívánt egy a műveletek.

    d. Válassza a **Strukturálatlan fájl dekódolását** lehetőséget a listából.   
![Képernyőkép a strukturálatlan fájl dekódolását beállítás](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Jelölje ki **a tartalomvezérlőt** . Ez a listája, mint a tartalom segítségével dekódolását korábbi lépéseket tartalmát hoz létre. Figyelje meg, hogy a *szervezet* a bejövő HTTP-kérés használandó a tartalomként dekódolását érhető el. A tartalom közvetlenül a **tartalom** vezérlőelemre dekódolását is adhat meg.     
- Jelölje ki a *szövegtörzs* címkét. Figyelje meg, a szervezet címke ettől **a tartalomvezérlő** .
- Kattintson a nevére, amelyet a tartalom dekódolását használatával séma. Az alábbi képernyőképen látható, hogy *OrderFile* a kijelölt séma nevét. A séma neve volna rendben feltöltődnek a integrációs figyelembe korábban.

 ![Képernyőkép a strukturálatlan fájl dekódolását párbeszédpanel](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Mentse a munkáját.  
![Képernyőkép: mentés ikon](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Ezen a ponton végzett állítsa be az összekötő dekódolását strukturálatlan fájlhoz. Egy teljesítménynövekedés alkalmazásban előfordulhat, hogy tárolni szeretné a dekódolt adat Salesforce például a vállalati verzió alkalmazásban. Egyszerűen felvehet egy művelet Salesforce a dekódolási művelettel kimenetként.

Az összekötő most tesztelje kérelmének végez a HTTP-végpontot, beleértve az XML-struktúrából a összehívás törzsében dekódolását szeretne.  

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomagot").  
