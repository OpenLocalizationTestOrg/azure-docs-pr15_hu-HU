<properties 
    pageTitle="Tudnivalók a nagyvállalati integrációs csomag dekódolását AS2 üzenet Connctor |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
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

# <a name="get-started-with-decode-as2-message"></a>Első lépések a dekódolását AS2 üzenet

Csatlakozás AS2 üzenet dekódolását biztonság és megbízhatóság létrehozása közben üzenetek továbbításához. A digitális aláírás visszafejtés és nyugták üzenet törlés értesítések (MDN) keresztül biztosít.

## <a name="create-the-connection"></a>A kapcsolat létrehozása

### <a name="prerequisites"></a>Előfeltételek

* Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre

* Integráció fiók dekódolását AS2 üzenet összekötő használatához szükséges. Nézze meg részletek [Integrációs fiókkal](./app-service-logic-enterprise-integration-create-integration-account.md), a [partnerek](./app-service-logic-enterprise-integration-partners.md) és a [AS2 szerződés](./app-service-logic-enterprise-integration-as2.md) létrehozása

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Dekódolását AS2 üzenetet az alábbi lépésekkel csatlakozhat:

1. [Egy logikai-alkalmazás létrehozása](./app-service-logic-create-a-logic-app.md) egy példát tartalmaz.

2. Ez az összekötő nincsenek olyan indítók. Más indítók segítségével a logika alkalmazást, például az eseményindító kérelem indítása.  Az alkalmazás logika tervezőben eseményindító adja, majd adja hozzá a művelet.  Jelölje be a Microsoft megjelenítése a felügyelt API-hoz a legördülő listára, és írja be a keresőmezőbe a "AS2".  Jelölje ki a AS2 – dekódolását AS2 üzenet

    ![Keresés AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Ha még nem korábban létrehozott minden integrációs fiók-kapcsolatot, megjelenik egy kérdés a kapcsolat adatai

    ![Integráció a kapcsolat létrehozása](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Adja meg a integrációs fiók adatait.  Szükség-e egy csillag tulajdonságai

  	| A tulajdonság   | Részletek |
  	| --------   | ------- |
  	| A kapcsolat neve *    | Adjon meg egy tetszőleges nevet a kapcsolat |
  	| Integráció fiókot * | Írja be a integrációs fiók nevére. Biztosíthatja, hogy a integráció és a logika alkalmazás Azure ugyanazon a helyen |

    Miután elkészült, a kapcsolat adatai keresse meg a következőhöz hasonló

    ![integráció a kapcsolat](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Jelölje ki a **létrehozása**
    
6. Értesítés a kapcsolat létrejött.  Ezután folytassa a más a logika alkalmazásban

    ![integráció a kapcsolat létrehozása](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Jelölje ki a szervezet és a fejlécek kérelem kimeneti értékeket a

    ![Adja meg a kötelező mezői](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>A AS2 dekódolását az alábbi műveleteket végzi

* A folyamatok AS2/HTTP-fejlécek
* Ellenőrzi az aláírást (Ha be van állítva)
* Az üzenetek visszafejti a (Ha be van állítva)
* Az üzenet kibontja az (Ha be van állítva)
* Az eredeti kimenő üzenet a beérkezett MDN összehangolása
* Frissítések és a nem megtagadás adatbázisrekordok hibához
* Írások rekordjainak AS2 jelentése
* A kimenet tartalom tartalma kódolt base64
* Meghatározza, egy MDN szükség, és hogy a MDN kell szinkronizált vagy aszinkron konfigurációjától függően a AS2 szerződés
* Létrehoz egy szinkron vagy aszinkron MDN (szerződés konfigurációk alapján)
* A MDN a korrelációs tokenek és a tulajdonságok beállítása

##<a name="try-it-for-yourself"></a>Próbálja ki a saját maga számára

Miért nem tegyen egy próbát. Kattintson [ide](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) a saját funkcióival logika alkalmazások AS2 teljesen műveleti logika alkalmazások terjesztése 

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag") 