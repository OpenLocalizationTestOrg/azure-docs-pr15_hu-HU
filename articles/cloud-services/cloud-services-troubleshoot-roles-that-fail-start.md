<properties
   pageTitle="Nem indul el, szerepköröket elhárítása |} Microsoft Azure"
   description="Az alábbiakban néhány gyakori oka miért egy felhőalapú szolgáltatásba szerepkör előfordulhat, hogy nem indul el. A problémák megoldására is találhatók."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Nem indul el Felhőszolgáltatásában szerepköröket – problémamegoldás

Az alábbiakban néhány gyakori problémák és megoldások a szerepköröket, amelyek nem indul el, kapcsolódnak Azure Cloud Services.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Hiányzó dll-EK és függőségek

Nem reagál szerepkörök és szerepköröket **inicializálás**, **elfoglalt**és **leállítása** államok közötti váltás is okozhatja hiányzó DLL-ek vagy összeállítások.

A hiányzó DLL-ek vagy összeállítások jelei lehet:

- A szerepkör-példány **inicializálás**, **elfoglalt**és **leállítása** Államokban van váltás.
- A szerepkör-példány helyezte **készen áll arra,** hogy, de a webes alkalmazás nyissa meg a lapon nem jelenik meg.

Vannak a jelen hibák kivizsgálása több ajánlott módszereket.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>A webes szerepkörbe hiányzó DLL problémáinak diagnosztizálása

Ha egy webhely telepíti, nyissa meg az weblapon szerepkört, és a böngészőben jeleníti meg az alábbihoz hasonló kiszolgálóhiba, akkor jelezheti, hogy egy DLL hiányzik.

