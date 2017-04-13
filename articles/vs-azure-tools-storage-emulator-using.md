<properties 
   pageTitle="A tároló irányító használata a Visual Studio és konfigurálása |} Microsoft Azure"
   description="A tároló irányító használata a Visual Studio és konfigurálása"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>A tároló irányító használata a Visual Studio és konfigurálása

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>– Áttekintés
Az Azure SDK Fejlesztői környezet a tárhely irányító, egy segédprogramot, amely a Blob várólista és táblázat tároló szolgáltatást a helyi fejlesztési számítógépen Azure-ban elérhető tartalmazza. Ha Ön épület egy felhőalapú szolgáltatásba, amely alkalmaz a Azure tároló szolgáltatást, vagy írása minden külső alkalmazás, amely felhívja a tároló szolgáltatást, ellenőrizheti a kódot a tárhely irányító szemben a helyi meghajtóra. A Microsoft Visual Studio Azure eszközeit integrálhatja a Visual Studio a tárhely irányító kezelését. Az Azure-eszközök a tárhely irányító adatbázis első használat inicializálni, a tároló irányító szolgáltatás elindul, amikor a probléma, vagy a Visual Studio kód hibakeresési, és csak olvasható hozzáférést biztosít a tárhely irányító adatokhoz az Azure tároló Explorer keresztül.

Részletes tájékoztatást a tárhely irányító, többek között a rendszerkövetelmények és egyéni konfigurációs című témakörben talál [Az Azure tároló irányító fejlesztés és tesztelése](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Vannak bizonyos funkciókat és közötti különbségeket a tárhely irányító szimulációk a Azure tároló szolgáltatást. Lásd: [eltérések között a tárhely irányító és Azure tárolása](./storage/storage-use-emulator.md) Azure SDK dokumentációjában találhat a közötti különbségekről olvashat.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Kapcsolati karakterlánc konfigurálása tároló irányító

Hozzáférhet a tárhely irányító belül szerepkörbe kódot, érdemes konfigurálása a kapcsolati karakterlánc, amely a tárhely irányító mutat, és, később módosítható, mutasson az Azure tárterület-fiókhoz. Kapcsolati karakterlánc kereséskonfigurációs beállítás a szerepkör a tárterület-fiókhoz való csatlakozáshoz futásidőben olvashatja. A csatlakozási_karakterlánc létrehozásával kapcsolatos további tudnivalókért olvassa el a [az Azure alkalmazás konfigurálása](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage)című témakört.

>[AZURE.NOTE] A tárhely irányító fiókot a kódból hivatkozás a **DevelopmentStorageAccount** tulajdonság segítségével térhet vissza. Ezt a megközelítést megfelelően működik, ha azt szeretné, hogy a tárhely irányító hozzáférhet a kód, de ha közzé szeretné tenni az Azure alkalmazást, akkor kell Azure tárterület-fiókját, és módosítsa a kódot a kapcsolati karakterlánc használni a közzétételre kapcsolati karakterlánc létrehozása. Ha eddig a tárhely irányító fiók és az Azure tároló fiók gyakran, a kapcsolati karakterlánc leegyszerűsíti a folyamathoz.

## <a name="initializing-and-running-the-storage-emulator"></a>Inicializálása, és a tárhely irányító fut

Megadhatja, hogy futtatni vagy a szolgáltatás a Visual Studio hibakeresési, Visual Studio automatikusan elindul a tárhely irányító. A megoldás Intézőben nyissa meg a helyi menü a **Azure** projekthez, és válassza a **Tulajdonságok parancsot**. A **fejlesztői** lapon az **Azure tároló irányító indítása** listában válassza **Igaz** (Ha nincs már meg ezt az értéket).

Az első alkalommal futtat, vagy a szolgáltatás a Visual Studióban, hibakeresési a a tárhely irányító elindít egy inicializálni folyamatot. Ez a folyamat helyi portok fenntartja tároló irányító, és a tárhely irányító adatbázist hoz létre. Miután elkészült, a folyamat nem kell futtassa újra, kivéve, ha a program törli a tárhely irányító adatbázis.

>[AZURE.NOTE] Az Azure-eszközök a június 2012 kiadással indulva a tárhely irányító fut, az SQL Express LocalDB alapértelmezés szerint. A korábbi verziókban az Azure-eszközök a tárhely irányító fut alapértelmezett példány használata SQL Express 2005 vagy 2008, amelyen telepíteni kell, mielőtt telepítheti az Azure SDK. A tároló irányító egy SQL Express vagy megnevezett elnevezett példányát vagy az alapértelmezett példány használata a Microsoft SQL Server szemben is futtathatók. Ha a konfigurálása a tárhely irányító futtassanak kívül az alapértelmezett példány egy példánya van szüksége, olvassa el a [használata az Azure tároló irányító fejlesztés és tesztelése](./storage/storage-use-emulator.md).

A tároló irányító a helyi tároló szolgáltatások állapotának megtekintése, és szeretné kezdeni, le, majd az új őket felhasználói felületet biztosít. Miután elindult a tároló irányító szolgáltatás, jelenítse meg a felhasználói felületet vagy indítása, vagy állítsa le a szolgáltatást, kattintson a jobb gombbal a ikonja az értesítési területen a Microsoft Azure irányító a Windows tálcán.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Kiszolgáló Explorer tároló irányító adatainak megtekintése

Az Azure tároló csomópont Server Explorer birtokában megtekintheti az adatokat, és a tárhely fiókok, beleértve a tárhely irányító blob és a táblázat adatainak beállításainak módosítása. [Böngészési és kezelése tároló kiszolgáló Intézővel](https://msdn.microsoft.com/library/azure/ff683677.aspx) talál további információt.
