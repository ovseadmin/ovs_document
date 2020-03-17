Open V2N Service Enabler 소개
=======================================

.. rst-class:: text-align-justify

About V2X Service 
-----------------------------------------
V2X는 Vehicle to Everything의 약자로 운전 중, 무선망을 이용하여 다른 차량 및 도로 등 인프라가 구축된 사물과 교통정보와 같은 정보를 교환하는 통신기술입니다. 

차량 간 통신(V2V, Vehicle to Vehicle), 차량과 도로 인프라 간 통신(V2I, Vehicle to Infra), 차량과 보행자 간 통신(V2P, Vehicle to Pedestrian), 차량과 개인 단말 간 통신(V2N, Vehicle to Nomadic Devices) 등 

도로 위의 차량에 적용 가능한 모든 형태의 통신 기술을 포함하는 개념으로, 다가올 자율주행 시대를 대비하기 위한 핵심기술로 학계 및 산업계에서 지속적인 연구개발이 진행되고 있습니다.

하지만 아직은 WAVE, C-V2X와 같은 주요 통신 프로토콜의 정리가 완료되지 않았고, 서비스를 위한 신규 통신 Infra 및 관련 지원 단말의 확산에 많은 시간적, 비용적 문제가 있어 실제 서비스 확산이 어려운 상황입니다.


SK Telecom의 V2X Service
-----------------------------
새로운 통신 방식 및 단말의 확산 문제를 넘어 현재 고객에게 유용한 서비스 제공하기 위해, SK Telecom(이하 SKT)에서는 Platform 기반의 V2X 기술을 개발, 상용 서비스를 하고 있습니다.

* Tmap V2X Service 화면 예시

.. image:: ../images/introduction/tmap.png 

|br|


주요 특징
~~~~~~~~~~~~~~~~~~~~~~~~~~

1. ** Connectivity Agnostic Function **

.. rst-class:: text-align-justify

SKT의 V2X는 통신기술의 제약을 받지 않습니다. WAVE, C-V2X 등을 위한 특정 칩셋을 탑재가 필요 없이, LTE, 5G 등을 통해 인터넷에 연결된 디바이스라면 Software Update만으로 V2X 서비스를 이용하실 수 있습니다.

2. ** V2X Service Problem Solver **

.. rst-class:: text-align-justify

효과적인 V2X를 위해서는 connectivity 관점이 아닌 서비스 관점에서 풀어야 할 숙제들이 많이 있습니다. 서비스 받는 차량들이 동일한 방향인지, 선행 혹은 후행인지, 차량들 간의 거리는 어떻게 되는지 등과 같은 도로상의 차량 간의 관계 분석이 필요합니다. Platform에서는 서비스에 필요한 다양한 정보 분석을 실시간, 저지연으로 진행 제공하고 있습니다.

3. ** Low Latency Performance **

.. rst-class:: text-align-justify

국제표준에서 요구하는 V2X 안전메세지를 위한 지연요구사항에서는 대부분 100ms 이내의 지연시간을 요구하고 있습니다.

.. rst-class:: text-align-justify

ETSI TC 102 637-2 표준 

.. image:: ../images/introduction/etsi standard.png

|br|

.. rst-class:: text-align-justify

SKT의 V2X Platform은 위의 기준 이상의 low latency 성능을 제공합니다. 

.. image:: ../images/introduction/latency.png


|br|

** 이와 같은 SKT 고유의 V2X 서비스 기술을 SKT V2N (Vehicle-to-Network) 서비스라고 부릅니다.


Open V2N Service Enabler
----------------------------

SKT의 Open V2N Service Enabler (이하 OVSE)는 현재 Tmap에 제공 중인 V2N 서비스를 OEM, 단말 제조사 등과 같은 외부 개발 파트너사에서 쉽게 개발할 수 있도록, API (Application Programming Interface)를 제공하는 Platform 입니다. 

.. image:: ../images/introduction/ovse concept.png


