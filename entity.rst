.. |br| raw:: html

   <br />

.. _entity-registration:

구성요소(Entity) 등록
=======================================

이 매뉴얼은 OVSE 플랫폼 사용을 위한 단말 등록절차를 설명하기 위한 것입니다.

SK open API 포털에서 프로젝트 생성 및 OVSE API 추가 후에는 HTTP 기반 REST API로 등록할 수 있습니다. 

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

OVSE 플랫폼 사용을 위해서는 단말이 등록되어야 하며, 유효한 auth token을 포함한 OVSE API로 등록할 수 있습니다. token 조회 방법은 :ref:`4.4 토큰 조회 <service-procedure-step3>` 을 참조하세요.
단말등록시 단말의 일련번호(SerialNo)와 단말비밀번호(CredentialsId)는 정해진 규칙을 따라야 하며,
둘 필드 모두 5자리의 company prefix로 시작해야 합니다. company prefix는  :ref:`7.2.2 내 회사 정보 조회 <api-specification_my-company-information>` API로 조회할 수 있습니다. 

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

+--------------------+---------+-----------+------------------------------------+
| Key                | Type    | Enum      | Description                        |
+====================+=========+===========+====================================+
| vendor             | string  |           | vendor or manufacturer             |
+--------------------+---------+-----------+------------------------------------+
| type               | string  | OVC-G,    | OVSE device type                   |
|                    |         | OVC-M     |                                    |
+--------------------+---------+-----------+------------------------------------+
| credentialsId      | string  |           | Device credential                  |
|                    |         |           | company prefix(5) + unique no.(15) |
+--------------------+---------+-----------+------------------------------------+
| serialNo           | string  |           | Device Serial No.                  |
|                    |         |           | company prefix(5) + unique no.     |
+--------------------+---------+-----------+------------------------------------+
| ~modelName~        | string  |           | ~device model name~                |
+--------------------+---------+-----------+------------------------------------+
| ~modelCode~        | string  |           | ~device model code~                |
+--------------------+---------+-----------+------------------------------------+
| ~additionalInfo~   | string  |           | ~additional device info~           |
+--------------------+---------+-----------+------------------------------------+

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"
    {
        "vendor": "SKT",
        "type": "OVC-G",
        "credentialsId":"{{prefix}}123456789012345",
        "serialNo":"{{prefix}}12345678911234"
    }


:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "128fe3e0-ab98-11ea-b482-911940102f00"
        },
        "createdTime": 1591848022149,
        "companyId": {
            "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
        },
        "vendor": "SKT",
        "type": "OVC-G",
        "additionalInfo": null,
        "activationRequired": false,
        "serialNo": "uio3512345678911234",
        "credentialsId": "uio35123456789012345"
    }

.. rst-class:: text-align-justify

요청이 성공하면(code:200) Response에서 Device ID를 얻을 수 있습니다. 
Device ID는 Response 데이터에 있는 id 필드 내의 id 값입니다. 
예시에 있는 128fe3e0-ab98-11ea-b482-911940102f00 값이 Device ID입니다.
|br|


