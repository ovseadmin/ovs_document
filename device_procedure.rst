.. |br| raw:: html

.. _device-registration:

Device 연동 절차
=================

이 매뉴얼은 OVSE 플랫폼에 연결되는 단말장치, OVC를 개발하는 파트너들을 위한 것입니다. 장치가 어떻게 플랫폼과 통신하는지 설명합니다.

OVSE 플랫폼을 사용하여 V2N Service Application을 개발/서비스 하고자 하는 파트너는 :ref:`5. 구성요소(Entity) 등록 <entity-registration>` 를 참고하십시오. 

App 개발자는 :ref:`7. API 규격 <api-specification>` 과 :ref:`9. SDK <platform-sdk>` 를 참고하십시오.

이 매뉴얼에서는 OVC가 OVSE 플랫폼과 어떻게 연결되는지 설명합니다. 실제 OVC 단말과 OVSE플랫폼이 주고받는 메세지에 대한 상세한 내용은 :ref:`8. 메세지 포맷 <message-format>` 을 참고하십시오.

OVSE 플랫폼과 OVC를 연동하여 서비스하기 위해서는 다음의 VSC Interface를 이해하여야 합니다. 그리고 :ref:`5. 구성요소(Entity) 등록 <entity-registration>`의 단말 등록이 완료된 상황임을 가정합니다. 


* **VSC Interface**

``VSC Interface`` 는 ``OVC`` 와  ``OVSE`` 간 인터페이스이며, 해당 인터페이스는 ``MQTT`` 프로토콜로 개발되었습니다.

본 섹션에서는 vsc interface 상에서 OVSE 기능을 활용한 프로토콜을 명세하며, 상세 프로토콜은 
`Device Types <https://ovs-document.readthedocs.io/en/latest/entity_architecture.html>`__ 에 명세된 단말의 타입에 따라 상이하게 기술됩니다.

.. note::
	
    본 절을 이해하기 위해서 아래 명시된 프로토콜 및 프로그래밍 언어에 대한 이해가 필요합니다.

        - MQTT Protocol
        - Node.js Program Language 



VSC Interface for ``OVC-g`` (a.k.a. vsc-g Interface)
----------------------------------------------------------------

OVC-g 단말을 위한 VSC Interface의 Flow는 다음과 같으며, 각 단계별로 절차를 ``Node.js`` 기반 코드로 소개합니다.

.. image:: /images/device_procedure/ovcg_procedure_v2.png
	:width: 100%
	:align: center


도식화된 Flow는 크게 4가지 단계 ``Stage`` 로 구성됩니다. 일반적으로 ``V2N Event Report`` 와 
``V2N Event Notification Reception`` 은 순서와 상관없이 이벤트 발생에 따라 비동기적으로 발생합니다. 

================================  ===================================================================
Stages                            Description              
================================  ===================================================================
Preparation                       | OVC-g가 OVSE 상호 간 서비스를 호출하기 위해서 필요한 연결, 인증, 푸시 메세지 수신을
                                  | 위한 설정 등 기본적인 항목을 준비하는 단계
                                  | 1. Connect to OVSE
                                  | 2. Subscribe a Topic for Receiving V2N Notification
                                  |
Location Report                   | OVC-g가 GPS로부터 수신한 현재 위치를 OVSE에 주기적으로 반복 보고하는 단계
                                  | 3. Publish OVC-g's Current Location
                                  |
V2N Event Report                  | OVC-g가 VAC로부터 전달받은 V2N Event를 OVSE에 보고하는 단계
                                  | 4. Publish V2N Event detected by OVC-g
                                  |
V2N Event Notification Reception  | OVSE가 타 OVC로부터 전달받은 V2N Event 중 해당 OVC-g와 연계된 Event를 
                                  | 푸시하여 OVC-g가 수신하는 단계
                                  | 5. Receive a V2N Event Notification relevant to OVC-g
                                  | 6. Publish the result of the notifcation message handling
