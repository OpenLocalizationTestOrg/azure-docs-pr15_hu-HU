### <a name="prerequisites"></a>Előfeltételek
- Egy Twilio fiók
- Egy igazolt Twilio telefonszámot, amely az SMS funkció képes fogadni.
- Egy, az SMS küldhet igazolt Twilio telefonszám

>[AZURE.NOTE] Ha egy Twilio próba-fiókot használ, csak elküldheti SMS telefonszámok **ellenőrizve** .  

Mielőtt használatba vehetné a Twilio fiók összefüggés-alkalmazásban, engedélyeznie kell a logika alkalmazást a Twilio fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon. 

Az a Twilio fiókhoz való csatlakozáshoz összefüggés-alkalmazás engedélyezése a lépések a következők:

1. A logika alkalmazás Tervező Twilio, kapcsolatot létrehozni, a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be a *Twilio* . Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Minden kapcsolat Twilio előtt eddig nem hozott létre, ha fogja kiírja a Twilio hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és elérni a Twilio fiók adatait:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Szüksége lesz az **Twilio fiókazonosítót** és **Twilio hozzáférési jogkivonat** irányítópultjáról Twilio, hogy jelentkezzen be a Twilio fiókkal most ragadni a következő két csatolt információk:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio és logikai hálózatok alkalmazások különböző neveket használja ezeket két adatra azonosításához. Itt látható, hogy meg kell jelölnie őket a összefüggés-alkalmazások párbeszédpanelen:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Kattintson a **kapcsolat létrehozása** gombra:  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logika alkalmazásban:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)