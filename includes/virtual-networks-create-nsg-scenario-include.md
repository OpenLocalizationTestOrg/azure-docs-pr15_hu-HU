## <a name="scenario"></a>Eset

Ehhez a dokumentumhoz jobban bemutatják, hogyan hozhat létre NSGs, az alkalmazási példát fog használja.

![VNet eset](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Ebben az esetben létrehoz egy NSG **TestVNet** virtuális hálózat minden alhálózathoz alábbiakban leírtak szerint: 

- **NSG-FrontEnd**. Az előtér NSG *FrontEnd* az alhálózathoz fog vonatkozni, és két szabályokat tartalmaz:  
    - **rdp-szabály**. Ez a szabály lehetővé teszi a *FrontEnd* alhálózat RDP-forgalmat.
    - **webhely-szabály**. Ez a szabály lehetővé teszi a *FrontEnd* alhálózathoz HTTP-forgalmat.
- **NSG-Kódmentes**. A háttér NSG *Kódmentes* az alhálózathoz fog vonatkozni, és két szabályokat tartalmaz: 
    - **sql-szabály**. Ez a szabály lehetővé teszi, hogy csak a *FrontEnd* alhálózat az SQL-forgalmat.
    - **webhely-szabály**. Ez a szabály letiltja az összes internetes forgalmat kötött a *Kódmentes* alhálózat.

Ha a szabályok létrehozása a DMZ-szerű esetben, ahol a hátsó vége alhálózat csak kaphatnak a bejövő forgalom az SQL az előtér-alhálózat, és nem fér hozzá az internethez, miközben az előtér-alhálózat kommunikálni az interneten, és csak a bejövő HTTP kérelem érkezik.
 
