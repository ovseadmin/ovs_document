OVSE 주요 구성요소 (Entity Architecture)
=======================================

.. rst-class:: text-align-justify

OVSE 플랫폼은 서비스 특성에 맞도록 설계된 유연한 데이터 구조를 지원합니다.

OVSE 서비스를 위한 Entity 간의 관계를 표현하면 다음과 같습니다.

.. image:: /images/entity_architecture/ovse_entity_arch.png
	:width: 100%
	:align: center

그 중 Company 내부의 관계는 다음과 같습니다.

.. image:: /images/entity_architecture/ovse_company.png
	:width: 100%
	:align: center


기본 구성요소 (Basic Entity)
-------------------------------
각각의 Entity들과 그 역할의 설명은 다음과 같습니다.

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

=============================   ==================================================================================================
구분                             설명
=============================   ==================================================================================================
Company                         | * V2N 서비스 단말의 제조사 혹은 관리 업체 (예: 블랙박스, IVI 제조사, 자체 Backend를 보유한 OEM ) 
                                | * 관리자 계정을 통해서 서비스 대상 단말을 등록 및 관리합니다.
                                | * 등록된 Device 들을 Director에게 할당합니다.
                                | * Company는 운영사(Master Company)와 협력사(Partner Company)로 구분됩니다.
                                |
                                |   운영사 (Master)
                                |
                                |   - OVSE 시스템 관리자에 의해서 등록됩니다.
                                |   - Device를 등록, 수정, 삭제할 수 있습니다.
                                |   - 협력사를 등록하고 수정, 삭제할 수 있습니다. (협력사가 등록한 협력사의 수정, 삭제도 가능)
                                |
                                |   협력사 (Partner)
                                |   
                                |   - Company 관리자에 의해서 등록됩니다.
                                |   - Device를 등록, 수정, 삭제할 수 있습니다.
                                |   - 협력사를 등록할 수 있습니다. (수정/삭제는 불가)
Director                        | * V2N Application Device를 소유/관리/운용하고 있는 사용자 
                                | * Device의 등록/삭제를 할 수 있으며, 타 Director가 등록한 Device는 접근할 수 없다.
Device                          | * OVSE와 플랫폼과 통신하여 V2N Application을 제공하는 주체. 
                                | * 차량의 위치, Event 정보를 센싱하여 플랫폼으로 전달하고, 플랫폼으로 부터 V2N Event 메세지를 수신하여 서비스한다. 
=============================   ==================================================================================================




