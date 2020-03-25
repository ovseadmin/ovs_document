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
        "ty" : 2, // Message Type

        // Payload
        "pld" : {
            "dev_type": 97,
            "dev_id": 3333,
            "speed": 60,
            "location": {
                "lat": 37.510296,
                "lon": 127.062512
            }
        }
    }


메세지의 종류는 크게 다음의 두 가지로 나뉩니다.

* OVC가 OVSE로 보내는 메세지
* OVC가 OVSE로 부터 수신하는 메세지 

두 카테고리 내에서도 메세지의 타입인 ``ty``에 따라 다양한 메세지의 종류를 구분 인식하게 됩니다.



OVC >> OVSE Message
``````````````````````

OVC가 OVSE로 보내는 메세지는 주기보고 타입과 비주기 보고 타입이 있습니다.

=========  ==================================
ty         설명
=========  ==================================
1          주기보고 OVCPosition 메세지
2          비주기보고 OVCEventReport 메세지
=========  ==================================


주기보고 메세지 타입
'''''''''''''''''''''''''''
주기보고 메세지는 OVSE 규격을 따르는 OVC가 주기적으로 차량의 운행 정보를 전달할 때 명세하는 메세지 타입입니다. 

아래 표와 같이 pld 내부에 OVC device type 타입별로 구분되어 있습니다. (고객사의 요청에 따라 추가 및 수정이 가능합니다.)

=========  ==================================
dev_type   설명
=========  ==================================
1          ADAS 단말의 OVCPosition 메세지
2          BlackBox 단말의 OVCPosition 메세지
3          IVI 단말의 OVCPosition 메세지
=========  ==================================

비주기보고 메세지 타입
'''''''''''''''''''''''''''
비주기보고 메세지는 OVSE 규격을 따르는 OVC가 내부의 Event Detection Algorithm에 따라 발생된 비주기 Event를 OVSE에 전송하는 메세지 입니다.

비주기 보고 메세지는 SKT가 Guide하는 Device Certification Process를 만족한 경우에 추가 등록 및 사용이 가능합니다.

(*Certified Program 추가 필요)

이는 아래 표와 같이 이벤트 타입별로 구분되어 있습니다. (*초안이며 추가 및 수정 필요*)

============  ==================================
event_type    설명
============  ==================================
101           급정거 발생 이벤트 메세지       
102           차량사고 발생 이벤트 메세지
103           졸음운전 발생 이벤트 메세지
============  ==================================


OVCPosition
```````````````````
OVC의 실시간 위치 정보를 전송하기 위한 데이터 포맷입니다.

=============  ====  ========  =============================================
Key            M/O   Type      Description
=============  ====  ========  =============================================
ts             M     Integer   메세지 전달 시간 (msec, epoch)
ty             M     Integer   메세지 타입 구분 
dev_type       M     Integer   OVC를 탑재한 단말의 타입
dev_id         M     String    OVSE에 등록된 단말 식별자
speed          O     Integer   현재 속도 값
location       M               | 현재 위치 좌표 (WGS84 Coordination)
                               | Child key로 "lat", "lon" 를 적시
=============  ====  ========  =============================================

``Example Data``

.. code-block:: json

    {
        // Header
        "ts" : 1571273913571, // timestamp
        "ty" : 1, // 주기보고 Message Type

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



OVCEventReport
```````````````````
OVC의 내부 알고리즘에 의해서 Detect된 도로상의 이벤트를 전송하는 데이터 포맷입니다.

================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
ts                M     Integer   메세지 전달 시간 (msec, epoch)
ty                M     Integer   메세지 타입 구분 (2)
dev_type          M     Integer   OVC를 탑재한 단말의 타입
dev_id            M     String    OVSE에 등록된 단말 식별자
event_type        M     Integer   발생 이벤트 식별자
distanceToEvent   O     Integer   이벤트 지점까지의 거리
location          M               | 이벤트 발생 위치 정보 (WGS84 Coordination)
                                  | Child key로 "lat", "lon" 를 적시
================  ====  ========  =============================================


``Example Data``

.. code-block:: json

    {
        // Header
        "ts" : 1571308818766, // timestamp
        "ty" : 2, // 비주기 이벤트 메세지 타입 

        "pld" : {
            "dev_type": 2,
            "dev_id": 3333,
            "event_type": 101, // 급정거 이벤트 발생 예
            "distanceToEvent": -10,
            "location": {
                "lat": 37.510296,
                "lon": 127.062512
            }
        }
    }


OVSE >> OVC Message
``````````````````````


위험 상황 경고
'''''''''''''''''''''''''''
================  ====  ========  =============================================
Key               M/O   Type      Description
================  ====  ========  =============================================
time              M     Integer   메세지 전달 시간 (msec, epoch)
eventType         M     Integer   이벤트 종류 (To-be-specified)
distanceToEvent   O     Integer   이벤트 지점까지의 거리
location          M               | 이벤트 발생 위치 정보 (WGS84 Coordination)
                                  | Child key로 "lat", "lon" 를 적시
================  ====  ========  =============================================


위험 상황 제보
'''''''''''''''''''''''''''

갓길 정지 차량 경고
'''''''''''''''''''''''''''

터널 사고 정보 제공
'''''''''''''''''''''''''''


긴급차량 접근 알림
```````````````````

급정거 알림
```````````````````