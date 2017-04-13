<properties
    pageTitle="Kattintson egy Linux virtuális PostgreSQL beállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti, állíthatja PostgreSQL Linux virtuális gépen Azure-ban"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Telepítse és állítsa be a PostgreSQL a Azure

PostgreSQL-speciális Megnyitás-forrásadatbázis hasonló Oracle és DB2. Vállalati kész funkciókat, például a teljes savas megfelelőség, a megbízható tranzakció alapú feldolgozása és a több elem verzió feldolgozási vezérlő tartalmazza. Támogatja a szabványok, például az ANSI SQL és SQL/MED (beleértve a külső adatok csomagolást az Oracle, MySQL, MongoDB és sok más) is. Érdemes erősen bővíthető JSON vagy kulcs érték-alapú alkalmazások feletti 12 lépésről nyelvek, GIN és GiST indexek, térbeli adatok támogatási és több NoSQL hasonló funkciók támogatása.

Ebben a cikkben megtanulhatja, hogyan telepítheti, állíthatja a PostgreSQL-Azure virtuális gépen futó Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>PostgreSQL telepítése

> [AZURE.NOTE] Már rendelkeznie kell egy Azure virtuális gépen futó Linux ebben az oktatóanyagban befejezéséhez. Szeretne létrehozni, és állítsa be a Linux virtuális a folytatás előtt, olvassa el a az [Azure Linux virtuális oktatóprogram](virtual-machines-linux-quick-create-cli.md)című témakört.

Port 1999 ebben az esetben a PostgreSQL-portot használja.  

Csatlakozás a Linux gitt keresztül létrehozott virtuális. Ha az első alkalommal az Azure Linux virtuális használ, elolvashatja, [hogyan kell használni SSH Linux Azure a](virtual-machines-linux-mac-create-ssh-keys.md) megtudhatja, hogy miként csatlakozhat gitt egy Linux virtuális.

1. A következő parancsot a legfelső szintű (rendszergazda) váltás:

        # sudo su -

2. Néhány terjesztését táblává telepíteni kell az PostgreSQL telepítése előtt. Ellenőrizze a distro ebben a listában, és futtassa a megfelelő parancsot:

    - Piros kalap alap Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian alap Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Töltse le a PostgreSQL át a legfelső szintű directory, és bontsa ki az a csomag:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    A fenti képen. Az [](https://ftp.postgresql.org/pub/source/)Index/pub/forrás/talál részletesebb letöltési címét.

4. Szeretné kezdeni a létrehozás, ezek a parancsok futtatása:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Ha szeretne létrehozni, amit épül fel, beleértve a dokumentáció (HTML- és férfi oldal) és a további modulok (contrib), futtassa a következő parancsot:

        # gmake install-world

    A következő kérdést kell megjelennie:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL konfigurálása

1. (Nem kötelező) A PostgreSQL-hivatkozás nem tartalmazza a verziószám lerövidítéséhez szimbolikus hivatkozás létrehozása:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Az adatbázis könyvtár létrehozása:

        # mkdir -p /opt/pgsql_data

3. Hozzon létre egy nem legfelső szintű felhasználót, és a felhasználói profil módosítása. Majd váltson az új felhasználóhoz is hozzárendeljen (a példában szereplőhöz *postgres* neve):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] PostgreSQL biztonsági okokból nem legfelső szintű felhasználó használja a inicializálni, indítása és leállítása az adatbázist.


4. Az alábbi parancsok beírásával a *bash_profile* fájl szerkesztése Ezek a sorok hozzáadódik a *bash_profile* fájl végén:

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Hajtsa végre a *bash_profile* fájlt:

        $ source .bash_profile

6. A telepítés ellenőrzése a következő parancs használatával:

        $ which psql

    Ha a telepítés befejeződött, a következő válasz jelennek meg:

        /opt/pgsql/bin/psql

7. A PostgreSQL-verziót is ellenőrizheti:

        $ psql -V

8. Az adatbázis inicializálni:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    A következő eredményt kell megjelennie:

![kép](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL beállítása

<!--    [postgres@ test ~]$ exit -->

Futtassa az alábbi parancsokat:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Módosítsa a /etc/init.d/postgresql fájlban két változó. Telepítési elérési útját PostgreSQL van állítva az előtag: **/opt/pgsql**. PGDATA értéke adatokat tároló elérési útját PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![kép](./media/virtual-machines-linux-postgresql-install/no2.png)

A fájlt, hogy végrehajtható módosítása:

    # chmod +x /etc/init.d/postgresql

Indítsa el a PostgreSQL:

    # /etc/init.d/postgresql start

Ellenőrizze, hogy a PostgreSQL végpontjának meg:

    # netstat -tunlp|grep 1999

Meg kell jelennie a következő kimenet:

![kép](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Csatlakozás a Postgres-adatbázishoz

Váltás a postgres felhasználó még egyszer:

    # su - postgres

Postgres adatbázis létrehozása:

    $ createdb events

Az éppen létrehozott események-adatbázis csatlakoztatása:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Hozzon létre és Postgres táblázat törlése

Most, hogy az adatbázis kapcsolt, a táblázatok akkor is létrehozhat.

Ha például hozhat létre egy új Postgres – példatáblázat a következő parancsot:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Most már beállított egy négy oszlop táblázat az alábbi oszlopnevek és korlátozásokkal:

1. A "név" oszlop el lett szabják 20 karakter hosszú lehet a VARCHAR parancsot.
2. A "élelmiszer" oszlop azt jelzi, hogy a élelmiszerhez, amely az egyes munkatársakhoz állapotba kerül. VARCHAR korlátozza a 30 karakter hosszú lehet, hogy a szöveget.
3. A "megerősített" oszlop rekordok e személyt szeretne a piknik van RSVP'd. Az értékek a "I" és "N" használhatók.
4. A "dátum" oszlopban látható, ha az azok regisztrált az esemény. Postgres igényel, hogy dátumok kell írni nn/éééé-mm-.

Meg kell jelennie a következő, ha a táblázat sikeresen létre:

![kép](./media/virtual-machines-linux-postgresql-install/no4.png)

A következő parancs használatával is ellenőrizheti a táblázatok szerkezetének:

![kép](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Adatok hozzáadása egy táblához

Először illesszen be egy sorba információkat:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Meg kell jelennie a kimenet:

![kép](./media/virtual-machines-linux-postgresql-install/no6.png)

A tábla, valamint hozzáadhatja néhány további személyeket. Néhány lehetőség, vagy létrehozhat saját:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Táblázatok megjelenítése

A következő parancs segítségével a tábla megjelenítése:

    select * from potluck;

A kimenet van:

![kép](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>A táblázatban lévő adatok törlése

A következő parancsot használja a táblázatban lévő adatok törlése:

    delete from potluck where name=’John’;

A törli az "Kovács" sor összes információt. A kimenet van:

![kép](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>A táblázatban lévő adatok frissítése

A következő paranccsal frissíteni az adatokat egy táblázatban. Ez egy Sandy erősítette, hogy kikkel van részt, így azt módosítja saját RSVP az "M" a "I":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>PostgreSQL kapcsolatos további információk
Most, hogy a PostgreSQL-Azure Linux virtuális gép a telepítés befejezése élvezheti használat Azure-ban. PostgreSQL kapcsolatos további tudnivalókért látogassa meg a [PostgreSQL-webhelyet](http://www.postgresql.org/).
