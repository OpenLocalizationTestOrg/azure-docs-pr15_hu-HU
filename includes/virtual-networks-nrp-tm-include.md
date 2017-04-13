## <a name="traffic-manager-profile"></a>Adatforgalom Manager profil

Adatforgalom manager és a gyermek végpont erőforrás engedélyezése a DNS-átirányításra végpontokhoz Azure-ban és az Azure-Ön kívül. Az ilyen forgalom terjesztési útválasztási házirend módszerek szabályozza. Adatforgalom manager is lehetővé teszi a végpontot állapot ellenőrzését, és forgalmának megfelelően keresztül zárólap állapotának alapján. 

| A tulajdonság | Leírás |
|---|---|
|**trafficRoutingMethod**| a lehetséges értékek *Teljesítmény*, *Weighted*és *Prioritás* : | 
| **dnsConfig** | A profil FQDN | 
| **Protocol (protokoll)** | nyomon követés protokollt, lehetséges értékei *HTTP* - és *HTTPS-forgalom*|
| **Port** | port figyelése |  
| **Elérési út** | elérési út figyelése |
| **Végpontok** |  tároló végpont erőforrások | 

### <a name="endpoint"></a>Végpont 

Zárólap forgalom Manager profiljának gyermek erőforrás. Ez azt jelenti, hogy a szolgáltatás vagy a beállított házirend a forgalom Manager profil erőforrás alapján webes végpontot, mely a felhasználónak a forgalom van meghatározva. 

| A tulajdonság | Leírás | 
|---|---| 
| **Típus** |  a típus, végpontjának lehetséges értékei *Azure vége pont* *Külső végpont*és *Egymásba ágyazott végpont* | 
| **targetResourceId** |  a szolgáltatás vagy a webes végpontjának nyilvános IP-címét. Ez lehet az Azure vagy külső zárólap. | 
| **Súly** | az adatforgalom-kezelés használt végpont súly. | 
| **Prioritás** | a végpont használta a feladatátvevő művelet prioritás |

Az adatforgalom-kezelő minta Json formátumban: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>További források

Olvassa el a [REST API-dokumentációjában forgalom Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) további információt.
