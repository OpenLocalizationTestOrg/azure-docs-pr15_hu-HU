<properties 
   pageTitle="Hozzon létre egy kereskedelmi Partner szerződés Azure App szolgáltatásban |} Microsoft Azure" 
   description="Partner rendelkezést kereskedési létrehozása" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>A kereskedelmi Partner szerződés létrehozása   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Kereskedelmi partnerek a B2B részt vevő személyek kommunikáció (vállalati verzió). Ha két partnerek kapcsolat létrehozásához, ezt nevezzük olyan *megállapodást*. A szerződés definiált alapul a kommunikációt a két partnerek kíván elérése és protokoll vagy adott átviteli. A különböző B2B protokollok és Azure alkalmazás szolgáltatás által támogatott átvitelek tartalmazza:

- AS2 (alkalmazási terület utasítás 2)
- EDIFACT (ENSZ elektronikus adatcsere felügyeleti, kereskedelmi és átviteli (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>B2B eseteket támogató BizTalk API-alkalmazások
A következő API-alkalmazások használata a sokoldalú, magától értetődő használhatóság az Azure-portálon e ezekre a lehetőségekre engedélyezése:


## <a name="biztalk-trading-partner-management-tpm"></a>Partnerek kezelése (TPM) kereskedési BizTalk
- Létrehozásának és kezelésének partnerek, profilok és identitások
- Tárterület és szerkesztése a sémák kezelése
- Tárolása és kezelése a tanúsítványok (használt AS2 protocol)
- Létrehozásának és kezelésének AS2 rendelkezést
- Létrehozásának és kezelésének EDIFACT rendelkezést (ideértendők a Küldés oldalán kötegelés)
- Létrehozásának és kezelésének X12 rendelkezést (ideértendők a Küldés oldalán kötegelés)

![][1]


## <a name="as2-connector"></a>AS2 összekötő
- A kapcsolódó TPM API alkalmazás példány definiált AS2 rendelkezést végrehajtása
- AS2 hibaelhárítási feldolgozás és nyomon követés információkat jelenít meg


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- A kapcsolódó TPM API alkalmazás példány definiált EDIFACT rendelkezést végrehajtása
- EDIFACT hibaelhárítási feldolgozás és nyomon követés információkat jelenít meg
- Állapot irányításának kötegekben (indítási és leállítási) tartalmaz, a kapcsolódó TPM API alkalmazás példány EDIFACT OK meghatározott


## <a name="biztalk-x12"></a>BizTalk X12
- X12 végrehajtja a kapcsolódó TPM API alkalmazás példány meghatározott rendelkezést 
- Feldolgozási és nyomon követés hibaelhárítási információkat X12 felületek
- Állapot irányításának kötegekben (indítási és leállítási) tartalmaz, X12 megadottak a kapcsolódó TPM API alkalmazás példány OK

Korábban megadott a AS2 X 12 és az API-alkalmazások EDIFACT vártnak megkövetelése a TPM API-at függvény.


## <a name="getting-started"></a>Első lépések
Kereskedelmi partner rendelkezést létrehozása:

1. Hozzon létre egy példánya az **BizTalk kereskedési Partner Management** összekötő. Ehhez a függvény üres SQL-adatbázishoz. Mielőtt kezdési ügyeljen arra, hogy az üres adatbázis elérhető, és használatra kész.
2. A sémák és igény szerint a rendelkezést tanúsítványok feltöltése. Ehhez a TPM példány létrehozott, és be "a sémák és/vagy a"Tanúsítványok"rész verziószámú böngészés
3. Tallózással keresse meg a létrehozott TPM példány és a **partnerek** részére történő lépés
4. Hozzon létre a kívánt partnereket. Is szükség szerint a profil szerkesztése, és adja hozzá a szükséges identitások
5. Most már segítségével **rendelkezést** rész rendelkezést. Amikor létrehoz egy szerződés, ki kell választania használt Protocol (protokoll). A hátralévő beállítási lehetőségek a kijelölt protokoll alapulnak.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
