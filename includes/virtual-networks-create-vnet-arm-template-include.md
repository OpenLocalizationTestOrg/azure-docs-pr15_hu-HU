## <a name="download-and-understand-the-arm-template"></a>Töltse le, és megértette az ARM sablon

Töltse le a meglévő ARM sablont hozhat létre egy VNet és két alhálózat a github, végezze el a módosításokat, előfordulhat, hogy szeretné használni, és felhasználhatja. Ehhez hajtsa végre az alábbi lépéseket.

1. Nyissa meg [a minta sablonként lap](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Kattintson a **azuredeploy.json**, és kattintson a **nyers**.
3. Mentse a fájlt a számítógépen egy helyi mappájában.
4. Ha ismeri a ARM sablonokat, ugorjon a 7.
5. Nyissa meg az éppen mentett fájlt, és tekintse meg a **Paraméterek** sorban 5 tartalmát. ARM sablon paramétereivel helyőrző értékeket, akkor töltheti a telepítés során.

    | Paraméter | Leírás |
    |---|---|
    | **hely** | Azure terület, ahol a VNet létrejön |
    | **vnetName** | Az új VNet nevét |
    | **addressPrefix** | A VNet CIDR formátumban-címterület használatára |
    | **subnet1Name** | Az első VNet nevét |
    | **subnet1Prefix** | Az első alhálózat CIDR tiltása |
    | **subnet2Name** | A második VNet nevét |
    | **subnet2Prefix** | A második alhálózat CIDR tiltása |

    >[AZURE.IMPORTANT] ARM-sablonok github díjtábláinak időbeli módosíthatja. Győződjön meg arról, mielőtt használni kezdi jelölje be a sablont.
    
6. Jelölje be a tartalom **erőforrások** csoportban, és figyelje meg a következőket:

    - **Írja be**. A sablon által létrehozott erőforrás típusú. Ebben az esetben **Microsoft.Network/virtualNetworks**, amely jelenítik meg a VNet.
    - **nevét**. Az erőforrás neve. Figyelje meg a **[parameters('vnetName')]**, ami azt jelenti neve lesz a telepítés során a felhasználó vagy paraméter fájl bemenetként kapott használatát.
    - **Tulajdonságok**. Az erőforrás tulajdonságok listája. Ez a sablon címét és alhálózat terület tulajdonságokat VNet létrehozása során használja.

7. Lépjen vissza [a minta sablonként lap](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Kattintson a **azuredeploy-paremeters.json**, és kattintson a **nyers**.
9. Mentse a fájlt a számítógépen egy helyi mappájában.
10. Nyissa meg az éppen mentett fájlt, és a paraméterek értékeinek szerkesztése. A saját forgatókönyv ismertetett VNet használja az alábbi értékeket.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Mentse a fájlt.
  