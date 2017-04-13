<properties
   pageTitle="Csatlakozás egy Azure tároló szolgáltatás fürtre |} Microsoft Azure"
   description="Csatlakozás egy Azure tároló szolgáltatás fürthöz egy SSH alagutas használatával."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Adatközpont/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Csatlakozzon az Azure tároló szolgáltatás fürthöz

Az Adatközpont/OS és Docker Swarm fürt Azure tároló szolgáltatás által telepített többi végpontok jelenítik meg. Ezeket a végpontokat használata azonban nem nyissa meg a külső világ. Annak érdekében, hogy ezeket a végpontokat kezelése, létre kell hoznia egy biztonságos rendszerhéj (SSH) alagutas. Egy SSH után alagutas létrejött, parancsokat a fürt végpontok ellen tekinthetők meg és a felhasználói felület fürt böngészőn keresztül saját rendszeren. A dokumentum végigvezeti Linux, az OS X és a Windows-SSH alagutas létrehozásának.

>[AZURE.NOTE] Egy fürt projektirányítási rendszerébe egy SSH munkamenet hozhat létre. Azonban ez nem javasoljuk. Munka közvetlenül a rendszer a kockázat konfigurációs véletlen módosítások közzététele.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Hozzon létre egy SSH alagutas Linux vagy OS X

Először is, amely történő hoz létre egy SSH alagutas Linux vagy OS X lesz a terheléselosztás mesteralakzatok nyilvános DNS-nevének megkereséséhez. Ehhez az erőforrás csoport kibontásához, hogy az egyes erőforrásokhoz jelenik meg. Keresse meg és jelölje ki a fő nyilvános IP-címét. Ekkor megnyílik egy a nyilvános IP-címet, amely magában foglalja a DNS-név adatait tartalmazó lap be. Ez a név későbbi felhasználás céljából menteni. <br />


![Nyilvános DNS-neve](media/pubdns.png)

Most egy rendszerhéj megnyitása, és futtassa a következő parancsot hol:

**Portja a elérhetővé tenni kívánt végpontjának port.** Méhrajt Ez az 2375. Az Adatközpont/OS használja a 80-as port.  
**A felhasználónév** szó a kapott, amikor telepítette a fürt felhasználó nevét.  
**DNSPREFIX** telepítette a fürt megadott DNS előtag.  
**Terület** eléri a erőforráscsoport a régió.  
**PATH_TO_PRIVATE_KEY** [Választható] elérési útját, amely megfelel a megadott a tároló szolgáltatás fürt létrehozásakor nyilvános kulcs a titkos kulcs. Ez a beállítás használata -i jelző.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> A SSH kapcsolat portja 2200 – nem a szabványos port 22.

## <a name="dcos-tunnel"></a>Adatközpont/OS alagutas

Nyissa meg a alagutas Adatközpont, OS-kapcsolódó végpontjait, hajtsa végre a parancs az alábbihoz hasonló:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Most már hozzáférhet az Adatközpont, OS-kapcsolódó végpontokat:

- ADATKÖZPONT/OPERÁCIÓS RENDSZER:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Hasonlóképpen a többi API-khoz minden alkalmazáshoz a alagutas keresztül érheti el.

## <a name="swarm-tunnel"></a>Méhrajt alagutas

Nyissa meg a méhrajt végpontra egy alagutas, hajtsa végre a parancs az alábbihoz hasonló:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Most már beállíthatja, hogy a DOCKER_HOST környezeti változó az alábbi képlettel történik. Továbbra is a Docker parancssori kezelőfelületről használja a szokásos módon.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Hozzon létre egy SSH alagutas Windows rendszeren

Több SSH alagutak létrehozása a Windows lehetőségek is rendelkezésre állnak. A dokumentum fog ismertetik, hogyan lehet gitt használni ehhez.

GITT letöltése a Windows rendszerben, és futtassa az alkalmazást.

Írja be az állomásnév tevődik a fürt rendszergazdai felhasználónevével és az első diaminta a fürt nyilvános DNS-nevét. A **Host Name** fog kinézni: `adminuser@PublicDNS`. Írja be a **Port**2200.

![GITT konfigurációs 1](media/putty1.png)

Jelölje ki a **SSH** és a **hitelesítéshez**. Vegye fel a titkos kulcs fájl hitelesítéshez.

![GITT konfigurációs 2](media/putty2.png)

Jelölje ki a **alagutak** , és állítsa be az alábbi portokat továbbítani:
- **Forrásport:** A beállítás – Adatközpont/OS 80 használható vagy a méhrajt 2375.
- **Cél:** Az Adatközpont/OS vagy localhost:2375 localhost:80 méhrajt használni.

A következő példa be van állítva az Adatközpont/OS, de fog keresni hasonló Docker Swarm.

>[AZURE.NOTE] 80-as port nem lehet használja-e alagutas létrehozásakor.

![3 gitt konfigurációja](media/putty3.png)

Ha végzett, mentse a kapcsolat beállításainak, és csatlakozás a gitt munkamenetet. Amikor csatlakoztatja, megjelenik a port konfiguráció gitt eseménynaplójában talál.

![GITT eseménynaplójában talál.](media/putty4.png)

A alagutas beállította az az Adatközpont/operációs rendszer, amikor a kapcsolódó végpont címen érhető el:

- ADATKÖZPONT/OPERÁCIÓS RENDSZER:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

A alagutas, hogy beállított-Docker Swarm, amikor a a Docker CLI keresztül elérheti a méhrajt fürt. Először nevű Windows környezet változó konfigurálása `DOCKER_HOST` érték ` :2375`.

## <a name="next-steps"></a>Következő lépések

Üzembe helyezéséhez és tárolók Adatközpont/OS vagy méhrajt kezelése:

- [Azure tároló szolgáltatás és az Adatközpont/operációs rendszer használata](container-service-mesos-marathon-rest.md)
- [A tároló Azure szolgáltatás és Docker méhrajt használatára](container-service-docker-swarm.md)
