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


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Azure automatizálási runbooks hozzáadása helyreállítási tervek – klasszikus


Ebben az oktatóanyagban ismerteti, hogyan integrálja az Azure webhely helyreállítási Azure automatizálási helyreállítási tervek bővítési megadására. A helyreállítási terv is téve a virtuális gépeken futó Azure webhely helyreállítási a másodlagos felhőben való replikáció és Azure esetek való replikáció védeni visszanyerése. Is segítik annak érdekében, hogy a helyreállítási **egységes pontos**, **megismételhető**, és **az automatikus**. A virtuális gépeken futó az Azure keresztül nem működnek, ha a integrálása az Azure automatizálási bővíti a helyreállítási tervek, és runbooks, így a hatékony automatizálási tevékenységek végrehajtásához lehetőséget nyújt.

Ha meg van nem leggyakoribb kapcsolatos Azure automatizálási még, jelentkezzen be [az alábbi](https://azure.microsoft.com/services/automation/) , és töltse le a minta parancsfájlok [Itt](https://azure.microsoft.com/documentation/scripts/). További tudnivalók a [Webhely-helyreállítás Azure](https://azure.microsoft.com/services/site-recovery/) , és hogyan téve a helyreállítási csomagok használatával Azure helyreállítási olvassa el [az alábbi](https://azure.microsoft.com/blog/?p=166264).

Az rövid oktatóprogram megnézi lesz az hogyan integrálja a Azure automatizálási runbooks helyreállítási tervek be. Azt, amely a korábban szükséges beavatkozás egyszerű feladatok automatizálása, és megtudhatja, hogy miként egy egyetlen kattintással helyreállítási művelet több lépésben helyreállítást konvertálása. Azt is, hogy hogyan háríthatja el egy egyszerű parancsfájl mentésük, ha vizsgálni.

## <a name="protect-the-application-to-azure"></a>Az alkalmazás Azure védelme

Egy egyszerű alkalmazás két virtuális gépeken futó álló tudassa velünk kezdeni. Ebben az esetben felkínálunk egy Fabrikam HRweb alkalmazását. A Fabrikam-HRweb-frontend és Fabrikam-Hrweb-kódmentes is Azure védett két virtuális gépeken futó Azure webhely helyreállítás segítségével. A webhely-helyreállítás Azure segítségével virtuális gépeken futó védelmét, kövesse az alábbi lépéseket.

1.  Engedélyezze a virtuális gépeken futó védelmét.

2.  Győződjön meg arról, hogy a virtuális gépeken futó kezdeti replikációs befejeződött, és replikálja.

3.  Várja meg, amíg a kezdeti replikáció befejeződik, és a replikáció állapot szerint védett.

![](media/site-recovery-runbook-automation/01.png)
---------------------

Ebben az oktatóanyagban azt a Fabrikam HRweb alkalmazás segítségével az Azure alkalmazás helyreállítási tervet hoz létre. Azt fogja integrálhatja a egy runbook, amely zárólap hozza létre a sikertelen a, majd a 80-as port weblapok szolgáló Azure virtuális gép fölé.

Első lépésként az alkalmazás a helyreállítási terv létrehozása.

## <a name="create-the-recovery-plan"></a>A helyreállítási terv létrehozása

Szeretné helyreállítani az Azure alkalmazás, a helyreállítási terv létrehozásához szükséges.
Megadhatja, hogy a virtuális gépeken futó visszanyerése sorrendjének helyreállítási-csomagot használ. A virtuális gépen, 1 csoport helyezett helyreállítása lesz, és indítsa el az első, és a virtuális gépen, 2 csoportban kövesse.

Hozzon létre egy helyreállítási tervezi, hogy az alábbi formátumban jelennek.

![](media/site-recovery-runbook-automation/12.png)

További információ a helyreállítási csomagokról, olvassa el a dokumentáció [Itt](https://msdn.microsoft.com/library/azure/dn788799.aspx "Itt").

Ezután a szükséges eltérések létrehozása az Azure automatizálás.

## <a name="create-the-automation-account-and-its-assets"></a>Az automatizálási fiók és fekteti létrehozása

Runbooks létrehozása az Azure automatizálási fiókra van szüksége. Ha nem már rendelkezik fiókkal, válassza a-lel Azure automatizálási lap ![](media/site-recovery-runbook-automation/02.png), és hozzon létre egy új fiókot.

1.  Nevezze el a fiókot az azonosításához.

2.  Adja meg a földrajzi területhez tartozik, ahová a fiókot.

Ajánlott, ha el szeretne helyezni a fiók ugyanabban a régióban, mint az automatikus rendszer-Helyreállítás tárolóból elemre.

![](media/site-recovery-runbook-automation/03.png)

Ezután hozzon létre fiók a következő eszközöket.

### <a name="add-a-subscription-name-as-asset"></a>Adja hozzá egy előfizetés nevére a digitális eszköz kiválasztása

1.  Adja hozzá az új beállítás ![](media/site-recovery-runbook-automation/04.png) Azure automatizálási eszközök és kiválasztása![](media/site-recovery-runbook-automation/05.png)

2.  Válassza ki a változó **karakterláncként**

3.  Adja meg változóinak nevéből **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Adja meg a tényleges Azure előfizetés nevét a változó értékét.

    ![](media/site-recovery-runbook-automation/07_1.png)

Meghatározhatja, hogy a fiókja, az Azure portál beállításai lapján az előfizetése nevére.

### <a name="add-an-azure-login-credential-as-asset"></a>Tárgyi eszköz egy Azure bejelentkezési hitelesítő adatok megadása

Azure automatizálási az előfizetés való kapcsolódáshoz Azure PowerShell használ, és ott az eltérések a működik. Ehhez kell hitelesíteni a Microsoft-fiók vagy egy munkahelyi vagy iskolai fiók segítségével.
A fiók hitelesítő adatait a runbook biztonságosan használható eszköz tárolhat.

1.  Adja hozzá az új beállítás ![](media/site-recovery-runbook-automation/04.png) Azure automatizálási eszközök és kattintson a![](media/site-recovery-runbook-automation/09.png)

2.  Válassza ki a hitelesítő adatok, **A Windows PowerShell hitelesítő adatok**

3.  Adja meg a nevét, **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Adja meg a felhasználónevét és jelszavát, hogy jelentkezzen be.

Most már mind a beállítások érhetők el az eszközeit.

![](media/site-recovery-runbook-automation/11.png)

További információ arról, hogy miként csatlakozhat az előfizetés PowerShell rendszer azért adja [Itt](../powershell-install-configure.md).

Ezután Azure automatizálási, feladatátvétel után az előtér-virtuális gép zárólap hozzáadhat egy runbook hoz létre.

## <a name="azure-automation-context"></a>Automatizálási Azure környezetben

Automatikus rendszer-Helyreállítás egy helyi változót segít mérvadó parancsfájlok írása a runbook továbbítja. Egy jövőben is, hogy a Felhőbeli szolgáltatástól és a virtuális gép azoknak a kiszámítható, azonban, hogy még nem mindig az esetet, például az egy bizonyos helyzetekben miatt hol a virtuális gép nevére Azure-ban nem támogatott karakterek miatt előfordulhat, hogy megváltozott történik. Az automatikus rendszer-Helyreállítás helyreállítási terv *helyi*részeként így létrehozójától átadott.

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


A VmMap billentyűt az környezetben is képes nyissa meg az automatikus rendszer-Helyreállítás virtuális tulajdonságok lapon, és tekintse meg a virtuális GUID tulajdonság azonosításához.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Az automatizálási runbook Szerző

Most hozzon létre a runbook kattintva nyissa meg a 80-as port az előtér-virtuális gépen.

1.  Hozzon létre egy új runbook **OpenPort80** nevű Azure automatizálási fiók

    ![](media/site-recovery-runbook-automation/14.png)

2.  Keresse meg a runbook Szerző nézetét, és a piszkozat módba.

3.  Először adja meg a változó tartományként a helyreállítási terv környezetben

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Az előfizetés a hitelesítő adatok és az előfizetés nevét használja tovább csatlakoztatása

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Megjegyzés: az Azure eszközök – **AzureCredential** , és itt **AzureSubscriptionName** használható.

5.  Most, adja meg a végpont adatait, és a globálisan egyedi azonosítója, a virtuális gép, amelynek meg szeretné elérhetővé tenni a végpontot. Ebben az esetben az előtér-virtuális gépet.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Az Azure végpont protokollt, a virtuális helyi portjához és a megfeleltetett nyilvános port jelöli ki. Ezek a változók végpontok hozzáadása VMs Azure parancsaival szükséges paraméterek. A VMGUID tárolja a globálisan egyedi azonosítója, a virtuális gép kell vonatkozni fognak.

6.  A parancsprogram most bontsa ki az környezetben a megadott virtuális GUID, és a végpont létrehozása a virtuális gépen hivatkoznak rá.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Elkészülte után a közzététel találati ![](media/site-recovery-runbook-automation/20.png) a parancsfájl végrehajtás érhető el.

A teljes parancsfájl az alábbi táblázat a hivatkozásnak

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Adja meg a parancsfájlt a helyreállítási terv

Amikor elkészült a parancsfájlt, felveheti a korábban létrehozott helyreállítási csomagot.

1.  A létrehozott helyreállítási tervet válassza a parancsfájlok hozzáadása után a csoport 2. ![](media/site-recovery-runbook-automation/15.png)

2.  Adja meg a parancsfájlt nevét. Ez a, ezen belül a helyreállítási terv megjelenítő parancsfájl csak egy rövid nevet.

3.  Azure parancsfájl – való áttérés válassza ki az Azure automatizálási fiók nevét.

4.  Jelölje ki az Ön által létrehozott runbook az Azure Runbooks.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Elsődleges ügyféloldali parancsfájlok

Azure egy áttérni végrehajtásakor vannak is megadhatja elsődleges ügyféloldali parancsfájlok végrehajtani. A parancsfájlok feladatátvételkor VMM kiszolgálói fog futni.
Elsődleges ügyféloldali parancsfájlok érhetők el csak csak leállás előtti tanulmányozása és kérdések feltevése leállítása paranccsal Ennek oka az, a elsődleges webhelyet, hogy általában nem érhető el, amikor katasztrófa éri megakadályozási.
Során feladatátvételhez csak akkor, ha úgy dönt az elsődleges hely műveletekhez, akkor próbálja az elsődleges oldalát parancsfájlok futtatásának. Ha még nem érhető el, vagy időtúllépés, a feladatátvételi továbbra is szerepelni fognak a virtuális gépeken futó helyreállítása.
Elsődleges ügyféloldali parancsfájlok érhetők el nem VMware/tényleges/Hyper-v webhelyek Azure való áttérés közben az Azure - védett VMM nélkül.
Jó helyen jár, ha, visszaállás az Azure a helyszíni, oldal elsődleges parancsfájlok (Runbooks) használható VMware kivételével mindegyik cél.

## <a name="test-the-recovery-plan"></a>A helyreállítási terv tesztelése

Miután elhelyezte a runbook a terv próba feladatátvevő kezdeményezzen, és a funkció működés közben. Mindig ajánlott próba áttérni tesztelje az alkalmazás és a helyreállítási terv annak érdekében, hogy nincsenek-e hibák futtatásához.

1.  Válassza ki a helyreállítási tervet, és a vizsgálat feladatátvétel kezdeményezni.

2.  Terv végrehajtása során láthatja, hogy a runbook teljesítette, vagy állapota keresztül nem.

    ![](media/site-recovery-runbook-automation/17.png)

3.  A részletes runbook adatvégrehajtás állapota a runbook az Azure automatizálási feladatok lapon is megjelenik.

    ![](media/site-recovery-runbook-automation/18.png)

4.  A feladatátvételi mellett az runbook végrehajtási eredmény befejeződése után megjelenik a végrehajtás sikeres-e, illetve nem megtalálhatók az Azure virtuális gép lapra, és megjeleníti a végpontok.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Minta parancsfájlok

Miközben az üzlet elévült automatizálása egy gyakran használt tevékenység hozzáadása a zárólap az Azure virtuális gép ebben az oktatóanyagban sikerült oszthat, számos más hatékony automatizálási feladatok Azure automatizálási. A Microsoft és az Azure automatizálási közösségi adja meg a minta runbooks, amelyek segítséget nyújtanak elsajátíthatja a saját a megoldások és segédprogram runbooks, amely nagyobb automatizálási feladatokhoz építőelemek használata. A gyűjteményből használatával őket és az alkalmazások az Azure-webhely helyreállítás hatékony egyetlen kattintással helyreállítási tervei összeállítása.

## <a name="additional-resources"></a>További források

[Azure automatizálási – áttekintés] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure automatizálási – áttekintés")

[Minta Azure automatizálási parancsfájlokat] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Minta Azure automatizálási parancsfájlokat")
