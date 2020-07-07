.. |br| raw:: html

Interface Specification
========================

vsc Interface
-------------

``vsc interface`` 는 ``OVC`` 와  ``OVS`` 간 인터페이스이며, 
해당 인터페이스은 ``MQTT`` 프로토콜로 개발되며, 
본 섹션에서는 vsc interface 상에서 OVS 기능을 활용한 프로토콜을 명세하며 상세 프로토콜은 
`Client Types <https://ovs-document.readthedocs.io/en/latest/client.html>`__ 에 명세된 단말의 타입에 따라 상이하게 기술됩니다.

.. note::
	
    본 절을 이해하기 위해서 아래 명시된 프로토콜 및 프로그래밍 언어에 대한 이해가 필요합니다.

        - MQTT Protocol
        - Node.js Program Language 



vsc Interface for ``OVC-G`` (a.k.a. vsc-g Interface)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

OVC-G 단말을 위한 vsc Interface의 Flow는 다음과 같으며, 각 단계별로 절차를 ``Node.js`` 기반 코드로 소개합니다.

.. image:: /images/interface_01.png
	:width: 100%
	:align: center


도식화된 Flow는 크게 4가지 단계 ``Stage`` 로 구성됩니다. 일반적으로 ``V2X Event Report`` 와 
``V2X Event Notification Reception`` 은 순서와 상관없이 이벤트 발생에 따라 비동적으로 발생합니다. 

================================  ===================================================================
Stages                            Description              
================================  ===================================================================
Preparation                       | OVC-G가 OVS 상호 간 서비스를 호출하기 위해서 필요한 연결, 인증, 푸시 메시지 수신을
                                  | 위한 설정 등 기본적인 항목을 준비하는 단계
Location Report                   | OVC-G가 GPS로부터 수신한 현재 위치를 OVS에 주기적으로 반복 보고하는 단계
V2X Event Report                  | OVC-G가 VAC로부터 전달받은 V2X Event를 OVS에 보고하는 단계
V2X Event Notification Reception  | OVS가 타 OVC로부터 전달받은 V2X Event 중 해당 OVC-G와 연계된 Event를 
                                  | 푸시하여 OVC-G가 수신하는 단계
================================  ===================================================================

아래부터는 상기 vsc-g Flow의 순서를 간단한 예제 코드와 함께 설명합니다.

1. 
``Connect to OVS`` 순서에서는 OVC-G가 OVS에 연결하는 단계입니다. MQTT Broker에 접속하는 connect 단계 
`MQTT Connect 참고 <https://www.hivemq.com/blog/mqtt-essentials-part-3-client-broker-connection-establishment/>`__ 와 동일합니다.
단, 접속할 때는 다음 Parameter를 적용하여 connect 합니다.

=============  =============================================
Parameters     Value
=============  =============================================
host           tcp://192.168.1.170
port           1883
username       발급된 고객사의 userName
password       발급된 고객사의 passWord
clientId       단말 식별 번호 (기능상 UserName과 동일하게 처리 가능)
cleanSession   true
keepAlive      60
=============  =============================================


``Example Code`` 

.. code-block:: javascript

    var mqtt = require('mqtt');

    //OVS 접속 및 설정 
    var messageSender = mqtt.connect({ 
        host: 192.168.1.170, 
        port: 1883, 
        username: {고객사에서 등록한 username},
        password: {고객사에서 등록한 password},
        clean: true,
        keepalive: 60,
        protocol: 'mqtt'
    });

    //OVS 접속 시도에 따른 Callback
    messageSender.on('connect', function(connack) {

        if (connack.cmd == 'connack'){
            // 성공적인 OVS 접속
        } else
            // 접속 실패, 및 원인 파악 필요
    });


2.
``Subscribe a topic for receiving V2X notification`` 순서에서는 
OVC-g가 향후에 V2X Event 수신 할 수 있도록 V2X Event을 제공하는 Topic에 Subscription을 합니다. 
Topic은 아래와 같은 룰을 따라 설정합니다.

=============  =============================================
Topic          v2x/device/{userName}
=============  =============================================

``Example Code`` 

.. code-block:: javascript

    messageSender.subscribe('v2x/device/{userName}, {qos: 1}, function(err, granted) {

        if (err)
        {
          // Topic에 정상적으로 Subscribe 되지 않는 경우 원인
        } else {
          // Topic에 정상적으로 Subscribe 된 경우       
        }
        
      });


3.

vsc Interface for ``OVC-m``
~~~~~~~~~~~~~~~~~~~~~~~~~~~