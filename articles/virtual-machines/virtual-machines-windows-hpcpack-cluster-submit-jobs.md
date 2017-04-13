<properties
 pageTitle="Elküldése a feladatokat, amelyeket az HPC csomagját fürt Azure-ban |} Microsoft Azure"
 description="Megtudhatja, hogy miként állíthat be egy helyi számítógép küldhetik feladatok HPC Pack fürthöz Azure-ban"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Egy helyszíni számítógépről egy HPC Pack fürthöz telepítését az Azure HPC feladatok kezdeményezése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Állítsa be a helyszíni ügyfélszámítógépet küldhetik feladatokat, amelyeket a [Microsoft HPC csomag](https://technet.microsoft.com/library/cc514029) fürtre Azure-ban. Ez a cikk bemutatja, hogyan állíthat be egy helyi számítógép ügyféleszközök elől feladat elküldése HTTPS a fürthöz Azure-ban. Ezzel a módszerrel több fürt felhasználó küldhet feladatok a felhőalapú HPC csomag fürtre, de a fő csomópont virtuális közvetlenül csatlakozik, és az Azure előfizetői hozzáférés nélkül.

![Azure-ban fürtre feladat elküldése][jobsubmit]

## <a name="prerequisites"></a>Előfeltételek

* A **fő csomópont HPC csomag telepítését, az az Azure virtuális** - azt javasoljuk, hogy az automatikus eszközöket használja egy [Azure quickstart útmutató sablon](https://azure.microsoft.com/documentation/templates/) vagy egy [Azure PowerShell-parancsprogramot,](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) például a fő csomópont és fürt telepítését. Szüksége van a központi csomópontot a DNS-neve és a csoport rendszergazdája, hajtsa végre a lépéseket a jelen cikkben hitelesítő adatait.

* Az **ügyfélszámítógép** - szüksége van egy olyan Windows vagy Windows Server ügyfélszámítógépen, futtatását is lehetővé teszi, hogy HPC csomag ügyfél segédprogramok (lásd: a [Rendszerkövetelmények](https://technet.microsoft.com/library/dn535781.aspx)). Ha csak a HPC csomag webes portál vagy REST API segítségével küldhetnek feladatokat, bármelyik ügyfélszámítógép lehetőség is használhatja.

* A [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=328024) **HPC Pack telepítési adathordozóját** - HPC Pack ügyfél segédprogramok, ingyenes telepítőcsomag HPC-csomag (HPC Pack 2012 R2) legújabb verziójának telepítéséhez érhető el. Győződjön meg arról, hogy, töltse le a megfelelő verziójú HPC csomag van telepítve, a virtuális központi csomópontra.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Lépés: 1: Telepítse és állítsa be a a webösszetevők a központi csomópontra

Ahhoz, hogy a feladatokat, amelyeket a fürt küldhetik HTTPS REST felület, győződjön meg arról, hogy a HPC csomag webösszetevők központi HPC csomag csomóponton vannak-e beállítva. Ha az azok nem már telepítve van, először telepítse a webösszetevők a HpcWebComponents.msi telepítőfájlt futtatásával. Ezután állítsa be a összetevőket a HPC PowerShell-parancsprogramot: **Set-HPCWebComponents.ps1**futtatásával.

Részletes tudnivalók a [telepítse a Microsoft HPC csomag webösszetevők](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Bizonyos Azure quickstart útmutató sablonok HPC csomag telepítése, és automatikusan konfigurálja a webösszetevők. A [HPC csomag IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) használatával hozza létre, ha tetszés szerint telepítse és állítsa be a webösszetevők a telepítés részeként.

**A webhely-összetevők telepítése**

1. A fő csomópont virtuális csatlakozzon a fürt rendszergazdai hitelesítő adataival.

2. A fő csomópont HpcWebComponents.msi futtatható a HPC csomag beállítási mappából.

3. Kövesse a varázsló az webösszetevők telepítése

**A webösszetevők konfigurálása**

1. A központi csomópontra indítsa el a HPC PowerShell rendszergazdaként.

2. A megfelelő helyre a konfigurációs parancsfájl címtár módosításához írja be a következő parancsot:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Állítsa be a többi felület és a HPC webes szolgáltatás, írja be a következő parancsot:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Jelöljön ki egy tanúsítványt kéri, válassza a tanúsítvány, amely megfelel a fő csomópont nyilvános DNS-nevét. Ha például a központi csomópontot a klasszikus telepítési modell segítségével virtuális telepít, ha a tanúsítvány neve a következőképpen néz CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Az erőforrás-kezelő telepítési modell használata esetén a tanúsítvány neve néz CN =&lt;*HeadNodeDnsName*&gt;. &lt; *régió*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Választhat a tanúsítvány később elküldésekor feladatok a központi csomópontra egy helyszíni számítógépről. Ne jelölje be vagy a tanúsítvány, amely megfelel a számítógép neve a központi csomópontot, az Active Directory-tartomány beállítása (például CN =*MyHPCHeadNode.HpcAzure.local*).

5. Adja meg a webes portál feladat elküldése, írja be a következő parancsot:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. A parancsprogram befejeződése után leállíthatja, és indítsa újra a HPC feladat Feladatütemező szolgáltatás írja be az alábbi parancsokat:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Lépés: 2: Telepítse a HPC csomag ügyfél segédprogramok helyszíni számítógépen

Ha szeretne a HPC csomag ügyfél segédprogramok telepítése a számítógépen, töltse le a HPC csomag a telepítés fájlokat (teljes telepítés) a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=328024). Ha a telepítés megkezdése válassza az **ügyfél-segédprogramok HPC csomag**a telepítés lehetőséget.

A központi csomópontra virtuális feladatok elküldése az HPC csomag ügyféleszközöket használatához, is kell tanúsítvány exportálja a központi csomópontot, és telepítse az ügyfélgépen. A tanúsítvány kell lennie. CER formátum.

**A központi csomópontot a tanúsítvány exportálása**

1. A központi csomópontra a helyi számítógép fiók egy Microsoft Management Console a tanúsítványok beépülő modul hozzáadása. A beépülő modul hozzáadása című témakör tartalmazza [a Tanúsítványok MMC Konzolhoz beépülő modul hozzáadása](https://technet.microsoft.com/library/cc754431.aspx).

2. Az konzolfáján, bontsa ki a **tanúsítványok – a helyi számítógép** > **személyes**, majd kattintson a **tanúsítványok**gombra.

3. Keresse meg a tanúsítvány a HPC csomag webösszetevők a konfigurált [lépés 1: Telepítse és állítsa be a webösszetevők a központi csomópontra](#step-1:-install-and-configure-the-web-components-on-the-head-node) (például CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Kattintson a jobb gombbal a tanúsítványt, és válassza a **Minden tevékenység** > **exportálja**.

5. Az exportálás varázslóban kattintson a **Tovább**gombra, és győződjön meg arról, hogy **nem, nem exportálja a titkos kulcs** van kiválasztva.

6. Kövesse a varázsló a DER kódolású bináris X.509-tanúsítvány exportálása a hátralévő lépéseket (. CER) formátumot.


**Az ügyfélgépen a tanúsítvány importálása**


1. Másolja az ügyfélszámítógépen mappára a központi csomópont exportált tanúsítványra.

2. Az ügyfélgépen futtatása certmgr.msc parancsot.

3. A tanúsítvány-kezelőben bontsa ki a **tanúsítványok – aktuális felhasználó** > **Megbízható legfelső szintű hitelesítésszolgáltatók**, kattintson a jobb gombbal a **tanúsítványok**, és válassza a **Minden tevékenység** > **Importálás**.

4. Importálása varázslóban kattintson a **Tovább** gombra, és kövesse a lépéseket a megbízható legfelső szintű hitelesítésszolgáltatók áruház exportálja a központi csomópont-tanúsítvány importálása.



>[AZURE.TIP] Előfordulhat, hogy a biztonsági figyelmeztetés, mert nem észlelhető az a központi csomópontra hitelesítésszolgáltató által az ügyfélgépen. Tesztelési célú, ez a figyelmeztetés figyelmen kívül, és fejezze be a tanúsítvány importálása.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Lépés 3: Futtatás próba feladatok a fürthöz

Ha ellenőrizni szeretné a konfigurációban, hibaelhárító feladatok a fürt Azure-ban a helyszíni számítógépről. Például segítségével HPC csomag grafikus eszközök vagy a parancssori parancsok küldhetnek a fürthöz feladatokat. A webes portál használatával küldhetnek feladatokat.


**A feladat Beküldési parancsai futtathatók az ügyfélszámítógépen**


1. Egy ügyfélszámítógépen, ahol a HPC csomag ügyfél segédprogramok telepített nyisson meg egy parancssort.

2. Írja be a minta parancsot. A csoport összes feladatok listáját, írja be például a hasonló attól függően, hogy a teljes DNS-nevét a központi csomópontot, az alábbi parancsot:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    vagy
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Az ütemező URL-cím használata a központi csomópontot, nem az IP-cím mezőbe a teljes DNS-nevét. Az IP-címet ad meg, ha hibaüzenet jelenik meg, hasonlóan, mint "a kiszolgálói tanúsítvány van szüksége, vagy hogy egy érvényes megbízhatósági lánc vagy helyét a megbízható legfelső szintű áruházban."

3. Amikor a rendszer kéri, írja be a felhasználónevét (a képernyőn &lt;tartománynév&gt;\\&lt;felhasználónév&gt;) és jelszavát a HPC fürt rendszergazda vagy egy másik fürt felhasználó konfigurált. Megadhatja a helyben további feladat műveletek hitelesítő adatait tárolja.

    A feladatok lista jelenik meg.


**HPC feladat Manager használatához az ügyfélszámítógépen**

1. Ha a feladat elküldésekor tartomány fürt felhasználó hitelesítő adatait a korábban nem tárolja, felveheti hitelesítőadat-kezelő a hitelesítő adatokat.

    egy. A Vezérlőpulton az ügyfélszámítógépen indítsa el a hitelesítőadat-kezelő elemre.

    b. Kattintson a **Windows hitelesítő adatok** > **hozzáadása egy általános hitelesítő adatait**.

    c billentyűkombinációt. Adja meg az Internet címét (például a https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler vagy a https://&lt;HeadNodeDnsName&gt;.&lt; régió&gt;.cloudapp.azure.com/HpcScheduler), és a felhasználó nevét (&lt;tartománynév&gt;\\&lt;felhasználónév&gt;) és a csoport rendszergazdája vagy egy másik, konfigurált fürt felhasználó jelszavát.

2. Az ügyfélgépen HPC Feladatkezelő indítása

3. **Jelölje be a címsor csomópont** párbeszédpanelen írja be a központi csomóponthoz Azure URL-CÍMÉT (például a https://&lt;HeadNodeDnsName&gt;. cloudapp.net vagy a https://&lt;HeadNodeDnsName&gt;.&lt; régió&gt;. cloudapp.azure.com).

    HPC Feladatkezelő megnyílik, és a feladatok listáját jeleníti meg a központi csomópontot.

**A központi csomóponton futó webes portál használata**

1. Indítson el egy webböngészőt az ügyfélszámítógépen, és adja meg a következő címeket, attól függően, hogy a teljes DNS-nevét a központi csomópont egyikét:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    vagy
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Írja be a tartomány hitelesítő adatok HPC fürt rendszergazdájának megjelenő biztonsági párbeszédpanelen. (Felveheti is többi fürt felhasználó a különféle szerepköröket. Lásd: [fürt-felhasználók kezelése](https://technet.microsoft.com/library/ff919335.aspx).)

    A webes portál a feladat listanézet nyílik meg.

3. Egy minta feladat, amely a "Helló, világ" karakterláncot ad vissza a fürt elküldéséhez kattintson a bal oldali navigációs **új feladatot** .

4. Az **Új feladat** lapon a **beküldött lapról**csoportban **HelloWorld**. A feladat Beküldési lap jelenik meg.

5. Kattintson a **Küldés**gombra. Ha a rendszer kéri, adja meg a tartomány hitelesítő adatok HPC fürt rendszergazdájának. A feladat elküldése, és a feladat azonosítója megjelenik a **Saját feladatok** lapon.

6. A feladat, amely az Ön által küldött eredményének megtekintéséhez kattintson a feladat azonosító, és kattintson a **Feladatok megtekintése** a parancs (a **kimeneti**) megtekintéséhez.

## <a name="next-steps"></a>Következő lépések

* Feladatok az Azure fürt a [HPC csomag REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)-val is küldhet.

* Küldhetik fürt feladatok Linux ügyfélről, című témakörben olvashat a Python minta [HPC csomag 2012 R2 SDK és példakódot](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