================================  ===================================================================

아래부터는 상기 vsc-g Flow의 순서를 간단한 예제 코드와 함께 설명합니다.

Preparation 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Preparation 단계는 OVSE에 접속하는 단계와 토픽 Subscription의 두 단계로 구성됩니다.


Connect to OVSE
''''''''''''''''''

``Connect to OVSE`` 순서에서는 OVC-g가 OVSE에 연결하는 단계입니다. MQTT Broker에 접속하는 connect 단계 
`MQTT Connect 참고 <https://www.hivemq.com/blog/mqtt-essentials-part-3-client-broker-connection-establishment/>`__ 와 동일합니다.
단, 접속할 때는 다음 Parameter를 적용하여 connect 합니다.

=============  ===========================================================
Parameters     Value
=============  ===========================================================
host           tcp://192.168.1.170 (*Will be changed*) 
port           1883 (*Will be changed*) 
username       발급된 고객사의 userName (할당 받은 Access Token (20자리) 값)
password       발급된 고객사의 passWord
clientId       단말 식별 번호 (기능상 UserName과 동일하게 처리 가능)
cleanSession   true
keepAlive      60
=============  ===========================================================

.. rst-class:: text-align-justify

Username 필드에는 해당 단말의 Credentials ID 값을 입력합니다. 단말의 Credentials ID 값은 `OVSE REST API /api/ovs/v1/device`__ 를 통해서 얻을 수 있습니다. cleanSession 필드가 true면 이전 세션 정보가 아직 존재할 경우 클라이언트와 서버에서 이전 세션 정보를 삭제합니다.


``Example Code`` 

.. code-block:: javascript

    var mqtt = require('mqtt');

    //OVSE 접속 및 설정 
    var messageSender = mqtt.connect({ 
        host: 192.168.1.170, 
        port: 1883, 
        username: {고객사에서 등록한 username},
        password: {고객사에서 등록한 password},
        clean: true,
        keepalive: 60,
        protocol: 'mqtt'
    });

    //OVSE 접속 시도에 따른 Callback
    messageSender.on('connect', function(connack) {

        if (connack.cmd == 'connack'){
            // 성공적인 OVSE 접속
        } else
            // 접속 실패, 및 원인 파악 필요
    });


Subscribe a Topic for Receiving V2N Notification
''''''''''''''''''''''''''''''''''''''''''''''''''''''

``Subscribe a Topic for Receiving V2N Notification`` 순서에서는 
OVC-g가 향후에 V2N Event 수신 할 수 있도록 V2N Event을 제공하는 Topic에 Subscription을 합니다. 
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


Location Report 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
OVSE 기반 V2N 서비스를 위해서는 OVC-g 단말의 위치가 주기적으로 OVSE로 전송이 되어야 합니다. 

Publish OVC-g's Current Location
''''''''''''''''''''''''''''''''''''''''''''''''''''''
``Publish OVC-g's Current Location`` 순서에서 선행되어야 하는 조건은 OVC-g 단말이 GPS 센서로 현재 자신의 위치 좌표를 받는 것입니다. 
GPS 좌표를 정상적으로 수신 한 경우에 OVC-g는 자신의 위치를 OVSE에 전달 ``Publish`` 합니다. 전달 시에는 다음의 Topic에 Publish를 합니다.

추가로 본 과정은 OVC-g가 GPS 좌표를 획득할때 마다 반복되며, 일반적으로 V2X 서비스 품질을 고려하여서는 1초마다 진행해야 하나 고객사의 입장에
따라 주기가 증가할 수 있으나 주기가 증가할 수록 일부 V2N 서비스 및 서비스 품질이 떨어집니다.

=============  =============================================
Topic          v2x/location
=============  =============================================

