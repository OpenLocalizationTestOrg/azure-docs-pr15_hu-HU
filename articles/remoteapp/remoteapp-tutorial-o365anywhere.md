<properties
   pageTitle="A ugyanazt az Office 365 élményének első Azure RemoteApp bármilyen eszközön |} Microsoft Azure"
   description="Megtudhatja, hogyan oszthat meg bármelyik Office 365 alkalmazást a felhasználókkal Azure RemoteApp használatával."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Az ugyanazon az Office 365 élményének első Azure RemoteApp bármilyen eszközről

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ez a cikk az Office 365 telepítése a vállalat bármely eszközön kiterjed. A felhasználók az azonos funkciók és az Android, az Apple és a Windows felhasználói élmény szerezhet be.

Azt fogja végrehajtásához Azure RemoteApp segítségével az Office 365 üzemeltető skála képes virtuális gépeken, hogy a felhasználók csatlakozhatnak az Azure-ban. Ez a beállítás a virtuális gépeken futó hívása "felhő webhelycsoport".

## <a name="create-a-cloud-collection"></a>Hozzon létre egy felhőalapú gyűjteményt

Először után létrehozott Azure-fiók, nyissa meg **RemoteApp** bal oldalán a hivatkozásra kattintva.
![Azure RemoteApp megjelenítő az Azure portálon](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Folytassa **Új** az alsó és a "rövid létrehozása" oldalán kattintson a webhelycsoport. Adja meg a nevét, a régió, az előfizetést, a csomagot, és az "Office Proffesional 2013", képet elvégezheti a szükséges.
![Létrehozása párbeszédpanel](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Miután végzett a képernyőn, a webhelycsoport létrehozásának folyamata kell kezdődnie. Ez igénybe vehet egy órával vagy így.

![Várakozás](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Miután a folyamat befejezése után fog megjelenni alábbihoz hasonló. Ha azt kattintson a **Közzététel** azt láthatja, hogy a legtöbb Office-alkalmazások közzétették nekünk már.
![Webhelycsoport létrehozása](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Közzétett alkalmazások](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Ezen a ponton felveheti is több felhasználó fér hozzá az ebben a gyűjteményben kattintson a **Felhasználói hozzáférés**.
![Felhasználói hozzáférés konfigurálása](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Most vegyük próbálja ki az csatlakoztatása az Office 365!

## <a name="connect-to-office-365"></a>Az Office 365 csatlakoztatása

Azt fogja fölé való [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/)központi görgessen lefelé, kattintson a gombra a **ügyfelek letöltése** az eszközre, használja az Azure RemoteApp ügyfélcsomag telepítése. Az alábbi képernyőképek a Windows rendszer.

Ha az alkalmazás elindul, a rendszer kéri, jelentkezzen be Microsoft-fiókjával (korábbi nevén a "Live azonosító"), most ugyanarra a számítógépre, mint az Azure-fiók használata. Ha be van jelentkezve a kell új meghívók szóló értesítés, kattintson a nincs, és meg kell jelennie a lista egyik alábbi. Fogadja el a meghívót, amely megfelel az Azure-fiók tulajdonosa e-mailben.

![Új meghívó](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Hogy néz ki új meghívók esetén.

![Fogadja el az alkalmazások](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

A meghívó elfogadása után meg kell jelennie a Azure RemoteApp ügyfél összes Office-alkalmazás.

![Alkalmazások listájában](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Kattintva az alkalmazás kell kezdése az Azure virtuális gép, és ezek egyikét kell lennie az összes beállítása! Fogják túlságosan nagyra értékelni!

![indítása](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![a PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
