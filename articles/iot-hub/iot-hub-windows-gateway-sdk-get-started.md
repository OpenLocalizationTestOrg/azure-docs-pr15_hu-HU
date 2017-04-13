<properties
    pageTitle="Első lépések a IoT központi átjáró SDK |} Microsoft Azure"
    description="Azure IoT átjáró SDK útmutató – ismerje meg az Azure IoT átjáró SDK használatakor alapfogalmak bemutatásához Windows használata."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT átjáró SDK (beta) – a Windows használatának első lépései

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Hogyan hozhat létre a minta

Mielőtt hozzákezdene, be kell [állítania a fejlesztői környezet] [ lnk-setupdevbox] a SDK használata a Windows számára.

1. Nyisson meg egy **VS2015 Fejlesztőeszközök parancssorából** parancssort.
2. Nyissa meg a legfelső szintű mappa a helyi példányt az **azure-iot-átjáró-sdk** tárházba.
3. Futtassa a **Eszközök\\build.cmd** parancsfájl. A parancsfájl létrehoz egy Visual Studio megoldásfájlt, a megoldást hoz létre, és a vizsgálatok futtatása. A Visual Studio megoldás **összeállítása** mappában a helyi példányt az **azure-iot-átjáró-sdk** tárházba a található.

## <a name="how-to-run-the-sample"></a>A minta futtatása

1. A **build.cmd** parancsfájl egy **összeállítása** nevű a helyi példányban a tárat, mappát hoz létre. Ez a mappa a következő példában használt két modulokat tartalmazza.

    Az összeállítás parancsfájl **logger_hl.dll** a helyezi a **összeállítása\\modulok\\naplózó\\hibakeresési** mappát és **hello_world_hl.dll** a a **összeállítása\\modulok\\hello_world\\hibakeresési** mappát. A JSON beállításfájl alább látható módon használja az alábbi elérési utak értéket a **modul elérési útja** .

2. A fájl **hello_world_win.json** a **minták\\hello_world\\src** mappában egy példa JSON beállításfájl Windows futtatásához a minta használható. Az alább látható példa JSON beállítások feltételezi, hogy a a IoT átjáró SDK tárat a **c** meghajtó gyökerébe klónozva. Ha egy másik helyre töltötte le, akkor a **modul elérési útja** értékek kiigazítás a **minták\\hello_world\\src\\hello_world_win.json** fájl lehetőséget.

3. A **logger_hl** modulhoz **argumentumok** szakaszában állítsa be a napló adatokat tartalmazó fájl elérési útját és nevét a **fájlnév** értéket.

    Ez a példa a Windows, a **c** meghajtó legfelső szintű a **log.txt** fájl írt JSON beállítások fájl.

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. A parancssorablakban nyissa meg a helyi példányt az **azure-iot-átjáró-sdk** tárházba a legfelső szintű mappájára.
4. A következő parancsot:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md