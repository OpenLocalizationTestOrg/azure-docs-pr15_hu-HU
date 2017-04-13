<properties
   pageTitle="Alkalmazás vagy szolgáltatás a felhasználó-specifikus Marathon |} Microsoft Azure"
   description="Hozzon létre egy alkalmazás vagy szolgáltatás a felhasználó-specifikus Marathon"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Tárolók, Marathon, Micro-szolgáltatások, Adatközpont/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Hozzon létre egy alkalmazás vagy szolgáltatás a felhasználó-specifikus Marathon

Azure tároló szolgáltatás fő kiszolgálók, amelyen azt előre Apache Mesos és Marathon biztosít. Ezeket az alkalmazásokat, a fürt téve használható, de a legjobb, ha nem használja a fő kiszolgálókat erre a célra. Például Marathon konfigurációja színcsúszkákkal és elő kell készítenie a fő kiszolgálókat maguk bejelentkezés módosítaná--ez egyedi fő kiszolgálókat, amelyeket némileg eltér a szabványos, és kell előkészítése, és önállóan kezelt javasolja. Emellett a konfiguráció szükséges egy csoport nem feltétlenül egy másik csapatnak optimális konfigurációt.

Ebben a cikkben megismerheti hogy miként vehet fel egy alkalmazás vagy szolgáltatás a felhasználó-specifikus Marathon.

Ez a szolgáltatás egyetlen felhasználó vagy csoport fog tartozni, mert szabadon azok megfelel módon állítja be. Azure tároló szolgáltatás is biztosítja, hogy a szolgáltatás futtatásához továbbra is. Ha nem sikerül a szolgáltatást, a Azure tároló szolgáltatás lesz a újraindítani. Az esetek többségében nem is észre legrövidebb leállás volt.

## <a name="prerequisites"></a>Előfeltételek

[Azure tároló szolgáltatás egy példánya Deploy](container-service-deployment.md) orchestrator típusú Adatközpont/OS, és [biztosítani, hogy az ügyfelek a fürtre csatlakozhat](container-service-connect.md). Is végezze el az alábbi lépéseket.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Hozzon létre egy alkalmazás vagy szolgáltatás a felhasználó-specifikus Marathon

Először hozzon létre egy JSON konfigurációs fájl, amely definiálja az alkalmazás szolgáltatás ki a létrehozni kívánt nevét. Itt használjuk `marathon-alice` keretrendszer neveként. Mentse a fájlt, az alábbihoz hasonló `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Ezután használja az Adatközpont/OS CLI az való telepítéséhez az Marathon példány a beállításokat, amelyek a konfigurációs fájl vannak beállítva:

```bash
dcos package install --options=marathon-alice.json marathon
```

Ekkor megjelenik a `marathon-alice` szolgáltatások lapján az Adatközpont/operációs rendszer felhasználói felületének futó szolgáltatás. A felhasználói felület lesz `http://<hostname>/service/marathon-alice/` Ha szeretne közvetlenül elérhető.

## <a name="set-the-dcos-cli-to-access-the-service"></a>A szolgáltatás eléréséhez az Adatközpont/OS CLI beállítása

Igény szerint állítsa be az Adatközpont/OS CLI új szolgáltatást eléréséhez állítsa a `marathon.url` tulajdonságot, mutasson a `marathon-alice` példány az alábbi képlettel történik:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Ellenőrizheti, hogy melyik példányának Marathon, amely a CLI ellen működik együtt az `dcos config show` parancsot. Visszatérés a parancs a fő Marathon szolgáltatás használata `dcos config unset marathon.url`.
