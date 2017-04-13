Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel, amely tartalmaz minden paraméterértékeket paraméterek nevű szakasz.
Be kell állítania a paramétert, ezek az értékek, amelyek alapján a telepít projekten vagy a környezet, amelyben telepít változhatnak. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban az erőforrásokat, a rendszer üzembe határozza meg. 

Paraméterek megadásakor a **allowedValues** mező segítségével adja meg, mely értékek felhasználó biztosíthat a telepítés során. A **defaultValue** mező segítségével értéket rendelhet a paramétert, ha nincs érték szerepel, a telepítés során.

A sablonban paramétereknél leírja azt.

### <a name="logicappname"></a>logicAppName

Hozhat létre a logikájának alkalmazás nevére.

    "logicAppName": {
        "type": "string"
    }