.. |br| raw:: html

.. _device-simulator:

Device Simulator
=================

본 시뮬레이터는 SKT의 OVS(Open V2X Service) 플랫폼 프로토콜을 따르는 Smartphone, BlackBox, ADAS 등의 단말 동작을 나타내는 시뮬레이터로,
`github ovse device simulator <https://github.com/ovseadmin/ovse_device_simulator>`__ 에서 받으실 수 있습니다. 

본 시뮬레이터는 :ref:`OVS플랫폼 규격 <ovs-webdocument-index>` 를 기반으로 구성되어 있습니다. 
상세한 프로토콜은 :ref:`규격의 8. 메시지 포맷 <message-format>` 을 참고하세요.

.. _device-simulator-introduction:

OVS Device Simulator Introduction
-----------------------------------


.. _device-simulator-architecture:

Device Simulator 구성
~~~~~~~~~~~~~~~~~~~~~~

본 시뮬레이터는 `node.js` 기반으로 구현되어 있으며, 기술 규격에 따라 MQTT 프로토콜에 준수하여 개발이 되어 있습니다. OVS 플랫폼 프로토콜에서 제공하는 단말 타입은 `OVC-G`, `OVC-M` 이며 해당 문서는 `OVC-G` 기준으로 설명되어 있습니다. 아래의 파일에 시뮬레이션 과정이 포함되어 있습니다.
 
.. code-block:: none

    device.js


.. _device-simulator-execution:

Device Simulator 실행 방법
~~~~~~~~~~~~~~~~~~~~~~~~~~~
본 시뮬레이터를 동작을 위해서는 기본적으로 node.js 가 설치되어 있어야 합니다. 
`node.js`의 설치 방법은 `node.js 홈페이지 <https://nodejs.org/en/>`__ 를 참고하세요.

.. code-block:: none

    $ git clone https://github.com/ovseadmin/ovse_device_simulator.git
    $ cd ovsClient_nodeJS
    $ npm install express, pem
    $ node device.js


.. _device-simulator-configuration:

Device Simulator 설정 방법
~~~~~~~~~~~~~~~~~~~~~~~~~~~

본 시뮬레이터 동작을 위한 설정은 본 Repository의 `config.js` 파일에 기술되어 있으며, 해당 설정을 통해 각자의 상황에 맞추어 시뮬레이션을 수행할 수 있습니다.
아래 코드의 내용 중에서 수정이 필요한 사항은 다음과 같습니다.

+--------------+--------------------------------------------------------------------------------------------------------------------+
| Key          | Description                                                                                                        |
+==============+====================================================================================================================+
| username     | MQTT Broker 접속에 필요한 Username입니다. :ref:`4. 서비스 세부 절차 <service-procedure>` 를 참조하여 발급받습니다. |
+--------------+--------------------------------------------------------------------------------------------------------------------+
| password     | MQTT Broker 접속에 필요한 Password입니다. :ref:`4. 서비스 세부 절차 <service-procedure>` 를 참조하여 발급받습니다. |
+--------------+--------------------------------------------------------------------------------------------------------------------+
| TxInterval   | 단말이 위치 데이터를 주기적으로 전송하는 주기를 의미합니다. (msec)                                                 |
+--------------+--------------------------------------------------------------------------------------------------------------------+
| numOfTxMsg   | 단말이 위치 데이터를 전송하는 총 갯수를 의미합니다.                                                                |
+--------------+--------------------------------------------------------------------------------------------------------------------+
| deviceType   | 시뮬레이션을 돌리고자 하는 디바이스 타입을 명시합니다. `OVC-G`, `OVC-M`                                            |
+--------------+--------------------------------------------------------------------------------------------------------------------+
| qos          | 단말이 OVS Platform에 전송하는 메시지 QoS를 명시합니다. 서비스 품질 보장을 위해서 1 이상으로 설정되어야 합니다.    |
+--------------+--------------------------------------------------------------------------------------------------------------------+

.. _device-simulator-flow:

OVS Device Simulator Flow
-----------------------------------
본 시뮬레이터는 아래의 Flow를 기반으로 작성되어 있으며, 아래의 Flow는 
`단말 프로시저 규격 <https://ovs-document.readthedocs.io/en/latest/procedure.html>`__ 과
`단말 전송 메시지 규격 <https://ovs-document.readthedocs.io/en/latest/message_format.html>`__ 을 참고 바랍니다.

