<properties
    pageTitle="Azure virtuális gépeken futó Azure automatizálást függőlegesen méretezheti |} Microsoft Azure"
    description="Hogyan függőlegesen méretezni a virtuális gép Windows Azure automatizálást értesítések figyelése válaszul"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machines-with-azure-automation"></a>Azure virtuális gépeken futó Azure automatizálást függőlegesen méretezése

Függőleges méretezés növelésével vagy csökkentésével válaszként a terhelést a gép az erőforrások során a rendszer. Azure-ban ez lehet megvalósítani a virtuális gép méretének módosítása. Ez az alábbi esetekben segíthet

- A virtuális gép nem gyakran használja, ha a méretét, a havi költségek csökkentése érdekében kisebb méretű lefelé
- Ha a virtuális gép csúcs terhelést lát, akkor átméretezhető méretének növelése a beosztását

A Vázlat lépések végrehajtásához megegyezik alatt

1. A virtuális gépeken futó eléréséhez Azure automatizálási beállítása
2. Az előfizetés az Azure automatizálási függőleges méret runbooks importálása
3. A runbook egy webhook hozzáadása
4. A virtuális gép értesítés hozzáadása

> [AZURE.NOTE] Az első virtuális gép, is méretezett méretéről méretétől miatt korlátozott lehet a fürt méretű az aktuális virtuális gép telepítve van az elérhető miatt. A jelen cikkben használt közzétett automatizálási runbooks azt gondoskodik a ebben az esetben, és csak belül méretezni a virtuális méret párban alatt. Ez azt jelenti, hogy Standard_D1v2 virtuális gép fog váratlanul nem méretezése való Standard_G5 és az Basic_A0 arányosan.

>| Virtuális méretű pár méretezése |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>A virtuális gépeken futó eléréséhez Azure automatizálási beállítása

A legfontosabb dolog kell tennie, hozzon létre egy Azure automatizálási fiókot, amely a virtuális gép méretezni használt runbooks fog tárolni. A legutóbb az automatizálási szolgáltatásai számára a "Futtatás fiókként" funkció, amely lehetővé teszi a beállítás be a szolgáltatás egyszerű automatikusan futó a runbooks a felhasználó nevében nagyon egyszerűen jelent meg. Erről további információt az alábbi cikkben:

* [Hitelesítő Runbooks Azure Futtatás mint fiókkal](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Az előfizetés az Azure automatizálási függőleges méret runbooks importálása

A szükséges függőlegesen a a virtuális gép átméretezés runbooks már közzétett az Azure automatizálási Runbook gyűjteményben. Szüksége lesz az előfizetés importálhatja őket. Elolvashatja, hogy miként importálhat runbooks a következő témakör elolvasásával.

* [Az Azure automatizálási Runbook és modul gyűjtemények](../automation/automation-runbook-gallery.md)

Az alábbi képen látható az importált kell runbooks

![Runbooks importálása](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>A runbook egy webhook hozzáadása

Miután importálta az runbooks kell felveszi egy webhook a runbook, egy virtuális gépről értesítés indított kell azt. Egy webhook létrehozása a Runbook részleteit az itt olvasható

* [Azure automatizálási webhooks](../automation/automation-webhooks.md)

Győződjön meg arról, mielőtt bezárja a webhook párbeszédpanel, ez a következő szakaszban lesz szükség szerint másolja át a webhook.

## <a name="add-an-alert-to-your-virtual-machine"></a>A virtuális gép értesítés hozzáadása

1. Jelölje ki a virtuális gép beállításai
2. Jelölje be az "Értesítési szabályok"
3. Jelölje be a "Értesítés hozzáadása"
4. Jelölje be minden olyan esetben az értesítésre, a metrikával
5. Jelöljön ki egy feltételt, amely eleget fog okozhat a riasztást fire
6. Jelölje ki a feltétel küszöbértéket az 5. teljesítendő
7. Jelölje ki az egy időszakra eredményhalmaz, amelyen a felügyeleti szolgáltatás ellenőrzi a feltétel és a küszöbérték lépéseket 5 és 6
8. Illessze be a kimásolt az előző szakaszban webhook.

![Virtuális géphez 1 értesítés hozzáadása](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Virtuális géphez 2 értesítés hozzáadása](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)
