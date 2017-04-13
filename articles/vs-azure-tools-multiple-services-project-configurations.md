<properties
   pageTitle="A Azure project több szolgáltatás konfiguráció beállítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként projektben Azure felhőalapú szolgáltatás konfigurálása a ServiceDefinition.csdef és ServiceConfiguration.cscfg fájlok módosításával."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>A Azure Project több szolgáltatás konfiguráció beállítása

Az Azure felhőalapú szolgáltatás project két konfigurációs fájl tartalmaz: ServiceDefinition.csdef és ServiceConfiguration.cscfg. Ezeket a fájlokat a felhőben Azure szolgáltatásalkalmazás mellékelve és Azure rendszerbe állított.

- A **ServiceDefinition.csdef** fájlt tartalmazza, hogy milyen szerepkörök benne többek között a felhőben szolgáltatásalkalmazás követelményei az Azure környezetben van szüksége a metaadat-alapú. A fájl a konfigurációs beállítások alkalmazása az összes előfordulását is tartalmaz. Ezeket a beállításokat a Azure szolgáltatás szolgáltatója futtatókörnyezet API futásidőben is olvashatók. Ez a fájl nem lehet frissíteni, a szolgáltatás futtatásakor Azure-ban.

- A **ServiceConfiguration.cscfg** fájl állítja be az értékeket a konfigurációs beállítások meghatározása a szolgáltatás-definíciós fájl, és futtassa az egyes szerepkör-példányokat számát adja meg. Ez a fájl frissíthető a felhőalapú szolgáltatás futtatásakor Azure-ban.

