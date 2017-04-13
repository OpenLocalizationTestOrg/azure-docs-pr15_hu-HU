<properties
   pageTitle="Bármely Windows-alkalmazás futtatásához Azure RemoteApp bármilyen eszközön |} Microsoft Azure"
   description="További információ a felhasználókkal bármely Windows-alkalmazás megosztása Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Bármely Windows-alkalmazás futtatásához Azure RemoteApp bármilyen eszközről

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Futtathatja bárhol a Windows-alkalmazás bármely eszközön pillanatban, komolyan – csak az Azure RemoteApp használatával. Egyéni alkalmazás írt 10 évvel ezelőtti megjelenésével, vagy akár az Office-alkalmazásokat, a felhasználók nem kell többé egy bizonyos operációs rendszeren működik (például a Windows XP) az néhány alkalmazásokhoz kötni.

Azure RemoteApp a felhasználók is használja a saját Android vagy Apple eszközök és beszerzése a azonos felület gépe van a Windows (vagy Windows rendszerű telefonok). Ez végezhető el az alkalmazást a Windows Azure a Windows virtuális gépeken futó gyűjteménye – a felhasználók hozzá tudnak férni őket bármely internetkapcsolattal rendelkeznek. 

Példa pontosan ehhez az Olvasson tovább.

Ebben a cikkben megyünk megosztása az Access az összes azoknak a felhasználóknak. BÁRMELY alkalmazásban is használhatja. Mindaddig, amíg telepítheti az alkalmazást a Windows Server 2012 R2 rendszerű számítógépen, megoszthatja azt az alábbi lépésekkel. Ellenőrizheti, hogy az [alkalmazás – követelmények](remoteapp-appreqs.md) győződjön meg arról, hogy az alkalmazás fog működni.

Felhívjuk a figyelmét arra, hogy az Access adatbázis, és azt szeretné, hogy az adatbázis lehet hasznos, mert azt fogja kell módon néhány további lépést, hogy a felhasználók az access Access-adatok megosztása. Ha az alkalmazás nem egy adatbázist, vagy a felhasználóknak engedélyezni szeretné a fájlmegosztás elérésére nem szükséges, ezek a lépések az oktatóprogram kihagyhatja

> [AZURE.NOTE] <a name="note"></a>Az oktatóprogram elvégzéséhez Azure-fiók van szüksége:
> - [Nyissa meg az ingyenes Azure-fiók](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)van lehetősége: jóváírások kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a megszokott ezek után akár tarthatja a fiók, és használata ingyenes Azure szolgáltatások, például webhelyek. A hitelkártya soha nem megterheljük, kivéve, ha kifejezetten az beállítások módosítása és kérdezzen rá kell fizetnie.
> - [MSDN előfizetői előnyeinek aktiválása](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)közül választhat: az MSDN-előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.


## <a name="create-a-collection-in-remoteapp"></a>A webhelycsoport létrehozása a RemoteApp

Egy webhelycsoport létrehozásával kell kezdenie. A webhelycsoport az alkalmazások és a felhasználók számára a tároló szolgál. Egy képet minden egyes webhelycsoport alapuló – létrehozhat saját vagy egyikével ellátni, az előfizetést. Ebben az oktatóanyagban programmal mutatjuk be az Office 2013 próba kép – benne, hogy az alkalmazás, amely azt meg szeretné osztani.

1. Az Azure-portálon görgessen le a bal oldali navigációs fában RemoteApp látható. Nyissa meg a lapot.
2. Kattintson a **Létrehozás RemoteApp gyűjteménye**.
3. Kattintson a **gyors létrehozása** , és adja meg a webhelycsoport nevét.
4. Jelölje ki a régió, a webhelycsoport létrehozásához használni kívánt. Az optimális használat érdekében jelölje be a terület, amely a legközelebb földrajzilag arra a helyre, ahol a felhasználók érik el az alkalmazást. Ha például ebben az oktatóanyagban a felhasználók fog kerülni Redmond, Washington. A legközelebbi Azure terület eléri a **Nyugati US**.
5. Jelölje ki a használni kívánt számlázási csomagot. A számlázási alapszintű 16 felhasználók közben a szabványos számlázási terv 10 felhasználó rendelkezik egy nagy Azure virtuális egy nagy Azure virtuális helyezi. Általános példaként az alapszintű működik nagyszerű adatok bejegyzéstípus munkafolyamathoz. Egy hatékonyság-alkalmazásokban, például az Office kívánt a szabványos csomagot.
6. Végül jelölje be az Office 2013 Professional verzió képe. Ezen a képen az Office 2013-alkalmazásokat tartalmazza. Csak egy emlékeztetőt – ezen a képen célszerű csak a próba-webhelycsoportok és POCs. Akkor "ezen a képen egy gyártási webhelycsoport nem használhatók.
7. Most kattintson a **webhelycsoport létrehozása RemoteApp**.

