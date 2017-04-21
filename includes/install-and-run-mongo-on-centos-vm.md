Kövesse ezeket a lépéseket követve telepítése és futtatása MongoDB egy virtuális gépen futó CentOS Linux rendszerhez.

> [AZURE.WARNING] MongoDB biztonsági funkciókat, például a hitelesítésről és az IP-cím kötés, alapértelmezés szerint nincs engedélyezve vannak. Mielőtt MongoDB munkakörnyezetben engedélyezni kell biztonsági funkciókat.  [Biztonság és a hitelesítéshez](http://www.mongodb.org/display/DOCS/Security+and+Authentication) talál további információt.

1. Állítsa be a csomag Management rendszer (YUM), hogy MongoDB telepítheti. Hozzon létre egy */etc/yum.repos.d/10gen.repo* fájlt a tárházba adatainak tárolására, és adja meg a következőket:

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Mentse a repó fájlt, és futtassa a következő parancsot a helyi csomag adatbázis frissítése:

        $ sudo yum update

3. A csomag telepítéséhez futtassa a következő parancsot a stabil legújabb MongoDB és a kapcsolódó eszköz telepítése:

        $ sudo yum install mongo-10gen mongo-10gen-server

    Várakozás közben MongoDB letölti és telepíti.

4. Hozzon létre egy adatkönyvtárának. Alapértelmezés szerint MongoDB tárolja az adatokat a */data/db* könyvtár, de könyvtár kell létrehoznia. Szeretné létrehozni a dokumentumot, a Futtatás:

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    MongoDB telepít Linux a további tudnivalókért lásd: a [Quickstart útmutató Unix][QuickstartUnix].

5. Az adatbázis indítása futtatása:

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    Minden napló üzenet irányítja át a */srv/datadrive/data/mongod.log* fájlt, MongoDB kiszolgáló elindul, és preallocates lapjának fájlokat. Eltarthat néhány percig, amíg a napló fájlokat lefoglalandó és figyeli a kapcsolatok MongoDB.

6. A MongoDB felügyeleti rendszerhéj kezdeményezéséhez nyissa meg a SSH vagy gitt külön ablakban, és futtassa:

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    A Beszúrás hozza létre az adatbázist.

7. Miután telepítette az MongoDB meg kell adnia zárólap, hogy MongoDB távolról is elérhető. Az adatkezelési portálon kattintson a **virtuális gépeken futó**, kattintson az új virtuális gép nevére, majd kattintson a **Végpontok**.
    
    ![Végpontok][Image7]

8. Kattintson a **Végpont hozzáadása** az oldal alján.
    
    ![Végpontok][Image8]

9. Zárólap felvételére a következő beállításokat:

 - **Név**: Mongo
 - **Protocol (protokoll)**: TCP
 - **Nyilvános Port**: 27017
 - **Magánjellegű Port**: 27017
 
 Ez lehetővé teszi a MongoDB való távolról is elérhető.



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
