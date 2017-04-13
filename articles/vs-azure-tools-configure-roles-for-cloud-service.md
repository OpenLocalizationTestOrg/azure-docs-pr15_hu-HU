<properties
   pageTitle="Állítsa be a szerepkörök az Azure felhőalapú szolgáltatás, a Visual Studio |} Microsoft Azure"
   description="Megtudhatja, hogy miként állíthatja be és szerepkörök Azure cloud Services konfigurálása a Visual Studio segítségével."
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

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Állítsa be a szerepkörök az Azure felhőalapú szolgáltatás, a Visual Studio

Az Azure felhőszolgáltatásba beállíthatja, hogy egy vagy több dolgozó vagy a webes szerepkörök. Minden szerepkörhöz kell határozza meg, hogy szerepkör beállításaitól és is beállíthatja, hogy szerepkör működésének. További információ a felhőszolgáltatások szerepkörök, a videó témakörben [Azure Cloud Services bemutatása](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). A felhőalapú szolgáltatás adatait tárolja a következő fájlokat:

- **ServiceDefinition.csdef**

    A szolgáltatás-definíciós fájl határozza meg, hogy a futtatókörnyezet beállításait a felhőalapú szolgáltatásba, például hogy milyen szerepkörök szükségesek, a végpontok és a virtuális gép méretét. A fájlban tárolt adatok egyike sem módosítható szerepköre futtatásakor.

- **ServiceConfiguration.cscfg**

    A szolgáltatás konfigurációs fájl szerepkörbe hány példánya futnak és az értékeket a szerepkörbe megadott beállítások konfigurálása A fájlban tárolt adatok módosítható a szerepköre futása közben.

Engedélyezni az alábbi beállítások különböző értékek tárolása szerepköre működésének, beállíthatja, hogy több szolgáltatás konfiguráció. Egy másik szolgáltatáscsaládba konfigurációs telepítési környezetben is használhatja. Ha például beállíthatja, hogy egy helyi szolgáltatás konfigurálása a helyi Azure tároló irányító használja, és hozzon létre egy másik szolgáltatás konfigurálása a felhőben az Azure tároló alkalmazás a tárhely fiók kapcsolati karakterláncot.

A Visual Studio hoz létre egy új Azure felhőalapú szolgáltatásba, két szolgáltatás konfiguráció alapértelmezés szerint létrejönnek. Ezek a konfigurációk a Azure projekt ad hozzá. A beállítások neve:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Az Azure felhőalapú szolgáltatás konfigurálása

Beállíthatja az Azure felhőszolgáltatásba megoldás Intézőből a Visual Studióban, az alábbi ábrán látható módon.

