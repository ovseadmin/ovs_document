OVSE의 주요 특징 및 기능
=======================================

.. rst-class:: text-align-justify

OVSE는 SKT에서 제공하는 OPEN API 플랫폼으로 V2N 솔루션을 개발 및 적용하고자 하는 파트너사들에게 보다 쉽고, 빠르게, 유용한 서비스를 안전하게 개발할 수 있는 기능들을 제공합니다.

아래 그림과 같이 코어 엔진 부분에 V2N Service Enabler, Map Matcher, Road Translator, Route Planning, Message Controller 등의 다양한 핵심기능이 구현되어 있습니다.

.. image:: /images/ovse_intro/ovse_arch.png

OVSE의 주요 특징 및 핵심 기능은 다음과 같습니다.


.. rst-class:: text-align-justify

주요 특징
-----------------------------------------

* 간편한 API 지원
* 유연한 연동 인터페이스 및 SDK 제공
* T맵 Compatible V2N Service 제공
* 원스탑 서비스 지원 


간편한 API 지원
```````````````````

.. rst-class:: text-align-justify

OVSE 플랫폼과 V2N Application Device/Server간 연동을 쉽게 개발할 수 있도록 간결하고, 다양한 API들을 제공하고 있습니다.

* Simple API

OVSE 연동 Device (예 : ADAS, IVI, Blackbox 등) 및 Server (예 : GPT 센터 등)에게는 최소한의 API만을 제공하여 개발의 복잡도를 낮춰 드립니다.  

이를 위해서 기존에 많이 사용되는 HTTP 프로토콜과 비교하여 저전력, 보다 빠른 데이터 처리, 가벼운 통신 규격을 지원하는 MQTT 프로토콜을 지원하고 있습니다.  


* Useful API

회사 등록/관리, Device 등록/관리, V2N Message 전송, 서비스 통계 관리 등과 같은 다양한 서비스를 간편하게 구현할 수 있습니다. 

Device 연동 API를 통해서 정보를 올려주면, OVSE 플랫폼에서 복잡한 분석 절차를 처리하고 RESTful API를 통해 고객들에게 원하는 정보를 제공해드립니다.   


유연한 연동 인터페이스 및 SDK 제공
``````````````````````````````

SKT에서는 V2N 서비스 개발 파트너사들을 다음의 두 가지 경우로 고려하고 있습니다.


V2N Partner with Device 
'''''''''''''''''''''''''''

.. rst-class:: text-align-justify

단말의 직접 연결을 희망하는 파트너사 (예: ADAS, IVI, 블랙박스 등의 제조업체)들을 의미합니다.

V2N 솔루션을 효과적으로 구현하기 위한 Device 연동 규격을 제공합니다. 그리고 서비스 품질관리와 보안을 보장하기 위하여 SKT에서는 Certification Program(*Cert Program은 추후 update 예정입니다*)을 운영합니다.


* 다양한 Device 연동 규격 

ADAS, Blackbox, IVI, 스마트폰 등을 통해서 V2N Application을 제공할 유연한 인터페이스를 제공합니다. 

OVSE의 메세지 전송 및 데이터 분석 기능을 활용하기 위해서는 SKT에서 제공하는 메시지 포맷 규약을 따라야 합니다. 


* 다양한 SDK 제공 

OVSE 플랫폼과 Device간의 연동 기능 개발을 보다 쉽게 하실 수 있도록 다양한 언어의 SDK를 제공합니다.  

또한 Starter Kit(링크 - 김경훈님 작업필요) 솔루션을 활용하여 쉽게 단말 연동 기능 시험 및 프로토타입 솔루션을 기획하실 수 있습니다.


V2N Partner with Server 
'''''''''''''''''''''''''''

디바이스를 OVSE에 직접 연결하지 않고, 보유하고 있는 Server를 이용하여 V2N 서비스를 이용하고자 하는 파트너들 (예: 자체 Backend를 보유한 OEM 등) 을 위한 인터페이스 역시 제공합니다. 

SKT의 지능형 도로 교통 정보 제공 플랫폼 (Intelligent Road Data Service 플랫폼, a.k.a. RUUT)를 통해서 V2N 서비스 연동 개발을 진행하실 수 있습니다.

RUUT는 고객 편의성, 데이터 및 인터페이스 호환성, 제공 정보의 밀도 향상을 목적으로 개발 되었으며, SKT의 고정밀 실시간 도로 교통 정보를 접근할 수 있는 상위 레벨 인터페이스 입니다.

자세한 절차들은 `RUUT 개발자 사이트 <https://ruut.readthedocs.io/>`__ 에 접속하거나 플랫폼에서 제공하는 `Open API (링크 추가 오픈 예정)>`__ 에서 확인하실 수 있습니다.



