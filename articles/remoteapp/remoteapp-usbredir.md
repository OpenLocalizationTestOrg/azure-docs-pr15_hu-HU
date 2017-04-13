<properties 
    pageTitle="Hogyan átirányítás Azure RemoteApp USB-eszközök? | Microsoft Azure" 
    description="Megtudhatja, hogy miként átirányítás Azure RemoteApp USB-eszközök használata." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hogyan átirányítás Azure RemoteApp USB-eszközök?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Eszközök átirányítása lehetővé teszi a felhasználóknak a számítógépen vagy táblagépen az alkalmazások között Azure RemoteApp csatolt USB-eszköz használata. Például ha megosztott Skype Azure RemoteApp keresztül, a felhasználók kell használni az eszköz fényképezőgépek.

További továbblépés előtt feltétlenül, olvassa el az [Azure RemoteApp használata átirányítás](remoteapp-redirection.md)USB átirányítás információkat. Azonban a javasolt nusbdevicestoredirect:s: * USB-webes fényképezőgépek valamiért nem működik, és az egyes USB-nyomtatók vagy USB-többfunkciós eszközök nem működnek. Tervezés és biztonsági okokból a Azure RemoteApp rendszergazda átirányításának engedélyezéséhez eszköz osztály globálisan egyedi azonosítója vagy példány Eszközazonosítót ahhoz a felhasználók használható eszközök.

Bár ez a cikk a webes kamera átirányítás folytatott beszélgetést, használhatja a hasonló megközelítés USB-nyomtatók és más USB-többfunkciós eszközök, amelyek nem irányította átirányítása a **nusbdevicestoredirect:s:*** parancsot.

## <a name="redirection-options-for-usb-devices"></a>USB-eszközök átirányítása lehetőségei
Azure RemoteApp nagyon hasonlít mechanizmusok USB-eszközök átirányítása szerint a lehetőségekből érhető el a távoli asztali szolgáltatásokat használja. A alaptechnológiát lehetővé teszi a megfelelő átirányítás módszer az egy adott eszközt, mind a legjobb magas szintű és RemoteFX USB-eszköz használatával átirányítása a **usbdevicestoredirect:s:** parancsot. Négy elemei erre a parancsra:

| Feldolgozási sorrendben | Paraméter           | Leírás                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Kijelöli az összes eszközön, amely a magas szintű átirányítás felvett nem. Megjegyzés:: tervezés * nem működik az USB-webes fényképezőgépek.  |
|                  | {Eszköz osztály globálisan egyedi azonosítója} | Kijelöli az összes eszközön, amelyek megfelelnek a megadott eszköz telepítési osztály.                                                           |
|                  | USB\InstanceID      | Az adott példányhoz azonosító megadott USB-eszköz kiválasztása                                                                  |
| 2                | -USB\Instance azonosító    | Eltávolítja a megadott eszköz az átirányítási beállításokat.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Egy USB-eszköz átirányítása az eszköz osztály globálisan egyedi azonosítója segítségével
Két módon keresse meg az eszköz osztály globálisan egyedi azonosítója átirányításhoz használható. 

Az első, hogy a [Rendszer általi eszköz beállítása osztályai számára elérhető szállítók](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx)használja. Válassza ki az eszközt, a helyi számítógéphez csatlakoztatott szükségleteknek leginkább megfelelő osztály. Digitális fényképezőgépek Ez lehet egy eszköz Imaging vagy videó rögzítése eszköz osztály. Végezze el az eszköz osztályok keresse meg a megfelelő osztály globálisan egyedi azonosítója, hogy működik-e a helyben csatlakoztatott USB-eszköz (abban az esetben, a webkamera) néhány kísérletezés kell.

Hatékonyabb módszerre, vagy a második lehetőség, az alábbi lépéseket követve keresse meg az eszközre osztály globálisan egyedi azonosítója:

1. Nyissa meg az Eszközkezelő parancsra, keresse meg az eszközt, hogy a rendszer átirányítja, és kattintson a jobb gombbal, és nyissa meg a Tulajdonságok parancsot.
![Nyissa meg az Eszközkezelő](./media/remoteapp-usbredir/ra-devicemanager.png)
2. A **Részletek** lapon válassza a tulajdonság **Osztály globálisan egyedi azonosítója**. Az érték, amely akkor jelenik meg az adott típusú eszköz osztály globálisan egyedi azonosítója.
![Kamera tulajdonságai](./media/remoteapp-usbredir/ra-classguid.png)
3. Az osztály globálisan egyedi azonosítója érték használatával átirányíthatja az eszközöket, amelyek felelnek meg.

Példa:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Az azonos parancsmag több eszköz átirányításának is összevonhatja. Példa: helyi tároló és egy USB-webkamera átirányításához parancsmag így néz ki:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Eszközök átirányítása beállításakor osztály globálisan egyedi azonosítója a rendszerünk átirányítja az összes eszközön, amelyek megfelelnek a megadott gyűjteményben osztály globálisan egyedi azonosítója. Ha például a helyi hálózaton több számítógépre, USB-webhely ugyanazon kamerával rendelkező esetén átirányítása összes a webes fényképezőgépek egyetlen parancsmag futtathatja.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Egy USB-eszköz átirányítása az Eszközazonosítót példány használatával

Ha külön pontosabban szeretné, és egy eszköz átirányítás szabályozni szeretné, a **USB\InstanceID** átirányítás paraméter is használhatja.

Ez a módszer nagyon nehéz részét keresi az USB-eszközazonosítót példány. Hozzáférés a számítógép és az adott USB-eszköz lesz szüksége. Ezután kövesse az alábbi lépéseket:

1. Az eszköz átirányítás Távoli asztali munkamenetben leírtak szerint engedélyezze [Hogyan használhatom az eszközök és források távoli asztali munkamenetben?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Nyissa meg a távoli asztali kapcsolaton, majd kattintson az **Egyéb beállítások elemre**.
3. A jelenlegi kapcsolatbeállítások RDP-fájl mentése a **Mentés másként** gombra.  
    ![A beállítások mentéséhez RDP-fájlként](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Válassza a fájl nevét és helyét, például "MyConnection.rdp" és "A PC\Documents", és mentse a fájlt.
5. Nyissa meg a MyConnection.rdp fájlt egy szövegszerkesztőben, és keresse meg az eszköz átirányítani kívánt példány azonosítója.

A példány azonosító most használni, a következő parancsmagot:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Segítse a szolgáltatás segítségével 
Tudta, hogy mellett ez a cikk minősítés, és a megjegyzések megadására lefelé alatt, módosíthatja a cikkben magát? Valami hiányzó? Valami hiba történt? DID lehet Írjon valamit, amely csak áttekinthetőbb legyen? Görgetés felfelé, és kattintson a **szerkesztése a GitHub** módosításához - e érkezzenek us véleményezésre, és majd, ha azt jelentkezzen, a őket, látni fogja a módosításokat, és itt javítása.