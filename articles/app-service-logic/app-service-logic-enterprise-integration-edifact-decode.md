<properties 
    pageTitle="Tudnivalók a nagyvállalati integrációs csomag dekódolását EDIFACT üzenet összekötő |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>Első lépések a dekódolását EDIFACT üzenet

Ellenőrzi, szerkesztése és az adott partner tulajdonságainak, hoz létre az egyes tranzakció XML-dokumentum és visszaigazoló feldolgozott tranzakció hoz létre.

## <a name="create-the-connection"></a>A kapcsolat létrehozása

### <a name="prerequisites"></a>Előfeltételek

* Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre

* Integráció fiók dekódolását EDIFACT üzenet összekötő használatához szükséges. Nézze meg részletek- [Integráció a fiókot](./app-service-logic-enterprise-integration-create-integration-account.md), a [partnerek](./app-service-logic-enterprise-integration-partners.md) és a [EDIFACT szerződés](./app-service-logic-enterprise-integration-edifact.md) létrehozása

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Dekódolását EDIFACT üzenetet az alábbi lépésekkel csatlakozhat:

1. [Egy logikai-alkalmazás létrehozása](./app-service-logic-create-a-logic-app.md) egy példát tartalmaz.

2. Ez az összekötő nincsenek olyan indítók. Más indítók segítségével a logika alkalmazást, például az eseményindító kérelem indítása.  Az alkalmazás logika tervezőben eseményindító adja, majd adja hozzá a művelet.  Jelölje be a Microsoft megjelenítése a felügyelt API-hoz a legördülő listára, és írja be a keresőmezőbe a "EDIFACT".  Jelölje ki a dekódolását EDIFACT üzenet

    ![Keresés EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Ha még nem korábban létrehozott minden integrációs fiók-kapcsolatot, megjelenik egy kérdés a kapcsolat adatai

    ![integráció fiók létrehozása](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Adja meg a integrációs fiók adatait.  Szükség-e egy csillag tulajdonságai

  	| A tulajdonság | Részletek |
  	| -------- | ------- |
  	| A kapcsolat neve * | Adjon meg egy tetszőleges nevet a kapcsolat |
  	| Integráció fiókot * | Írja be a integrációs fiók nevére. Biztosíthatja, hogy a integráció és a logika alkalmazás Azure ugyanazon a helyen |

    Miután elkészült, a kapcsolat adatai keresse meg a következőhöz hasonló

    ![Hozzon létre fiókot a integrációja](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Jelölje ki a **létrehozása**

6. Értesítés a kapcsolat létrejött

    ![kapcsolat fiókadatokat integrációja](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Jelölje ki a EDIFACT strukturálatlan fájlhoz üzenet dekódolását

    ![Adja meg a kötelező mezői](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT dekódolását jelent követő

* A szerződés megoldható egyező minősítő a feladó és címzett minősítő & azonosító azonosító
* Több csomópontokat is egy üzenetben különálló kettéválaszthatja-e.
* A boríték elleni kereskedelmi partner szerződés ellenőrizte
* Adatcsere visszafejti.
* Ellenőrzi, szerkesztése és az adott partner tulajdonságainak tartalmaz
    * Adatcsere boríték szerkezetének ellenőrzése.
    * A borítékok, a vezérlő sémának séma ellenőrzése.
    * Séma érvényesítése az adatok tranzakció által beállított elemek az üzenet séma alapján.
    * Szerkesztése érvényességi végrehajtott tranzakciók által beállított adatelemeket
* Ellenőrzi, hogy a adatcsere-, csoport- és tranzakció beállítása vezérlő számokat nem ismétlődő elemeket (Ha be van állítva) 
    * Annak vizsgálata, hogy korábban kapott csomópontokat is ellen interchange ellenőrző szám. 
    * Annak vizsgálata, hogy más adatcsere csoport vezérlőelem számoknak csoport ellenőrző szám. 
    * Ellenőrzi, hogy a tranzakció ellenőrző szám beállítása a csoport többi tranzakció beállítása vezérlő számmal összevetve.
* Létrehoz egy XML-dokumentum minden tranzakció halmazára.
* A teljes interchange XML alakít át. 
    * Osztott Interchange mint tranzakció beállítása – felfüggesztheti tranzakció készletek hiba: elemzi beállítása egy külön XML-dokumentumokba interchange a tranzakciók. Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd EDIFACT dekódolását felfüggeszti a csak adott tranzakció készleteket. 
    * Felosztott Interchange mint tranzakció beállítása – felfüggesztheti interchange hiba: elemzi beállítása egy külön XML-dokumentumokba interchange a tranzakciók.  Ha egy vagy több tranzakció állít be a interchange sikertelen érvényességi, majd EDIFACT dekódolását felfüggeszti a teljes interchange.
    * Adatcsere megőrzése - tranzakció készletek felfüggesztése hiba: a teljes kötegelt adatcserét XML-dokumentum létrehozása. EDIFACT dekódolását felfüggeszti a csak ezek érvényesítése továbbra is az összes többi tranzakció készletek folyamat során nem sikerül tranzakció beállítása
    * Adatcsere megőrzése – felfüggesztheti interchange hiba: a teljes kötegelt adatcserét XML-dokumentum létrehozása. Ha egy vagy több tranzakció állít be a interchange érvényesítése nem sikerül, majd a teljes interchange EDIFACT dekódolását felfüggeszti a 
* Hozza létre a technikai (-vezérlőelem) és/vagy funkcionális nyugta (Ha be van állítva).
    * A technikai nyugtát vagy a CONTRL ACK-jelentések a teljes fogadott interchange szintaktikai ellenőrzése eredményét.
    * Egy funkcionális nyugta nyugtázza elfogadhatja vagy elutasíthatja a kapott interchange vagy csoport

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag") 