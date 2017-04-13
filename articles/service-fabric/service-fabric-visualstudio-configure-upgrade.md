<properties
   pageTitle="A frissítés szolgáltatás háló alkalmazás beállítása |} Microsoft Azure"
   description="Útmutató: a szolgáltatás háló-alkalmazások frissítés a Microsoft Visual Studio segítségével a beállításainak konfigurálása"
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>A szolgáltatás háló alkalmazás a frissítés konfigurálása a Visual Studio

Visual Studio eszközök Azure Service háló frissítési támogatása helyi vagy távoli fürt közzététel. Két szempontból helyett cseréje tesztelés és hibakeresés közben az alkalmazás újabb verziója az alkalmazás frissítése:

- A frissítés során alkalmazás adatok nem vesznek el.
- Elérhetősége marad magas így nem kell minden szolgáltatáskimaradás a frissítés során, ha elég szolgáltatás példányainak húzza szét a frissítési tartományok.

Vizsgálatok szemben az alkalmazások futtatható, miközben a frissítendő van.

## <a name="parameters-needed-to-upgrade"></a>Frissítés szükséges paraméterek

Választhat kétféle környezetben: normál és frissítés. A szokásos telepítés törli az előző telepítési adatokat, és a fürtre, adatok, amíg egy frissítési telepítési őrzi meg azt. Ha frissít egy szolgáltatás háló alkalmazásban a Visual Studióban, meg kell adnia a alkalmazás frissítési paraméterei és állapot ellenőrzése házirendeket. Frissítési paraméterek alkalmazás segítségével szabályozhatja a frissítés, amíg az állapot-ellenőrzés házirendek megtudhatja, hogy a frissítés sikeres volt-e. Lásd: [szolgáltatás háló alkalmazás frissítése: frissítési paraméterek](service-fabric-application-upgrade-parameters.md) további információt.

Vannak olyan három frissítési közül: *figyelt* *UnmonitoredAuto*és *UnmonitoredManual*.

  - Egy figyelt frissítés automatizálja a frissítés és az alkalmazás állapot-ellenőrzése.

  - UnmonitoredAuto frissítésének automatizálja a frissítést, de kihagyja az állapot-ellenőrzése.

  - Ekkor-UnmonitoredManual frissítésre kell manuális frissítése az összes frissítési tartomány.

Minden egyes frissítési módhoz paraméterek különböző csoportjaihoz. Ha többet szeretne megtudni a rendelkezésre álló frissítési beállítások a [alkalmazás frissítése a paraméterek](service-fabric-application-upgrade-parameters.md) című részben.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>A Visual Studióban szolgáltatás háló alkalmazás frissítése

Ha a Visual Studio szolgáltatás háló eszközök használ szolgáltatási háló alkalmazás frissítése, megadhatja egy közzétételi folyamat egy szokásos telepítés, hanem frissítésre kell **az alkalmazás frissítése** jelölőnégyzet bejelölésével.

### <a name="to-configure-the-upgrade-parameters"></a>A frissítési paraméterek beállítása

1. Kattintson a **Beállítások** gomb mellett a jelölőnégyzetet. Megjelenik a **Frissítés paraméterek szerkesztése** párbeszédpanel. A **Frissítés paraméterek szerkesztése** párbeszédpanel a figyelt UnmonitoredAuto és UnmonitoredManual frissítési módot támogat.

2. Válassza ki a használni kívánt használja, és töltse ki a paraméter rács frissítési módját.

    Az egyes paraméterek alapértelmezett értéket tartalmaz. A választható paraméter *DefaultServiceTypeHealthPolicy* megnyitja a kivonat táblázat beviteli. Íme egy példa kivonat beviteli táblázatformátumot *DefaultServiceTypeHealthPolicy*az:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* egy másik választható paraméter, amely egy kivonat táblázat beviteli a következő formátumban:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Íme egy valós életben példa:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Ha UnmonitoredManual frissítési módot választja, manuálisan kell indítania a egy PowerShell-konzol továbbra is, és a frissítési folyamat befejezéséhez. Hivatkozhat [szolgáltatás háló alkalmazás frissítése: Speciális témakörök](service-fabric-application-upgrade-advanced.md) megtudhatja, hogyan manuális frissítés működik.

## <a name="upgrade-an-application-by-using-powershell"></a>Alkalmazás frissítése a PowerShell használatával

A szolgáltatás háló alkalmazás frissítése PowerShell-parancsmagok is használhatja. [Oktatóprogram frissítési szolgáltatás háló alkalmazás](service-fabric-application-upgrade-tutorial.md) és a [Kezdő-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) talál részletes információt.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Adja meg az alkalmazás nyilvánvalóan fájlban házirend állapot ellenőrzése

Minden service, szolgáltatás háló alkalmazásban beállíthatja, hogy a saját állapot bírálja felül az alapértelmezett házirend paramétert. Az alkalmazás nyilvánvalóan fájlban következő paraméterértékeket lehet nyújtani.

A következő példa bemutatja, hogyan használhatja az alkalmazás jegyzék az egyes szolgáltatásokhoz egyedi állapot ellenőrzése házirendjének.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Következő lépések
Az alkalmazások telepítésével kapcsolatos további tudnivalókért lásd: a [Deploy Azure Service háló egy meglévő alkalmazást](service-fabric-deploy-existing-app.md).
