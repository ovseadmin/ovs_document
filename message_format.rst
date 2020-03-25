.. |br| raw:: html

.. _message-format:

메세지 포맷
==============================

.. rst-class:: text-align-justify

OVSE 플랫폼에 연동되는 다양한 Open V2N Service Device (이하 OVC) 들이 플랫폼에 전송하는 메세지에 대해서 정의합니다.

이 매뉴얼은 단말이 MQTTS 프로토콜로 OVSE 플랫폼과 연동하기 위한 메세지 포맷입니다. 

Entity 등록을 위한 HTTP Rest API 사용은 :ref:`5. 구성요소(Entity) 등록 <entity-registration>` 문서를, 

App 개발자를 위한 OVSE API 는 :ref:`7. API 규격<api-specification>` 문서를,

단말의 연동 절차를 위해서는 :ref:`6. Device 연동 절차 <device-registration>` 문서를 참고하시기 바랍니다.

(*Message Format을 header, body로 변경할지 검토 필요*)

메세지의 종류는 크게 다음의 두 가지로 나뉩니다.

* OVC가 OVSE로 보내는 메세지
* OVC가 OVSE로 부터 수신하는 메세지 


OVC -> OVSE Message Format
-----------------------------

OVC가 OVSE로 보내는 메세지는 OVCPosition과 OVCEventReport가 있습니다.

OVCPosition
```````````````````
OVC의 실시간 위치 정보를 전송하기 위한 데이터 포맷입니다.

=============  ====  ========  =============================================
Key            M/O   Type      Description
=============  ====  ========  =============================================
dev_type       M     Integer   OVC를 탑재한 단말의 타입
time           M     Integer   메시지 전달 시간 (msec, epoch)
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


OVCEventReport
```````````````````
OVC의 내부 알고리즘에 의해서 Detect된 도로상의 이벤트를 전송하는 데이터 포맷입니다.

(단, 이벤트 발생의 적절성 여부는 SKT 담당자 및 Certified Program을 통한 검토가 선행되어야 합니다.)

(*Certified Program 추가 필요)

================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
time              M     Integer   메시지 전달 시간 (msec, epoch)
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


OVSE -> OVC Message Format
-----------------------------


위험 상황 경고
``````````````````````````````

위험 상황 제보
``````````````````````````````

갓길 정지 차량 경고
``````````````````````````````

터널 사고 정보 제공
``````````````````````````````


긴급차량 접근 알림
```````````````````

급정거 알림
```````````````````