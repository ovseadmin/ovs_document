.. |br| raw:: html

.. _message-format:

메세지 포맷
==============================

.. rst-class:: text-align-justify

OVSE 플랫폼에 연동되는 다양한 Open V2N Service Device (이하 OVC) 들이 플랫폼에 전송하는 메세지에 대해서 정의합니다.

이 매뉴얼은 단말이 MQTTS 프로토콜로 OVSE 플랫폼과 연동하기 위한 메세지 포맷입니다. 

Entity 등록을 위한 HTTP Rest API 사용은 :ref:`5. 구성요소(Entity) 등록 <entity-registration>` 문서를, App 개발자를 위한 OVSE API 는 :ref:`7. API 규격<api-specification>` 문서를, 단말의 연동 절차를 위해서는 :ref:`6. Device 연동 절차 <device-registration>` 문서를 참고하시기 바랍니다.


.. note::

   본 장에 명세된 표 내용 중 ``M`` / ``O`` 는 ``Mandatory`` / ``Optional`` 의 약자로, Mandatory는 필수로 포함해야 하는 데이터를 Optional은 필요에 따라 기입이 여부를 개발사에서 판단하시면 됩니다.



메세지 기본 구조
-----------------------------

OVSE 플랫폼의 기본 메세지 구조는 ``Header`` 와 ``Payload`` 형태로 구조화 되어 있습니다. 각 메세지는 해당 메세지의 타입인 ``ty`` 로 구분하고 ``ty`` 에 따라 ``pld`` child의 내용이 상이합니다.

.. role:: underline
        :class: underline

:underline:`Example Code` :

.. code-block:: json

    {
        // Header
        "ts" : 1571273913571, // timestamp
        "ty" : 11, // Message Type

        // Payload
        "pld" : {
            "dev_type": 2,
            "dev_id": 3333,
            "speed": 60,
            "location": {
                "lat": 37.510296,
                "lon": 127.062512
            }
        }
    }


메세지의 종류는 크게 다음의 두 가지로 나뉩니다.

* OVC가 OVSE로 보내는 메세지 (ty: 1*)
* OVC가 OVSE로 부터 수신하는 메세지 (ty: 2*)

그리고 이를 방향성 및 주기/비주기 성으로 ``ty``을 세분화 하면 다음과 같습니다.

=========  ==================================
ty         설명
=========  ==================================
11         주기보고 OVCPosition 메세지
12         비주기보고 OVCEventReport 메세지
22         비주기 V2N 알림 메세지 
=========  ==================================


OVC >> OVSE Message
-----------------------------

OVC가 OVSE로 보내는 메세지는 주기보고 타입과 비주기 보고 타입이 있습니다.

주기보고 메세지 타입 (OVCPosition)
``````````````````````````````````
주기보고 메세지는 OVSE 규격을 따르는 OVC가 주기적으로 차량의 운행 정보를 전달할 때 명세하는 메세지 타입입니다. 

=============  ====  ========  =============================================
Key            M/O   Type      Description
=============  ====  ========  =============================================
ts             M     Integer   | 메세지 전달 시간 (msec, epoch)
                               | * YYYYMMDDHH24MISS
ty             M     Integer   메세지 타입 구분 
dev_type       M     Integer   OVC를 탑재한 단말의 타입
dev_id         M     String    OVSE에 등록된 단말 식별자
speed          O     Integer   현재 속도 값
location       M               | 현재 위치 좌표 (WGS84 Coordination)
                               | Child key로 "lat", "lon" 를 적시
=============  ====  ========  =============================================

아래 표와 같이 pld 내부에 OVC device type 타입별로 구분되어 있습니다. (고객사의 요청에 따라 추가 및 수정이 가능합니다.)

=========  ==================================
dev_type   설명
=========  ==================================
1          ADAS 단말의 OVCPosition 메세지
2          BlackBox 단말의 OVCPosition 메세지
3          IVI 단말의 OVCPosition 메세지
=========  ==================================


``Example Data``

.. code-block:: json

    {
        // Header
        "ts" : 1571273913571, // timestamp
        "ty" : 11, // 주기보고 Message Type

        "pld" : {
            "dev_type": 2,
            "dev_id": 3333,
            "speed": 60,
            "location": {
                "lat": 37.510296,
                "lon": 127.062512
            }
        }
    }



