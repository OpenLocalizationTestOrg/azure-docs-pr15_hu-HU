### <a name="prerequisites"></a>Előfeltételek

A [Szolgáltatás Bus](https://azure.microsoft.com/services/service-bus/) -fiókkal kell rendelkezniük.  

Azure Service Bus fiókja egy logikai alkalmazásban használata előtt engedélyeznie kell a service bus fiókhoz való csatlakozáshoz a logika alkalmazás. Szerencsére ezt megteheti a könnyen a logika alkalmazásból, az Azure portálon.  

A szolgáltatás Bus fiókhoz való csatlakozáshoz az összefüggés-alkalmazás engedélyezése a lépések a következők:  

1. A logikai alkalmazás tervező szolgáltatás Bus, kapcsolat létrehozásához válassza a **Microsoft megjelenítése felügyelt API-hoz** a legördülő listában. A Keresés mezőbe írja be az **szolgáltatás bus** . Jelölje ki az eseményindító vagy a használni kívánt művelet.  
    ![Szolgáltatás Bus kapcsolat kép 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Minden kapcsolat szolgáltatás Bus előtt eddig nem hozott létre, ha kérni fogja a szolgáltatás Bus hitelesítő adatait. Ezek a hitelesítő adatok használatával engedélyezheti a logika alkalmazás való kapcsolódáshoz és elérni a szolgáltatás Bus fiók adatait. A szolgáltatás Bus összekötő a szolgáltatás Bus névtér van szüksége a kapcsolati karakterlánc. Azt is szükséges engedélyek **kezelése** . Ha a kapcsolati karakterlánc a névtér vagy egy adott személy annak meghatározásához, hogy jó módszer a `EntityPath` paraméter. Ha igen, még nem a megfelelő kapcsolati karakterlánc összefüggés-alkalmazásokban.  
    ![Szolgáltatás Bus kapcsolat-karakterlánc](./media/connectors-create-api-servicebus/connectionstring.png)

1. Után kapott a kapcsolati karakterláncot, a névtér, használhatja az API-kapcsolat összefüggés-alkalmazásokban.  
    ![Szolgáltatás Bus kapcsolat kép 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logika alkalmazásban.  
    ![Szolgáltatás Bus kapcsolat kép 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
