<properties
   pageTitle="Csatlakoztassa a telefont használ C Linux |} Microsoft Azure"
   description="Csatlakoztassa a telefont a Azure IoT csomagja előre távoli felügyeleti megoldás az alkalmazás, a C Linux futó ismerteti."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Csatlakoztassa az eszközt a távoli felügyeleti előre beállított megoldás (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Készítése és a minta C ügyfél Linux futtatása

Az alábbi eljárások megtudhatja, hogy miként hozhat létre ügyfélalkalmazás, a C és a beépített és a Ubuntu Linux, futtatni, amely kommunikál a távoli felügyeleti előre beállított megoldás. A lépések elvégzéséhez szeretne egy eszközt Ubuntu 15.04 vagy 15.10 verzióját futtató. A folytatás előtt telepítse a csomagokat az Ubuntu eszközön használja a következő parancsot:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Az ügyfél-tárak telepítése mobileszközön

Az ügyfél Azure IoT központi tárakat telepítheti az Ubuntu eszközön használja a **apt get** -command csomagként érhetők el. Hajtsa végre az alábbi lépéseket a szoftvercsomag telepítésekor IoT központi ügyfél tár és a fejlécsorok fájlokat tartalmazó Ubuntu számítógépre:

1. A gép adja hozzá a AzureIoT tár:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Telepítse az azure-iot-sdk-c-fejlesztők csomag

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Az eszköz viselkedésének kód hozzáadása

A Ubuntu számítógépen hozzon létre egy nevű **távoli\_figyelése**. Az a **távoli\_figyelése** mappa létrehozása a négy fájlok **main.c** **távoli\_monitoring.c**, **távoli\_monitoring.h**, és **CMakeLists.txt**.

A IoT központi szerializáló ügyfél tárak adatmodell használatával az eszköz IoT központi és a parancsok kap IoT központból küld üzenet formátumának megadása.

1. Egy egyszerű szövegszerkesztő programban, nyissa meg a **távoli\_monitoring.c** fájlt. Adja hozzá a következő `#include` kimutatások:

    ```
    #include "iothubtransportamqp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Adja hozzá a következő változó nyilatkozatokat után a `#include` kimutatások. Cserélje ki a helyőrző [eszközazonosítót] és [eszköz kulcs] az ellenőrzési távoli megoldás irányítópult mobileszközére értékű. A IoT központi hostname (állomásnév) az irányítópult [IoTHub neve] helyett használhatja. Ha például **contoso.azure-devices.net**a IoT központi állomásnév esetén cserélje [IoTHub neve] **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Adja hozzá a következő kódot a modellt, amely lehetővé teszi, hogy az eszköz használatával kommunikál a IoT központi megadása. A modell Itt adhatja meg, hogy az eszköz elküldi hőmérsékleti, külső hőmérsékleti, páratartalom, és olyan eszközazonosítót telemetriai szerint. Az eszköz is küld az eszközről a metaadat-alapú IoT-hubon keresztül csatlakozott, beleértve az eszközt támogató parancsok listája. Ez az eszköz válaszol a parancsok **SetTemperature** és **SetHumidity**:

    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);

    DECLARE_STRUCT(SystemProperties,
      ascii_char_ptr, DeviceID,
      _Bool, Enabled
    );

    DECLARE_STRUCT(DeviceProperties,
      ascii_char_ptr, DeviceID,
      _Bool, HubEnabledState
    );

    DECLARE_MODEL(Thermostat,

      /* Event data (temperature, external temperature and humidity) */
      WITH_DATA(int, Temperature),
      WITH_DATA(int, ExternalTemperature),
      WITH_DATA(int, Humidity),
      WITH_DATA(ascii_char_ptr, DeviceId),

      /* Device Info - This is command metadata + some extra fields */
      WITH_DATA(ascii_char_ptr, ObjectType),
      WITH_DATA(_Bool, IsSimulatedDevice),
      WITH_DATA(ascii_char_ptr, Version),
      WITH_DATA(DeviceProperties, DeviceProperties),
      WITH_DATA(ascii_char_ptr_no_quotes, Commands),

      /* Commands implemented by the device */
      WITH_ACTION(SetTemperature, int, temperature),
      WITH_ACTION(SetHumidity, int, humidity)
    );

    END_NAMESPACE(Contoso);
    ```

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Az eszköz viselkedés kód hozzáadása

A függvények végrehajtani, ha az eszköz kapja a parancsokat a központi, és küldje el a központi szimulált telemetriai kód hozzáadása.

1. Adja hozzá a következő függvények, amikor az eszköz kapja a modellben definiált **SetTemperature** és a **SetHumidity** parancs végrehajtása:

    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }

    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```

