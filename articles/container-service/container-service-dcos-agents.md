<properties
   pageTitle="Nyilvános és titkos Adatközpont/OS ügynök készletek ACS |} Microsoft Azure"
   description="A nyilvános és titkos ügynök készletek működése a egy Azure tároló szolgáltatás fürthöz."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Adatközpont/OS ügynök készletek Azure tároló szolgáltatás

Adatközpont/OS Azure tároló szolgáltatási ügynökök nyilvános vagy magánjellegű készletek osztja fel. A telepítő vagy kvótáját kisegítő lehetőségek a tároló szolgáltatás gépek között érintő tehetők. A gép lehet érhető el az interneten (nyilvános) vagy belső (személyes) tartani. Ez a cikk rövid áttekintést ad meg, miért vannak a nyilvános és titkos erőforráskészlethez tartozik.

### <a name="private-agents"></a>A magánjellegű ügynökök beállítása

Magánjellegű ügynök csomópontok futtassa a átirányítható hálózaton keresztül. A hálózat révén a nyilvános zóna biztonsági útválasztó vagy a rendszergazda zóna csak érhető el. Alapértelmezés szerint az Adatközpont/OS elindítja a magánjellegű ügynök csomópontok-alkalmazásokat. Olvassa el az [Adatközpont/OS dokumentáció](https://dcos.io/docs/1.7/administration/securing-your-cluster/) hálózati biztonsággal kapcsolatos további információkat.

### <a name="public-agents"></a>Nyilvános ügynökök beállítása

Nyilvános ügynök csomópontok Adatközpont/OS alkalmazások és -szolgáltatások futtassa a nyilvánosan hozzáférhető hálózaton keresztül. Olvassa el az [Adatközpont/OS dokumentáció](https://dcos.io/docs/1.7/administration/securing-your-cluster/) hálózati biztonsággal kapcsolatos további információkat.

## <a name="using-agent-pools"></a>Ügynök készletek használata

Alapértelmezés szerint a **Marathon** bármilyen új alkalmazás a *magánjellegű* ügynök csomópontok üzembe helyezése. Ha a explicit módon telepíteni az alkalmazást az alkalmazás létrehozása során a *nyilvános* csomópontot. Kattintson a **választható** fülre, és írja be a **slave_public** az **Erőforrás-szerepkörök elfogadott** érték. Ez a folyamat dokumentált [Itt](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) és [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentációjában.

## <a name="next-steps"></a>Következő lépések

Olvassa el [az Adatközpont/OS tárolók kezelésével](container-service-mesos-marathon-ui.md)kapcsolatban további tudnivalókat.

Ismerje meg, [Nyissa meg a tűzfalat](container-service-enable-public-access.md) a Adatközpont/OS tároló nyilvános elérésének engedélyezése Azure által biztosított.