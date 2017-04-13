<properties
   pageTitle="Azure virtuális gép környezet, amelyben tölthetők le |} Microsoft Azure"
   description="Megtudhatja, hogy miként automatizált virtuális gép telepítési és konfigurálása a Microsoft Azure tölthetők le használatával"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Azure virtuális gép környezet, amelyben tölthetők le automatizálása

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Tölthetők le a kézbesítési automatizálási kiváló eszköz és állapot konfiguráció szükséges.

A legújabb cloud-api verziójának megjelenése az tölthetők le itt Azure-zökkenőmentes integráció, biztosítva kiépítése és üzembe konfigurációs államok egyetlen parancs segítségével.

Ebben a cikkben lehet kell követnie Azure virtuális gépeken futó kiépítése, és ismerteti, hogy hozzon létre egy házirendet, vagy a "Szakácskönyv", és kattintson a szakácskönyv bevezetéshez az Azure virtuális gép keresztül a tölthetők le környezet beállításához.

Lássunk hozzá!

## <a name="chef-basics"></a>Tölthetők le alapjai

Mielőtt elkezdené, e ajánlja fel, ellenőrizheti, hogy az alapfogalmak: tölthetők le. Nincs remek anyagi <a href="http://www.chef.io/chef" target="_blank">Itt</a> , valamint a javasoljuk, mielőtt megkísérelne az útmutató van egy gyors olvasható. Tudok azonban alapvető fog recap, azt az első lépések előtt.

Az alábbi ábra szemlélteti a magas szintű architektúrájának tölthetők le.

![][2]

Tölthetők le összetevője három fő építészeti: tölthetők le Server, tölthetők le ügyfél (csomópont) és tölthetők le Workstation.

A tölthetők le kiszolgáló a kezelési pont és két lehetőség a tölthetők le kiszolgáló: szolgáltatott megoldást vagy a helyszíni megoldás. Akkor használja a szolgáltatott megoldás.

A tölthetők le-ügyfél (csomópont), akkor a agent, hogy tartománya kiszolgálókon helyezkedik el.

A tölthetők le munkaállomás a rendszergazda munkaállomás, ahol azt a házirendek létrehozása, majd hajtsa végre a kezeléséhez szükséges parancsokat. Azt a parancsot **Kés** kezelését saját tölthetők le munkaállomásról.

Amely a "Cookbooks" és "Recepteket" is szerepel. Ezek a házirendek azt határozza meg, és alkalmazhatja azt kiszolgálóiról hatékony.

## <a name="preparing-the-workstation"></a>A munkaállomás előkészítése

Első lépésként lehetővé teszi, hogy a munkaállomás előkészítése. A szokásos Windows munkaállomás használom. A config fájlok és cookbooks tárolására szolgáló könyvtár létrehozása szükség.

Először létre kell hoznia egy C:\chef nevű könyvtárat.

Ezután hozzon létre c:\chef\cookbooks úgynevezett második könyvtárában.

Most már szükség az Azure fájl letöltése, hogy tölthetők le kommunikálni az Azure előfizetés.

