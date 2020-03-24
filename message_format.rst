.. |br| raw:: html

.. _message-format:

메시지 포맷
==============================

.. rst-class:: text-align-justify

OVSE 플랫폼에 연동되는 다양한 Device들이 플랫폼에 전송하는 메세지에 대해서 정의합니다.

이 매뉴얼은 단말이 MQTTS 프로토콜로 OVSE 플랫폼과 연동하기 위한 메시지 포맷입니다. Entity 등록을 위한 HTTP Rest API 사용은 :ref:`5. 구성요소(Entity) 등록 <entity-registration>` 문서를, App 개발자를 위한 OVSE API 는 :ref:`7. API 규격<api-specification>` 문서를 참고하십시오. 

또한 단말의 연동 절차를 위해서는 :ref:`6. Device 연동 절차 <device-registration>` 문서를 참고하시기 바랍니다.

메시지 포맷은 크게 다음의 두 가지로 나뉩니다.

* 


메시지 기본 구조
-----------------------------

OVSE 플랫폼의 기본 메시지 구조는 ``Header`` 와 ``Payload`` 형태로 구조화 되어 있습니다. 각 메시지는 해당 메시지의 타입인 ``ty`` 로 구분하고 ``ty`` 에 따라 ``pld`` child의 내용이 상이합니다.


