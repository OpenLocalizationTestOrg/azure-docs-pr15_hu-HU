<properties
   pageTitle="CLI használ szolgáltatási háló fürt személlyel |} Microsoft Azure"
   description="Azure CLI használatáról a szolgáltatás háló fürtre vezérléséhez"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Az Azure CLI használatával háló fürt szolgáltatás használata

A Linux gépek Linux az Azure CLI használ szolgáltatási háló fürt használhatók interaktív módon.

Az első lépés kérése a CLI a mely számjegy munkatárs a legújabb verzióját, és állítsa be a mappa elérési útját a következő parancsok végrehajtása az:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Az egyes parancsok támogat, beírhatja a súgó adott parancs a parancs neve. Automatikus – befejezés a parancsok használata támogatott. Például a következő parancs lépve súgó az összes alkalmazás parancsot. 

```sh
 azure servicefabric application 
```

A súgó egy adott parancs, az alábbi példában látható további szűrhető:

```sh
 azure servicefabric application  create
```

Ha engedélyezni szeretné a CLI automatikus-kiegészítés, futtassa az alábbi parancsokat:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

A következő parancsok végrehajtása csatlakozzon a fürthöz, és a csomópontok megjelenítése a fürt:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Elnevezett paraméterek és találja, hogy dátumról van szó, írhat – Súgó parancs után. Példa:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Amikor csatlakozik egy olyan számítógépen **, amely nem része a fürt**több gép fürt, használja az alábbi parancsot:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

A PublicIPorFQDN címke cserélje a valós IP-címének vagy teljesen minősített tartománynév szükség szerint. Csatlakozáskor a több elem gép fürtre gépi **részét képező a fürt**, használja az alábbi parancsot:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Használhatja a PowerShell, vagy a Linux szolgáltatás háló fürt vezérléséhez CLI létrehozása az Azure portálon keresztül. 

**Figyelmeztetés:** Ezeket a fürt nem biztonságos, így, előfordulhat, hogy kell megnyitása meg az egyik párbeszédpanelt a nyilvános IP-címet a fürt jegyzék hozzáadásával.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Csatlakozás szolgáltatás háló fürthöz az Azure CLI használatával

A következő Azure CLI parancsok bemutatják, hogyan csatlakozhat egy biztonságos fürt. A tanúsítvány részleteinek egyeznie kell a fürt csomópontok tanúsítványt.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Ha a tanúsítvány hitelesítésszolgáltatók (CA) tartalmaz, például az alábbi példa a--hitelesítésszolgáltató tanúsítványának elérési út paramétert hozzá szeretné adni szüksége: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Ha több hitelesítésszolgáltató, használja az elválasztó vesszővel.
 
Ha a közös a tanúsítvány neve nem egyezik meg az internetkapcsolat endpoint, használhatja a paraméter is `--strict-ssl` megkerülje az igazolás, ahogy az alábbi parancsot: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Ha szeretné az hitelesítésszolgáltató igazolás átugrása, hozzáadhat a – elutasítás jogosulatlan paraméter, ahogy az alábbi parancsot: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
A kapcsolódás után kell a fürt vezérléséhez más CLI parancsai futtathatók. 

## <a name="deploying-your-service-fabric-application"></a>A szolgáltatás háló alkalmazás telepítése

Hajtsa végre a következő parancsokat, regisztrálja, és indítsa el a szolgáltatásalkalmazás háló:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Az alkalmazás frissítése

A folyamat hasonlít a [Windows folyamat](service-fabric-application-upgrade-tutorial-powershell.md)).

Összeállítása, másolja a vágólapra, regisztrálni és az alkalmazás létrehozása a legfelső szintű-címtárból. Ha az alkalmazás példányának neve háló: / MySFApp, és a típus MySFApp, a parancsok táblázatkifejezést a következő lesz:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Az alkalmazás a módosítást, és a módosított szolgáltatás újraépítéséhez.  A módosított szolgáltatás nyilvánvalóan fájl (ServiceManifest.xml) frissített-verzióra, és frissítse a Service (és kódot vagy Config vagy adatokat értelemszerűen). Az alkalmazás és a módosított szolgáltatás frissült-gyel is módosíthat a az alkalmazás jegyzék (ApplicationManifest.xml).  Ezután másolja a vágólapra, regisztrálhatja a frissített alkalmazást az alábbi parancsokat:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Most kiindulhat az alkalmazás frissítése az alábbi parancsot:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Figyelheti a alkalmazás frissítés SFX használatával. Néhány perc alatt az alkalmazás lenne a frissített.  Frissített alkalmazás egy hiba miatt próbálja is, és jelölje be az Automatikus visszaállítási szolgáltatás háló funkciója.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="copying-of-the-application-package-does-not-succeed"></a>Nem sikerült az alkalmazáscsomag másolása

Jelölje be, ha `openssh` telepítve van. Alapértelmezés szerint Ubuntu asztali nincs telepítve van. Telepítse a következő parancsot:

```
 sudo apt-get install openssh-server openssh-client**
```

Ha a probléma nem szűnik meg, próbálja meg letiltása a PAM ssh a **sshd_config** fájlt, az alábbi parancsokkal módosításával:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Ha a probléma továbbra is fennáll, növelje számát ssh munkamenetek hajtja végre az alábbi parancsokat:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Billentyűparancsok használata a ssh (nem pedig a jelszó) hitelesítési nem még támogatott (mivel a platformot használja ssh csomagok másolása), úgy használja helyette a jelszó-hitelesítést.


## <a name="next-steps"></a>Következő lépések

Állítsa be a fejlesztői környezet, és a Linux fürtre egy szolgáltatás háló alkalmazás telepítéséhez.
