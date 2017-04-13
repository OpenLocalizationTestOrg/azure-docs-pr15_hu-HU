<properties
   pageTitle="Azure függvények használata összefüggés-alkalmazások |} Microsoft Azure"
   description="Megtudhatja, hogy miként logika alkalmazások Azure függvények használata"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Azure függvényeivel logika alkalmazással

Egyéni kódrészletek a és C# node.js Azure függvények használatával egy logikai appon belül futtathatja.  [Azure függvények](../azure-functions/functions-overview.md) felajánlja a különféle számítások kiszolgáló ingyenes a Microsoft Azure-ban. Ezek a hasznos, ha az alábbi feladatok elvégzésére:

* Speciális formázás vagy számítási mezők összefüggés-alkalmazásból
* A munkafolyamaton belül a számítások végzése
* Logika alkalmazások C# vagy node.js által támogatott funkciók működését bővítése

## <a name="create-a-function-for-logic-apps"></a>Függvény-logika alkalmazások létrehozása

Azt javasoljuk, hogy hoz létre egy új függvény az Azure függvények portálon az **Általános Webhook - csomópont** vagy **Általános Webhook - C#** sablonok használatával. Ez automatikus-kitölt egy sablont, amely elfogadja `application/json` logika alkalmazásból.  Ha bejelöli a **Integrate** lap Azure függvények legyen **mód** lista **Webhook** és az **Általános JSON** **Webhook típusát** .  Funkciók, amelyek a sablonok automatikus felderítése és szerepel a logika alkalmazások Tervező területen **Azure függvény a tartományban lévő.**

Webhook függvények fogadnia a felkérést, és a via módszer átadásakor egy `data` változó. A tartalom tulajdonságainak elérhet pont jelölés használatával, például `data.foo`.  Ha például egy egyszerű JavaScript függvény, amely egy DateTime típusú érték konvertálja a dátumot karakterlánc néz ki a következő példa:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Azure függvényeket logika alkalmazásból

A Tervező Ha kattintson a **Műveletek** menüben közül választhat **a régió az Azure függvények**.  Ez a listák a tárolók az előfizetésben, és lehetővé teszi, hogy a válassza ki a hívni kívánt függvényt.  

Miután kiválasztotta a függvényt, a program kéri a adja meg a bemeneti tartalom-objektumot. Az üzenet, amely a logika alkalmazás küld a függvényt, és egy JSON-objektum kell lennie. Például ha át a Salesforce-eseményindító az **Utolsó módosítás** dátuma, a függvény tartalom előfordulhat, hogy néznek ki:

![Utolsó modfied dátuma][1]

## <a name="trigger-logic-apps-from-a-function"></a>Eseményindító logika alkalmazások függvény

Hogy az is lehetséges, szeretne elindítani egy függvényen belül egy összefüggés-alkalmazást.  Ehhez egyszerűen kézi az eseményindító hozzon létre egy logikai alkalmazást. További információ című [logika hívható végpontok szerint](app-service-logic-http-endpoint.md).  Ezután a függvényen belül a kézi eseményindító URL-címét a tartalom, amely el szeretné küldeni a logika alkalmazásba a HTTP POST készítése.

### <a name="create-a-function-from-the-designer"></a>A Tervező függvény létrehozása

Egy node.js webhook függvényt, a belül a Tervező is létrehozhat. Első lépésként válassza ki **a régióban Azure függvények** , és válassza a tároló a függvény.  Ha még nincs telepítve a tároló, meg kell hozzon létre egyet az [Azure függvények portálon](https://functions.azure.com/signin). Válassza **Az új létrehozása**.  

A sablon alapján számítja ki a kívánt adatokat szeretne létrehozni, adja meg a helyi objektum, amelyet használni szeretne átadásakor függvény. A JSON-objektum kell lennie. Ha például az FTP művelet adja meg a fájlt a tartalom, ha a helyi tartalom fog kinézni:

![Helyi tartalom][2]

>[AZURE.NOTE] Az objektum nem lett leadott karakterláncként, mert a tartalom hozzáadódik közvetlenül a JSON-tartalom. Azonban program hibát, ha nem a JSON jogkivonat (Ez azt jelenti, hogy egy karakterláncot vagy egy objektum/tömb JSON). Karakterláncként leadott azt, hogy egyszerűen hozzáadhatja a árajánlatok ebben a cikkben az első ábrán látható módon.

A Tervező akkor szövegközi hozhat létre függvény sablonból hoz létre. Változók előre a környezetben, amelyet használni szeretne a függvényt átadásakor alapján jönnek létre.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