![Felhőalapú gyűjtemény RemoteApp létrehozása](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

A webhelycsoport létrehozása elindul, de azt is igénybe vehet egy óra.

Most már készen áll a felhasználók hozzáadása.

## <a name="share-the-app-with-users"></a>Az alkalmazás megoszthatja a felhasználókkal

Ha a webhelycsoport létrehozása sikeresen befejeződött, pedig közzététele az Access felhasználókat, és adja hozzá a felhasználókat, akik azt hozzáféréssel kell rendelkeznie.

Ha megnyit a Azure RemoteApp csomópont nullától távolabbra volt a webhelycsoport létrehozása közben, először vissza az Azure kezdőlapján alkalmazással kezdeményezhet.

2. Kattintson a további beállítások eléréséhez és konfigurálása a webhelycsoport korábbi létrehozott gyűjteményben.
![Egy új RemoteApp felhő gyűjtemény](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. A **Közzététel** lapon kattintson a **Közzététel** a képernyő alján, és válassza a **Közzététel Start menü Programok**.
![Közzététel RemoteApp programhoz](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Jelölje ki a listából közzé szeretné tenni az alkalmazások. A célra választottunk Access. Kattintson a **kész**gombra. Várja meg az alkalmazások közzététel befejezéséhez.
![Közzététel az Access a RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Amikor az alkalmazás befejeződik a közzététel címsor fölé a **Felhasználói hozzáférés** fülre kattintva adja hozzá a felhasználókat, hogy az alkalmazások hozzáféréssel kell rendelkeznie. Adja meg a felhasználók nevével (e-mail cím), és kattintson a **Mentés**gombra.

![Felhasználók felvétele az RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Most pedig a felhasználók tájékoztatása a következő új alkalmazásokat, és hogyan érheti el őket. Ehhez a felhasználók küldjön egy üzenetet mutató őket a távoli asztali ügyfél letöltési URL-CÍMÉT.
![Az ügyfél RemoteApp URL-cím letöltése](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Állítsa be az Access programhoz

Egyes alkalmazások további konfiguráció szükséges, keresztül RemoteApp telepítése után. Mindenekelőtt eléréséhez, megyünk Azure, amely bármely felhasználó hozzáférhet a fájlmegosztás létrehozása. (Ha nem szeretné, hogy, létrehozhat egy [hibrid webhelycsoport](remoteapp-create-hybrid-deployment.md) [helyett a felhőben webhelycsoport], amely lehetővé teszi, hogy a felhasználók fájlok és a helyi hálózaton információk elérése.) Kell majd, akkor tájékoztassa a felhasználókat a számítógépén lévő helyi meghajtón hozzárendelése az Azure fájlrendszer.

Az első rész a rendszergazdájaként teheti meg. Ezután van néhány lépést a felhasználók számára.

1. Indítsa el a parancssor (cmd.exe) közzétéve. A **Közzététel** lapon jelölje be a **cmd parancsot**, és kattintson **Közzététel > Közzététel program elérési út segítségével**.
2. Adja meg az alkalmazást, és az elérési út nevét. A célra "a fájlkezelő használata" a neve és a "% SYSTEMDRIVE%\windows\explorer.exe" a mappa elérési útjaként.
![A Közzététel gombra a cmd.exe fájlt.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Most az Azure [tároló fiók](../storage/storage-create-storage-account.md)létrehozásához szükséges. Miénk "accessstorage," nevű, válassza ki, hogy értelmes nevet. (Misquote Highlander, hogy nem lehet csak egy "accessstorage.") ![Our Azure tárterület-fiók](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Most már térjen vissza az irányítópult, az elérési út érheti el a tárhely (végpont hely). Ezzel egy kicsit lesz, ezért ügyeljen valahol másolja.
![A tároló fiók elérési út](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Ezután a tárterület-fiók létrehozása után van szüksége az access elsődleges kulcs. Kattintson a **kezelés hívóbetűk**, és másoljon belőle az access elsődleges kulcs.
6. Ezután a környezet a tárterület-fiók beállítása, hozzon létre egy új fájlmegosztás hozzáférést. Futtassa a következő parancsmagok jogú a Windows PowerShell ablakban:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Úgy, hogy mi a megosztás ezeket a Futtatás parancsmagok:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Ezután következik a felhasználó. Első lépésként van a felhasználók [RemoteApp ügyfél](remoteapp-clients.md)telepítése. Ezután a felhasználók szükséges feleltesse meg egy adott Azure fájlmegosztás létrehozott fiókjából meghajtót, és adja hozzá az Access-fájljait. Ez az, hogy hogyan működnek, amely:

1. A RemoteApp ügyfélprogram esetén az access-a közzétett alkalmazások. Indítsa el a cmd.exe programot.
2. A következő parancsot a fájlmegosztás a számítógépről egy meghajtó megfeleltetéséhez:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Ha a **/ állandó** paraméter Igen értékre állítja, a megfeleltetett meghajtó munkamenetek között áll fenn.
1. Ezután indítsa el a Fájlkezelőben alkalmazás RemoteApp. A fájl megosztása a megosztott alkalmazásban használni kívánt Access fájlok másolása
![Fájlok elérése elhelyez egy Azure megosztása](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Végül nyissa meg az Accesst, és nyissa meg az adatbázist, amely csak megosztott. Meg kell jelennie a felhőből futó Access adatai.
![A valós a felhőből futó Access-adatbázisok](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Most már használja az Access bármilyen eszközről - Önnek elég gondoskodnia arról RemoteApp ügyfél telepítése.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Most, hogy elsajátította egy webhelycsoport létrehozása, próbáljon meg egy [webhelycsoport, amely használja az Office 365-ben](remoteapp-tutorial-o365anywhere.md). Vagy létrehozhat egy [hibrid webhelycsoport ](remoteapp-create-hybrid-deployment.md)hozzáférhetnek a helyi hálózaton.

<!--Image references-->
 
