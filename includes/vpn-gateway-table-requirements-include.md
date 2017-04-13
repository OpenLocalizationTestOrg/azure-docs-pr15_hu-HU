Az alábbi táblázat a PolicyBased és RouteBased VPN átjárók vonatkozó követelményeket. Az alábbi táblázat az erőforrás-kezelő és a klasszikus telepítési modellek vonatkozik. A Klasszikus modell PolicyBased VPN átjárók ugyanaz, mint statikus átjárók, illetve az átjárók útvonal-alapú dinamikus átjárók megegyezik.


|   | **PolicyBased Basic virtuális Magánhálózati átjáró** | **RouteBased Basic virtuális Magánhálózati átjáró** | **RouteBased Standard virtuális Magánhálózati átjáró**   | **RouteBased nagy teljesítmény virtuális Magánhálózati átjáró** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Webhely kapcsolatot (S2S)**  | PolicyBased VPN konfigurálása        | RouteBased VPN konfigurálása  | RouteBased VPN konfigurálása     | RouteBased VPN konfigurálása    |
| **Pont webhely kapcsolat (P2S**)      | Nem támogatott   | Támogatott (is megtalálhatók, az S2S)  | Támogatott (is megtalálhatók, az S2S)  | Támogatott (is megtalálhatók, az S2S) |
| **Hitelesítési módszer**                 |    Előre megosztott kulcs  | A S2S kapcsolatot, tanúsítványok P2S connectivity az előre megosztott kulcs | A S2S kapcsolatot, tanúsítványok P2S connectivity az előre megosztott kulcs | A S2S kapcsolatot, tanúsítványok P2S connectivity az előre megosztott kulcs |
| **S2S kapcsolatok maximális száma**       | 1                              | 10                                                                    | 10                                | 30                               |
| **P2S kapcsolatok maximális száma**       | Nem támogatott                  | 128                                                                   | 128                               | 128                              |
|**Aktív útválasztás (BGP)**           | Nem támogatott                  | Nem támogatott                                                         | Támogatott                     | Támogatott                   |
 