본 순서에서 메세지를 전달할때는 다음 메세지를 ``JSON`` 형태로 포함합니다.
(이는 :ref:`8. 메세지 포맷 <message-format>`의 VDPosition에 해당합니다.)

=============  ====  ========  =============================================
Key            M/O   Type      Description
=============  ====  ========  =============================================
dev_type       M     Integer   OVC-g를 탑재한 단말의 타입
time           M     Integer   메세지 전달 시간 (msec, epoch)
dev_id         M     String    OVSE에 등록된 단말 식별자
speed          O     Integer   현재 속도 값
location       M               | 현재 위치 좌표 (WGS84 Coordination)
                               | Child key로 "lat", "lon" 를 적시
=============  ====  ========  =============================================

``Example Data``

.. code-block:: json

    {
        "dev_type": 97,
        "time": 1571273913571,
        "dev_id": 3333,
        "speed": 60,
        "location": {
            "lat": 37.510296,
            "lon": 127.062512
        }
    }

``Example Code``

.. code-block:: javascript

  var locationReportData = {
    "dev_type": {dev_type},
    "time": new Date().getTime(),
    "dev_id": {deviceID},
    "speed": {speed},
    "location": {
      "lat": {lat},
      "lon": {lon}
    }
  };

  sendingMSG = JSON.stringify(eval(locationReportData));
  messageSender.publish('v2x/location', sendingMSG, {qos: 1}, function());

Publish V2N Event detected by OVC-g
''''''''''''''''''''''''''''''''''''''''''''''''''''''
``Publish V2N Event detected by OVC-g`` 순서에서는 OVC-g가 VAC로부터 
해당 단말이 인식한 V2N Event를 수신 받은 경우, 이를 OVSE에 리포팅하여 OVSE가 다른 OVC 에게 전달하는 과정을 유도하는 과정을 기술합니다.

Topic은 아래와 같은 룰을 따라 설정합니다.

=============  =============================================
Topic          v2x/event
=============  =============================================

본 순서에서 메세지를 전달할때는 다음 메세지를 ``JSON`` 형태로 포함합니다.
(이는 :ref:`8. 메세지 포맷 <message-format>`의 VDEventReport에 해당합니다.)

================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
time              M     Integer   메세지 전달 시간 (msec, epoch)
eventType         M     Integer   이벤트 종류 (To-be-specified)
distanceToEvent   O     Integer   이벤트 지점까지의 거리
location          M               | 이벤트 발생 위치 정보 (WGS84 Coordination)
                                  | Child key로 "lat", "lon" 를 적시
================  ====  ========  =============================================

``Example Data``

.. code-block:: json

    {
        "time": 1571308818766,
        "eventType": 1,
        "distanceToEvent": -10,
        "location": {
            "lat": 37.51477,
            "lon": 127.060067
        }
    }

``Example Code``

.. code-block:: javascript

  var v2xEventReportData = {
    "time": new Date().getTime(),
    "eventType": 1,
    "distanceToEvent": -10,
    "location": {
      "lat": latitudeValue[sequence % latitudeValue.length],
      "lon": longitudeValue[sequence % latitudeValue.length]
    } 
  };

  sendingMSG = JSON.stringify(eval(v2xEventReportData));
  messageSender.publish(utils.eventTopic, sendingMSG, {qos: config.qos}, function(){
    console.log(colors.cyan('Message [JSON | ' + Buffer.from(JSON.stringify(eval(v2xEventReportData))).length + ' Bytes] : ' 
    + JSON.stringify(eval(v2xEventReportData), 0, 2) + '\n'));
  });


V2N Event Notification Reception 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Receive a V2N Event Notification relevant to OVC-g
''''''''''''''''''''''''''''''''''''''''''''''''''''''

``Receive a V2N Event Notification relevant to OVC-g`` 순서는 OVSE에서 타 OVC로부터 수신 받은 V2N 이벤트 중에서 
해당 OVC-g와 연계된 이벤트인 경우에 해당 이벤트 메세지를 푸시 형태로 수신하는 순서입니다. 

