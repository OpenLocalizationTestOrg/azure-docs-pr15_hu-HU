<properties
   pageTitle="Azure automatizálási runbooks hozzáadása a helyreállítási terv |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan Azure webhely helyreállítási most lehetővé teszi, hogy Azure automatizálási használata összetett feladatok elvégzéséhez az Azure helyreállítás során helyreállítási tervek bővítése"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure automatizálási runbooks hozzáadása a helyreállítási terv


Ebben az oktatóanyagban ismerteti, hogyan integrálja az Azure webhely helyreállítási Azure automatizálási helyreállítási tervek bővítési megadására. A helyreállítási terv is téve a virtuális gépeken futó Azure webhely helyreállítási a másodlagos felhőben való replikáció és Azure esetek való replikáció védeni visszanyerése. Is segítik annak érdekében, hogy a helyreállítási **egységes pontos**, **megismételhető**, és **az automatikus**. A virtuális gépeken futó az Azure keresztül nem működnek, ha a integrálása az Azure automatizálási bővíti a helyreállítási tervek, és runbooks, így a hatékony automatizálási tevékenységek végrehajtásához lehetőséget nyújt.

Ha meg van nem leggyakoribb kapcsolatos Azure automatizálási még, jelentkezzen be [az alábbi](https://azure.microsoft.com/services/automation/) , és töltse le a minta parancsfájlok [Itt](https://azure.microsoft.com/documentation/scripts/). További tudnivalók a [Webhely-helyreállítás Azure](https://azure.microsoft.com/services/site-recovery/) , és hogyan téve a helyreállítási csomagok használatával Azure helyreállítási olvassa el [az alábbi](https://azure.microsoft.com/blog/?p=166264).

Ebben az oktatóanyagban megnézi lesz az hogyan integrálja a Azure automatizálási runbooks helyreállítási tervek be. Azt, amely a korábban szükséges beavatkozás egyszerű feladatok automatizálása, és megtudhatja, hogy miként egy egyetlen kattintással helyreállítási művelet több lépésben helyreállítást konvertálása. Azt is, hogy hogyan háríthatja el egy egyszerű parancsfájl mentésük, ha vizsgálni.

## <a name="customize-the-recovery-plan"></a>A helyreállítási terv testreszabása

1. Az erőforrás lap a helyreállítási terv tudassa velünk kezdje azzal operning. Láthatja, hogy a helyreállítási terv van két virtuális gépeken futó helyreállítás hozzáadni. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Kattintson a Testreszabás gombra való felvételének egy runbook megkezdéséhez. Ekkor megnyílik a helyreállítási terv lap testreszabása.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Kattintson a start csoport 1 a jobb gombbal, és válassza a "hozzáadása bejegyzés művelet" hozzáadása.

4. Jelölje be az új lap parancsfájl választható.

5. Nevezze el a "Helló, világ" parancsfájl.

6. Válassza az automatizálási fiók nevét. Ez a az Azure automatizálási fiókot. Ne feledje, hogy ehhez a fiókhoz bármely Azure földrajzi lehet, de működnie kell az ugyanabban az előfizetésben, mint a webhely helyreállítási tárolóból elemre.

7. Jelöljön ki egy runbook az automatizálási fiókot. Ez az első csoport a helyreállítás a helyreállítási terv végrehajtása során futtatható parancsfájl.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Az OK gombra kattintva mentse a parancsfájl. A Küldés művelet csoport csoport 1 vegyük a parancsfájlt: indítása.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>A parancsfájlok hozzáadása dolgozhat pontjai

1. Kattintson a parancsfájl a jobb gombbal, és válassza a "delete lépés" vagy "parancsfájl frissítése".

2. Parancsfájl a futtathatók az Azure feladatátvevő közben a helyszíni az Azure, és a futtathatók Azure egy elsődleges ügyféloldali parancsfájl Leállításig, mint a helyszíni az Azure visszaállás során.

3. Parancsfájl futtatásakor azt fogja tölteni a helyreállítási terv környezetben.

Az alábbi képen látható, hogyan a helyi változót.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Az alábbi táblázat tartalmazza a nevét és leírását az környezetben mindegyik változó.

**Változó neve** | **Leírás**
---|---
RecoveryPlanName | Terv futtatott neve. Segít, hogy az azonos parancsfájl használatával neve alapján művelet végrehajtása
FailoverType | Itt adhatja meg, hogy a feladatátvételi tesztelése, tervezett vagy nem tervezett.
FailoverDirection | Adja meg, hogy helyreállítási elsődleges és másodlagos
Csoportazonosító | Keresse meg azt a csoportot számot a helyreállítási terv belül a terv fut
VmMap | Tömb összes a virtuális gépeken futó csoportjában
VMMap kulcs | Minden egyes virtuális egyedi kulcs (GUID). Érdemes ugyanaz, mint a virtuális gép VMM azonosítója szükség szerint.
RoleName | A helyreállítása folyamatban Azure virtuális gép neve
CloudServiceName | Azure felhőalapú szolgáltatás neve alatt, amely a virtuális gép jön létre.
CloudServiceName (az erőforrás-kezelő telepítési modell) | Azure erőforráscsoport neve alatt, amely a virtuális gép jön létre.


## <a name="using-complex-variables-per-recovery-plan"></a>Komplex változók per helyreállítási terv használatával

Időnként előfordulhat egy runbook szükséges csak a RecoveryPlanContext több információt. Nincs első osztályú mechanizmusa paraméter egy runbook átadni nem. Jó helyen jár, ha több helyreállítási tervek keresztül az azonos parancsfájl használni kívánt is használja a helyreállítási megtervezése helyi változót "RecoveryPlanName" és használ a kísérleti technikával egy Azure automatizálási komplex változó használni egy runbook alatt. Az alábbi példában látható, hogyan hozzon létre három különböző komplex változó eszközök és a hierarchiák használata a runbook a helyreállítási előfizetésére alapján.

Fontolja meg, hogy szeretne-e használni egy runbook 3 további paramétereket. Tudassa velünk JSON űrlapba kódolását {"Var1": "testautomation", "Var2": "Nem tervezett", "Var3": "PrimaryToSecondary"}

[Ε komplex változó](../automation/automation-variables.md#variable-types Complex variable) használatával hozzon létre egy új automatizálási eszközt.
Nevezze el a változó, ahogyan <RecoveryPlanName>- paraméterek.
A hivatkozás Itt hozhat létre a [komplex változó](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

Különböző helyreállítási csomag esetén neve megegyezik a változó

1. recoveryPlanName1 >-Paraméterek
2. recoveryPlanName2 >-Paraméterek
3. recoveryPlanName3 >-Paraméterek

Ezután a parancsfájl hivatkozik, a paraméterek

1. A RP neve kinyerése a $rpname = $Recoveryplancontext változó
2. Tárgyi eszköz $paramValue az első = "$($rpname)-paraméterek"
3. Ezzel a komplex változóként a helyreállítási terv hívja fel a Get-AzureAutomationVariable [-AutomationAccountName] <String> -$paramValue nevet.

Példaként a komplex változó paraméter beszerzése a SharepointApp helyreállítási tervet, hozzon létre egy Azure automatizálási komplex változó "SharepointApp-paraméterek" nevű.

Használni, akkor a helyreállítási terv a változó kinyerése a Get-AzureAutomationVariable utasítással eszköz [-AutomationAccountName] <String> [-neve] $paramValue. [További részletekért hivatkozás Ez](https://msdn.microsoft.com/library/dn913772.aspx)

Ezzel a módszerrel a azonos parancsfájl használható különböző helyreállítási csomagra, a terv adott komplex változó tárolása az eszközöket.

## <a name="sample-scripts"></a>Minta parancsfájlok

Parancsfájlok, közvetlenül importálhatja a automatizálási fiókba tárházából olvassa el [Kristian kínai szöveg MOBILE tárháza parancsfájlok](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

A parancsprogram itt egy üzembe összes erőforrás-kezelő Azure-sablon a parancsfájlok alatt

* NSG

A NSG runbook nyilvános IP-címek hozzárendelése minden virtuális belül a helyreállítási tervet, és azt a virtuális hálózati adaptereken csatolása, amelyek lehetővé teszik az alapértelmezett kapcsolati hálózati biztonsági csoport

* PublicIP

A nyilvános IP-runbook nyilvános IP-címek hozzárendelése a helyreállítási terv belül minden virtuális. A tűzfal beállításainak belül minden Vendég függ hozzáférést a gép és alkalmazások


* CustomScript

A CustomScript runbook nyilvános IP-címek hozzárendelése minden virtuális belül a helyreállítási tervet, és azt fog húzza a parancsfájlt, a hivatkozott sablon a telepítés során egy egyéni parancsfájl-bővítmény telepítése

* NSGwithCustomScript

A runbook nyilvános IP-címek rendel a helyreállítási tervet, egyéni parancsfájl-bővítmény használatával, és csatlakoztassa a virtuális hálózati adaptereken NSG lehetővé tevő telepítés belül minden virtuális NSGwithCustomScript alapértelmezett bejövő és kimenő kommunikáció távoli eléréséhez

## <a name="additional-resources"></a>További források

[Azure automatizálási szolgáltatásai fiókként futtatása](../automation/automation-sec-configure-azure-runas-account.md)

[Azure automatizálási – áttekintés] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure automatizálási – áttekintés")

[Minta Azure automatizálási parancsfájlokat] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Minta Azure automatizálási parancsfájlokat")
