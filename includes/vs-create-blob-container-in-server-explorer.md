Azure tároló sorban várakozó **Server Explorer**Visual Studio segítségével hozhat létre.

![Kiszolgáló Explorer BLOB][Image1]

1. A **Nézet** menüben válassza a **Kiszolgáló Explorer**.
2. A kiszolgáló Intézőben bontsa ki az **Azure** csomópont vásárlásáról az előfizetéshez tartozó, a **tárhely** csomópontot, és a csomópontra a tárterület-fiókom az Azure csatlakoztatott tárhelyszolgáltatáshoz megadott.
3. Jelölje ki a **sorban várakozó** csomópontot, és válassza a **Nyomtatási sor létrehozása** a helyi menüből.
4. Írja be a tároló nevét, és kattintson **az OK gombra**.   

Alapértelmezés szerint a új tároló privát, és meg kell adnia a tárhely hívóbetű tároló BLOB letöltése érdekében. Ha azt szeretné, végezze el a fájlokat a tároló nyilvános, jelölje be a tároló **Kiszolgáló Intéző** , majd nyomja meg az `F4` a **Tulajdonságok** ablak megjelenítéséhez. Állítsa az **nyilvános olvasási hozzáférést** **Blob**. Az interneten bárki láthatja a BLOB-tárolóban nyilvános, de módosíthatja vagy törölheti őket, csak akkor, ha rendelkezik a megfelelő hozzáférési billentyűt.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png