기존 2번 순서에서 Subscribe한 Topic으로 해당 메세지를 수신하게 되며, 해당 단계를 구현하기 위한 샘플 코드는 아래와 같습니다.

``Example Code``

.. code-block:: javascript

    messageSender.on('message', function(topic, message) {
        var msgs = message.toString();
        var topic = topic.toString();
        var requestId = topic.toString().split('/')[5];

        // 수신한 V2X 메세지 로그 출력
        if (msgs != null){      
        console.log(colors.magenta(' == Receive the V2N event Message from OVSE == ') + '\n');
        console.log(colors.magenta('Topic :' + topic + '\n' 
        + 'Message : ' + JSON.stringify(JSON.parse(msgs), 0, 2) + '\n'));

        // 수신한 메세지 처리 결과를 OVSE에 보고하는 함수 호출 
        // 상기 함수는 다음 단계와 연계됨
        responseOVSEventMsg(requestId);
        }
    });

그리고 이때 수신되는 Event 메세지의 종류는 :ref:`8. 메세지 포맷 <message-format>`의 OVSE V2N Message를 참고하시기 바랍니다.

Publish the result of the notifcation message handling
''''''''''''''''''''''''''''''''''''''''''''''''''''''

``Publish the result of the notifcation message handling`` 순서는 OVC에서 5번째 순서에서 수신한 이벤트를 
처리한 결과를 OVSE로 송신하는 순서입니다. 본 순서는 향후 OVSE를 활용하는 고객사들께서 V2N 서비스 통계 자료 제공에 중요한 과정입니다.

OVSE에서 발송한 메세지의 처리 결과를 일정 시간(To-be-specified) 내 수신하지 못하면 정상 처리가 안된 것으로 간주합니다. 

처리 결과 코드 (To-be-specified)

``Example Code``

.. code-block:: javascript

  function responseOVSEventMsg(arg){

      var sendingMessageObj = {
        "results" : 2000
      };

      var sendingMessageJSON = JSON.stringify(sendingMessageObj, 0, 2);

      messageSender.publish(utils.eventAckTopic, sendingMessageJSON, {qos: config.qos}, function() {      
        console.log(colors.magenta(' == Successfully sending a ACK message to OVSE == ') + '\n');
        console.log(colors.cyan('Message : ' + sendingMessageJSON) + '\n');
      });     
  }



VSC Interface for ``OVC-m``
------------------------------------------------

OVC-m 단말을 위한 VSC Interface의 Flow는 다음과 같으며, 각 단계별로 절차를 ``Node.js`` 기반 코드로 소개합니다.

.. image:: /images/device_procedure/ovcm_procedure_v1.png
	:width: 100%
	:align: center


OVC-m 과 OVC-g의 가장 큰 차이는 T맵의 사용 유무입니다. 

OVC-m은 T맵을 가지고 있으므로, 현재 자신의 위치 정보를 자체적으로 판단하여 OVSE와 통신할 수 있습니다. 

그래서 OVC-g와 다른 Flow를 보이며, 이에 따라 주고받는 데이터도 달라집니다.

도식화된 Flow는 크게 4가지 단계 ``Stage`` 로 구성됩니다. 가장 중요한 부분은 ``Location Topic Update`` 부분입니다.

================================  ===================================================================
Stages                            Description              
================================  ===================================================================
Preparation                       | OVC-m과 OVSE 상호 간 서비스를 호출하기 위해서 필요한 연결, 인증의 기본 준비 단계
                                  |
Location Topic Update             | OVC-m이 탑재한 T맵을 기반으로 현재 위치의 도로정보를 파악하고, 
                                  | 이를 기반으로 V2N 이벤트 송/수신을 위한 Topic을 만드는 단계
                                  |
V2N Event Report                  | OVC-m에서 감지한 V2N Event를 OVSE에 보고하는 단계
                                  |
