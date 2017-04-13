<properties 
   pageTitle="Egy közzétett felhőszolgáltatásba IntelliTrace és a Visual Studio hibakeresési |} Microsoft Azure"
   description="A közzétett felhőszolgáltatásba IntelliTrace és a Visual Studio hibakeresése"
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

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>A közzétett felhőszolgáltatásba IntelliTrace és a Visual Studio hibakeresése

##<a name="overview"></a>– Áttekintés

IntelliTrace jelentkezhet a szerepkör-példány teljes körű hibakeresési adatok végrehajtásakor Azure-ban. Ha szeretne megtalálni a probléma okát, végezze el a kódot a Visual Studióban, mintha az Azure-ban futó azt a IntelliTrace naplók segítségével. Gyakorlatilag azt mondja IntelliTrace kulccsal rekordok programkód és környezet adatok az Azure alkalmazás fut, mint az Azure-ban egy felhőalapú szolgáltatásba, és lejátszhatja a rögzített adatokat a Visual Studio segítségével. Alternatívájaként is használhatja a távoli hibakereséshez közvetlen csatolása egy Azure-ban futó felhőalapú szolgáltatást. Lásd: a [Felhőszolgáltatások hibakeresése során](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace csak hibakeresési esetek számára, és munkakörnyezeti telepítés nem használható.

>[AZURE.NOTE] IntelliTrace, ha a Visual Studio nagyvállalati telepítve van, és az Azure alkalmazás célok .NET-keretrendszer 4 vagy újabb verzióját is használhatja. IntelliTrace az Azure szerepkörök adatait gyűjti össze. A virtuális gépeken futó a szerepkörökhöz mindig a 64 bites operációs rendszerek futnak.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Az Azure alkalmazások IntelliTrace konfigurálása

IntelliTrace engedélyezi az Azure alkalmazáshoz, hozzon létre, és az alkalmazás a Visual Studio Azure projekt közzététele. Meg kell adnia IntelliTrace az Azure alkalmazás előtt Azure közzététele. Ha az alkalmazás közzététele IntelliTrace beállítása nélkül, de majd úgy dönt, hogy, akkor tegye közzé újra a Visual Studio az alkalmazást. További tudnivalókért lásd: a [közzétételi egy felhőalapú szolgáltatásba, az Azure eszközeivel](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Ha készen áll az Azure-alkalmazások telepítése, győződjön meg arról, hogy a projekt build célok szeretné **hibakeresése**vannak-e beállítva.

1. Nyissa meg a helyi menü az Azure projekt megoldás Explorerben, és válassza a **Közzététel**lehetőséget.
 
    A közzététel Azure varázsló jelenik meg.

1. Naplógyűjtés IntelliTrace az alkalmazás a felhőben közzétételekor, jelölje be a **IntelliTrace engedélyezése** jelölőnégyzetet.

    >[AZURE.NOTE] Lehetősége van engedélyezni a IntelliTrace vagy adatgyűjtés az Azure alkalmazás közzétételekor. Mindkettő nem engedélyezett.

1. IntelliTrace alapkonfiguráció testreszabásához válassza a **Beállítások** hivatkozásra.

    Az IntelliTrace beállításai párbeszédpanel jelenik meg, az alábbi ábrán látható módon. Megadhatja, hogy milyen események napló, e, a hívás vonatkozó információk gyűjtését, mely modulok és folyamatok összegyűjtéséhez jelentkezik és a felvétel foglaláshoz mennyi hely. IntelliTrace kapcsolatos további tudnivalókért lásd: [a IntelliTrace hibakeresés](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

A IntelliTrace napló (az alapértelmezés szerinti mérete 250 MB) IntelliTrace beállításaiban megadott maximális mérete körkörös naplófájlt. IntelliTrace naplók begyűjtési a virtuális gép a fájlrendszerben található fájlra. A naplók kérésekor pillanatkép abban az időpontban venni, és a helyi számítógépre letöltött.

Az Azure alkalmazás közzétételét követően az Azure, eldöntheti, ha IntelliTrace engedélyezve van a Azure kiszámítania csomópont a kiszolgáló Intézőben, az alábbi képen látható módon:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>A szerepkör-példány letöltési IntelliTrace naplók

A szerepkör-példány IntelliTrace naplók letölthető a **Kiszolgáló Explorer** **Cloud Services** csomópontot. Bontsa ki a **Cloud Services** csomópontot, amíg a példány érdeklik keresse meg azt, nyissa meg a helyi menü erre az előfordulásra vonatkozóan, és válassza a **IntelliTrace naplók megtekintése**. A IntelliTrace naplók töltődnek le a helyi számítógépen könyvtárában található fájlra. Minden alkalommal, amikor kér, hogy a IntelliTrace jelentkezik, létrejön egy új pillanatkép.

A naplók letöltésekor Visual Studio az Azure tevékenységnapló ablakban jeleníti meg a tevékenység előrehaladását. Ahogy az alábbi ábrán látható, akkor bővítheti a művelet további részleteket lásd a vonal elemre.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Továbbra is a Visual Studióban munka közben a IntelliTrace naplók töltődnek le. A napló befejezte letöltését, kattintva automatikusan megnyílik a Visual Studióban.

>[AZURE.NOTE] A IntelliTrace naplók tartalmazhat kivételeket, amelyek az keretrendszer hoz létre, és ezt követően kezeli. Belső keretrendszer kód részeként a normál szerepkör, indul, akkor előfordulhat, hogy nyugodtan figyelmen kívül hagyhatja azokat a kivételek hoz létre.

## <a name="see-also"></a>Lásd még:

[Hibakeresési Cloud Services](https://msdn.microsoft.com/library/ee405479.aspx)

