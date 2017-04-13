## <a name="connect-to-outlookcom"></a>Csatlakozás az Outlook.com-hoz

### <a name="prerequisites"></a>Előfeltételek
- Az Outlook.com-fiókba

Az Outlook.com-fiók egy logikai alkalmazásban használata előtt engedélyeznie kell a logika alkalmazást az Outlook.com-fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon. 

Az Outlook.com-fiókhoz való csatlakozáshoz az összefüggés-alkalmazás engedélyezése a lépések a következők:

1. Minden logika alkalmazás kell indítja el az eseményindító, hogy hoz létre a logika alkalmazást, a Lekérdezéstervező megnyitásakor és listáját jeleníti meg, amely elindítja a összefüggés-alkalmazás indításakor:

  ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. A Keresés mezőbe írja be a "az outlook". Figyelje meg a listában "Az Outlook" az összes indítók lista nevére a szűrt:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Jelölje be az **Office 365 Outlook – új e-mailek**.   
  Ha még nem hozott létre bármely kapcsolatokat, az Outlook alkalmazást, fog kiírja az Outlook.com hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és hozzáférni az Outlook.com-fiók adatait:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Az Outlook a szükséges hitelesítő adatait, és jelentkezzen be:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
  Ez azt. Most már létrehozott egy kapcsolatot az Outlook. A kapcsolat érhetők el a bármely más logika alkalmazásban létrehozott való használatra.