V2N Event Notification Reception  | OVSE가 타 OVC로부터 전달받은 V2N Event 중 해당 OVC-m와 연관된 Event를 
                                  | 푸시하여 OVC-m이 수신하는 단계
================================  ===================================================================

아래부터는 상기 vsc-m Flow의 순서를 간단한 예제 코드와 함께 설명합니다.

Preparation 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Connect to OVSE
''''''''''''''''''

``Connect to OVSE`` 는 OVC-m이 OVSE에 연결하는 단계로 OVC-g와 동일합니다. MQTT Broker에 접속하는 connect 단계 
`MQTT Connect 참고 <https://www.hivemq.com/blog/mqtt-essentials-part-3-client-broker-connection-establishment/>`__ 와 동일합니다.
단, 접속할 때는 다음 Parameter를 적용하여 connect 합니다.

=============  ===========================================================
Parameters     Value
=============  ===========================================================
host           tcp://192.168.1.170 (*Will be changed*) 
port           1883 (*Will be changed*) 
username       발급된 고객사의 userName (할당 받은 Access Token (20자리) 값)
password       발급된 고객사의 passWord
clientId       단말 식별 번호 (기능상 UserName과 동일하게 처리 가능)
cleanSession   true
keepAlive      60
=============  ===========================================================

.. rst-class:: text-align-justify

Username 필드에는 해당 단말의 Credentials ID 값을 입력합니다. 단말의 Credentials ID 값은 `OVSE REST API /api/ovs/v1/device`__ 를 통해서 얻을 수 있습니다. cleanSession 필드가 true면 이전 세션 정보가 아직 존재할 경우 클라이언트와 서버에서 이전 세션 정보를 삭제합니다.


``Example Code`` 

.. code-block:: javascript

    var mqtt = require('mqtt');

    //OVSE 접속 및 설정 
    var messageSender = mqtt.connect({ 
        host: 192.168.1.170, 
        port: 1883, 
        username: {고객사에서 등록한 username},
        password: {고객사에서 등록한 password},
        clean: true,
        keepalive: 60,
        protocol: 'mqtt'
    });

    //OVSE 접속 시도에 따른 Callback
    messageSender.on('connect', function(connack) {

        if (connack.cmd == 'connack'){
            // 성공적인 OVSE 접속
        } else
            // 접속 실패, 및 원인 파악 필요
    });


Location Topic Update 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
OVC-m 단말이 OVSE와 연동하여 V2N 서비스를 하기 위해서는 OVSE에서 Support하는 Topic의 구독이 필요합니다.

Topic을 Generation하는 과정은 아래 그림과 같습니다. 

.. image:: /images/device_procedure/ovcm_topicgen_v3.png
	:width: 100%
	:align: center

GPS Acquisition
''''''''''''''''''
가장 먼저 OVC-m 단말이 해야하는 것은 내부의 GPS 센서로부터 현재 자신의 위치 좌표를 받는 것입니다. 
최소 1초 간격으로 GPS 값을 읽어 위치를 파악하는 것을 추천합니다. 이보다 더 느려지면 V2N 서비스 품질의 저하가 있을 수 있습니다.

Map Matching
''''''''''''''''''
GPS 좌표를 정상적으로 수신 한 경우, 3개 이상의 연속된 값이 누적 되면 이를 기반으로 T맵 내부 맵매칭을 진행합니다. 
좌표 한개로도 값이 리턴은 됩니다. 단, 3개 이상의 연속된 좌표가 있어야 도로 방향성 등을 고려한 더 정확한 맵매칭이 이루어질 수 있습니다.

Topic Generation
''''''''''''''''''
맵매칭 결과로 T맵 내부에서 관리하는 Road Link 관리 정보를 받아, 이를 이용해서 Topic을 만듭니다.
Topic을 만들기 위해 필요한 parameter는 meshId, linkId, linkDirection의 3가지 이며 그 Rule은 다음과 같습니다.


