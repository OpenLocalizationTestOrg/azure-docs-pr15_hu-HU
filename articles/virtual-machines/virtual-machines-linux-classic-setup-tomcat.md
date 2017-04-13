<properties
    pageTitle="Kattintson egy Linux virtuális Apache Tomcat beállítása |} Microsoft Azure"
    description="Ismerje meg, hogy miként állíthat be Apache Tomcat7 az Azure virtuális gép (virtuális) Linux operációs rendszert futtató használatával."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>A Microsoft Azure virtuális Linux gépen Tomcat7 beállítása

Apache Tomcat (vagy egyszerűen Tomcat, korábban is Dzsakarta Tomcat) egy Megnyitás webkiszolgáló, és a Apache szoftver Foundation (ASP) által fejlesztett servlet tároló. Tomcat végrehajtja a Java Servlet és a vasárnap Microsystems JavaServer lapok (JSP) alkalmazásra vonatkozó technikai adatok, és a tiszta Java HTTP webkiszolgálói környezetben, amelyben Java-kód futtatásához. A legegyszerűbb konfigurációban Tomcat folyamat egyetlen operációs rendszer fut. Ez a folyamat fut Java virtuális gépet (JVM). Minden HTTP kérelem böngészőben Tomcat, a Tomcat folyamat egy külön szál feldolgozása.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ez az útmutató tomcat7 telepítése egy Linux képet, és beállítaná őket a Microsoft Azure-ban.  

Ismerkedhet meg:  

-   Hogyan lehet virtuális gép létrehozása az Azure-ban.
-   Hogyan lehet a virtuális gép előkészítése tomcat7.
-   Hogyan lehet telepíteni tomcat7.

