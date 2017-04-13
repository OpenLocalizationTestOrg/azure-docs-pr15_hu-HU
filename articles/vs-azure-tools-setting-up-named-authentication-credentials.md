<properties
   pageTitle="Állítsa be a hitelesítő nevű |} Microsoft Azure"
   description="Megtudhatja, hogyan hitelesítő adatok megadására, hogy a Visual Studio segítségével kéréseit az Azure Visual Studio Azure alkalmazás közzététele vagy egy meglévő felhőalapú szolgáltatást Lync-hitelesítést végezni. "
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

# <a name="setting-up-named-authentication-credentials"></a>Névvel ellátott hitelesítő beállítása

A Visual Studio Azure alkalmazás közzététele vagy Lync-egy meglévő felhőalapú szolgáltatást, meg kell adnia a hitelesítő adatokat, amelyekkel a Visual Studio kéréseit az Azure hitelesítést végezni. Vannak olyan több helyen, ahol jelentkezhet be a hitelesítő adatok megadására a Visual Studio. Például a kiszolgáló Intézőből, megnyithatja a helyi menü az **Azure** csomóponthoz és válassza a **Csatlakozás az Azure**. Amikor jelentkezik be, az előfizetési adatokat a Azure-fiókjához tartozó érhető el a Visual Studióban, és további kell tennie semmi nem.

Azure eszközök is támogat a egy régebbi módon, adja meg a hitelesítő adatokat, az előfizetést (.publishsettings fájl) segítségével. Ez a témakör ismerteti az ezzel a módszerrel Azure SDK 2.2 továbbra is használható.

Az alábbi elemek szükség a Azure-hitelesítést.

- Az előfizetés azonosítója

- Egy érvényes X.509 v3 tanúsítvány

>[AZURE.NOTE] A X.509 v3 tanúsítvány kulcs hosszát legalább 2048 bites kell lennie. Azure elutasítja bármely tanúsítvány, amely nem felel meg ezt a követelményt vagy, amely nem érvényes.

Visual Studio az előfizetés azonosítója, a tanúsítvány adatainak együtt használja, mint a hitelesítő adatait. Az előfizetés (fájl .publishsettings), amely tartalmazza a tanúsítvány nyilvános kulcs hivatkozott a megfelelő hitelesítő adatokkal. Az előfizetés fájl tartalmazhat több előfizetéshez tartozó hitelesítő adatokat.

Az előfizetési adatokat az **Új/szerkesztése előfizetési** párbeszédpanelen szerkesztheti a témakör későbbi című cikkben ismertetett módon.

Hozzon létre egy tanúsítványt, saját maga szeretné, ha [létrehozása, és töltse fel az Azure tartozó adatkezelési tanúsítvány](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) utasításait hivatkozik, és manuálisan töltse a tanúsítványt az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Ezek a hitelesítő adatok kezelése a felhőalapú szolgáltatások igénylő Visual Studio hitelesítést végezni szemben a Azure tároló szolgáltatást kérelmének szükséges hitelesítő adatokat nem.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Módosíthatja, vagy a Visual Studióban hitelesítő exportálása

Beállítása, módosítása, vagy exportálja a hitelesítő az **Új előfizetés** párbeszédpanelen, amely akkor jelenik meg, ha az alábbi műveletek valamelyikét hajtja végre:

- A **Kiszolgáló Explorer**nyissa meg a helyi menü az **Azure** csomóponthoz, válassza **Az előfizetések kezelése**, kattintson a **tanúsítványok** fülre, és válassza az **Új** vagy a **Szerkesztés** gombra.

- A **Közzététel Azure alkalmazás** varázslóban az Azure felhőszolgáltatásba közzétételekor válassza a **kezelés** listában **Válassza az előfizetés** , majd kattintson a tanúsítványok fülre, és válassza az **Új** vagy a **Szerkesztés** gombra.

Az alábbi eljárás feltételezi, hogy az **Új előfizetés** párbeszédpanel meg nyitva.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>A Visual Studióban hitelesítő beállítása

1. A **meglévő tanúsítvány jelölje ki** a hitelesítési lista válassza ki egy tanúsítványt.

1. A gombra kattintva **másolja a teljes elérési útját** . A tanúsítvány (.cer fájl) elérési útját másolja a vágólapra.

    >[AZURE.IMPORTANT] A Visual Studio Azure alkalmazás közzétételéhez a ebben a tanúsítványban töltse fel az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

1. A tanúsítvány az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)feltöltése:

    1. Válassza az Azure-portálon hivatkozásra.

         Ekkor megnyílik az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885) .

    1. Jelentkezzen be az [Azure klasszikus portálra](http://go.microsoft.com/fwlink/?LinkID=213885), és kattintson a **Cloud Services** gombra.

    1. Válassza a felhőbeli szolgáltatástól Önt érdeklő.

        A szolgáltatás a lapon nyílik meg.

    1. A **tanúsítványok** lapon válassza a **Feltöltés** gombra.

    1. Illessze be az imént létrehozott .cer fájl teljes elérési útját, és írja be a megadott jelszót.