``Example Code`` 

.. code-block:: javascript

    private String genUniqueTopic(short meshId, int linkId, short linkDirection) {
        int meshLink = (meshId << 16) | linkId;
        return "NEW:" + Integer.toString(meshLink) + Short.toString(linkDirection);
    }

위 샘플 코드의 결과로 나오는 값이 OVC-m 단말에서 구독해야하는 Topic이 됩니다. 

``Example Data``

.. code-block:: json

    {
        "meshid": 57150000,
        "linkid": 4333,
        "location": {
            "lat": 37.37913974,
            "lon": 127.12722608
        }
    }

위 데이터의 경우 Topic ``NEW:3745425731``을 만들어냅니다. (T맵 맵정보 업데이트에 따라 값은 변화될 수 있습니다.)

Topic Subscription & Un-Subscription
''''''''''''''''''''''''''''''''''''''''''''''''''''''
마지막으로 이 만들어진 토픽이 기존과 같은 것인지 아닌지 비교하여 (i.e. 도로에 변화가 있는 것인지 아닌지 판단하여)
변경이 있다면 기본 Topic을 Un-Subscribe하고, 새로울 Topic에 Subscribe 합니다. 
변경이 없다면 기존 Topic Subscription을 유지합니다.

이를 샘플코드로 설명하면 다음과 같습니다.

``Example Code``

.. code-block:: javascript

   public void updateTopic(short meshId, int linkId, short linkDirection, RoadType roadType){

        if(this.meshId!=meshId || this.linkId!=linkId || this.linkDirection!=linkDirection){

            this.meshId = meshId;
            this.linkId = linkId;
            this.linkDirection = linkDirection;

            if(roadType == RoadType.HIGHWAY || roadType == RoadType.URBAN_HIGHWAY){
                if(this.currentRoadId != null) {
                    unsubscribeTopic(currentTopic);
                }
            }

            this.currentTopic = this.genUniqueTopic(meshId, linkId, linkDirection);

            if(roadType == RoadType.HIGHWAY || roadType == RoadType.URBAN_HIGHWAY){
                this.subscribeTopic(this.currentTopic);
            }
        } 
    }

위와 같이 주기적으로 OVC-m 단말 장착차량의 도로상 이동 정보를 체크하여, Topic을 만들고, 구독을 하면, V2N 서비스를 위한 준비를 마친 것이 됩니다. 

추가로 본 과정은 일반적인 V2N 서비스 품질을 고려하여서는 최소 1초마다 진행되는 것이 적절합니다.
고객사의 입장에 따라 주기가 증가할 수 있으나, 주기가 증가할 수록 일부 V2N 서비스 품질이 떨어지게 됩니다.


Publish V2N Event detected by OVC-m
''''''''''''''''''''''''''''''''''''''''''''''''''''''
``Publish V2N Event detected by OVC-m`` 순서에서는 OVC-m가 VAC로부터 
해당 단말이 인식한 V2N Event를 수신 받은 경우, 이를 OVSE에 리포팅하여 OVSE가 다른 OVC 에게 전달하는 과정을 유도하는 과정을 기술합니다.

Topic은 아래와 같은 룰을 따라 설정합니다.

=============  =============================================
Topic          v2x/event
=============  =============================================

본 순서에서 메세지를 전달할때는 다음 메세지를 ``JSON`` 형태로 포함합니다.
(이는 :ref:`8. 메세지 포맷 <message-format>`의 VDEventReport에 해당합니다.)

================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
time              M     Integer   메세지 전달 시간 (msec, epoch)
eventType         M     Integer   이벤트 종류 (To-be-specified)
distanceToEvent   O     Integer   이벤트 지점까지의 거리
location          M               | 이벤트 발생 위치 정보 (WGS84 Coordination)
                                  | Child key로 "lat", "lon" 를 적시