Töltse le a közzétételi az [alábbi](https://manage.windowsazure.com/publishsettings/)beállításokat.

A közzétételi beállításokat tartalmazó fájl mentése C:\chef.

##<a name="creating-a-managed-chef-account"></a>Felügyelt tölthetők le fiók létrehozása

Szolgáltatott tölthetők le fiókot regisztrálni [Itt](https://manage.chef.io/signup).

A regisztrációs folyamat során a rendszer kéri egy új szervezeti létrehozásához.

![][3]

Amikor létrejött a szervezet, töltse le a starter Kit csomagban.

![][4]

> [AZURE.NOTE] Figyelmeztetés visszaáll az, hogy a billentyűk kérdés jelenik meg, érdemes ok van még nincs konfigurálva még meglévő infrastruktúra, a folytatáshoz.

A starter kit zip-fájl a szervezet config fájlokat és a billentyűk tartalmazza.

##<a name="configuring-the-chef-workstation"></a>A tölthetők le munkaállomás konfigurálása

Bontsa ki a C:\chef tölthetők le-starter.zip tartalmát.

Az összes fájl a tölthetők le-starter\chef-repó másolása\.tölthetők le a c:\chef címtárhoz.

A címtár most hasonlóan kell kinéznie az alábbi példa.

![][5]

Most már van, hogy felveszi a Azure közzétételi fájlt c:\chef legfelső szintű négy fájlokat.

PEM fájlok tartalmaznak a szervezet és a rendszergazda titkos kulcsokat kommunikáció, miközben a knife.rb fájl tartalmaz a Kés konfigurációs. A knife.rb fájl szerkesztése lesz szükség.

Nyissa meg a fájlt a szerkesztővel, valamint módosíthatja a "cookbook_path" eltávolítása a /. / a elérési, így látható a következő módon jelenik meg.

    cookbook_path  ["#{current_dir}/cookbooks"]

A következő is hozzáadhat a nevét az Azure tükröző sor beállításfájl közzététele.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

A knife.rb fájl letöltése az alábbi példa hasonlóan kell kinéznie.

![][6]

Ezek a sorok biztosítja kés c:\chef\cookbooks cookbooks könyvtárában hivatkozik, és a Azure Publish Settings fájlt is használja az Azure műveletek során.

## <a name="installing-the-chef-development-kit"></a>Az tölthetők le szoftverfejlesztői csomag telepítése

Következő [Töltse le és telepítse](http://downloads.getchef.com/chef-dk/windows) a ChefDK (tölthetők le Development Kit) tölthetők le munkaállomástól beállításához.

![][7]

Telepítse a c:\opscode alapértelmezett helye. A telepítés körül a 10 percig tart.

Erősítse meg, a PATH változó bejegyzéseket tartalmaz C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Ha még nem létezik, győződjön meg arról, hogy az elérési utak hozzáadása.

*MEGJEGYZÉS: AZ ELÉRÉSI ÚT SORRENDJÉNEK FONTOS!* Ha a opscode elérési út nem a megfelelő sorrendben problémák lesz.

Indítsa újra a munkaállomástól, a folytatás előtt.

Ezután a Kés Azure bővítmény telepíti azt. Ezzel a megoldással a "Azure a beépülő modul" Kés.

A következő parancsot.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] A – előtti argumentum köszönhetően igen kés Azure a beépülő modul legújabb készletét API-hoz való hozzáférést biztosító RC legújabb verzióját.

Valószínű, hogy egy függőségek számát is települ egy időben.

![][8]


Annak érdekében, hogy minden helyesen van beállítva, futtassa a következő parancsot.

    knife azure image list

Ha minden helyesen beállítva, megjelenik a rendelkezésre álló Azure képek Görgessen végig listáját.

Gratulálok. A munkaállomás be van állítva!

##<a name="creating-a-cookbook"></a>Egy Szakácskönyv létrehozása

Egy Szakácskönyv tölthetők le kíván végrehajtani a felügyelt ügyfél a parancsok definiálnak használt. Nem túl bonyolult feladat létrehozása egy Szakácskönyv, és azt a paranccsal **tölthetők le szakácskönyv készítése** saját Szakácskönyv sablon készítése. E fog hív a Szakácskönyv webkiszolgáló szeretném, hogy egy házirendet, amely automatikusan telepíti az IIS szerint.

A C:\Chef címtár csoportban a következő parancsot.

    chef generate cookbook webserver

A művelet létrehozza a könyvtárban C:\Chef\cookbooks\webserver fájlokat. Most már szükség azt szeretné, hogy a felügyelt virtuális gépen végrehajtani a tölthetők le ügyfél parancsok a definiálhatja.

A parancsok a fájl default.rb tárolja. A fájlban lehet fogja kell meghatározására parancsok IIS telepíti, az IIS és másolja a sablonfájlt wwwroot mappájában.

Módosítsa a C:\chef\cookbooks\webserver\recipes\default.rb fájlt, és írja be a következő sorokat.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Ha végzett, mentse a fájlt.

## <a name="creating-a-template"></a>Sablon létrehozása

Korábban említett azt, a default.html lapként használt sablonfájl létrehozásához szükséges.

A következő parancsot a sablon készítése.

    chef generate template webserver Default.htm

Nyissa meg most a C:\chef\cookbooks\webserver\templates\default\Default.htm.erb fájlt. Szerkessze a fájlt, néhány egyszerű "Helló, világ" HTML-kód hozzáadásával, és mentse a fájlt.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Töltse fel a Szakácskönyv a tölthetők le kiszolgáló

Ebben a lépésben azt vannak véve a Szakácskönyv, hogy a helyi számítógépen által létrehozott másolatát, majd feltölti a tölthetők le szolgáltatott kiszolgáló. Miután feltöltött, a Szakácskönyv jelenik meg a **házirend** lapon.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Egy kés Azure virtuális készülék terjesztése

Azt fogja most az Azure virtuális gép telepíteni, és a "Webkiszolgáló" Szakácskönyv, amelyen telepíti az IIS webes szolgáltatás és az alapértelmezett weblap alkalmazni.

Ehhez az **azure kés kiszolgáló létrehozása** paranccsal.

AM, például a parancs megjelenik a következő.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

A paraméterek magától értetődő. Az adott változó helyettesítő és futtatása.

> [AZURE.NOTE] Keresztül a a parancssorból lehet vagyok is automatizálása a végpontot hálózati szűrő szabályok – tcp-végpontok paraméter használatával. A weblaplapként megnyitott 80-as és az weblapon és RDP-munkamenet hozzáférést biztosít a 3389 be.

A parancs futtatása után az Azure portált, és láthatja, hogy a gép kiépítése megkezdéséhez.

![][13]

A parancssor jelenik meg a Tovább gombra.

![][10]

A telepítés befejezése után, akkor láthatja a való csatlakozáshoz a webes szolgáltatás 80-as port fölé azt a port volna megnyitni, amikor azt kiépítéstől parancsával kés Azure virtuális gépen. A csak a felhőbeli szolgáltatástól virtuális gép virtuális számítógéphez állapotában csatlakozom fogja azt felhőalapú szolgáltatás URL-címét.

![][11]

Amint látható, kaptam kreatív együtt a HTML-kód

Ne felejtse el azt egy RDP-munkamenet port 3389 keresztül az Azure klasszikus portálról keresztül is csatlakozhat.

E kívánom Ez hasznos lett! Lépjen és induljon el a infrastruktúra kód utazás az Azure még ma!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
