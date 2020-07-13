.. |br| raw:: html

Architecture
=======================================

Functional Architecture Model
-----------------------------

.. image:: /images/arc_01.png
	:width: 100%
	:align: center


Open V2X Service ``OVS`` 의 기능적 구조 모델은 상기 도식과 같습니다.
기본적으로 시스템은 ``Enabler Layer`` 와 ``Application Layer`` 로 구분됩니다.

- ``Enabler Layer`` : V2X Application 개발을 위해서 필요한 기능을 제공하는 계층으로 일반적으로 API 형태로 기능들이 외부로 제공됩니다.
- ``Application Layer`` : Enabler Layer를 통해서 제공된 기능을 기반으로 개발되는 V2X 어플리케이션 계층으로 일반적으로 고객사에서 개발하는 영역을 지칭합니다.

본 기술 문서에는 Enabler Layer에서 제공하는 기능의 인터페이스 상의 프로토콜 및 메세지를 명세하며, 
고객사께서 명세된 내용을 기반으로 V2X Application Layer 개발하실 수 있도록 하는 것이 본 기술 문서의 목적이라고 이해하시면 됩니다. 

.. note::
	
    Enabler Layer의 Open V2X Service Enabler Client 영역은 SKT에서 SDK 또는 Simulator를 제공할 예정이오나, 
    고객사에서 직접 개발하실 수 있도록 ``vsc Interface`` 도 오픈할 예정입니다.


Functional Entity and Interfaces Description
--------------------------------------------

Functional Entity Description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============  =============  ===================================================================
Entity Name   Scope          Description              
============  =============  ===================================================================
OVS           In-Scope       | ``Open V2X Service Enabler Server`` 의 약자로 V2X Application을
                             | 개발을 위해 필요한 다양한 기능을 제공하는 백앤드 서버로써 주요 기능은 아래
                             | ``Service Functions of OVS`` 절에서 명세합니다. 
                             | 본 엔티티의 기능은 ``vsc interface`` 와 ``vss interface`` 를 통해 외부로 제공됩니다.
OVC           In-Scope       | ``Open V2X Service Enabler Client`` 의 약자로 단말용 V2X Application을
                             | 개발을 위해 필요한 다양한 기능을 제공하는 클라이언트 미들웨어입니다.
                             | 본 엔티티의 기능은 ``vcc interface`` 를 통해서 외부로 제공됩니다.
VAS           Out-of-Scope   to-be-specified
VAC           Out-of-Scope   to-be-specified
============  =============  ===================================================================


.. note::
	
    상기 표에 컬럼 중 ``Scope`` 컬럼은 OVS 전체 시스템에서 제공하는 엔티티인지 여부를 명세합니다. 
    ``In-Scope`` 인 엔티티는 OVS에서 제공되는 엔티티이며, ``Out-of-Scope`` 인 엔티티는 고객사에서 직접 개발하시는 영역을 의미합니다.


Functional Interfaces Description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

==============  =============  ===================================================================
Interface Name  Scope          Description              
==============  =============  ===================================================================
vsc             In-Scope       | ``OVS`` 와 ``OVC`` 간 인터페이스로 ``OVC`` 의 위치 정보나 V2X Event 등의
                               | 데이터 및 ``OVS`` 주요 서비스 기능이 호출되는 인터페이스입니다.
                               - Binding Protocol : MQTT
vss             In-Scope       | ``OVS`` 와 ``VAS`` 간 인터페이스로 ``OVS`` 를 통해 서비스되는 V2X 서비스 통계
                               | 정책 등을 설정할 수 있는 기능이 호출되는 인터페이스입니다.
                               - Binding Protocol : REST API through HTTP
vcc             Out-of-Scope   to-be-specified
asc             Out-of-Scope   to-be-specified
==============  =============  ===================================================================


Service Functions of OVS
------------------------

OVS 시스템의 핵심 엔티티는 OVS 엔티티입니다. 
외부 고객사에서 V2X 서비스를 쉽게 개발할 수 있도록 V2X에 필요한 주요 기능 ``Service Functions`` 을 구현하여 인터페이스를 통해서 제공합니다.

OVS를 통해서 제공되는 주요 기능은 아래 도식과 같으며, 주요 기능의 상세 설명은 아래 표에 기술합니다.

.. image:: /images/arc_02.png
	:width: 100%
	:align: center


=========================  ===================================================================
Functions                  Description              
=========================  ===================================================================
Common V2X Service Logic   to-be-specified
Map Matcher                to-be-specified
Location Analyser          to-be-specified
Message Controller         to-be-specified
Interworking Proxy         to-be-specified
Route Planning             to-be-specified
AAA                        to-be-specified
Event Logs                 to-be-specified
=========================  ===================================================================



Deployment Scenarios (Case Studies)
-----------------------------------