<properties
   pageTitle="Telepítse az Adatközpont/OS CLI |} Microsoft Azure"
   description="Telepítse az Adatközpont/OS CLI."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Tárolók, Micro szolgáltatások Adatközpont/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Ez az Adatközpont, OS-alapú ACS fürt a. Nincs szükség az ehhez méhrajt-alapú ACS fürtre vonatkozóan.

Először [az Adatközpont, OS-alapú ACS fürthöz csatlakozni](../articles/container-service/container-service-connect.md). Miután ezt megtette, telepítheti az Adatközpont/OS CLI az ügyfélgépen az alábbi parancsokat:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Régi verzióját Python használatakor Észreveheti néhány "InsecurePlatformWarnings". Nyugodtan figyelmen kívül hagyhatja, ezek.

Első lépések a rendszerhéj újraindítása nélkül, hogy futtatni:

```bash
source ~/.bashrc
```

Ez a lépés nem szükséges új ismertetése indításakor.

Most, hogy telepítve van-e a CLI győződhet meg:

```bash
dcos --help
```