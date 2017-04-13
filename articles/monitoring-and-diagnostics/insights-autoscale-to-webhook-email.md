<properties
    pageTitle="Automatikus méretezéssel műveletek segítségével küldje el e-mailek és webhook értesítéseket. | Microsoft Azure"
    description="Megtudhatja, hogy miként Automatikus méretezéssel műveletek segítségével hívja fel a webes URL-címek és Azure Monitor értesítő e-mailek küldése. "
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Automatikus méretezéssel műveletek használata elküldése e-mailek és webhook értesítések Azure Monitor

Ebből a cikkből megtudhatja, hogyan beállítása eseményindítók, hogy hívja fel az adott webhely URL-címeket, vagy küldése e-mailek automatikus méretezéssel műveletek Azure-ban alapján.  

## <a name="webhooks"></a>Webhooks
Webhooks lehetővé teszi az Azure értesítések átirányítása az egyéb rendszerek utáni feldolgozás vagy egyéni értesítéseket. Például az értesítésre továbbítás kezelje a bejövő webes kérelmet küldhet az SMS-hibák naplója, értesítse a Csevegés segítségével vagy üzenetküldés csapatnak szolgáltatások szolgáltatások stb. A webhook URI érvényes HTTP vagy HTTPS-végpont kell lennie.

## <a name="email"></a>E-mailben
E-mailek küldhetők bármely érvényes e-mail címet. A rendszergazdák és az előfizetés, amelyen a szabály fut, további rendszergazdák is értesítést kap.


## <a name="cloud-services-and-web-apps"></a>Felhőszolgáltatások és a Web Apps alkalmazások
Akkor is kiválaszthatják az Azure portálról az a Felhőszolgáltatások és a kiszolgálófarm (a Web Apps alkalmazások).

- Válassza ki a **skála** mérőszám.

![az objektum méretarányát](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtuális gép skála beállítása
Az újabb virtuális gépeken futó az erőforrás-kezelő (virtuális gép skála beállítása) létrehozott beállíthatja a REST API-t, erőforrás-kezelő sablonokat, PowerShell és CLI. A portál illesztő egyelőre nem érhető el.
A REST API-val, vagy az erőforrás-kezelő sablon használata esetén tartalmazza a következő lehetőségeket az értesítések elemet.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|A mező                              |Kötelező? |Leírás|
|---                                |---        |---|
|művelet                          |igen        |Az értéknek "Skála" kell lennie.|
|sendToSubscriptionAdministrator    |igen        |értéke "igaz" vagy "false értéket" kell lennie.|
|sendToSubscriptionCoAdministrators |igen        |értéke "igaz" vagy "false értéket" kell lennie.|
|customEmails                       |igen        |értéke null [] vagy e-mailek karakterlánc tömbje lehet.|
|webhooks                           |igen        |az érték üres, vagy érvénytelen Uri lehet|
|serviceUri                         |igen        |egy érvényes https Uri|
|Tulajdonságok                         |igen        |érték üres {} kell lennie, vagy kulcs – érték párokká is tartalmazhat.|


## <a name="authentication-in-webhooks"></a>Webhooks-hitelesítés
Van két hitelesítési URI űrlap:

1. Jogkivonat-alap hitelesítés, a Mentés a webhook URI lekérdezési paraméterként jogkivonat azonosítóval. Ha például https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Alapszintű hitelesítés, amikor használni egy felhasználói Azonosítóját és jelszavát. Ha példáulhttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatikus méretezéssel értesítés webhook tartalom séma
Ha az Automatikus méretezéssel értesítés jön létre, a következő metaadatok szerepel a webhook tartalom:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|A mező  |Kötelező?|    Leírás|
|---|---|---|
|állapot |igen    |Az állapot, amely azt jelzi, hogy Automatikus méretezéssel művelet lett létrehozva|
|művelet| igen |Példányok növekedését "Skála meg" legyen és példányok csökkenése lesz az "Skála a"|
|környezet|   igen |Az Automatikus méretezéssel művelet környezetben|
|időbélyeg| igen |Az Automatikus méretezéssel művelet elindításakor időbélyeg|
|azonosító |igen|   Az Automatikus méretezéssel beállítása erőforrás-kezelő azonosítója|
|név   |igen|   Az Automatikus méretezéssel beállítás neve|
|Részletek|   igen |A műveletet az Automatikus méretezéssel szolgáltatás beállításikon és a példányok száma változása ismertetése|
|subscriptionId|    igen |Előfizetés azonosítója, a cél erőforrás méretezett alatt|
|resourceGroupName| igen|    A target erőforrás méretezett alatt álló erőforráscsoport neve|
|Erőforrásnév   |igen|   Az éppen méretezett cél erőforrás neve|
|Erőforrástípus   |igen|   A három támogatott érték: "microsoft.classiccompute/domainnames/slots/roles" - szerepkörök, a "microsoft.compute/virtualmachinescalesets" - virtuális gép skála halmazok menügombra, és a "Microsoft.Web/serverfarms" – Web App felhőalapú szolgáltatás|
|resourceId |igen|A target erőforrás éppen méretezett erőforrás-kezelő azonosítója|
|portalLink |igen    |Az összefoglaló lapra, a cél erőforrás Azure portal hivatkozása|
|oldCapacity|   igen |Az aktuális (régi) példányok száma amikor Automatikus méretezéssel skála PERT|
|newCapacity|   igen |Az új példányok száma, hogy az Automatikus méretezéssel, amelyekkel méretarányos az erőforrás|
|Tulajdonságok|    nem| Nem kötelező. < Kulcs értékét > beállítása pár (például szótár < karakterlánc, karakterlánc >). A Tulajdonságok mező kitöltése nem kötelező. Egyéni felhasználói felület vagy összefüggés-alapú alkalmazás munkafolyamat kulcsok és értékek, amelyek a tartalom is átadandó adhat meg. Egy alternatív egyéni tulajdonságok vissza átadni a kimenő hívás webhook módja a webhook (a lekérdezés paraméterei) magát URI használata|
