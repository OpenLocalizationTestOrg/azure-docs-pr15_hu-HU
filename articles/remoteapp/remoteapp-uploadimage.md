
<properties
    pageTitle="Egyéni kép feltöltése az Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogy miként tölthet fel egy egyéni képe az Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Egyéni kép feltöltése az Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Most, hogy az egyéni sablon képe nem hozott létre, vagy a módosításokkal frissített, kell, hogy a kép feltöltése a Azure RemoteApp kép tárba. Az alábbi lépésekkel.


## <a name="before-you-start"></a>Előzetes teendők

1.      Ellenőrizze, hogy a saját kép megfelel-e a [kép követelmények](remoteapp-imagereqs.md) és az [alkalmazás követelményeknek](remoteapp-appreqs.md).
2.      Telepítse az [Azure PowerShell-modult](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Lépésről lépésre vonatkozó egyéni kép feltöltése

1.      Nyissa meg az Azure Kezelőportálja, és nyissa meg a távoli alkalmazás formájában használva lapot.
2.      A **Webhelysablonok képét** lapon kattintson a **Töltse fel** az oldal alján.
4.      A kép egy rövid nevet, és adja meg a fiók tárolóhelyen. Győződjön meg róla a helyet a RemoteApp webhelycsoport ugyanazon a helyen, vagy egy helyet, ahová hozzon létre egy újat.
5.      Amikor a rendszer kéri, töltse le a parancsfájl a helyi számítógépre.
6.      A parancs paramétereket a szövegmezőbe a vágólapra másolása
7.      Nyisson meg egy magasabb szintű a Windows PowerShell-ablakot.
8.      A Windows PowerShell jogú ablakból nyissa meg a parancsfájlt letöltött tartalmazó ugyanabban a könyvtárban.
9.      Illessze be a másolt parancsot, és nyomja le az **ENTER billentyűt**.

    A feltöltés folyamat elindul, és időtartam függhet, számos tényező, többek között a hálózat sebességétől és a kép méretének

11.    Ha a feltöltés nem sikerül hálózati megszakítása vagy dolog, amit hasonlóan, mindig folytathatja a feltöltés folyamatának megkezdése. Ha folytatni szeretné egy feltöltés, újra a parancssorból azonos futtatása.

> [AZURE.WARNING] Soha ne módosítsa a feltöltés parancsfájl. Adott ellenőrzések annak érdekében, hogy a kép megfelel-e a kép követelmények és az alkalmazás követelmények végrehajtását.

## <a name="common-problems"></a>Gyakori problémák

- Ellenőrizze, hogy a Windows PowerShell, nem Azure PowerShell használata. Telepítenie kell ezeket a az Azure PowerShell-modult, mert bizonyos modulokat a feltöltés közben van szükség.
- Soha nem változtatja meg a parancsfájlt, az ellenőrzések vannak-e a kényelmesebbé.
- A virtuális fájl kizárja magát az alkalmazásból feltöltés során, ha a fájlt vagy helyezze át azt egy új helyre és a kísérlet feltöltés újra. Előfordulhat, hogy bizonyos megakadályozza, hogy a feltöltés a Windows folyamatot.  
