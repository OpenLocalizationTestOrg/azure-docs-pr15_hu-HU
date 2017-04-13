<properties
   pageTitle="Log Analytics riasztási webhook minta"
   description="A műveletek futtathatja a kérdésre adott választ egy napló Analytics-értesítés egyik egy *webhook*, amely lehetővé teszi, hogy egy külső folyamat révén egyetlen HTTP kérelem indítása. Példa egy webhook művelet létrehozása a napló Analytics figyelmeztetést tartalékidő használatával az alábbiakban ismertetjük."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>A napló Analytics-értesítések Webhooks

A műveletek futtathatja a válasz, a [napló Analytics értesítés](log-analytics-alerts.md) egyik egy *webhook*, amely lehetővé teszi, hogy egy külső folyamat révén egyetlen HTTP kérelem indítása.  Részletek az értesítések és az értesítés [napló Analytics](log-analytics-alerts.md) webhooks olvashat

Ebben a cikkben módszeren végigvezetjük példa egy webhook művelet létrehozása a napló Analytics figyelmeztetést tartalékidő, amely üzenetküldési szolgáltatás használatával.

>[AZURE.NOTE] Ez a példa befejezéséhez tartalékidő fiókkal kell rendelkeznie.  Iratkozzon fel a [slack.com](http://slack.com)on ingyenes fiókot.

## <a name="step-1---enable-webhooks-in-slack"></a>Lépés: 1 – a tartalékidő engedélyezése webhooks
2.  Jelentkezzen be a [slack.com](http://slack.com)tartalékidőt.
3.  Válasszon egy csatornát, a bal oldali ablaktáblában a **csatornák** szakaszában.  A csatorna, amely az üzenetet küld az.  Az alapértelmezett csatornák, például **Általános** vagy **véletlen**közül választhat.  Egy gyártási esetben valószínűleg külön csatorna például **criticalservicealerts**hozna létre. <br>

    ![A csatornák tartalékidő](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Kattintson **az alkalmazás hozzáadása vagy egyéni integráció** a Alkalmazástárához megnyitásához.
3.  *Webhooks* írja be a keresőmezőbe, és válassza a **Bejövő WebHooks**. <br>

    ![A csatornák tartalékidő](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  A csoport neve mellett kattintson a **telepítés** gombra.
5.  Kattintson a **konfiguráció hozzáadása**gombra.
6.  Jelölje ki a a csatornát, amely a szeretné, és válassza a **bejövő WebHooks hozzáadása integrációs**ebben a példában használatára.  
6. **Webhook URL-cím**másolása.  Meg fog kell beillesztése ezt a beállítást. <br>

    ![A csatornák tartalékidő](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Lépés: 2 - napló Analytics riasztási szabály létrehozása
1.  [Hozzon létre egy szabályt](log-analytics-alerts.md) , amelyen a következő beállításokat.
    - Lekérdezés:```    Type=Event EventLevelName=error ```
    - Jelölje be az Ez az értesítés minden: 5 percig tart
    - A találatok száma: nagyobb, mint 10
    - A időkeret fölé: 60 perc
    - Válassza az **Igen gombot** **Webhook** és **nem** a további lehetőségeket.
7. Illessze be a tartalékidő URL-CÍMÉT a **Webhook URL-cím** mezőbe.
8. Válassza a lehetőséget, amelyet fel szeretne **venni egy egyéni JSON tartalom**.
9. Formázott *szöveg*nevű paraméterrel JSON hasznos tartalékidő vár.  Ez az a szöveg, amelyek akkor jelennek meg az üzenetben hoz létre.  A riasztási paramétereknek közül is használhatja a *#* szimbólum, például a következő példának megfelelően.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Példa JSON tartalom](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Mentheti a szabályt a **Mentés** gombra.

10. Várja meg a megfelelő időben lehet létrehozni és majd jelölje be a tartalékidő olyan üzenet, amely a következőhöz hasonló lesz riasztás.

    ![Példa webhook a tartalékidő](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Speciális webhook tartalom a tartalékidő

Testre szabhatja a bejövő üzenetek tartalékidő az öröklődést. További tudnivalókért lásd: a [Bejövő Webhooks](https://api.slack.com/incoming-webhooks) a tartalékidő webhelyen. Az alábbiakban hasznos összetettebb formázással multimédiás üzenet létrehozása:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Ez egy üzenetet szeretne készítése tartalékidő az alábbihoz hasonló.

![Példa üzenet tartalékidő](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Összefoglalás

A riasztási szabály helyen, és azt szeretné, hogy minden alkalommal teljesül a feltétel tartalékidő küldött üzenet.  

Ez a példa csak egy olyan művelet, amelyet jelzést válaszul hozhat létre.  Létrehozhat egy webhook műveletet, amely egy másik külső szolgáltatás meghívja, egy runbook műveletet szeretne indítani egy runbook Azure automatizálási vagy a levelek küldése saját magának vagy a többi e-mail művelet.   

## <a name="next-steps"></a>Következő lépések

- Megismerheti a [napló Analytics értesítések](log-analytics-alerts.md) , beleértve az egyéb műveletek kapcsolatos.
- [Az Azure automatizálás létrehozása runbooks](../automation/automation-webhooks.md) , amely egy webhook meghívhatók.