================  ====  ========  =============================================

``Example Data``

.. code-block:: json

    {
        "time": 1571308818766,
        "eventType": 1,
        "distanceToEvent": -10,
        "location": {
            "lat": 37.51477,
            "lon": 127.060067
        }
    }

``Example Code``

.. code-block:: javascript

  var v2xEventReportData = {
    "time": new Date().getTime(),
    "eventType": 1,
    "distanceToEvent": -10,
    "location": {
      "lat": latitudeValue[sequence % latitudeValue.length],
      "lon": longitudeValue[sequence % latitudeValue.length]
    } 
  };

  sendingMSG = JSON.stringify(eval(v2xEventReportData));
  messageSender.publish(utils.eventTopic, sendingMSG, {qos: config.qos}, function(){
    console.log(colors.cyan('Message [JSON | ' + Buffer.from(JSON.stringify(eval(v2xEventReportData))).length + ' Bytes] : ' 
    + JSON.stringify(eval(v2xEventReportData), 0, 2) + '\n'));
  });



V2N Event Notification Reception 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Receive a V2N Event Notification relevant to OVC-m
''''''''''''''''''''''''''''''''''''''''''''''''''''''

``Receive a V2N Event Notification relevant to OVC-m`` 순서는 OVSE에서 타 OVC로부터 수신 받은 V2N 이벤트 중에서 
해당 OVC-m와 연계된 이벤트인 경우에 해당 이벤트 메세지를 푸시 형태로 수신하는 순서입니다. 

기존 2번 순서에서 Subscribe한 Topic으로 해당 메세지를 수신하게 되며, 해당 단계를 구현하기 위한 샘플 코드는 아래와 같습니다.

``Example Code``

.. code-block:: javascript

    messageSender.on('message', function(topic, message) {
        var msgs = message.toString();
        var topic = topic.toString();
        var requestId = topic.toString().split('/')[5];

        // 수신한 V2N 메세지 로그 출력
        if (msgs != null){      
        console.log(colors.magenta(' == Receive the V2N event Message from OVSE == ') + '\n');
        console.log(colors.magenta('Topic :' + topic + '\n' 
        + 'Message : ' + JSON.stringify(JSON.parse(msgs), 0, 2) + '\n'));

        // 수신한 메세지 처리 결과를 OVSE에 보고하는 함수 호출 
        // 상기 함수는 다음 단계와 연계됨
        responseOVSEventMsg(requestId);
        }
    });

그리고 이때 수신되는 Event 메세지의 종류는 :ref:`8. 메세지 포맷 <message-format>`의 OVSE V2N Message를 참고하시기 바랍니다.


Publish the result of the notifcation message handling
''''''''''''''''''''''''''''''''''''''''''''''''''''''

``Publish the result of the notifcation message handling`` 순서는 OVC에서 4번째 순서에서 수신한 이벤트 처리 결과를 
OVSE로 송신하는 순서입니다. 본 순서는 향후 OVSE를 활용하는 고객사들께서 V2N 서비스 통계 자료 제공에 중요한 과정입니다.

OVSE에서 발송한 메세지의 처리 결과를 일정 시간(To-be-specified) 내 수신하지 못하면 정상 처리가 안된 것으로 간주합니다. 

처리 결과 코드 (To-be-specified)

``Example Code``

.. code-block:: javascript

  function responseOVSEventMsg(arg){

      var sendingMessageObj = {
        "results" : 2000
      };

      var sendingMessageJSON = JSON.stringify(sendingMessageObj, 0, 2);

      messageSender.publish(utils.eventAckTopic, sendingMessageJSON, {qos: config.qos}, function() {      
        console.log(colors.magenta(' == Successfully sending a ACK message to OVSE == ') + '\n');
        console.log(colors.cyan('Message : ' + sendingMessageJSON) + '\n');
      });     
  }