![Felhőalapú szolgáltatás konfigurálása](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Az Azure felhőalapú szolgáltatás konfigurálása

1. Azure **Megoldást Explorer**projektjét minden szerepkör beállításához nyissa meg a helyi menü, a szerepkör a Azure projektben, és válassza a **Tulajdonságok**.

    A Visual Studio-szerkesztő megjelenik egy lapon a szerepkör a nevet. A mezőket a **beállítás** lapon láthatók.

1. **Konfigurációjának** listájában válassza ki a használni kívánt szolgáltatás beállításai nevét.

    Ha azt szeretné, ha módosítani szeretné az összes a szerepkör a szolgáltatás beállításait, megadhatja, hogy **Az összes konfiguráció**.

    >[AZURE.IMPORTANT] Ha úgy dönt, hogy egy adott szolgáltatás konfigurációja, egyes tulajdonságai le vannak tiltva, mert azok csak beállítható, hogy az összes konfiguráció. A tulajdonságok módosítása az összes konfiguráció kell választania.

    Most már megadhatja bármely engedélyezett módosítása, hogy a Nézet fülre.

## <a name="change-the-number-of-role-instances"></a>Szerepkör-példányok számának módosítása

A felhőalapú szolgáltatás a teljesítmény javítása érdekében módosíthatja a szerepkör futó példányát, a felhasználók és a betöltés az adott jogosultság várható a alapján számát. Azure-ban a felhőbe szolgáltatás futtatásakor külön virtuális gép szerepkörbe minden példányában jön létre. Ez hatással van az a felhő szolgáltatás telepítése számlázását. Számlázással kapcsolatos további tudnivalókért lásd: [Microsoft Azure esetében a számla](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Szerepkör-példányok számának módosítása

1. Válassza a **beállítás** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza ki a frissíteni kívánt szolgáltatás beállításai.

    >[AZURE.NOTE] Beállíthatja, hogy az adott szolgáltatás konfiguráció esetén, vagy az összes szolgáltatás konfiguráció példányok száma.

1. **Példányok száma** mezőbe írja be a szerepkör elindítani kívánt példányok száma.

    >[AZURE.NOTE] Egyes példányok külön virtuális gépen fut, a felhőalapú szolgáltatás Azure közzétételekor.

1. Válassza a **Mentés** gombra a módosítások mentéséhez a szolgáltatás konfigurációs fájl az eszköztáron.

## <a name="manage-connection-strings-for-storage-accounts"></a>Kapcsolat karakterláncok tároló fiókok kezelése

Hozzáadása, eltávolítása vagy módosítása a kapcsolati karakterláncot a szolgáltatás konfigurációk. Másik szolgáltatáscsaládba konfigurációk esetén érdemes lehet különféle kapcsolati karakterláncot. Például érdemes lehet egy helyi kapcsolati karakterláncot az értéket tartalmazó helyi szolgáltatás konfiguráció `UseDevelopmentStorage=true`. Érdemes azt is, állítsa be a felhőalapú szolgáltatás konfiguráció, amely a tárterület-fiókot használ az Azure-ban.

>[AZURE.WARNING] Azure tároló kulcs fiókadatait tároló fiók kapcsolati karakterlánc beírásakor ezt az információt vannak tárolva helyileg a szolgáltatás konfigurációs fájl. Jó helyen jár ez az információ jelenleg nem szövegként tárolt titkosított.

Egy másik elemet az egyes konfigurációjának használatával nem rendelkezik a felhőalapú szolgáltatás használata különféle kapcsolati karakterláncot, vagy módosítsa a kódot, amikor teszi közzé a felhőalapú szolgáltatás Azure. A kód is használhatja ugyanazt a nevet a kapcsolati karakterlánc, és az érték különböző, szolgáltatás konfigurációja, ha generál a felhőalapú szolgáltatás, vagy a közzététel lehetőséget választja.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Kapcsolat karakterláncok tároló fiókok kezelése

1. Kattintson a **Beállítások** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza ki a frissíteni kívánt szolgáltatás beállításai.

    >[AZURE.NOTE] Frissítheti az egy adott szolgáltatás konfigurálása kapcsolat karakterláncok, de ha hozzáadása vagy törlése a kapcsolati karakterlánc kell választania kell a beállításokat.

1. Kapcsolati karakterlánc hozzáadásához válassza a **Beállítás hozzáadása** gombra. Új bejegyzés bekerül a listában.

1. **A szöveg mezőbe** írja be a nevet, amelyet a kapcsolati karakterlánc a használni kívánt.

1. A **típus** legördülő listában válassza a **Kapcsolati karakterlánc**.

1. A kapcsolati karakterlánc érték módosításához válassza a három pontra (…) gombra. Ekkor megjelenik a **Tárhely kapcsolati karakterlánc létrehozása** párbeszédpanel.

1. A helyi tároló fiók irányító használatához válassza a **Microsoft Azure tároló irányító** választógombot, és válassza ki a az **OK** gombra.

1. Azure-ban tárterület-fiók használata esetén válassza az **előfizetés** választógombot, és válassza ki a kívánt tárterület-fiókot.

1. Egyéni hitelesítő adatokat, válassza a beállítások **manuálisan kell megadni a hitelesítő adatok** gombra. Írja be a tárterület-fiók nevét, és az elsődleges vagy a második billentyűt. Információ arról, hogy miként hozhat létre egy tárterület-fiókot, és adja meg a részleteket a tárhely fiók **Tároló kapcsolati karakterlánc létrehozása** párbeszédpanelen annak témakörben [szeretne közzétenni, vagy a Visual Studio Azure alkalmazások telepítése](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Kapcsolati karakterlánc törléséhez jelölje ki a kapcsolati karakterláncot, és válassza a a **Beállítás eltávolítása** gombra.

1. Válassza a módosítások mentéséhez a szolgáltatás konfigurációs fájl az eszköztáron a **Mentés** ikonra.

1. A kapcsolati karakterláncot, a szolgáltatás konfigurációs fájl elérésére, akkor be kell szereznie a konfigurációs beállítás értékét. A következő kód szemlélteti egy példa, ahol blob-tárolóhoz hoz létre, és feltölteni a kapcsolati karakterlánc használatával adatok `MyConnectionString` a szolgáltatás konfigurációs fájl, amikor a felhasználó úgy dönt, **Button1** a webes szerepkör az Azure felhőalapú szolgáltatás Default.aspx lapján. Adja hozzá a következő Default.aspx.cs kimutatások használatával:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Nyissa meg a Default.aspx.cs Tervező nézetben, és felvehet egy gombot az eszköztárról. Adja hozzá a következő kódot a `Button1_Click` módot. Ez a kód használja `GetConfigurationSettingValue` úgy juthat az az érték a szolgáltatás konfigurációs fájl esetében a kapcsolati karakterlánc. Majd blob jön létre a tárterület-fiókot a kapcsolati karakterláncban hivatkozott `MyConnectionString` , és végül a program felveszi szöveget a blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Az Azure felhőszolgáltatásában használandó egyéni beállítások hozzáadása

A szolgáltatás konfigurációs fájl egyéni beállítások lehetővé teszik, akkor adjon meg egy nevet, és egy értéket az egy adott szolgáltatás konfigurálása. Előfordulhat, hogy szeretné egy szolgáltatás konfigurálása a felhőszolgáltatásában olvasása a beállítás értékét, és ezt az értéket a logika a kódban szabályozhatja e beállítás használatával. Anélkül, hogy a szolgáltatás csomagot vagy a felhőalapú szolgáltatás működik, ha újraépítéséhez módosíthatja ezeket a szolgáltatás konfigurációs értékeket. A kód egy beállítás megváltozásakor ellenőrizni az értesítéseket. Lásd: [RoleEnvironment.Changing esemény](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Hozzáadása, eltávolítása vagy a szolgáltatás konfigurációk egyéni beállításokat módosíthatja. Következő karakterláncok másik szolgáltatáscsaládba konfigurációk esetén érdemes lehet különböző értékeket.

Egy másik elemet az egyes konfigurációjának használatával nem rendelkezik a felhőalapú szolgáltatás használata különböző karakterláncok, vagy módosítsa a kódot, amikor teszi közzé a felhőalapú szolgáltatás Azure. A kód is használhatja ugyanazt a nevet a karakterlánc, és az érték különböző, szolgáltatás konfigurációja, ha generál a felhőalapú szolgáltatás, vagy a közzététel lehetőséget választja.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Egyéni a beállításokat az Azure felhőszolgáltatásában hozzáadása

1. Kattintson a **Beállítások** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza ki a frissíteni kívánt szolgáltatás beállításai.

    >[AZURE.NOTE] Egy adott szolgáltatás konfigurálása a karakterláncok frissítheti, de ha hozzáadása vagy törlése a karakterlánc kell választania kell a **Beállításokat**.

1. Egy karakterlánc hozzáadásához válassza a **Beállítás hozzáadása** gombra. Új bejegyzés bekerül a listában.

1. **A szöveg mezőbe** írja be a nevet, amelyet a karakterlánc használni kívánt.

1. A **típus** legördülő listában válassza a **karakterlánc értéket**.

1. Szeretne felvenni, vagy módosítsa a karakterlánc értékét, az **érték** szöveg mezőbe írja be az új értéket.

1. Karakterlánc törléséhez jelölje ki a, és válassza a **Beállítás eltávolítása** gombra.

1. Válassza a **Mentés** gombra a módosítások mentéséhez a szolgáltatás konfigurációs fájl az eszköztáron.

1. A karakterlánc a szolgáltatás konfigurációs fájl elérésére, akkor be kell szereznie a konfigurációs beállítás értékét.

    Akkor győződjön meg arról, hogy a következő használata kimutatások már hozzáadott Default.aspx.cs ugyanúgy, mint az előző eljárás elvégzett.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Adja hozzá a következő kódot a `Button1_Click` módszer, hogy a kapcsolati karakterlánc elérése ugyanúgy a karakterlánc eléréséhez. A kód végre tud hajtani a szolgáltatás konfigurációs fájl használt beállítások karakterlánc értéke alapján meghatározott kód.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Az egyes szerepkör-példány helyi tárhely kezelése

Helyi rendszer fájltárolás szerepkörbe minden példányában is hozzáadhat. Helyi adatok itt, amely nem szükséges, más szerepkörök elérhető tárolhat. Adatokról, nem kell egy táblázat, blob vagy SQL-adatbázis tárolási Mentés ide tárolhatók. Ha például használhatja gyorsítótár-adatok, előfordulhat, hogy újra kell használni kell a helyi tároló is. A gyorsítótárban tárolt adatokat másik szerepkör előfordulását nem használhatók. 

Helyi tároló beállítások alkalmazása az összes szolgáltatás konfiguráció. Csak hozzáadása, eltávolítása és módosítása az összes szolgáltatás konfiguráció helyi tároló.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Az egyes szerepkör-példány helyi tárhely kezelése

1. Kattintson a **Helyi tároló** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza **Az összes konfiguráció**.

1. Vegyen fel egy helyi tároló, válassza a **Helyi tár hozzáadása** gombot. Új bejegyzés bekerül a listában.

1. **A szöveg mezőbe** írja be a nevet, amelyet a helyi tárolására használni kívánt.

1. A **méret** szöveg mezőbe írja be a méret van szüksége a helyi tároló MB.

1. Szeretné eltávolítani az adatokat a helyi tároló, a rendszer a szerepkör a virtuális gép esetén, jelölje be a **szerepkör a karbantartás Lomtár** jelölőnégyzetet.

1. Egy meglévő helyi tároló bejegyzés módosításához válassza a sor, frissítenie kell. Akkor módosíthatja a mezőket a fenti lépések leírt módon.

1. Egy helyi tároló bejegyzés törléséhez válassza a tárterület-bejegyzést a listában, és válassza a **Helyi tároló eltávolítása** gombra.

1. A szolgáltatás konfigurációs fájlokat a módosítások mentéséhez válassza az eszköztáron a **Mentés** ikonra.

1. A helyi tároló felvett a szolgáltatás konfigurációs fájl elérésére, akkor be kell szereznie a helyi erőforrás kereséskonfigurációs beállítás értékét. A következő kódsorokat segítségével elérheti ezt az értéket, és hozzon létre egy **MyStorageTest.txt** nevű fájlt, és tesztadatokat egy szövegsor írja be a fájl. Ez a kód a `Button_Click` módszer, amelyet az előző eljárás:

1. Akkor győződjön meg arról, hogy a következő használata kimutatások vannak felvéve Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Adja hozzá a következő kódot a `Button1_Click` módot. A helyi tároló hoz létre a fájlt, és a próba adatot ír be, hogy a fájl.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Nem kötelező) Ez a felhőalapú szolgáltatás helyileg futtatásakor létrehozott fájlok megtekintéséhez kövesse az alábbi lépéseket:

  1. Futtassa a webes szerepkört, és válassza a **Button1** , győződjön meg róla, hogy a kód belül `Button1_Click` nevű kap.

  1. Az értesítési területen, a nyissa meg a helyi menü az Azure ikont, majd válassza a **Megjelenítése kiszámítania irányító felhasználói felület**. Az **Azure kiszámítania irányító** párbeszédpanel jelenik meg.

  1. Jelölje be a webes szerepkör.

  1. Válassza a menüsor **eszközök**, **Nyissa meg a helyi tárolóba**. A Windows Intéző-ablak jelenik meg.

  1. A menüsávon **MyStorageTest.txt** írja be a **keresett** szöveg mezőbe, és válassza a keresés indítása az **ENTER billentyűt** .

    A keresési eredmények között jelenik meg a fájlt.

  1. A fájl tartalmának megtekintéséhez nyissa meg a helyi menü a fájlt, és válassza a **Nyissa meg**.

## <a name="collect-cloud-service-diagnostics"></a>Felhőalapú szolgáltatás diagnosztika összegyűjtése

Diagnosztikai adatgyűjtés az Azure felhőalapú szolgáltatáshoz. Az adatok bekerül a tárterület-fiókjába. Másik szolgáltatáscsaládba konfigurációk esetén érdemes lehet különféle kapcsolati karakterláncot. Például érdemes lehet egy helyi tároló fiók UseDevelopmentStorage az értéket tartalmazó helyi szolgáltatás konfiguráció = true. Érdemes azt is, állítsa be a felhőalapú szolgáltatás konfiguráció, amely a tárterület-fiókot használ az Azure-ban. Azure diagnosztika kapcsolatos további tudnivalókért olvassa el a naplózás adatok összegyűjtése Azure diagnosztika segítségével című témakört.

>[AZURE.NOTE] A helyi szolgáltatás beállításai már helyi erőforrások használatára van beállítva. Ha a felhőalapú szolgáltatás beállításai közzététele az Azure felhőalapú szolgáltatást használ, a kapcsolati karakterlánc megadott közzétételekor is használható kapcsolat diagnosztika karakterlánchoz, kivéve, ha a kapcsolati karakterlánc megadott. Ha csomagolásakor a Visual Studio segítségével felhőalapú szolgáltatás, a kapcsolati karakterláncot az a szolgáltatás beállításai nem változik.

### <a name="to-collect-cloud-service-diagnostics"></a>A gyűjtendő felhőalapú szolgáltatás diagnosztika

1. Válassza a **beállítás** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza a szolgáltatás beállításai, amelyet frissíteni, vagy válassza ki **Az összes konfiguráció**.

    >[AZURE.NOTE] Módosíthatja, hogy egy adott szolgáltatás konfigurálása a tárterület-fiók, de ha engedélyezése vagy letiltása a diagnosztika választania kell a beállításokat.

1. Diagnosztikai engedélyezéséhez jelölje be a **Diagnosztika engedélyezése** jelölőnégyzetet.

1. Az érték a tárterület-fiókom módosításához válassza a három pontra (…) gombra.

    Ekkor megjelenik a **Tárhely kapcsolati karakterlánc létrehozása** párbeszédpanel.

1. Helyi kapcsolati karakterlánc használatához válassza Azure tároló irányító lehetőséget, és válassza az **OK** gombra.

1. Az Azure előfizetéshez tartozó tárterület-fiók használata esetén válassza ki az **előfizetése** funkciót.

1. A helyi kapcsolati karakterláncot az tárterület-fiók használata esetén válassza ki a **manuálisan kell megadni a hitelesítő adatok** funkciót.

    További információ arról, hogy miként hozhat létre egy tárterület-fiókot, és adja meg a részleteket a tárhely fiók **Tároló kapcsolati karakterlánc létrehozása** párbeszédpanelen annak témakörben [szeretne közzétenni, vagy a Visual Studio Azure alkalmazások telepítése](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. A **fiók nevét**a használni kívánt tároló fiók kiválasztása

    Ha manuális beírása a tárhely fiók hitelesítő adatait, másolja vagy írja be az elsődleges kulcs **fiókkulcs**. A kulcs az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)is másolható. Másolni szeretné a kulcsot, ezeket a lépéseket a **Tárterület-fiókok** megtekintése az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885):
    
  1. Jelölje ki a tárterület-fiókot a felhőalapú szolgáltatás szeretne használni.

  1. Válassza a **Hívóbetűk kezelése** gomb a képernyő alján található. A **Hívóbetűk kezelése** párbeszédpanel jelenik meg.

  1. A hívóbetű másolni, válassza a **vágólapra másolás** gombra. Most már a **fiókkulcs** mezőbe illessze be a kulcsot.

1. A tároló fiók ad meg, mint a kapcsolati karakterlánc a diagnosztikai (és gyorsítótárazás) közzéteheti a felhőalapú szolgáltatás Azure, a **frissítés fejlesztési tároló kapcsolat karakterláncok diagnosztikai és Azure adathordozós gyorsítótár-fiók hitelesítő adatait, amikor Azure közzététele** jelölőnégyzet bejelölésével használatára.

1. Válassza a **Mentés** gombra a módosítások mentéséhez a szolgáltatás konfigurációs fájl az eszköztáron.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>A virtuális gép minden szerepkör használt méretének módosítása

Beállíthatja, hogy minden szerepkörhöz virtuális gép méretét. Csak a mérete az összes szolgáltatás konfiguráció állíthat be. Ha bejelöli a gép kisebb méretű, majd Processzor magmintákat, memóriát, valamint a merevlemezre kevesebb tárterület történik. Az elosztott sávszélesség is értéke kisebb. További információt a következő méretét és a hozzárendelt erőforrások [Cloud Services méretben](cloud-services/cloud-services-sizes-specs.md)látható.

Azure virtuális gépeken szükséges erőforrásokat hatással van a felhőalapú szolgáltatást futtató Azure-ban költségét. Azure számlázással kapcsolatos további tudnivalókért lásd: [Microsoft Azure esetében a számla](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>A virtuális gép méretének módosítása

1. Válassza a **beállítás** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza **Az összes konfiguráció**.

1. Jelölje ki a virtuális gép a szerepkör a méretét, válassza a megfelelő méretét a **virtuális méret** listából.

1. Válassza a **Mentés** gombra a módosítások mentéséhez a szolgáltatás konfigurációs fájl az eszköztáron.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Végpontok és tanúsítványok a szerepkörök kezelése

A Protocol (protokoll), a portszámot megadásával és HTTPS-, az SSL-tanúsítvány adatait hálózati végpontok beállíthatja. Mielőtt június 2012 kiadásai HTTP, a HTTPS és a TCP támogatja. A június 2012 kiadás azok protokollok és UDP. A számítási irányító a bemeneti végpontok UDP nem használható. A Protocol (protokoll) csak a belső végpontokat is használhatja.

Javíthatja a Azure felhőszolgáltatásba biztonsága, a HTTPS protokollt használó végpontok hozhat létre. Ha például ha egy felhőalapú szolgáltatásba való vételi megrendeléseket vevők által használt, érdemes győződjön meg arról, hogy adataikat biztonságos SSL használatával.

Belső használható vagy a külső felekkel, a végpontokat is hozzáadhat. Külső végpontok beviteli végpontok nevezik. Beviteli zárólap lehetővé teszi, hogy a felhasználók a felhőben szolgáltatásban egy másik access pontra. Ha a WCF-szolgáltatás, érdemes lehet kattintva jelenítse meg a webes szerepkör érheti ezt a szolgáltatást a belső végpont.

>[AZURE.IMPORTANT] Csak akkor frissíthető az összes szolgáltatás konfiguráció végpontok.

Ha HTTPS-végpont ad hozzá, akkor használja az SSL-tanúsítvány. Ehhez tanúsítványok társítása szerepköre, az összes szolgáltatás konfiguráció, és használja a végpontok.

>[AZURE.IMPORTANT] A szolgáltatás nem csomagolása tanúsítványok. A tanúsítványok külön-külön kell feltölteni az Azure az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)keresztül.

A szolgáltatás konfigurációk társítania bármely kezelése a tanúsítványok csak a felhőalapú szolgáltatás fut Azure alkalmazhatók. Ha a helyi fejlesztői környezet fut a felhőalapú szolgáltatás, olyan szabványos tanúsítvány, kezeli az Azure számítási irányító használja.

### <a name="to-add-a-certificate-to-a-role"></a>A szerepkörbe felvenni egy tanúsítványt

1. Kattintson a **tanúsítványok** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza **Az összes konfiguráció**.

    >[AZURE.NOTE] Szeretne hozzáadni vagy eltávolítani a tanúsítványok, jelölje be az összes konfiguráció. Szükség esetén frissítheti a név és az egy adott szolgáltatás konfigurálása a ujjlenyomat.

1. A szerepkör a tanúsítvány hozzáadásához válassza a **Tanúsítvány hozzáadása** gombra. Új bejegyzés bekerül a listában.

1. **A szöveg mezőbe** írja be a tanúsítvány nevét.

1. A **Tár helye** listában válassza ki a helyet, a tanúsítvány, amelyet fel szeretne.

1. A **Tárolási név** listában válassza az üzlet, jelölje ki a tanúsítvány kívánt.

1. A tanúsítvány hozzáadásához válassza a három pontra (…) gombra. A **Windows biztonsági** párbeszédpanel jelenik meg.

1. Válassza ki azt a tanúsítványt, amely a listából, és válassza az **OK** gombra.

    >[AZURE.NOTE] Amikor felvesz egy tanúsítványt a tanúsítvány áruházból, a rendszer automatikusan hozzáadja bármely köztes meg a beállítások. Köztes tanúsítványok annak érdekében, hogy helyesen állítsa be a szolgáltatás SSL is kell Azure feltöltve.

1. Ha törölni szeretne egy tanúsítványt, a tanúsítványt válassza, és válassza a a **Tanúsítvány eltávolítása** gombra.

1. Az eszköztáron, és a módosítások mentéséhez szolgáltatás konfigurációs fájl válassza a **Mentés** ikonra.

### <a name="to-manage-endpoints-for-a-role"></a>A szerepkör a végpontok kezelése

1. Kattintson a **Végpontok** fülre.

1. A **Szolgáltatás konfigurációja** listában válassza **Az összes konfiguráció**.

1. Zárólap hozzáadásához válassza a **Végpont hozzáadása** gombra. Új bejegyzés bekerül a listában.

1. **A szöveg mezőbe** írja be a nevet, amelyet a végpont használni kívánt.

1. Válassza ki a végpontot, akkor a **típus** listában.

1. A **Protocol** -listáról, válassza ki a szükséges végpont protokollt.

1. Beviteli zárólap esetén **Nyilvános Port** mezőbe írja be a nyilvános portot kell használni.

1. A **Magánjellegű Port** mezőbe írja be a magánjellegű portot kell használni.

1. Ha a végpontok van szüksége a https protokollt, a **SSL-tanúsítvány neve** listában válassza a egy tanúsítványt használatához.

    >[AZURE.NOTE] Ebben a listában a tanúsítványok, amelyeket felvett a szerepkör a **tanúsítványok** lapon láthatók.

1. Válassza a **Mentés** gombra a módosítások mentéséhez szolgáltatás konfigurációs fájl az eszköztáron.

## <a name="next-steps"></a>Következő lépések
További tudnivalók a Visual Studióban Azure projektek [az Azure-projekt beállítása](vs-azure-tools-configuring-an-azure-project.md)elolvasásával. További tudnivalók a felhőalapú szolgáltatás séma [Séma hivatkozás](https://msdn.microsoft.com/library/azure/dd179398)elolvasásával.
