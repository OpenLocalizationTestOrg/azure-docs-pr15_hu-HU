### <a name="app-service-plan"></a>Alkalmazás szolgáltatáscsomagja

Hoz létre a web app szolgáltatója szolgáltatás megtervezése. Akkor adja meg a terv **hostingPlanName** paraméterrel nevét. A terv helye az erőforráscsoport használt ugyanazon a helyen. Árak réteg és dolgozó méretének adható **termékváltozat** és **workerSize** paraméterei

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

