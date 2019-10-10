Architecture
=======================================

Functional Architecture Model
-----------------------------

.. image:: /images/arc_01.png
	:width: 100%
	:align: center


Open V2X Service Enabler ``OVS`` 의 기능적 구조 모델은 상기 도식과 같습니다.
기본적으로 시스템은 ``Enabler Layer`` 와 ``Application Layer`` 로 구분됩니다.

- Enabler Layer : V2X Application 개발을 위해서 필요한 기능을 제공하는 계층으로 일반적으로 API 형태로 기능들이 외부로 제공됩니다.
- Application Layer : Enabler Layer를 통해서 제공된 기능을 기반으로 개발되는 V2X 어플리케이션 계층으로 일반적으로 고객사에서 개발하는 영역을 지칭합니다.

본 기술 문서에는 Enabler Layer에서 제공하는 기능의 인터페이스 상의 프로토콜 및 메세지를 명세하며, 
고객사께서 명세된 내용을 기반으로 V2X Application Layer 개발하실 수 있도록 하는 것이 본 기술 문서의 목적이라고 이해하시면 됩니다. 

.. note::
	
    Enabler Layer의 Open V2X Service Enabler Client 영역은 SKT에서 SDK 또는 Simulator를 제공할 예정이오나, 
    고객사에서 직접 개발하실 수 있도록 ``vsc 인터페이스`` 도 오픈할 예정입니다.


Functional Entity Description
-----------------------------

===========  =======================  ========================================
Entity Name  Description              Note
===========  =======================  ========================================
OVS          2019/10                  Initial Publish
===========  =======================  ========================================