2. Adja hozzá a következő üzenetet küld IoT központi függvényt:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Adja hozzá a következő függvény, amely csatlakozik, a szerializálási tárba, a SDK csomagjában talál:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Adja hozzá a következő függvény IoT központban összekapcsolása, küldje el üzenetek fogadására és a központi választani. Figyelje meg, hogyan az eszköz küld a metaadatait magát, a parancsok támogat, IoT-hubhoz csatlakozik, többek között. A metaadatok lehetővé teszi, hogy a megoldás frissítése állapotát az eszköz **futtatása** az irányítópulton:

    ```
    void remote_monitoring_run(void)
    {
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
      {
        if (serializer_init(NULL) != SERIALIZER_OK)
        {
          printf("Failed on serializer_init\r\n");
        }
        else
        {
          IOTHUB_CLIENT_CONFIG config;
          IOTHUB_CLIENT_HANDLE iotHubClientHandle;

          config.deviceId = deviceId;
          config.deviceKey = deviceKey;
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
          iotHubClientHandle = IoTHubClient_Create(&config);
          if (iotHubClientHandle == NULL)
          {
            (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
          }
          else
          {
            Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
            if (thermostat == NULL)
            {
              (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
            }
            else
            {
              STRING_HANDLE commandsMetadata;

              if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
              {
                printf("unable to IoTHubClient_SetMessageCallback\r\n");
              }
              else
              {

                /* send the device info upon startup so that the cloud app knows
                   what commands are available and the fact that the device is up */
                thermostat->ObjectType = "DeviceInfo";
                thermostat->IsSimulatedDevice = false;
                thermostat->Version = "1.0";
                thermostat->DeviceProperties.HubEnabledState = true;
                thermostat->DeviceProperties.DeviceID = (char*)deviceId;

                commandsMetadata = STRING_new();
                if (commandsMetadata == NULL)
                {
                  (void)printf("Failed on creating string for commands metadata\r\n");
                }
                else
                {
                  /* Serialize the commands metadata as a JSON string before sending */
                  if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                  {
                    (void)printf("Failed serializing commands metadata\r\n");
                  }
                  else
                  {
                    unsigned char* buffer;
                    size_t bufferSize;
                    thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                    /* Here is the actual send of the Device Info */
                    if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                    {
                      (void)printf("Failed serializing\r\n");
                    }
                    else
                    {
                      sendMessage(iotHubClientHandle, buffer, bufferSize);
                    }

                  }

                  STRING_delete(commandsMetadata);
                }

                thermostat->Temperature = 50;
                thermostat->ExternalTemperature = 55;
                thermostat->Humidity = 50;
                thermostat->DeviceId = (char*)deviceId;

                while (1)
                {
                  unsigned char*buffer;
                  size_t bufferSize;

                  (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                  if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed sending sensor value\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                  ThreadAPI_Sleep(1000);
                }
              }

              DESTROY_MODEL_INSTANCE(thermostat);
            }
            IoTHubClient_Destroy(iotHubClientHandle);
          }
          serializer_deinit();
        }
        platform_deinit();
      }
    }
    ```
    
    Hivatkozás Íme egy minta **DeviceInfo** üzenetet IoT központi indításakor:

    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
    
    Hivatkozások a következő példa **Telemetriai** IoT központi küldött üzenet:

    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
    
    A hivatkozást a következő **parancs** kapott IoT központból minta:
    
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>A forrás szerkesztésével meghívni a remote_monitoring_run függvény

Nyissa meg a **remote_monitoring.h** fájlt egy szövegszerkesztőben. Adja hozzá a következő kódot:

```
void remote_monitoring_run(void);
```

Nyissa meg a **main.c** fájlt egy szövegszerkesztőben. Adja hozzá a következő kódot:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Az ügyfélalkalmazás létrehozásához használandó CMake

A következő lépések bemutatják, hogyan az ügyfélalkalmazás létrehozásához használandó *CMake* .

1. Egy egyszerű szövegszerkesztő programban nyissa meg a **CMakeLists.txt** fájlt a **remote_monitoring** mappában.

2. Adja meg az alábbi utasításokat, hogyan hozhat létre az ügyfélalkalmazás meghatározása:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. A **remote_monitoring** mappában hozzon létre egy mappát, CMake hoz létre *Ellenőrizze* fájlok tárolására, és kattintson a parancsokat **cmake** , és **végezze el** az alábbi képlettel történik:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Futtassa az ügyfél-alkalmazást, és telemetriai küldése IoT hubhoz:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

