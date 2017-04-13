<properties 
    pageTitle="Tudnivalók a nagyvállalati integrációs csomag kódolását X12 Connctor üzenet |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="Partnerek, a nagyvállalati integrációs csomag és logika alkalmazással használata" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-encode-x12-message"></a>Első lépések a kódolása X12 üzenet

Ellenőrzi, szerkesztése és az adott partner tulajdonságainak, szerkesztése tranzakció beállítja a interchange az XML-kódolású üzeneteket alakítja, és kéri a műszaki, illetve funkcionális nyugta

## <a name="create-the-connection"></a>A kapcsolat létrehozása

### <a name="prerequisites"></a>Előfeltételek

* Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre

* Integráció fiók kódolása x12 üzenet összekötő használatához szükséges. Részletek hogyan hozhat létre, [Integráció a fiókot](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerek](./app-service-logic-enterprise-integration-partners.md) és [X12 szerződés](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Kódolása X12 üzenetet az alábbi lépésekkel csatlakozhat:

1. [Egy logikai-alkalmazás létrehozása](./app-service-logic-create-a-logic-app.md) tartalmaz egy példa

2. Ez az összekötő nincsenek olyan indítók. Más indítók segítségével a logika alkalmazást, például az eseményindító kérelem indítása.  Az alkalmazás logika tervezőben eseményindító adja, majd adja hozzá a művelet.  Jelölje ki a Microsoft megjelenítése a felügyelt API-hoz a legördülő listára, és írja be "x12" a Keresés mezőbe.  Jelölje ki bármelyik X12-X12 kódolását szerződés név szerint üzenetet vagy X12-X 12 üzenet szerint identitások kódolását.  

    ![x12 keresése](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Ha még nem korábban létrehozott minden integrációs fiók-kapcsolatot, megjelenik egy kérdés a kapcsolat adatai

    ![integráció fiók kapcsolata](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Adja meg a integrációs fiók adatait.  Szükség-e egy csillag tulajdonságai

  	| A tulajdonság | Részletek |
  	| -------- | ------- |
  	| A kapcsolat neve * | Adjon meg egy tetszőleges nevet a kapcsolat |
  	| Integráció fiókot * | Írja be a integrációs fiók nevére. Biztosíthatja, hogy a integráció és a logika alkalmazás Azure ugyanazon a helyen |

    Miután elkészült, a kapcsolat adatai keresse meg a következőhöz hasonló

    ![integráció a fiók kapcsolata létrehozott](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Jelölje ki a **létrehozása**

6. Értesítés a kapcsolat létrejött.

    ![kapcsolat fiókadatokat integrációja](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-kódolását X12 üzenet szerződés név szerint

7. Jelölje ki a X12 szerződés kódolása a legördülő lista és az xml az üzenetből.

    ![Adja meg a kötelező mezői](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-kódolását X12 üzenet szerint identitások

7.  Adja meg a küldő azonosítója, a feladó minősítő, a címzett azonosító és a címzett minősítő a X12 a szerződés feltételeit.  Jelölje ki a kódolás xml-üzenet

    ![Adja meg a kötelező mezői](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 kódolását hatással van a következő:

* Szerződés felbontása összekapcsolja a feladó és címzett helyi tulajdonságait.
* XML-kódolású üzeneteket átalakítása szerkesztése tranzakció beállítja a adatcsere szerkesztése adatcsere serializes.
* Transaction beállítása élőfej- és pótkocsi szegmensek vonatkozik
* Létrehoz egy interchange ellenőrző szám, csoport vezérlőelem szám és az minden kimenő interchange tranzakció beállítása ellenőrzési száma
* Felváltja a tartalom adatok Elválasztókkal
* Ellenőrzi, szerkesztése és a partner-specifikus tulajdonságai
    * Az üzenet séma ellen tranzakció által beállított adatok elemek séma érvényesítése
    * Szerkesztése érvényességi végrehajtott tranzakciók által beállított adatelemeket.
    * Bővített érvényességi végrehajtott tranzakciók által beállított adatelemeket
* A műszaki, illetve funkcionális nyugta kéri, (Ha be van állítva).
    * A műszaki nyugta eredményeként élőfej érvényességi hoz létre. A műszaki nyugta tartozó egy interchange fejléc és a cím címzett feldolgozása állapotának jelentések
    * Egy funkcionális nyugta eredményeként szervezet érvényességi hoz létre. A funkcionális nyugta-jelentések egyes a kapott dokumentum feldolgozása közben fellépő hiba

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag") 

