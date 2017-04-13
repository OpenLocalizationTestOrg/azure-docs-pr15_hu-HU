## <a name="scenario"></a>Eset

Ehhez a dokumentumhoz jobban bemutatják, hogyan hozhat létre UDRs, az alkalmazási példát fog használja.

![KÉP LEÍRÁSA](./media/virtual-network-create-udr-scenario-include/figure1.png)

Ebben az esetben létrehoz egy UDR az *első vége alhálózat* és egy másik UDR *vissza vége alhálózat* alábbiakban leírtak szerint: 

- **UDR-FrontEnd**. Az előtér UDR *FrontEnd* az alhálózathoz fog vonatkozni, és egy útvonal tartalmazza:  
    - **RouteToBackend**. A visszahívási vége alhálózat a **FW1** virtuális géphez Ez az útvonal minden forgalom küld.
- **UDR-Kódmentes**. A háttér UDR *Kódmentes* az alhálózathoz fog vonatkozni, és egy útvonal tartalmazza: 
    - **RouteToFrontend**. Ez az útvonal minden forgalom az előtér-alhálózat a **FW1** virtuális géphez küld.

Ezek az útvonalak kombinációját biztosítja, hogy az összes adatforgalmat egy alhálózat között van használatban, mint egy virtuális készülék **FW1** virtuális gép továbbítja. Meg kell kapcsolnia a, megkaphatja a többi VMs adatforgalmat ahhoz, hogy virtuális IP-továbbítás.
