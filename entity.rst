.. |br| raw:: html

   <br />

.. _entity-registration:

구성요소(Entity) 등록
=======================================

이 매뉴얼은 OVSE 플랫폼 사용을 위한 단말 등록절차를 설명하기 위한 것입니다.

SK Open API 포털에서 프로젝트 생성 및 OVSE API 추가 후에는 HTTP 기반 REST API로 등록할 수 있습니다. 

Device와 플랫폼을 연동하는 방법은 :ref:`6. Device 연동 절차 <device-procedure>` 와 :ref:`8. 메시지 포맷 <message-format>` 을 참고하십시오. 

App 개발자는 :ref:`7. API 규격 <api-specification>` 과 :ref:`9. Device Simulator <device-simulator>` 를 참고하십시오.


.. _service-registration-api:

서비스 및 회사 등록 (Service Registration)
-----------------------------------

서비스 및 회사는 포털내 프로젝트에서 OVSE API 포함시 자동으로 등록됩니다. 


.. _director-registration:

관리자 등록 (Director Registration)
-----------------------------------

관리자는 프로젝트내 멤버 추가시 자동으로 등록됩니다. 


.. _device-registration:

단말 등록 (Device Registration)
-------------------------------

OVSE 플랫폼 사용을 위해서는 단말이 등록되어야 하며, 유효한 token을 포함한 OVSE API로 등록할 수 있습니다. token 조회 방법은 :ref:`4.4 토큰 조회 <service-procedure-step3>` 을 참조하세요.

.. _device-registration-api:

단말 등록 API
~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify


.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+--------------------------------------------+
| **POST**   | `/api/ovs/v1/device <https://TBD>`__       |
+------------+--------------------------------------------+


- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | Bearer {{token}} | auth token   |
+-----------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+--------------------+---------+-----------+---------------------------------+
| Key                | Type    | Enum      | Description                     |
+====================+=========+===========+=================================+
| vendor             | string  |           | vendor or manufacturer          |
+--------------------+---------+-----------+---------------------------------+
| type               | string  | OVS-G     | OVSE device type                |
|                    |         | OVS-M     |                                 |
+--------------------+---------+-----------+---------------------------------+
| credentialsId      | string  |           | Access Token                    |
|                    |         |           | prefix(5) + unique no.(15)      |
+--------------------+---------+-----------+---------------------------------+
| serialNo           | string  |           | Device Serial No.               |
+--------------------+---------+-----------+---------------------------------+
| ~modelName~        | string  |           | ~device model name~             |
+--------------------+---------+-----------+---------------------------------+
| ~modelCode~        | string  |           | ~device model code~             |
+--------------------+---------+-----------+---------------------------------+
| ~additionalInfo~   | string  |           | ~additional device info~        |
+--------------------+---------+-----------+---------------------------------+

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

    {
        "serialNo": "SN1234567890",
        "credentialsId": "00000000000000000002",
        "modelName": "Brand Name",
        "modelCode": "BN-001",        
        "vendor": "sk",
        "type": "OVS-g",
        "additionalInfo": "string"
    }


:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "05a55bc0-bf63-11e7-8bdf-af923035d741"
        },
        "createdTime": 1509581767542,
        "companyId": {
            "id": "c7fc12a0-beea-11e7-8bdf-af923035d741"
        },
        "directorId": {
            "id": "13814000-1dd2-11b2-8080-808080808080"
        },
        "serialNo": "SN1234567890",
        "credentialsId": "00000000000000000002",
        "modelName": "Brand Name",
        "modelCode": "BN-001",        
        "vendor": "sk",
        "type": "OVS-g",
        "additionalInfo": "string"
    }

.. rst-class:: text-align-justify

요청이 성공하면(code:200) Response에서 Device ID를 얻을 수 있습니다. 
Device ID는 Response 데이터에 있는 id 필드 내의 id 값입니다. 
예시에 있는 05a55bc0-bf63-11e7-8bdf-af923035d741이 Device ID입니다.
|br|


