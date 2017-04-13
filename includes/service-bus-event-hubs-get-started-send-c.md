## <a name="send-messages-to-event-hubs"></a>Esemény hubok üzeneteket küldeni

Ebben a szakaszban a C alkalmazás események küldeni az esemény központi ír azt. A [Projekt Apache Qpid](http://qpid.apache.org/)a Proton AMQP diatárban fogjuk használni. Ez a szolgáltatás Bus sorban várakozó és témakörök használata AMQP c megjelenített [Itt](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504), papírlapnak megfelelő. További információ a [Qpid Proton](http://qpid.apache.org/proton/index.html)dokumentációjában.

1. [Qpid AMQP Messenger lapra](http://qpid.apache.org/components/messenger/index.html)kattintson a **Telepítés Qpid Proton** hivatkozásra, és kövesse az utasításokat a környezettől függően. Feltételezzük fog egy Linux környezet; Ha például egy [Azure Linux virtuális](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04 együtt.

2. A Proton tárban fordítása, a következő csomagok telepítése:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Töltse le a [Qpid Proton tárban](http://qpid.apache.org/proton/index.html) található, és bontsa ki azt, például:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Hozzon létre egy összeállítás directory, a fordítás és a telepítés:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. A munka címtárában **sender.c** a következő tartalommal nevű új fájl létrehozása. Ne feledje, hogy az érték az esemény központi és névtér nevét helyettesítő (általában az utóbbi van `{event hub name}-ns`). A korábban létrehozott **SendRule** is cserélje le a kulcs URL-címként kódolt verzióját. URL-kódolás teheti azt [az alábbi](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Feltételezve, hogy a **ÖET**a fájl fordítási:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] Kód, a azt használja az 1-kimenő ablakban a kimenő üzenetek minél korábban beállítást. Általánosságban elmondható az alkalmazás próbálja meg köteg üzenetek növelheti a teljesítményt. Lásd: [Qpid AMQP Messenger lapon](http://qpid.apache.org/components/messenger/index.html) a Qpid Proton tár ez, és a többi környezetben, és, amelynek kötések nyújtott platformokon használatáról további információt (jelenleg Perl, PHP, Python és fonetikus).
