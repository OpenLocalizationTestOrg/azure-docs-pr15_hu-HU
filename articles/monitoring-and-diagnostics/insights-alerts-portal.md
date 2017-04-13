<properties
    pageTitle="Az Azure-szolgáltatások értesítések létrehozása az Azure portal segítségével |} Microsoft Azure"
    description="Az Azure portal segítségével indító értesítések és automatizálási megadott feltételek teljesülése esetén Azure értesítések létrehozása."
    authors="rboucher"
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
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Az Azure-szolgáltatások értesítések létrehozása az Azure portal segítségével

> [AZURE.SELECTOR]
- [Portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan állíthat be az Azure portálon Azure értesítéseket.   

Az ellenőrző mértékek vagy események, a Azure szolgáltatások alapján értesítés kaphat.

- **Metrikus értékek** – a riasztási eseményindítók, ha egy adott mérőszám értékének metszéspontja mindkét irányba hozzárendelése küszöbértéket. Ez azt jelenti, akkor mindkét eseményindítók amikor először teljesül a feltétel és majd utána mikor amely feltétel van már nem teljesül.    
- **Tevékenység naplóbeli események** - értesítés válthatnak *minden* esemény, illetve csak bizonyos számú események fordul elő.


Hajtsa végre a következő, amikor elindítja azt, hogy jelzést adhatja meg:

- a szolgáltatás-rendszergazda, és további rendszergazdák értesítő e-mailek küldése
- e-mailt küldjenek további e-mailek megadott feltételeknek.
- hívja fel a webhook
- Indítsa el az Azure runbook (csak az Azure portál) végrehajtása

Állítsa be, és riasztási szabályok használatával kapcsolatos tájékozódáshoz

- [Azure portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [parancssori kezelőfelületről](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-val](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Hozzon létre egy szabályt egy mérőszám az Azure portálján a

1. Keresse meg a [portálon](https://portal.azure.com/)figyelése érdeklik, és jelölje ki azt az erőforrás.

2. A figyelés szakasz csoportjában jelölje be a **riasztások** vagy **riasztási szabályokat** . A szöveg és ikon kissé eltérő lehet a különböző erőforrásokat.  

    ![Figyelése](./media/insights-alerts-portal/AlertRulesButton.png)


3. Válassza a **Hozzáadás értesítés** parancsot, és töltse ki a mezőket.

    ![Értesítés hozzáadása](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nevét** a értesítés szabály, és válassza a egy **leírást**, amelyet az értesítő e-mailben is láthatók.
5. Jelölje ki a kívánt figyelheti, majd válasszon egy **feltételt** és **küszöb** értéket a mérőszám **metrikus** . Az **időszak** , amely a metrikus szabály nem teljesül, a riasztási eseményindítók előtt is választotta. Így például az időszak "PT5M" használja, és a figyelmeztetés keres Processzor 80 %-nál, ha az értesítésre akkor indítja, amikor a Processzor már egységes fenti 80 %-os 5 percig tart. Miután az első eseményindító fordul elő, akkor újra akkor indítja, amikor az 5 perc alatt 80 %-os maradjon a Processzor. A Processzor mértéket 1 percenként fordul elő.   

6. Jelölje be a **e-mailek tulajdonosok...** , ha azt szeretné, hogy kell küldve a figyelmeztető akkor indul el, ha a rendszergazdák és a további rendszergazdák.

7. Ha azt szeretné, hogy értesítést kapnak, amikor akkor indul el, a figyelmeztető további e-mailekhez, felveheti a **további rendszergazdai email(s)** mezőben. Több e-mailek egymástól pontosvesszővel –*email@contoso.com;email2@contoso.com*

8. Ha azt szeretné, hogy a ennek neve a figyelmeztető akkor indul el, ha akkor **Webhook** területén érvényes URI.

9. Azure automatizálási használata esetén választhat egy Runbook futtatja, amikor akkor indul el, az értesítésre.

10. Kattintson az **OK** feladónak hozni az értesítést.   

Néhány percen belül a figyelmeztető aktív, és elindítja a korábban leírt módon.

## <a name="managing-your-alerts"></a>Az értesítések kezelése

Miután létrehozott egy figyelmeztetés, kiválaszthatja azt, és:

- A metrikus küszöbértékét és az előző napra tényleges értékeit mutató diagram megtekintése.
- Szerkesztheti vagy törölheti azt.
- **Letiltása** vagy **engedélyezése** , ha ideiglenes leállításához és önéletrajz értesítésekkel az, hogy az értesítést.



## <a name="next-steps"></a>Következő lépések

* , [Áttekintést kaphat a Azure figyelése](monitoring-overview.md) többek között a típusú adatok összegyűjtése és figyelése.
* További tudnivalók [az értesítések konfigurálása webhooks](insights-webhooks-alerts.md).
* További tudnivalók az [Azure automatizálási Runbooks](..\automation\automation-starting-a-runbook.md).
* [A diagnosztikai naplók áttekintése](monitoring-overview-of-diagnostic-logs.md) és részletes magas-gyakoriság mértékek összegyűjtheti az Ön szolgáltatása.
* Ismerkedés a [Mértékek webhelycsoport áttekintése](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
