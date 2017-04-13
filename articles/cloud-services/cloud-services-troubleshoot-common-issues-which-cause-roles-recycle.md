<properties
   pageTitle="Újrafelhasználás Felhőszolgáltatásba szerepkörök okait |} Microsoft Azure"
   description="Egy felhőalapú szolgáltatás szerepkört, hogy a rendszer akkor váratlanul hasznosítja újra okozhatják jelentős legrövidebb leállás. Az alábbiakban a gyakori problémákat okozó szerepkörök kell a rendszer, amely segíthet csökkentheti az állásidőt."
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

# <a name="common-issues-that-cause-roles-to-recycle"></a>Gyakori szerepkörök Lomtár okozó problémák

Ez a cikk néhány telepítési problémák okait ismerteti, és hibaelhárítási tippek segít a probléma megoldásához. Jelzi, hogy probléma van az alkalmazás esetén a szerepkör-példány nem indul el, vagy azt a inicializálásakor, elfoglalt és leállítása államok közötti cycles.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Hiányzó futtatókörnyezet függőségek

Ha az alkalmazás szerepkörbe bármelyik összeállítás, amely nem része a .NET-keretrendszer vagy az Azure tár felügyelt, explicit módon szerepelnie kell, hogy összeállítás alkalmazáscsomag. Ne feledje, hogy más Microsoft keretek nem érhetők el a Azure alapértelmezés szerint. Ha szerepköre támaszkodik ilyen keretet, ezeket összeállítások alkalmazáscsomag hozzá kell adnia.

Mielőtt összeállítása és az alkalmazás csomag, ellenőrizze az alábbiakat:

- Ha a Visual studio használ, ellenőrizze a **Helyi példányt** tulajdonság értéke **Igaz** , az egyes hivatkozott összeállítás a projekt, amely nem része az Azure SDK vagy a .NET-keretrendszer.

- Győződjön meg arról, hogy a fájl nem hivatkozik bármely még nem használt összeállítások a fordítás elemet.

- A **Művelet összeállítása** minden .cshtml fájl **tartalom**van beállítva. Ez az biztosítja, hogy a fájlok jelenik meg megfelelően a csomagban, és lehetővé teszi, hogy más hivatkozott fájlok jelennek meg a csomagban.

## <a name="assembly-targets-wrong-platform"></a>Összeállítás célok megfelelő platform

Azure egy 64 bites környezetben. A 32 bites cél lefordított .NET szerelvények emiatt a Azure nem fognak működni.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Szerepkör esetén nem kezelt kivételek okoz közben inicializálni vagy leállítása

A kivételek okozott [RoleEntryPoint] osztály, amelyek tartalmazzák még a [OnStart] [OnStop]és módszerek [futtatása] , módszerekkel esetén nem kezelt kivételek. Esetén nem kezelt kivételt fordul elő, az alábbi lehetőségek közül választhat, ha a szerepkör fog Lomtár. Ha a szerepkör a többször van újrafelhasználás, akkor előfordulhat, hogy kell esetén nem kezelt kivétel kiváltása minden alkalommal, amikor megkísérli elindítani.

## <a name="role-returns-from-run-method"></a>A Futtatás módszer szerepkör adja eredményül.

A [Futtatás] módszerrel végtelen időre szóló futtatásához íródott. Ha a kód felülírja a [Futtatás] módszer, hogy inaktív állapotban kell maradnia végtelen időre szóló. Ha a [Futtatás] módot ad vissza, a szerepkör rendszer akkor hasznosítja újra.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Hibás DiagnosticsConnectionString beállítás

Ha az alkalmazás Azure diagnosztika használ, a szolgáltatás konfigurációs fájl kell adnia a `DiagnosticsConnectionString` beállításra. Ezt a beállítást meg kell határoznia a tárterület-fiók HTTPS-kapcsolat Azure-ban.

Ahhoz, hogy a `DiagnosticsConnectionString` beállítás helyességét, mielőtt beállítaná a alkalmazáscsomag Azure, ellenőrizze a következőket:  

- A `DiagnosticsConnectionString` pontokat beállítást Azure-ban egy érvényes tárterület-fiókkal.  
  Alapértelmezés szerint ez a beállítás mutat a emulált tároló fiók explicit módon, kell módosítása ezt a beállítást, mielőtt beállítaná a alkalmazáscsomag. Ha ez a beállítás nem módosítja, a szerepkör-példány indítsa el a diagnosztikai monitor alkalommal, amikor kivételt elő. Ez a szerepkör példányt végtelen időre szóló Lomtár jelenhet meg.

- A kapcsolati karakterlánc a következő [formátumban](../storage/storage-configure-connection-string.md)kell megadni. (A protokoll meg kell adni, a HTTPS.) *MyAccountName* cserélje ki a nevét, valamint a tárterület-fiókot, *MyAccountKey* az access használatával:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Ha az alkalmazás a Microsoft Visual Studio Azure-eszközök használatával, a [tulajdonságlapokat](https://msdn.microsoft.com/library/ee405486) használatával állítsa ezt az értéket.

## <a name="exported-certificate-does-not-include-private-key"></a>Exportált tanúsítványt nem része a titkos kulcs

A webes szerepkör SSL alatt futtatásához gondoskodnia kell arról, hogy az exportált management tanúsítvány tartalmazza-e a titkos kulcs. Ha a *Windows-kezelő tanúsítvány* használatával exportálja a tanúsítványt, feltétlenül **a titkos kulcs exportálását** beállítással válassza az **Igen lehetőséget** . A tanúsítvány PFX formátumban, amely jelenleg támogatott csak formátumban kell exportálni.

## <a name="next-steps"></a>Következő lépések

További [hibaelhárítási cikkek](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) a felhőszolgáltatások megtekintése

Újrafelhasználás a [Kevin Williamson blogsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)esetek további szerepkörének megjelenítéséhez.

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Futtatása]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
