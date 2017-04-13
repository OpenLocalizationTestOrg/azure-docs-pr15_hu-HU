<properties 
    pageTitle="Azure RemoteApp hibaelhárítás - alkalmazás indítása és a kapcsolat hibáit |} Microsoft Azure" 
    description="Megtudhatja, hogy miként problémáinak indítása és csatlakozás Azure RemoteApp alkalmazások." 
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
    ms.author="elizapo" />



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Azure RemoteApp – Hibaelhárítás indítási és a kapcsolat alkalmazáshibák 

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Az Azure RemoteApp alkalmazásainak indítandó okokból lehet, hogy nem. Ez a cikk ismerteti a különböző okai és a felhasználók kaphat, ha a hibaüzenetek próbál indítsa el az alkalmazást. Ez a csatlakozási hibák is bemutat. (De ez a cikk nem foglalkozik problémák, amikor megpróbál bejelentkezni az Azure RemoteApp ügyfél.)  

Olvassa el a leggyakoribb hibaüzeneteket alkalmazás indítása és a kapcsolat hibák miatt információt.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Azt érkezik beállításának... Próbálja meg ismét az 10 perc.

Ez a hiba azt jelzi, hogy Azure RemoteApp felhasználót szükséges a kapacitás teljesítéséhez van méretezését. A háttérben további Azure RemoteApp virtuális példányok éppen létrehozott kezelheti a felhasználók kapacitás igényeinek. A szokásos Ez körülbelül 5 percig tart, de legfeljebb 10 perc is eltelhet. Egyes esetekben előfordulhat ez elég gyorsan nem történik, és azonnal erőforrások szükségesek. Példa egy 9 AM eset: Ha sok felhasználó kell használni az alkalmazást az Azure RemoteAppn egyszerre. Ez történik, ha azt engedélyezheti a háttér **kapacitás módban** . Ehhez nyissa meg az Azure támogatási jegyek, és vagy e-mailek velünk a [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Minden egyes, az előfizetés azonosítója szerepeltetni a kérést.  

![Azt is beállításának első](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Tudta nem automatikus-újra az alkalmazások, indítsa újra el az alkalmazás  

Ez a hibaüzenet gyakran látható ha használta az Azure RemoteApp és 4 óra már alvó PC-n, majd írja és majd felébresztette PC-n az Azure RemoteApp ügyfél megpróbálja automatikus újracsatlakozáshoz és a határidő túllépte.  Kérje meg a felhasználóknak, hogy vissza szeretne térni az alkalmazást, és ismételje meg a nyissa meg a Azure RemoteApp ügyfélprogramból.

![Tudta nem automatikus-csatlakoznak az alkalmazások](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>A temp profillal kapcsolatos problémák 

Ez akkor fordul elő, ha a felhasználói profil (a felhasználói profil lemez) nem sikerült csatlakoztatni, és a felhasználó ideiglenes profil érkezett.  Rendszergazdák kell a gyűjteményben az Azure-portálon nyissa meg, és nyissa meg a **munkamenetek** fülre, majd megkísérli **Kijelentkezés** a felhasználó. Ez a felhasználói munkamenet - elhagyja a teljes naplózási kényszerítése, majd a felhasználót, próbálja meg ismét indítsa el az alkalmazást. Ha ez nem sikerül Azure ügyfélszolgálatát és vagy e-mail velünk a [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp leállt

Ez a hibaüzenet azt jelzi, hogy az Azure RemoteApp ügyfél problémát okoz, és újra kell indítani. Kérje meg a felhasználókat bezárásához: válassza ki, **Zárja be a program** , és indítsa újra az Azure RemoteApp ügyfél.  Ha a probléma továbbra is a Megnyitás és Azure támogatási jegyek és vagy e-mail velünk a [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp leállt](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Hiba történt a távoli asztali kapcsolat volt az erőforrás eléréséhez. Ismét a kapcsolat, vagy forduljon a rendszergazdához

Ez az általános hibaüzenetet - Azure ügyfélszolgálatát és vagy e-mail velünk a [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) , kiderítheti azt. 

![Általános Azure RemoteApp üzenet](./media/remoteapp-apptrouble/ra-apptrouble4.png) 