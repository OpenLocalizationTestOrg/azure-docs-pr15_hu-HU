<properties
    pageTitle="Kapcsolatos hibák elhárítása: Létrehozása és gépi tanulási munkaterület csatlakoztatása |} Microsoft Azure"
    description="A hozhat létre és Azure gépi tanulási munkaterülethez csatlakozik a gyakori problémák megoldásának"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Hibaelhárítási útmutatója: hozzon létre és gépi tanulási munkaterülethez csatlakoztatása

Ez az útmutató egyes megoldások gyakran történt kihívásokkal Azure gépi tanulási munkaterületek beállításakor.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Munkaterület-tulajdonos

Gépi tanulási új munkaterület létrehozásához írja be a MUNKATERÜLET tulajdonosa mező azonosítója kell érvényes Microsoft-fiókkal (korábbi nevén Windows Live ID), például john-contoso@live.com vagy john-contoso@hotmail.com. Nem lehet nem Microsoft-fiókkal, például a vállalati e-mail fiókot. Ingyenes Microsoft-fiók létrehozásához keresse fel a [www.live.com](http://www.live.com).

Figyelje meg, hogy a munkaterület létrehozása az Azure-portálra bejelentkezéshez használt fiókkal nem automatikusan rendelkezik *Nyissa meg a* munkaterületet, kivéve, ha az adott fiókból tulajdonosaként adja meg. Gépi tanulási Studio munkaterület megnyitásához kell bejelentkeznie Microsoft-Account a munkaterület tulajdonosaként megadott vagy meghívót kap a tulajdonos, ha be szeretne kapcsolódni a munkaterület kell. Az Azure klasszikus portálról azt is megteheti, azonban *kezelése* a munkaterületen található, amely tartalmazza az azt jelenti, hogy tulajdonosának módosítása, és állítsa be az access.

Munkaterület kezelésével kapcsolatban további tudnivalókért lásd: [az Azure gépi tanulási munkaterület kezelése].

[Az Azure gépi tanulási munkaterület kezelése]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Engedélyezett területek

Jelenleg gépi tanulási érhető el korlátozott számú régiók. "Ha előfizetése nem tartalmazza e régiók egyikére, előfordulhat, hogy hibaüzenet jelenik meg, telepítette, nincs előfizetések engedélyezett régiókban."

Szeretne kérni, hogy az előfizetés terület adhatók hozzá, jelölje ki a **Partnert a Microsoft támogatási** az Azure klasszikus portálról, válassza a **Számlázás** a probléma típusa, és kövesse a képernyőn megjelenő utasításokat a kérelem.

![Lépjen kapcsolatba a Microsoft terméktámogatás][screen1]

## <a name="storage-account"></a>Tárterület-fiók

A gépi tanulási szolgáltatás egy tároló fiók adatainak tárolására van szüksége. Egy meglévő tárterület-fiókot is használhatja, vagy létrehozhat egy új tárterület-fiókot (ha kvóta tároló új fiók létrehozása) az új gépi tanulási munkaterület létrehozásakor.

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Munkaterület létrehozása][screen2]

Az új gépi tanulási munkaterület létrehozását követően jelentkezzen gépi tanulási Studio a munkaterület tulajdonosaként megadott Microsoft-fiók használatával. Ha a hibaüzenet, "Munkaterület nem található" (hasonlóan, mint az alábbi képernyőképen), használja az alábbi lépéseket a cookie-k törlése.

![Nem található munkaterület][screen3]

**Cookie-k törlése**

Ha Internet Explorert használ, kattintson a jobb felső sarokban az **eszközök** gombra, és válassza az **Internetbeállítások parancsot**.  

![Internetbeállítások párbeszédpanel][screen4]

Az **Általános** lapon kattintson a **Törlés...**

![Az Általános lap][screen5]

**A böngészési előzmények törlése** párbeszédpanelen ellenőrizze, hogy a **cookie-k és webhelyadatok** ki van jelölve, és kattintson a **Törlés**gombra.

![Cookie-k törlése][screen6]

Miután a cookie-k törlődnek, indítsa újra a böngészőt, és kattintson a [Microsoft Azure gépi tanulási](https://studio.azureml.net) lapot. Amikor felhasználónevet és jelszót kéri, adja meg a Microsoft-fiókot a munkaterület tulajdonosaként megadott.

Célunk köszönhetően a gépi tanulási lehető is zökkenőmentesen elvégezhető. Kérjük, fel minden olyan megjegyzést, és a problémák az [Azure gépi tanulási fórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) segítse a Felszolgálás jobb.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
