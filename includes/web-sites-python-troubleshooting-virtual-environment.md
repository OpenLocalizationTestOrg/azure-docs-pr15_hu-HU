A telepítési parancsfájlt a virtuális környezet kibocsátása kihagyja a Azure, ha azt észleli, hogy már létezik egy kompatibilis virtuális környezet.  Ez felgyorsítható telepítési jelentősen.  Mezőpont kihagyja csomagokat, amely már telepítve vannak.

Bizonyos esetekben célszerű lehet váltani az, hogy virtuális környezet törlése.  Érdemes a teendő, ha úgy dönt, hogy a tárházba szerepeltetni virtuális környezetet.  Érdemes azt is, a teendő, ha módosítani szeretné az egyes csomagok volna, vagy a tesztelés requirements.txt módosításait.

Néhány lehetőség a meglévő Azure virtuális környezetében kezelése:

### <a name="option-1-use-ftp"></a>1 beállítást: FTP használata

Csatlakozás a kiszolgálóhoz az FTP-ügyfél és is törli a boríték mappát.  Ne feledje, hogy néhány FTP-ügyfél (például webböngésző) lehetséges, hogy csak olvasható, és nem teszi lehetővé, bizonyára tudni szeretné, hogy ügyeljen arra, hogy az FTP-ügyfél használata lehetőséget, törölje az mappáját.  A FTP állomásnév és a felhasználó az [Azure-portált](https://portal.azure.com)a webalkalmazás lap jelenik meg.

### <a name="option-2-toggle-runtime"></a>2 lehetőség: Váltása futtatókörnyezet

Íme egy alternatív megoldás, hogy mi az, hogy a telepítési parancsfájlt törlődik a boríték mappát, ha azt nem egyezik meg a kívánt verziót Python FAKT előnyeit.  Ezzel gyakorlatilag a meglévő környezet törlése, és hozzon létre egy újat.

1. Váltás egy másik verzióját Python (keresztül runtime.txt vagy az **Alkalmazás beállításai** lap az Azure-portálon)
1. mely számjegy leküldéses bizonyos változásokat (figyelmen kívül hagyja a mezőpont-telepítési hibák ha vannak ilyenek)
1. Térjen vissza a Python kezdeti verziója
1. mely számjegy újra leküldéses módosításokat

### <a name="option-3-customize-deployment-script"></a>Lehetőség 3: Telepítési parancsfájlt testreszabása

Ha testre szabta a telepítési parancsfájlt, módosíthatja a kódot be kényszeríti a törölni kívánt boríték mappát deploy.cmd.