비주기보고 메세지 타입 (OVCEventReport)
``````````````````````````````````````````
비주기보고 메세지는 OVSE 규격을 따르는 OVC가 내부의 Event Detection Algorithm에 따라 발생된 비주기 Event를 OVSE에 전송하는 메세지 입니다.

비주기 보고 메세지는 SKT가 Guide하는 Device Certification Process를 만족한 경우에 추가 등록 및 사용이 가능합니다.

(*Certified Program 추가 필요)

================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
ts                M     Integer   | 메세지 전달 시간 (msec, epoch)
                                  | * YYYYMMDDHH24MISS
ty                M     Integer   메세지 타입 구분 
dev_type          M     Integer   OVC를 탑재한 단말의 타입
dev_id            M     String    OVSE에 등록된 단말 식별자
event_type        M     Integer   Event 종류 식별자
event_id          M     String    Unique event 식별자
distanceToEvent   O     Integer   | 이벤트 지점까지의 거리
                                  | + : 전방
                                  | - : 후방
location          M               | 이벤트 발생 위치 정보 (WGS84 Coordination)
                                  | Child key로 "lat", "lon" 를 적시
================  ====  ========  =============================================

비주기 이벤트는 그 종류를 pld 내부의 event_type으로 구분하고 있습니다. (*초안이며 추가 및 수정 필요*)

============  ==================================
event_type    설명
============  ==================================
201           급정거 발생 이벤트 메세지       
202           차량사고 발생 이벤트 메세지
203           졸음운전 발생 이벤트 메세지
============  ==================================


``Example Data``

.. code-block:: json

    {
        // Header
        "ts" : 1571308818766, // timestamp
        "ty" : 12, // 비주기 이벤트 메세지 타입 

        "pld" : {
            "dev_type": 2,
            "dev_id": 3333,
            "event_type": 201, 
            "event_id": 1021,
            "distanceToEvent": 679,
            "location": {
                "lat": 37.510296,
                "lon": 127.062512
            }
        }
    }



OVSE >> OVC Message
-----------------------------
OVSE에서 OVC로 다양한 V2N 이벤트 알림 메세지가 전달됩니다. 
티맵, 소방방재청, 지자체 (도로공사 등), 그리고 다른 OVC 등을 통해서 수집된 이벤트에 대한 알림 메세지이며 그 종류 및 내용은 다음과 같습니다.


================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
ts                M     Integer   | 메세지 전달 시간 (msec, epoch)
                                  | * YYYYMMDDHH24MISS
ty                M     Integer   Event 메세지 타입 구분
event_id          M     String    Unique event 식별자
event_type        M     Integer   알림 메세지 타입
tunnel            M     Boolean   Tunnel 안의 이벤트인지 아닌지 (급정거는 모두 FALSE)
distanceToEvent   O     Integer   | 이벤트 지점까지의 거리
                                  | + : 전방
                                  | - : 후방
location          M               | 이벤트 발생 위치 정보 (WGS84 Coordination)
                                  | Child key로 "lat", "lon" 를 적시
================  ====  ========  =============================================


``Example Data``

.. code-block:: json

    {
        // Header
        "ts" : 1571308818766, // timestamp
        "ty" : 22, // V2N 알림 메세지 이벤트 타입 (비주기, 하방)

        "pld" : {
            "event_id": 12123, 
            "event_type: 1286, // 보행자 이벤트 발생 예
            "tunnel": TRUE, 
            "distanceToEvent": 1400,
            "location": {
                "lat": 37.510296,
                "lon": 127.062512
            }
        }
    }


각 이벤트 타입별 세부 detail 정보는 다음과 같습니다.

============  ==================================
event_type    발생 이벤트 메세지 설명
============  ==================================
0             전방 급정거 발생      
258           전방 차량 정체 
513           전방 사고 발생
534           전방 정지차 주의
1281          전방 낙하물 주의
1286          전방 보행자 주의
1793          전방 차량 역주행 주의
9732          후방 경찰차 접근
9734          후방 구급차 접근
9736          후방 소방차 접근
============  ==================================

