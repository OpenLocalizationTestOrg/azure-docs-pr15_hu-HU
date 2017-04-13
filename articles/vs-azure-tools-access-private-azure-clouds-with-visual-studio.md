<properties 
   pageTitle="A Visual Studio magánjellegű Azure felhő elérése |} Microsoft Azure"
   description="Megtudhatja, hogy miként Visual Studio segítségével saját felhő erőforrások eléréséhez."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>A Visual Studio magánjellegű Azure felhő elérése

##<a name="overview"></a>– Áttekintés

Alapértelmezés szerint a Visual Studio nyilvános Azure felhő többi végpontok támogatja. Ez lehet probléma, bár, ha használni szeretné a Visual Studio magánjellegű Azure felhő. Tanúsítványok használatával állítsa be a Visual Studio magánjellegű Azure felhő többi végpontok eléréséhez. Elérheti ezeket az Azure keresztül tanúsítványok beállításfájl közzététele.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>A magánjellegű Azure eléréséhez Visual Studio a felhőbe

1. Az [Azure klasszikus portált](http://go.microsoft.com/fwlink/?LinkID=213885) a személyes felhő töltse le a Közzététel beállításai fájlt, vagy a közzététel beállításfájl forduljon a rendszergazdához. Azure nyilvános verziójának töltse le ezt a hivatkozást az [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (A letöltött fájl kell kiterjesztése .publishsettings.)

1. A **Kiszolgáló Explorer** a Visual Studióban válassza az **Azure** csomópontot, és a helyi menüben válassza az **Előfizetések kezelése** parancsot.

    ![Az előfizetések parancs kezelése](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. **Microsoft Azure előfizetések kezelése** párbeszédpanelen kattintson a **tanúsítványok** fülre, és válassza az **Importálás** gombra.

    ![Azure tanúsítványok importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. **Importálás Microsoft Azure előfizetések** párbeszédpanelen tallózással keresse meg azt a mappát, ahol meg a közzétételi beállításokat tartalmazó fájl mentve, válassza a fájl, majd válassza az **Importálás** gombra. A tanúsítványok a közzétételi beállításokat tartalmazó fájl importálása Visual Studio. Most már tud majd kommunikálni a magánjellegű felhő erőforrásait.

    ![Importálás közzétételi beállításokat](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Következő lépések

[A Visual Studio-Azure Felhőszolgáltatásba közzététele](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Útmutató: letöltés importálása a Közzététel beállításai és az előfizetés adatai](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

