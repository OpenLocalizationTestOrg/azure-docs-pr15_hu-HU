## <a name="scenario"></a>Eset

Ehhez a dokumentumhoz jobban bemutatják, hogyan hozhat létre egy VNet és alhálózat, az alkalmazási példát fog használja.

![VNet eset](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Ebben az esetben egy **TestVNet** a **192.168.0.0./16**fenntartott CIDR blokkja nevű VNet hoz létre. A VNet fogja tartalmazni a következő alhálózat: 

- **FrontEnd**, **192.168.1.0/24** segítségével saját CIDR parancsot.
- **Kódmentes**, **192.168.2.0/24** segítségével saját CIDR parancsot.

 