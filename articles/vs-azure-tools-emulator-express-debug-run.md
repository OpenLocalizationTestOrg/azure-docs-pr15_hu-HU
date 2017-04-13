<properties
   pageTitle="Express irányító futtatása és használatával hibakeresési egy felhőalapú szolgáltatásba, a helyi számítógépen |} Microsoft Azure"
   description="Irányító Express használatával futtatni, és egy felhőalapú szolgáltatásba, egy helyi számítógép hibakeresése"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Az irányító Expresst használja futtatni, és egy felhőalapú szolgáltatásba, egy helyi számítógép hibakeresése

Express irányító használatával tesztelése, és egy felhőalapú szolgáltatásba hibakeresési Visual Studio futtatása rendszergazdaként nélkül. Beállíthatja, hogy a project beállításai irányító Express vagy a teljes irányító, attól függően, hogy a felhőalapú szolgáltatás követelményeinek. A teljes irányító kapcsolatos további tudnivalókért olvassa el [a kiszámítania irányító az Azure alkalmazás futtatása](./storage/storage-use-emulator.md)című témakört. Irányító Express tartalmazta először Azure SDK 2.1-es és Azure SDK 2.3, kezdve az alapértelmezett irányító.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>A Visual Studio IDE irányító Express használata

Amikor az Azure SDK 2.3 vagy újabb verziójában új projektet hoz létre, irányító Express már ki van jelölve. Meglévő projektekhez SDK korábbi verziójával készített kövesse az alábbi lépéseket irányító Express kiválasztásához.

### <a name="to-configure-a-project-to-use-emulator-express"></a>A projekt irányító Express használatához konfigurálása

1. Az Azure projekt helyi menüben válassza a **Tulajdonságok parancsot**, és válassza a **webhely** fülre.

1. A **Helyi fejlesztési kiszolgáló**területen válassza a **beállítás használata az IIS Express** gombot. Irányító Express nem kompatibilis a IIS-webkiszolgálón.

1. **Irányító**, csoportban válassza a beállítás **Használata irányító Express** gombot.

    ![Irányító Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Indítása a parancssorban irányító Express

A parancssorablakban indítsa el az Azure kiszámítania irányító, csrun.exe, az elsőbbségi verzióját /useemulatorexpress elemre.

## <a name="limitations"></a>Korlátozások

Express-irányító használata előtt tartsa szem előtt bizonyos korlátozások:

- Irányító Express nem kompatibilis a IIS-webkiszolgálón.

- A felhőalapú szolgáltatás több szerepkört is tartalmazhat, de minden szerepkör korlátozódik egy példány.

- 1000 alatti portszámokat nem érhetők el. Például ha egy olyan portot 1000 alatti rendszerint használó hitelesítési szolgáltatója van, előfordulhat, hogy módosítania ezt az értéket a portszámot 1000 fölé.

- Az Azure kiszámítania irányító vonatkozó korlátozások vonatkoznak irányító Express. Legfeljebb 50 szerepkör-példányok per telepítési például nem lehet. Lásd [a számítási irányító az Azure alkalmazás futtatása](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Következő lépések

[Hibakeresési Cloud Services](https://msdn.microsoft.com/library/azure/ee405479.aspx)
