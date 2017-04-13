Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel, amely tartalmaz minden paraméterértékeket paraméterek nevű szakasz.
Be kell állítania a paramétert, ezek az értékek, amelyek alapján a telepít projekten vagy a környezet, amelyben telepít változhatnak. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve. 

Paraméterek megadásakor a **allowedValues** mező segítségével adja meg, mely értékek felhasználó biztosíthat a telepítés során. A **defaultValue** mező segítségével értéket rendelhet a paramétert, ha nincs érték szerepel, a telepítés során.

A sablonban paramétereknél leírja azt.

### <a name="sitename"></a>siteName

A web App alkalmazásban, amely a létrehozni kívánt neve.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

A használandó szolgáltatója a web app alkalmazás szolgáltatáscsomagja neve.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>Raktári szám

Az Office csomagra vonatkozó árak réteg.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

A sablon ehhez a paraméterhez megengedett értékeket meghatározza, és egy alapértelmezett értéket (S1) rendeli, ha nem ad meg értéket.

### <a name="workersize"></a>workerSize

A futtatási terv (kicsi, közepes vagy nagy) példány méretét.

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
A sablon határozza meg, amely a paraméter (0, 1 vagy 2) engedélyezett értékeket, és hozzárendel egy alapértelmezett értéket (0), ha nem ad meg értéket. Az értékek felelnek meg kicsi, közepes és nagyméretű.