T맵 Compatible V2N Service 제공
```````````````````````````````````

.. rst-class:: text-align-justify

월 1000만명 이상이 사용하는, 국내 1위 모바일 네비게이션인 T맵과 동일한 V2N 서비스를 제공합니다. 급정거, 응급차량 출동 알림, 도로공사 C-ITS 실시간 정보 등 과 같은 서비스를 동일하게 구현하실 수 있습니다. 


원스톱 서비스 지원
```````````````````````````````````

.. rst-class:: text-align-justify

SKT는 V2N 솔루션 및 서비스를 기획하는 파트너들의 사업 성공을 위해서 기획부터 기술개발, 서비스 런칭까지 원스톱 서비스를 지원합니다.

또한 사업 런칭 후에도 파트너들의 다양한 요구사항에 대한 전문적인 대응을 통하여 안정적으로 사업이 유지되도록 지원합니다.


주요 기능
-----------------------------------------

OVSE 플랫폼은 파트너업체들이 시장에서 경쟁력 있는 V2N Application 쉽게 개발할 수 있도록 내부에 다음과 같은 다양한 기능이 구현되어 있습니다.

* V2N Service Enabler
* Map Matcher
* Road Translator
* Route Planning
* Message Controller
* AAA

V2N Service Enabler
````````````````````````````````
V2N Service Enabler (이하 VSE)는 V2N 서비스를 하기 위한 도로 내의 다양한 상황을 분석하고, 각 서비스의 조건에 맞게 V2N 대상 차량을 실시간, 저지연으로 분석 하는 역할을 합니다.

차량의 선후 관계, 동일차선/반대차선, 각각의 거리는 어떻게 되는지, 공공으로 부터 들어오는 정보는 어떻게 되는 지 등을 종합적으로 분석하여 알람이 필요한 차량에게만 선별적으로 메세지를 전달합니다. 

그리고 이런 기능은 하기의 Map Matcher, Road Translator, Route Planning, Message Controller 등과의 연계를 통해 이루어집니다.

.. image:: /images/ovse_intro/vse.png


Map Matcher
````````````````````````````````
Map Matcher는 Device에서 올라온 GPS 좌표를 SKT가 관리하는 T맵 도로 정보로 변환하는 역할을 합니다. 이 기능을 통해 Map을 가지고 있지 않은 Device들도 위치한 도로를 구분하고 V2N 서비스를 받으실 수 있습니다.

.. image:: /images/ovse_intro/mapmatching.png


Road Translator
````````````````````````````````
Road Translator는 T맵 내의 각 도로 링크의 연결 구조, 링크 정보, 링크 체계 간 변환 등을 하여, 도로 선후 연결 관계, 도로의 종류, 속성 등을 파악하는 역할을 합니다. 

도로간의 관계와 속성을 종합적으로 고려하여 VSE에서 도로 상황 분석 및 메세지 전송을 합니다.


Route Planning
````````````````````````````````
Route Planning (이하 RP) Origin-Destination(O-D)에 따른 T맵의 경로를 검색하고, 경로상에 해당하는 도로링크 정보를 전달하는 역할을 합니다. 

V2N 서비스 시나리오에 따라 RP를 활용하여, 메세지 전달 대상을 파악합니다. (예: 응급차량 출동 알람)

.. image:: /images/ovse_intro/routeplanning.png


Message Controller
````````````````````````````````
Message Controller는 단말들과 통신하여 데이터를 송/수신 하는 역할을 합니다.

외부 단말 (V2N Partners' Devices)들의 위치/이벤트 데이터를 수집하고 분석하여, 관련된 V2N 단말 그룹에 이벤트 메시지를 실시간으로 전달합니다.


AAA
````````````````````````````````````````````````````````````````
OVSE는 AAA, 다시 말해 Authentication, Authorization, Accounting을 위한 기능들을 지원합니다. 

REST API를 기반으로 단말을 등록, 인증하고 관리하며, 등록된 정상 단말에 한해서만 V2N 서비스를 제공하고 있습니다.

자세한 프로세스는 서비스 등록 절차를 참고하시기 바랍니다.