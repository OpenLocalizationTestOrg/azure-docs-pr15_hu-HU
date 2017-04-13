<properties
    pageTitle="Első lépések a IoT központi átjáró SDK |} Microsoft Azure"
    description="A Azure IoT átjáró SDK forgatókönyv Linux ismerje meg az Azure IoT átjáró SDK használatakor alapfogalmak mutatja be."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT átjáró SDK (beta) - Linux használatának első lépései

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Hogyan hozhat létre a minta

Mielőtt hozzákezdene, be kell [állítania a fejlesztői környezet] [ lnk-setupdevbox] a Linux a SDK csomagjában talál.

1. Nyissa meg a rendszerhéj.
2. Nyissa meg a helyi példányt az **azure-iot-átjáró-sdk** tárházba, a legfelső szintű mappában.
3. A **tools/build.sh** parancsfájl futtatásához. A parancsfájl a **cmake** segédprogram hozzon létre egy **összeállítása** a legfelső szintű mappájára a helyi példányt az **azure-iot-átjáró-sdk** tárházba nevű mappát, és egy makefile létrehozásához. A parancsprogram a megoldást hoz létre, majd a vizsgálatok futtatása.

> [AZURE.NOTE]  Minden alkalommal futtatja a **build.sh** parancsfájlt, törli, és ezután létrehozza a legfelső szintű mappájára a helyi példányt az **azure-iot-átjáró-sdk** tárházba **összeállítása** mappájában.

## <a name="how-to-run-the-sample"></a>A minta futtatása

1. A **build.sh** parancsfájl az eredményt a helyi példányt az **azure-iot-átjáró-sdk** tárházba **összeállítása** mappájában hoz létre. Ide tartoznak a két modulokat, a következő példában használt.

    Az összeállítás parancsfájl **liblogger_hl.so** a helyezi a **build/modulok/naplózó/** mappát és **libhello_world_hl.so** a a **build/modulok/hello_world/** mappát. A JSON beállításfájl alább látható módon használja az alábbi elérési utak értéket a **modul elérési útja** .

2. A **minták/hello_world/src** mappa fájl **hello_world_lin.json** egy példa JSON beállításfájl Linux futtatásához a minta használható. Az alább látható példa JSON beállítások feltételezi, hogy a minta futtatja a helyi példányt az **azure-iot-átjáró-sdk** tárházba gyökerében.

3. A **logger_hl** modulhoz **argumentumok** szakaszában állítsa be a napló adatokat tartalmazó fájl elérési útját és nevét a **fájlnév** értéket.

    Ez a példa, amely azt a mappát, ahol a minta futtatása a **log.txt** ír Linux JSON beállítások fájlok.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. A rendszerhéj nyissa meg a **azure-iot-átjáró-sdk** mappát.
4. A következő parancsot:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