A Microsoft Visual Studio Azure eszközeit adja meg a tulajdonságlapot, amelyek segítségével az e-fájlokban tárolt konfigurációs beállítások megadása. A tulajdonságlapok kattintson duplán a szerepkör hivatkozás az Azure felhőalapú szolgáltatás project Solution Explorer, a videolejátszó alatt vagy kattintson a jobb gombbal a szerepkör hivatkozást, és válassza a **Tulajdonságok**, az alábbi ábrán látható módon.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Az alapul szolgáló sémák szolgáltatás meghatározására és szolgáltatás konfigurációs fájl kapcsolatos tudnivalókért lásd: a [Séma hivatkozás](https://msdn.microsoft.com/library/azure/dd179398.aspx). További információt a szolgáltatás konfigurációja megtudhatja, [hogy miként Cloud Services konfigurálása](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Szerepkör tulajdonságainak konfigurálása

Egy webes szerepkör és dolgozó szerepkörbe tulajdonságlapját hasonlóak, bár ki az alábbi szakaszok emelni, néhány különbségek vannak.

A **gyorsítótár** lapon konfigurálhatja az Azure-gyorsítótár-szolgáltatás.

### <a name="configuration-page"></a>A konfiguráció lapon

Az **beállítása** lapon megadhatja a tulajdonságok:

**Példányok**

A tulajdonság **példány** száma a szolgáltatás működnie kell a szerepkör példányainak száma.

A **virtuális méret** tulajdonság **Kis Extra**, **kicsi**, **Közepes**, **nagy**vagy **Nagy felesleges**.  További tudnivalókért lásd: a [Felhőszolgáltatások méretét](./cloud-services/cloud-services-sizes-specs.md).

**Indítási művelet** (Csak a webes szerepkör)

Megadhatja, hogy a Visual Studio kell indítsa el a HTTP-végpontok vagy a HTTPS-végpont, vagy mindkét webböngészőben hibakeresési indításakor a tulajdonság értékét.

Csak akkor, ha már megadta HTTPS-végpont a szerepkör a HTTPS-végpont lehetőség érhető el. Egy HTTPS-végpont tulajdonság **Végpontok** lapon adhatja meg.

Ha már hozzáadta a HTTPS-végpont, a HTTPS-végpont beállítás alapértelmezés szerint engedélyezve van, és a Visual Studio elindítja a végpont böngészőben nemcsak a böngészőben a HTTP-végpont hibakeresése során indításakor. Ez feltételezi, hogy engedélyezve vannak-e mindkét indítási beállítások megadása.

**Diagnosztikai**

A webes szerepkör alapértelmezés szerint engedélyezve van a diagnosztika. Az Azure felhő projekt- és szolgáltatásfiók használata a helyi tároló irányító vannak beállítva. Ha készen áll a Azure szeretne telepíteni, választhat a csapatépítő gomb (**…**) frissíteni a tárterület-fiókot Azure tárolási használni a felhőben. Diagnosztikai adatok átviheti a tárterület-fiók igény, vagy automatikusan rendszeres időközönként. Azure diagnosztika kapcsolatos további tudnivalókért olvassa el a [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](./cloud-services/cloud-services-dotnet-diagnostics.md)című témakört.

## <a name="settings-page"></a>Beállítások lap

A **Beállítások** lapon a szolgáltatás beállításai felvehet. Konfigurációs beállítások neve – érték párokká. A szerepkör a futó kód erről a megadott beállításokat használja a [Azure felügyelt tár](http://go.microsoft.com/fwlink?LinkID=171026)osztályok futásidőben értékeket. A [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) módszer kifejezetten, egy névvel ellátott konfigurációs beállítások futásidőben értékét adja eredményül.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Kapcsolati karakterlánc tárterület-fiókhoz konfigurálása

Kapcsolati karakterlánc kereséskonfigurációs beállítás kapcsolat és a hitelesítéshez információval szolgál a tárhely irányító vagy a Azure tároló fiókjához. Amikor a kódot kell Azure tároló szolgáltatások adatok eléréséhez – Ez azt jelenti, hogy blob, várólista vagy Táblázatadatok – egy szerepkörben futó kódból be kell meghatározása a tárterület-fiókhoz tartozó kapcsolati karakterlánc.

Kapcsolati karakterlánc, amely mutat Azure tárterület-fiókkal kell használnia egy definiált formátumnak. A csatlakozási_karakterlánc létrehozásával kapcsolatos további tudnivalókért lásd: [Azure tárhely a Csatlakozási_karakterlánc konfigurálása](./storage/storage-configure-connection-string.md).

Amikor készen áll a szemben a Azure tároló szolgáltatást a szolgáltatás tesztelése, vagy közvetlenül a telepítéshez használni a felhőalapú szolgáltatás Azure, módosíthatja bármely, mutasson a tárhely Azure-fiókjába a csatlakozási_karakterlánc értékét. Jelölje ki a (**…**), és válassza az **Enter tárolási fiók hitelesítő adatait**. Adja meg a fiókinformációkat, amely tartalmazza a fiók nevét, és a fiókkulcs. **Tárterület-fiók kapcsolati karakterlánc** párbeszédpanelen is jelezheti hogy szeretne-e a alapértelmezett HTTPS-végpont (az alapértelmezett beállítás), az alapértelmezett HTTP végpontok és az egyéni végpontok. Előfordulhat, hogy kívánja használni az egyéni végpontok, ha [egyéni tartománynevet blob-adatok az Azure tároló fiók konfigurálása](./storage/storage-custom-domain-name.md)ismertetett módon a szolgáltatásra, egyéni tartománynevet regisztrálta.

>[AZURE.IMPORTANT] Módosítania kell a kapcsolat karakterláncok, mutasson a tárhely Azure-fiókjába, mielőtt beállítaná a szolgáltatásban. Művelet sikertelen, előfordulhat, hogy a szerepkör nem indítása vagy a inicializálásakor, elfoglalt, és a leállítása állam közötti váltáshoz.

## <a name="endpoints-page"></a>Végpontok lapon

Dolgozói szerepkörbe beállíthatja, hogy HTTP, HTTPS vagy TCP végpontok tetszőleges számú. Beviteli végpontok, amely külső ügyfelek számára érhető el, és a érhetők el az egyéb szerepkörök, a szolgáltatást futtató belső végpontokon, a végpontokat is lehet.

- Elérhetővé szeretné tenni a HTTP zárólap külső ügyfelek és böngészők, a bemeneti végpont típusának módosítása, és adja meg nevét, és egy nyilvános portszámot.

- Elérhetővé szeretné tenni egy HTTPS-végpont külső ügyfelek és böngészők, **a bemeneti**az végpont típusának módosítása, és adja meg nevét, egy nyilvános portszámot és kezelési tanúsítvány nevét.

    Figyelje meg, hogy előtt megadhatja, hogy a kezelés tanúsítvány, meg kell adni a tanúsítvány **tanúsítványok** tulajdonság lapon.

- Elérhetővé szeretné tenni zárólap belső hozzáférést a felhőszolgáltatásában más szerepkörök, belső az végpont típusának módosítása, és adja meg nevét, és a végpont lehetséges magánjellegű portokat.

## <a name="local-storage-page"></a>Helyi tároló lap

A **Helyi tároló** tulajdonságlapja segítségével szerepkörbe egy vagy több helyi tárolási erőforrások lefoglalása. Helyi tároló erőforrás a fájlrendszerben az Azure virtuális gép, amelyben szerepkörbe egy példánya fut fenntartott könyvtárában.

## <a name="certificates-page"></a>Tanúsítványok lap

A **tanúsítványok** lapon társíthat tanúsítványok szerepköre. A tanúsítványok, amelyeket felvett a HTTPS-végpont beállítása a **Végpontok** tulajdonságlap használható.

A **tanúsítványok** tulajdonságlapja a tanúsítványok tájékoztatást ad a konfigurációjának. Figyelje meg, hogy a tanúsítványok nem mellékelve a szolgáltatás; a tanúsítványok külön-külön kell feltölteni az Azure az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)keresztül.

Szeretne társítani egy tanúsítványt szerepköre, adja meg a tanúsítvány nevét. Ez a név segítségével a tanúsítvány hivatkozik, a **Végpontok** tulajdonságlap HTTPS-végpont konfigurálásakor. Ezután adja meg, hogy a tanúsítvány tároló **Helyi számítógép** vagy az **Aktuális felhasználó** és a tár nevére. Végül adja meg a tanúsítvány ujjlenyomat. Ha a tanúsítványt az aktuális User\Personal (a) áruházból, adhatja meg a tanúsítvány ujjlenyomat választhatja ki a tanúsítvány feltöltött listáját. Tetszés szerinti helyen tárolhatja helyezkedik el, ha kézzel írja be a ujjlenyomat értéket.

Amikor felvesz egy tanúsítványt a tanúsítvány áruházból, bármilyen köztes automatikusan megjelennek a konfigurációs beállításait. Helyesen a SSL a szolgáltatás konfigurálása, ezek köztes is az Azure kell feltölteni.

Bármely társít a szolgáltatás kezelése a tanúsítványok alkalmazása a szolgáltatás csak akkor, ha fut a felhőben. A helyi fejlesztői környezet fut a szolgáltatás, használja a szabványos tanúsítvány, amely a számítási irányító kezeli.

## <a name="configuring-the-azure-cloud-service-project"></a>Az Azure felhőalapú szolgáltatás project konfigurálása

Egy teljes Azure felhőalapú szolgáltatás project vonatkozó beállításokat, először nyissa meg a helyi menü az, hogy a projekt csomópontját, és válassza a Tulajdonságok kattintva nyissa meg a tulajdonságlap. A következő táblázat mutatja az adott tulajdonság lapok.

|A tulajdonságlap|Leírás|
|---|---|
|Alkalmazás|Ezen a lapon megjelenítheti az Azure-eszközök, amelyek a felhőalapú szolgáltatás project használ, és az eszközök a legújabb verzióra frissítheti verzióját információt.|
|Események létrehozása|Ezen a lapon, az események előtti és utáni összeállítás is beállíthatja.|
|Fejlesztési|Ezen a lapon, az összeállítás konfigurációs utasításokat és utáni összeállítás eseményeket futtatásához feltételeket is megadhat.|
|Webes|Ezen a lapon, a beállításokat, a webkiszolgáló valamely adhatja.|