![Kiszolgálóhiba a "/" alkalmazást.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Egyéni hibák kikapcsolásával problémáinak diagnosztizálása

Részletesebb hibaadatokat megtekintheti a webes szerepkör az egyéni hiba mód beállításához ki állásba a web.config beállításával, és a szolgáltatás újbóli.

Részletesebb hibák megtekintése a távoli asztali használata nélkül:

1. A Microsoft Visual Studio alkalmazásban nyissa meg azt a megoldást.

2. Az **Intéző megoldást**keresse meg a fájlt, és nyissa meg.

3. A system.web szakaszban keresse meg a fájlt, és vegye fel a következő sort:

    ```xml
    <customErrors mode="Off" />
    ```

4. Mentse a fájlt.

5. Újracsomagolására és telepítsen újra a szolgáltatást.

Miután a szolgáltatás üzlethez van, látni fogja a egy hibaüzenet jelenik meg a hiányzó összeállítás vagy DLL nevét.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>A hiba távolról megtekintésével problémáinak diagnosztizálása

A szerepkör eléréséhez, és teljesebb hibaadatokat távolról tekintheti meg a távoli asztal is használhatja. A hibák megtekintése a távoli asztali használatával kövesse az alábbi lépéseket:

1. Győződjön meg arról, hogy telepítve van-e az 1.3-as vagy újabb Azure SDK.

2. A Visual Studio segítségével a megoldás a telepítés során válassza a "Konfigurálása távoli asztali kapcsolat...". A távoli asztali kapcsolat konfigurálásával kapcsolatos további tudnivalókért lásd: [A távoli asztal Azure szerepkörök](../vs-azure-tools-remote-desktop-roles.md).

3. A Microsoft Azure klasszikus portálon után a példány állapotjelzője, **készen áll**, kattintson a szerepkör példányok.

4. Kattintson a menüszalag **Távelérési** részén a **Csatlakozás** ikonra.

5. A távoli asztali beállítása során megadott hitelesítő adataival jelentkezzen be a virtuális gépen.

6. Nyissa meg a parancs ablakot.

7. Típus `IPconfig`.

8. Megjegyzés: a IPv4-cím értéket.

9. Nyissa meg az Internet Explorert.

10. Írja be a cím és a webes alkalmazás nevére. Ha például `http://<IPV4 Address>/default.aspx`.

Navigálás a webhelyen található most vissza pontosabb hibaüzenetek jelennek meg:

* Kiszolgálóhiba a "/" alkalmazást.

* Leírás: A esetén nem kezelt kivétel történt az aktuális webhely kérés végrehajtása során. Tekintse át a Papírhalom követés további információt a hibáról, és hol származik a kódot.

* A kivétel adatai: System.IO.FIleNotFoundException: nem tölthető fájl vagy összeállítás "Microsoft.WindowsAzure.StorageClient, verzió 1.1.0.0, kulturális = = semleges, NyilvánosKulcsTokenje = 31bf856ad364e35" vagy valamelyik függőségét. A rendszer nem találja a megadott fájlt.

Példa:

![A "/" alkalmazás közvetlen kiszolgálóhiba](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>A számítási irányító használatával problémáinak diagnosztizálása

A Microsoft Azure számítási irányító segítségével diagnosztizálása és a Hiányzó függőségek és web.config hibák elhárításához.

A legjobb eredményt az ezzel a módszerrel a diagnosztikai számítógépen vagy, amelynek tiszta telepítése a Windows virtuális gépen kell használni. Legjobb hasonlóan az Azure környezetben, használja a Windows Server 2008 R2 x64.

1. Telepítse az [Azure SDK](https://azure.microsoft.com/downloads/)önálló verzióját.

2. A fejlesztői gépen összeállítása a felhőalapú szolgáltatás project.

3. A Windows Intézőben keresse meg a felhőben szolgáltatási projekt a bin\debug mappát.

4. Másolja a .csx mappát és .cscfg fájlt a számítógépen, amelyen szeretné hibakeresése problémák esetén.

5. A TISZTÍT gépen, nyissa meg a egy Azure SDK parancssorablakot, és írja be `csrun.exe /devstore:start`.

6. A parancssorba írja be a `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Amikor megkezdi a szerepkört, látni fogja az Internet Explorer hiba részletes információt. Szabványos Windows hibaelhárítási eszközök segítségével további diagnosztizálása a problémát.

## <a name="diagnose-issues-by-using-intellitrace"></a>IntelliTrace használatával problémáinak diagnosztizálása

Dolgozó és a .NET-keretrendszer 4 használó webes szerepkörök használható [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), amely érhető el [A Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Kövesse ezeket a lépéseket követve IntelliTrace engedélyezve van a szolgáltatás üzembe helyezéséhez:

1. Győződjön meg arról, hogy telepítve van-e az 1.3-as vagy újabb Azure SDK.

2. A megoldást üzembe helyez a Visual Studio segítségével. A telepítés során jelölje be a **.NET-4-es szerepkörök engedélyezése IntelliTrace** jelölőnégyzetet.

3. A példány elindul, amint nyissa meg a **Kiszolgáló Explorer**.

4. Bontsa ki a **Azure\\Cloud Services** csomópontot, és keresse meg a telepítést.

5. Bontsa ki a telepítést, amíg meg nem jelenik a szerepkör-példányok. Kattintson a jobb gombbal a példányok egyik.

6. Válassza a **Nézet IntelliTrace naplók**. Az **Összefoglaló IntelliTrace** nyílik meg.

7. Keresse meg a kivételek csoportban az összefoglalásra. Kivételek vannak, ha a szakasz neve **Kivétel adatokat**.

8. Bontsa ki a **Kivétel adatokat** , és keresse meg a **System.IO.FileNotFoundException** hibákat, az alábbihoz hasonló:

![Kivétel, hiányzó fájlok vagy adatok összeállítás](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Hiányzó dll-EK és összeállítások cím

Hiányzó DLL- és összeállítási hibák megoldására kövesse az alábbi lépéseket:

1. A Visual Studio alkalmazásban nyissa meg azt a megoldást.

2. Az **Intéző megoldás** **hivatkozások** mappájának megnyitása

3. Kattintson az összeállítás azonosítani a hibát.

4. A **Tulajdonságok** ablaktáblában keresse meg a **helyi példányt tulajdonság** , és adja meg az értéket **Igaz**.

5. Telepítsen újra a felhőalapú szolgáltatást.

Miután ellenőrizte az összes hiba javítása után, telepítheti a a szolgáltatás nem ellenőrzi a **.NET-4-es szerepkörök engedélyezése IntelliTrace** jelölőnégyzetet.

## <a name="next-steps"></a>Következő lépések

További [hibaelhárítási cikkek](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) a felhőszolgáltatások megtekintése

Felhőalapú szolgáltatás szerepkör kapcsolatos problémák elhárítása az Azure PaaS számítógépes diagnosztikai adatok használatával című témakörben talál [Kevin Williamson blogsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
