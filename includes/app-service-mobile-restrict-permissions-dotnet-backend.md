
Alapértelmezés szerint egy Mobile alkalmazásban kódmentes az API-khoz is hívható névtelenül. Ezután szüksége elérésének korlátozására csak a hitelesített ügyfeleknek.  

+ **Node.js kódmentes (keresztül portal)** :  
    
    A Mobile-alkalmazás **beállításait** **Egyszerűen táblázatokat** és válassza a táblázat. Az **engedélyek módosítása**gombra, jelölje be a **csak a hitelesített hozzáférés** az összes engedélyt, majd kattintson a **Mentés**gombra. 

+ **.NET kódmentes (C#)**:  

    A kiszolgáló projekt keresse meg azt a **vezérlők** > **TodoItemController.cs**. Adja hozzá a `[Authorize]` attribútum a **TodoItemController** osztály az alábbi képlettel történik. Elérésének korlátozására csak bizonyos módszerek, is alkalmazhat az attribútum csak az osztály helyett ezeket a módszereket. A kiszolgáló projekt közzé.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js kódmentes (keresztül Node.js kód)** :  
    
    A táblázat hozzáférés hitelesítést igényel, a Node.js kiszolgáló parancsfájl a következő sor hozzáadása:


        table.access = 'authenticated';

    Ha további információra kíváncsi, olvassa el [hogyan: hitelesítés megkövetelése a táblák access](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Megtudhatja, hogy miként töltheti le a quickstart útmutató kódja a project a webhelyről, lásd: [hogyan: Töltse le a Node.js kódmentes quickstart útmutató kód projekt mely számjegy használatával](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

