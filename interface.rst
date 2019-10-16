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
	
    본 절을 이해하기 위해서 다음의 기본 이해가 필요합니다.

        - MQTT Protocol
        - Node.js Program Language 


vsc Interface for ``OVC-g``  
~~~~~~~~~~~~~~~~~~~~~~~~~~~

OVC-g 단말을 위한 vsc Interface의 Flow는 다음과 같으며, 각 단계별로 절차를 ``Node.js`` 기반 코드로 소개합니다.



vsc Interface for ``OVC-m``
~~~~~~~~~~~~~~~~~~~~~~~~~~~