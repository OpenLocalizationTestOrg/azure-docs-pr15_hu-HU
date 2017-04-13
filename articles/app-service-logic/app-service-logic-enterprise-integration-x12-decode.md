<properties 
    pageTitle="Tudnivalók a nagyvállalati integrációs csomag dekódolását X12 Connctor üzenet |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
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

# <a name="get-started-with-decode-x12-message"></a>Első lépések a dekódolási X12 üzenet

Ellenőrzi, szerkesztése és az adott partner tulajdonságainak, hoz létre az egyes tranzakció XML-dokumentum és visszaigazoló feldolgozott tranzakció hoz létre.

## <a name="create-the-connection"></a>A kapcsolat létrehozása

### <a name="prerequisites"></a>Előfeltételek

* Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre

* Integráció fiók dekódolási X12 üzenet összekötő használatához szükséges. Részletek hogyan hozhat létre, [Integráció a fiókot](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerek](./app-service-logic-enterprise-integration-partners.md) és [X12 szerződés](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Dekódolási X12 üzenetet az alábbi lépésekkel csatlakozhat:

1. [Egy logikai-alkalmazás létrehozása](./app-service-logic-create-a-logic-app.md) tartalmaz egy példa

2. Ez az összekötő nincsenek olyan indítók. Más indítók segítségével a logika alkalmazást, például az eseményindító kérelem indítása.  Az alkalmazás logika tervezőben eseményindító adja, majd adja hozzá a művelet.  Jelölje ki a Microsoft megjelenítése a felügyelt API-hoz a legördülő listára, és írja be "x12" a Keresés mezőbe.  Jelölje ki a X12 – X12 dekódolását üzenet

    ![x12 keresése](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Ha még nem korábban létrehozott minden integrációs fiók-kapcsolatot, megjelenik egy kérdés a kapcsolat adatai

    ![integráció fiók kapcsolata](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Adja meg a integrációs fiók adatait.  Szükség-e egy csillag tulajdonságai

  	| A tulajdonság | Részletek |
  	| -------- | ------- |
  	| A kapcsolat neve * | Adjon meg egy tetszőleges nevet a kapcsolat |
  	| Integráció fiókot * | Írja be a integrációs fiók nevére. Biztosíthatja, hogy a integráció és a logika alkalmazás Azure ugyanazon a helyen |

    Miután elkészült, a kapcsolat adatai keresse meg a következőhöz hasonló
    
    ![integráció a fiók kapcsolata létrehozott](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Jelölje ki a **létrehozása**
    
6. Értesítés a kapcsolat létrejött.

    ![kapcsolat fiókadatokat integrációja](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. SELECT X12 sík dekódolását fájl üzenet

    ![Adja meg a kötelező mezői](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 dekódolását hatással van a következő

* A boríték elleni kereskedelmi partner szerződés ellenőrizte
* Létrehoz egy XML-dokumentum minden tranzakció halmazára.
* Ellenőrzi, szerkesztése és a partner-specifikus tulajdonságai
    * Szerkezeti érvényességi szerkesztése és a bővített séma érvényesítése
    * Adatcsere boríték szerkezetének ellenőrzése.
    * A borítékok, a vezérlő sémának séma ellenőrzése.
    * Séma érvényesítése az adatok tranzakció által beállított elemek az üzenet séma alapján.
    * Szerkesztése érvényességi végrehajtott tranzakciók által beállított adatelemeket 
* Ellenőrzi, hogy a adatcsere-, csoport- és tranzakció beállítása vezérlő számok ne legyenek-e ismétlődő elemeket
    * Annak vizsgálata, hogy korábban kapott csomópontokat is ellen interchange ellenőrző szám.
    * Annak vizsgálata, hogy más adatcsere csoport vezérlőelem számoknak csoport ellenőrző szám.
    * Ellenőrzi, hogy a tranzakció ellenőrző szám beállítása a csoport többi tranzakció beállítása vezérlő számmal összevetve.
* A teljes interchange XML alakít át. 
    * Osztott Interchange mint tranzakció beállítása – felfüggesztheti tranzakció készletek hiba: elemzi beállítása egy külön XML-dokumentumokba interchange a tranzakciók. Ha egy vagy több tranzakció állít be a interchange sikertelen az érvényesítés, X12 dekódolási felfüggeszti a csak adott tranzakció készleteket.
    * Felosztott Interchange mint tranzakció beállítása – felfüggesztheti interchange hiba: elemzi beállítása egy külön XML-dokumentumokba interchange a tranzakciók.  Ha egy vagy több tranzakció állít be a interchange sikertelen az érvényesítés, X12 dekódolási felfüggeszti a teljes interchange.
    * Adatcsere megőrzése - tranzakció készletek felfüggesztése hiba: a teljes kötegelt adatcserét XML-dokumentum létrehozása. X12 dekódolási felfüggeszti a csak adott érvényesítése nem sikerül tranzakció készleteket, miközben továbbra minden más tranzakció feldolgozása állítja be
    * Adatcsere megőrzése – felfüggesztheti interchange hiba: a teljes kötegelt adatcserét XML-dokumentum létrehozása. Ha egy vagy több tranzakció állít be a interchange sikertelen az érvényesítés, X12 dekódolási felfüggeszti a teljes interchange 
* A műszaki, illetve funkcionális nyugta hoz létre, (Ha be van állítva).
    * A műszaki nyugta eredményeként élőfej érvényességi hoz létre. A műszaki nyugta jelentések tartozó egy interchange fejléc és a cím címzett feldolgozása állapotát.
    * Egy funkcionális nyugta eredményeként szervezet érvényességi hoz létre. A funkcionális nyugta-jelentések egyes a kapott dokumentum feldolgozása közben fellépő hiba

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag") 


