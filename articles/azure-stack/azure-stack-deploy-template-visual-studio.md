<properties
    pageTitle="A Visual Studio Azure egymást fedő sablonok telepítése |} Microsoft Azure"
    description="További információ a Visual Studio Azure egymást fedő sablonok telepítése."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Sablonok használata a Visual Studio Azure egymást fedő telepítése

Erőforrás-kezelő Azure sablonok telepítse az Azure Papírhalom Ez a Visual Studio segítségével.

Erőforrás-kezelő sablonok üzembe helyezéséhez és az alkalmazás egyetlen, összehangolt műveletben az erőforrások kiépítése.

1.  Nyissa meg a Visual Studio 2015 Update 1.

2.  Kattintson a **fájl**fülre, kattintson az **Új**gombra, és kattintson az **Új projekt** párbeszédpanel **Azure erőforráscsoport**.

3.  Írja be egy **nevet** az új projekt, és kattintson **az OK**gombra.

4.  **Azure-sablon kiválasztása** párbeszédpanelen kattintson **a Windows virtuális gépre**, és kattintson **az OK**gombra.

  Az új projekten akkor a **sablonok** csomópontot a **Megoldás-tallózó** panelen kibontásával elérhető sablonok listájának láthatja.

5.  A **Megoldás-tallózó** panelen kattintson a jobb gombbal a projekt nevére, kattintson a **központi telepítés**, majd kattintson az **Új telepítési**.

6.  **Az erőforráscsoport Deploy** párbeszédpanelen az **előfizetés** legördülő jelölje be a Microsoft Azure Papírhalom előfizetését.

7.  Az **Erőforráscsoport** listában válassza a meglévő erőforráscsoport, vagy hozzon létre egy újat.

8.  Az **erőforrás csoport helye** listában válasszon egy helyet, és válassza a **Deploy**.

9.  **Paraméterek szerkesztése** párbeszédpanelen adja meg a paraméterek (Ez a sablon típusától függően változnak) értékeit, és kattintson a **Mentés**gombra.

## <a name="next-steps"></a>Következő lépések

[A parancssorból sablonok telepítése](azure-stack-deploy-template-command-line.md)