Feltételezzük, hogy a képernyőolvasók már van egy Azure-előfizetést.  Ha nem tudja regisztrál az ingyenes próbaverziót [http://azure.microsoft.com](https://azure.microsoft.com/)a. Ha az MSDN előfizetéssel rendelkezik, olvassa el a [Microsoft Azure speciális árak: MSDN MPN és Bizspark előnyeinek](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Azure kapcsolatos további tudnivalókért lásd: [Mi az Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Ez a témakör feltételezi, hogy tomcat és Linux alapismeretei munka.  

##<a name="phase-1-create-an-image"></a>1 fázis: Hozzon létre egy kép
Ez a fázis létrehoz egy Linux kép használata Azure virtuális géphez.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Lépés: 1: Egy SSH hitelesítési kulcs generálása
SSH fontos eszközök rendszergazdák számára. Az access biztonsági alapján emberi által meghatározott jelszó beállítása viszont nem ajánlott. Rosszindulatú felhasználók is szétválasztani a rendszer felhasználónév és jelszó gyenge alapján.

A jó hír az, hogy van-e hagyja nyitva a távoli hozzáférés, és nem kell aggódnia amiatt jelszavak lehetőséget. Hitelesítéssel aszimmetrikus titkosítási módszer áll. A felhasználó titkos kulcs, amely megadja a hitelesítési. Jelszó-hitelesítést teljesen letiltása a felhasználói fiók még zárolhatja.

Egy másik ezt a módszert előnye, hogy nem szükséges bejelentkezni a különböző kiszolgálók különböző jelszavakat. A személyes titkos kulccsal összes-kiszolgálókon, amelyeket nem lehet az, hogy ne feledje, hogy több jelszavak hitelesítheti.

Hogy az is lehetséges, ezzel a módszerrel jelszó védő bejelentkezni.

Kövesse ezeket a lépéseket a SSH hitelesítési kulcs generálása.

1.  Töltse le és telepítse a következő helyen található puttygen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Futtassa a PUTTYGEN. EXE.
3.  Kattintson a **Létrehozás** az kulcs létrehozásához. A folyamat növelésével véletlenszerűséget helyezve az egérrel a ablakot az üres területre.  
![][1]
4.  A létrehozás folyamat után Puttygen.exe jelennek meg a létrehozott használatával. Példa:  
![][2]
5.  Jelölje ki és másolja a vágólapra a nyilvános kulcs **billentyűt** , és mentse publicKey.pem nevű fájlban. Nem kattintson a **Mentés nyilvános kulcs**, mert a mentett nyilvános kulcs fájl formátuma eltér szeretnénk nyilvános kulcs.
6.  Kattintson a **Mentés titkos kulcs** gombra, és mentse a privateKey.ppk nevű fájlban.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Lépés: 2: A kép elkészítéséhez az Azure-portálon.
Az [Azure portálon](https://portal.azure.com/)kattintson az **Új** a tálcájának hozhat létre a képet, a szükségletek Linux kép kiválasztása gombra. Az alábbi példában a Ubuntu 14.04 képet.
![][3]

A **Host Name mezőbe** , adja meg a nevét, és az Internet-ügyfélprogramok hozzáférhetnek a virtuális számítógéphez URL-CÍMÉT. Határozza meg a DNS-neve, például a tomcatdemo azonosító utolsó részét, és Azure hoz létre a tomcatdemo.cloudapp.net URL-cím.  

**SSH hitelesítési kulcsot**a kulcs értékét másolja a **publicKey.pem** fájl, amely tartalmazza a nyilvános kulcshoz puttygen által generált.  
![][4]

Egyéb beállítások konfigurálása, szükség szerint, és kattintson a Létrehozás gombra.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>2 fázis: A virtuális gép előkészítése Tomcat7
Ez a fázis végpont tomcat forgalom konfigurálása, és csatlakoztassa a virtuális új számítógépre.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Lépés: 1: Nyissa meg a HTTP-port webes hozzáférés engedélyezése
Egy nyilvános és titkos port együtt (TCP vagy UDP) protokoll lévő Azure állnak. A magánjellegű portja a olyan portot, amely a virtuális gépen figyeli a szolgáltatás. A nyilvános portja a port, amelyet az Azure felhőszolgáltatásba figyeli a külső felhasználókkal a bejövő, az internetes forgalmat.  

TCP 8080 portja az alapértelmezett portszámot, amely tomcat figyeli a. A port megnyitása az Azure zárólap lehetővé teszi, és más internetkapcsolat ügyfelek is lapok tomcat.  

1.  Az Azure-portálon kattintson a **Tallózás** -> **virtuális gép**, és válassza ki a virtuális gép létrehozott.  
![][5]
2.  A virtuális géphez zárólap hozzáadásához kattintson a **Végpontok** mezőbe.
![][6]
3.  Kattintson a **hozzáadása**gombra.  
    1.  A **végpontot**írja be egy nevet a végpont végpontot, és írja be a **Nyilvános Port**80.  

        Ha a 80 beállította, nem szükséges szeretné hozzáadni a portszámot, amely lehetővé teszi, hogy tomcat elérése URL-CÍMÉT. Ha például http://tomcatdemo.cloudapp.net.    

        Ha egy másik érték, például 81, úgy, hogy fel kell vennie a portszámot tomcat eléréséhez az URL-címre. Ha például http://tomcatdemo.cloudapp.net:81 /.
    2.  Írja be saját Port 8080. Alapértelmezés szerint tomcat figyeli 8080 portot. Ha módosította az alapértelmezett portja tomcat meghallgatása, a magánjellegű Port ugyanaz, mint a tomcat meghallgatása port frissítenie kell.  
    ![][7]

4.  Kattintson az **OK gombra** a végpont hozzáadása a virtuális gép.



###<a name="step-2-connect-to-the-image-you-created"></a>Lépés: 2: A kép létrehozott csatlakozni.
Megadhatja, hogy minden SSH eszköz a virtuális géphez való csatlakozásra. Ebben a példában gitt használjuk.  

A tartománynév a virtuális gép először lekérése az Azure-portálra. **Kattintson a Tallózás** -> **virtuális gépeken futó** -> nevét a virtuális gép -> **Tulajdonságok**, és keresse meg a **Tartomány neve** mezőben az a **tulajdonságokat** csempére.  

Szerezze be a portszámot SSH kapcsolatok **SSH** mezőből. Lássunk egy példát.  
![][8]

Töltse le a gitt [Itt](http://www.putty.org/) .  

A letöltés után kattintson a végrehajtható fájl gitt. EXE. Az alapvető beállítások állítson be az állomásnév, és a virtuális gép tulajdonságainak nyert portszámot. Lássunk egy példát:  
![][9]

A bal oldali ablaktáblában kattintson a **kapcsolat** -> **SSH** -> **Auth** , és kattintson a fázis 1 puttygen **tallózással keresse meg** a **privateKey.ppk** fájl, amely tartalmazza a titkos kulcs elérési útja generálja: hozzon létre egy képet. Lássunk egy példát:  
![][10]

Kattintson a **Megnyitás**gombra. Előfordulhat, hogy figyelmezteti, egy üzenet mezőben. Ha állította be a DNS-nevét, és a portszámot megfelelően, kattintson az **Igen**gombra.
![][11]  


Meg kell jelennie a következőket:  
![][12]

Írja be a felhasználó nevét a virtuális gép fázis 1 létrehozásakor megadott: hozzon létre egy képet. A következő hasonló jelennek meg:  
![][13]





##<a name="phase-3-install-software"></a>3 fázis: Telepítse a szoftvert
A fázis telepítse a Java-futtatókörnyezet, tomcat és más tomcat összetevőket.  

###<a name="java-runtime-environment"></a>Java-futtatókörnyezet
Tomcat Java nyelven íródott. Kétféle Java fejlesztői csomagok (JDKs) (OpenJDK és Oracle JDK), és a lehetőségek közül választhat.  

>AZURE. Megjegyzés: Mindkét JDKs szinte osztályok ugyanazt kódot kell a Java API, de a virtuális gép kódját ténylegesen különböző. Amikor a tárak, OpenJDK általában lezárt üzenetekhez használandó általában az Oracle megnyitott tárak használni. Ha pedig az Oracle JDK további osztályok és néhány rögzített hibák és az Oracle JDK további halmaza, mint OpenJDK.

Az alábbi parancsok töltse le a különböző JDKs.  

Megnyitás-jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

az Oracle-jdk  

-   A JDK letöltése az Oracle-webhelyről:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   A JDK fájlok tárolásához könyvtár létrehozása:  

        sudo mkdir /usr/lib/jvm  

-   Ki kell olvasni a JDK fájlokat át a/usr/tár vagy jvm/directory:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Az Oracle JDK beállítása JVM alapértelmezés szerint:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Teszt:
A következő parancs segítségével vizsgálata Java-futtatókörnyezet megfelelően van-e telepítve:  

    java -version  

Ha telepítette az open-jdk, meg kell jelennie a következőhöz hasonló üzenet:![][14]

Ha telepítette az oracle-jdk, meg kell jelennie a következőhöz hasonló üzenet:![][15]

###<a name="tomcat7"></a>Tomcat7
Az alábbi paranccsal tomcat7 telepítése:  

    sudo apt-get install tomcat7  

Ha nem használ tomcat7, használja a megfelelő változata, ez a parancs.  

####<a name="test"></a>Teszt:

Annak ellenőrzéséhez, ha tomcat7 telepítése sikerült, tallózással keresse meg a tomcat kiszolgálónév DNS (http://tomcatexample.cloudapp.net/ az ebben a cikkben a minta URL-cím). Ha például a következő oldal jelenik meg, van-e telepítve tomcat7 helyes-e.
![][16]


###<a name="install-other-tomcat-components"></a>Más Tomcat-összetevők telepítése
Vannak egyéb választható tomcat összetevők is telepítheti.  

A **sudo apt gyorsítótár keresési tomcat7** paranccsal minden összetevő megjelenítéséhez. Az alábbi parancsok példák néhány hasznos kijelzők telepítéséhez.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>4 fázis: Tomcat konfigurálása
Ez a fázis tomcat Ön kezeli.
###<a name="start-and-stop-tomcat7"></a>Elindítása és leállítása tomcat7
A tomcat7 kiszolgáló automatikusan elindul a telepítésekor aktiválni. Is is elindíthatja saját magát, a következő parancsot:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 leállítása:  

    sudo /etc/init.d/tomcat7 stop

Tomcat7 állapotának megtekintése:  

    sudo /etc/init.d/tomcat7 status

Tomcat szolgáltatások újraindítása:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Tomcat felügyelete
Módosíthatja a Tomcat felhasználói konfigurációs fájl az alábbi paranccsal rendszergazdai hitelesítő adataival beállítása:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Lássunk egy példát:  
![][17]  

>AZURE. Megjegyzés: A rendszergazdai felhasználónevével erős jelszó létrehozása.  

Ez a fájl szerkesztése, után indítsa újra tomcat7-szolgáltatások a következő parancsot, győződjön meg arról, hogy a módosítások érvénybe:  

    sudo /etc/init.d/tomcat7 restart  

Nyissa meg a böngészőt, és írja be az URL-cím **http://<your tomcat server DNS name>/manager/html**. Ez a cikk például az URL-je http://tomcatexample.cloudapp.net/manager/html.  

A csatlakozás után meg kell jelennie a következőhöz hasonló:  
![][18]

##<a name="common-issues"></a>Gyakori kérdések

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>A virtuális gép Tomcat és Moodle nem tud hozzáférni az internetről

-   **A jelenség**  
Tomcat fut, de az Tomcat alapértelmezett lap nem látható a böngészőben.
-   **Lehetséges legfelső szintű eset**   
    1.  A tomcat meghallgatását port nem ugyanaz, mint a személyes Port a virtuális gép végpont tomcat forgalmához.  

        Jelölje be a nyilvános és titkos portot végpont beállításokat, és ellenőrizze, hogy a magánjellegű portja ugyanaz, mint a tomcat meghallgatása port. Lásd: 1 fázis: Utasításokért kép létrehozása a virtuális gép végpontok beállításáról.  

        Határozza meg a tomcat meghallgatását portot, nyissa meg a /etc/httpd/conf/httpd.conf (piros-kalap kiadás) vagy /etc/tomcat7/server.xml (Debian kiadás). Alapértelmezés szerint a tomcat meghallgatását portja 8080. Lássunk egy példát:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Ha szeretné, hogy Debian vagy Ubuntu, és módosíthatja az alapértelmezett port a Tomcat meghallgatása (például 8081) egy virtuális gépet használ, is célszerű megnyitni a port az operációs rendszer az. Első lépésként nyissa meg a profil:  

            sudo vi /etc/default/tomcat7  

        Kattintson az utolsó sor vegye ki a megjegyzésjeleket, és módosítsa "Igen" "nem".  

            AUTHBIND=yes

    2.  A tűzfal letiltotta a meghallgatását portja tomcat.

        Ha csak a helyi állomáswebhelyén a Tomcat alapértelmezett lap látható, a probléma oka valószínűleg, hogy a tűzfal blokkolja a port, amely szerint tomcat navigációval van. A w3m eszközzel keresse meg az weblapon. Az alábbi parancsok w3m telepítése, és nyissa meg a Tomcat alapértelmezett lapot:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Megoldás**
    1. Ha a tomcat meghallgatása port nem ugyanaz, mint a személyes Port a forgalmat a virtuális géphez végpont, akkor kell módosítani a magánjellegű Port ugyanaz, mint a tomcat meghallgatása port.   

    2.  Tűzfal/iptables okozza a problémát, ha a következő sorok hozzáadása /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Figyelje meg, hogy a második sorban csak akkor szükséges, a https-forgalom.  

        Győződjön meg róla a fenti olyan sort, amely globálisan volna korlátozhatja a hozzáférést, például a következő:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Frissítse a iptables, futtassa a következő parancsot:  

            service iptables restart  

        Ez a CentOS 6.3 teszteléssel.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Engedély megtagadva mikor projektfájlokat /var/lib/tomcat7/webapps feltöltés /  

-   **A jelenség**  
A virtuális géphez csatlakozhat, és nyissa meg azt a /var/lib/tomcat7/webapps és közzététele a webhelyen (például FileZilla) bármely SFTP ügyfél használata esetén meg hibaüzenet a következőhöz hasonló:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Lehetséges legfelső szintű eset** Nincs engedélye a /var/lib/tomcat7/webapps mappa eléréséhez van.  
-   **Megoldás**  
A legfelső szintű fiók szükséges jogosultsági letölthető. Módosíthatja, hogy a mappa tulajdonjogának legfelső szintű kiépítésekor a gép használt felhasználónév. Íme egy példa azureuser fiók nevű:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    A -R beállítás használatával könyvtárában szereplő összes fájl engedélyeinek túl alkalmazni.  

    Figyelje meg, hogy ez a parancs könyvtárak is működik. A -R lehetőséget a fájlok és a címtárban szereplő könyvtárak engedélyeinek módosítása Lássunk egy példát:  

        sudo chown -R username:group directory  

    Ez a parancs a tulajdonjog (felhasználók és csoportok) az összes fájlhoz és könyvtárak és maga könyvtárat belül változik.  

    A következő parancs csak változik a könyvtár mappát engedélyt, de a fájlokat és mappákat a címtárban belül egyedül elhagyja.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