.. image:: /images/device_simulator_flow_ovcg.png
	:width: 100%
	:align: center

제공해드리는 단말 시뮬레이터의 코드에는 위 Flow의 각 순서와 대응되는 주석이 표기되어 있습니다. 아래의 동작 예시 설명을 통해 자세히 알아보도록 하겠습니다.


.. _device-simulator-behavior-example:

Device Simulator 정상 동작 예시
~~~~~~~~~~~~~~~~~~~~~~~~~~~

정상적으로 설정된 Device Simulator의 동작 예시입니다. MQTT 연결에 필요한 `connetionOptions` 설정하여 MQTT object `client`를 생성합니다.

.. code-block:: none

    ////////////////////////////////////////////////
    // Flow #1 : Request Connection
    ////////////////////////////////////////////////

    const connectionOptions = {
        host: config.host,
        port: config.port, 
        username: config.username,
        password: config.password,
        clean: true,
        protocol: 'mqtt',
        reconnectedPeriod: config.reconnectedPeriod,
        connectTimeout: config.connectTimeout
    };
    const client = mqtt.connect(connectionOptions);


`client`가 MQTT Broker와 연결이 완료되면 V2N service message를 수신하기 위한 **Subscribe 함수** 와 단말의 위치정보를 주기적으로 전송하기 위한 **Publish 함수** 를 호출합니다.

.. code-block:: none

    client.on("connect",function(connack){
        if (connack.cmd=='connack'){
            console.log("MQTT Connection success!");
            // serialNo 이용 subscribe
            subscribe(utils.deviceTopic+serialNo);
            // 주기적 위치 전송
            publish(utils.locationTopic);
        }else{
            console.log("MQTT connection fail");
        }    
    });


`client`가 `serialNo`를 이용하여 생성한 `topic`으로 MQTT Broker에 Subscribe 기능을 수행합니다.


.. code-block:: none

    ////////////////////////////////////////////////
    // Flow #2. Subscribe a topic for V2N services 
    ////////////////////////////////////////////////

    function subscribe(topic){
        client.subscribe(topic,{qos: config.qos},function(err, granted){
            if (!err){
                console.log(topic+' subscription success!');
                console.log(topic+' subscription is granged: '+ JSON.stringify(granted));
            }else{
                console.log(topic+' subscription fail!');
            }
        });
    }

`client`가 Publish하는 데이터는 크게 2개 종류로 구분됩니다. 
첫째, 주기적으로 전송하는 **위치데이터** 와 둘째, V2N 이벤트가 발생했을 때 전송하는 **V2N 이벤트 데이터** 입니다. 각각은 `topic`에 따라 구분됩니다.

.. code-block:: none

    function publish(topic){
        var idx =0;
        switch (topic){
            ////////////////////////////////////////////////
            // Flow #3. Publish current location
            ////////////////////////////////////////////////
            case utils.locationTopic:
                if (client.connected == true){...
                },config.TxInterval)};
                break;

            ////////////////////////////////////////////////
            // Flow #4. Publish V2N event
            ////////////////////////////////////////////////
            case utils.eventTopic:
                if (client.connected == true){...
                } 
                break;
        }
    }


`client`가 Subscribe하고 있는 `topic`에 의해 메세지를 수신한 경우 사용자에게 알림 메세지를 전달합니다. 수신하는 메세지의 종류는 크게 2개 종류로 구분됩니다. 첫 째, 특정 단말에게 **정보성 메세지** 를 전달하는 메세지와 둘 째, 긴급제동알림 서비스, 전방 낙하물 주의 등 **V2N 서비스 메세지** 입니다. 

.. code-block:: none

    ////////////////////////////////////////////////
    // Flow #5. Receive a V2N service message
    ////////////////////////////////////////////////

    client.on('message',function(topic, message){
            var obj = JSON.parse(message);
            // North bound 통한 noti.
            if (obj.type == 9999){
                console.log(colors.yellow(obj.message));   
            }
            // event message
            else{
                var et_str = utils.typeParsing(obj.type);
                var tn_str = utils.tunnelParsing(obj.tunnel);
                var dte_str = utils.distanceToEventParsing(obj.distanceToEvent);
                console.log(colors.yellow(tn_str + " "+ dte_str + " " + et_str + "입니다! 조심하세요!"));
                console.log("Rx topic is "+ topic);
                console.log("Rx message is "+ message); 
            }      
    });
