.. |br| raw:: html

.. _api-specification:

Northbound API Specification 
=======================================

About Northbound API Specification
---------------------------------------

OVSE 플랫폼은 인증, 회사 및 단말 관리, 통계를 위한 Northbound API를 제공합니다. 


.. _api-specification_rest-api:

REST API
-----------

OVSE northbound는 다음과 같은 REST API를 제공합니다. 상세한 내용은 `OVSE Document <https://ovs-document.readthedocs.io/en/latest/index.html>`__ 내용을 참고하시기 바랍니다.

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify


=========  ===================================================  ===========  =====================================================  ======  ======  =====
| 구분      |  설명                                              | Method    | URL                                                  | SA    | CA    | D 
=========  ===================================================  ===========  =====================================================  ======  ======  =====
 Auth       | -  로그인                                          | POST      | /api/auth/login                                      | O     | O     | O 
            | -  토큰 갱신                                       | POST      | /api/auth/token                                      | O     | O     | O 
            | -  비밀번호 변경                                   | POST      | /api/auth/changePassword                             | O     | O     | O 
            | -  임시비밀번호 변경                               | POST      | /api/auth/resetPasswordByEmail                       | O     | O     | O 
            | -  사용자 이메일 검색                              | POST      | /api/auth/findUserEmail                              | O     | O     | O 
---------  ---------------------------------------------------  -----------  -----------------------------------------------------  ------  ------  -----
 Company    | -  회사 정보 등록                                  | POST      | /api/ovs/v1/company                                  | O     | X     | X 
            | -  회사 정보 조회                                  | GET       | /api/ovs/v1/company/{companyId}                      | O     | O     | O 
            | -  회사 정보 수정                                  | PUT       | /api/ovs/v1/company/{companyId}                      | O     | O     | X 
            | -  회사 삭제                                       | DELETE    | /api/ovs/v1/company/{companyId}                      | O     | O     | X 
            | -  내 회사 정보 조회                               | GET       | /api/ovs/v1/company/me                               | O     | O     | O 
            | -  특정 Service Type에 속하는 회사 리스트 조회     | GET       | /api/ovs/v1/companies                                | O     | X     | X 
            | -  등록된 전체 회사 수 조회                        | GET       | /api/ovs/v1/companies/all                            | O     | X     | X 
            | -  특정 Service Type에 속하는 회사의 수 조회       | GET       | /api/ovs/v1/companies/cnt                            | O     | X     | X 
            | -  회사 관리자 등록                                | POST      | /api/ovs/v1/company/{companyId}/admin                | O     | O     | X 
            | -  회사 관리자 수정                                | PUT       | /api/ovs/v1/company/{companyId}/admin/{adminId}      | O     | O     | X 
            | -  회사 관리자 삭제                                | DELETE    | /api/ovs/v1/company/{companyId}/admin/{adminId}      | O     | O     | X 
            | -  회사 관리자 리스트 조회                         | GET       | /api/ovs/v1/company/{companyId}/admins               | O     | O     | X 
            | -  소유한 단말 리스트 조회                         | GET       | /api/ovs/v1/company/{companyId}/devices              | O     | O     | O 
            | -  특정 회사 모든 단말에 메시지 전달               | POST      | /api/ovs/v1/company/{companyId}/message              | O     | O     | O 
---------  ---------------------------------------------------  -----------  -----------------------------------------------------  ------  ------  -----
 Device     | -  단말 등록                                       | POST      | /api/ovs/v1/device                                   | O     | O     | O 
            | -  SerialNo로 단말 조회                            | GET       | /api/ovs/v1/device                                   | O     | O     | O 
            | -  단말 정보 조회                                  | GET       | /api/ovs/v1/device/{deviceId}                        | O     | O     | O 
            | -  단말 정보 수정                                  | PUT       | /api/ovs/v1/device/{deviceId}                        | O     | O     | O 
            | -  단말 삭제                                       | DELETE    | /api/ovs/v1/device/{deviceId}                        | O     | O     | O 
            | -  전체 단말 리스트 조회                           | GET       | /api/ovs/v1/devices                                  | O     | O     | O 
            | -  특정 Service Type에 속하는 단말의 수 조회       | GET       | /api/ovs/v1/devices/cnt                              | O     | O     | O 
            | -  소유한 전체 단말 수	                         | GET       | /api/ovs/v1/devices/owned/cnt                        | O     | O     | O 
            | -  단말별 메시지 전달                              | POST      | /api/ovs/v1/device/{deviceId}/message                | O     | O     | O 
---------  ---------------------------------------------------  -----------  -----------------------------------------------------  ------  ------  -----
 Stats      | -  특정 회사 모든 단말의 기간별 이벤트 통계        | GET       | /api/ovs/v1/company/{companyId}/statistics/event     | O     | O     | O 
            | -  특정 단말 기간별 이벤트 통계                    | GET       | /api/ovs/v1/device/{deviceId}/statistics/event       | O     | O     | O 
=========  ===================================================  ===========  =====================================================  ======  ======  =====
.. rst-class:: text-align-justify

[API 리스트3]

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

=========  ===================================================  ===========  =====================================================  ======
| 구분      |  설명                                              | Method    | URL                                                  | SA  
=========  ===================================================  ===========  =====================================================  ======
 Auth       | -  로그인                                          | POST      | /api/auth/login                                      | O   
            | -  토큰 갱신                                       | POST      | /api/auth/token                                      | O   
            | -  비밀번호 변경                                   | POST      | /api/auth/changePassword                             | O   
            | -  임시비밀번호 변경                               | POST      | /api/auth/resetPasswordByEmail                       | O   
            | -  사용자 이메일 검색                              | POST      | /api/auth/findUserEmail                              | O   
---------  ---------------------------------------------------  -----------  -----------------------------------------------------  ------
 Company    | -  회사 정보 등록                                  | POST      | /api/ovs/v1/company                                  | O   
            | -  회사 정보 조회                                  | GET       | /api/ovs/v1/company/{companyId}                      | O   
            | -  회사 정보 수정                                  | PUT       | /api/ovs/v1/company/{companyId}                      | O   
            | -  회사 삭제                                       | DELETE    | /api/ovs/v1/company/{companyId}                      | O   
            | -  내 회사 정보 조회                               | GET       | /api/ovs/v1/company/me                               | O   
            | -  특정 Service Type에 속하는 회사 리스트 조회     | GET       | /api/ovs/v1/companies                                | O   
            | -  등록된 전체 회사 수 조회                        | GET       | /api/ovs/v1/companies/all                            | O   
            | -  특정 Service Type에 속하는 회사의 수 조회       | GET       | /api/ovs/v1/companies/cnt                            | O   
            | -  회사 관리자 등록                                | POST      | /api/ovs/v1/company/{companyId}/admin                | O   
            | -  회사 관리자 수정                                | PUT       | /api/ovs/v1/company/{companyId}/admin/{adminId}      | O   
            | -  회사 관리자 삭제                                | DELETE    | /api/ovs/v1/company/{companyId}/admin/{adminId}      | O   
            | -  회사 관리자 리스트 조회                         | GET       | /api/ovs/v1/company/{companyId}/admins               | O   
            | -  소유한 단말 리스트 조회                         | GET       | /api/ovs/v1/company/{companyId}/devices              | O   
            | -  특정 회사 모든 단말에 메시지 전달               | POST      | /api/ovs/v1/company/{companyId}/message              | O   
---------  ---------------------------------------------------  -----------  -----------------------------------------------------  ------
 Device     | -  단말 등록                                       | POST      | /api/ovs/v1/device                                   | O   
            | -  SerialNo로 단말 조회                            | GET       | /api/ovs/v1/device                                   | O   
            | -  단말 정보 조회                                  | GET       | /api/ovs/v1/device/{deviceId}                        | O   
            | -  단말 정보 수정                                  | PUT       | /api/ovs/v1/device/{deviceId}                        | O   
            | -  단말 삭제                                       | DELETE    | /api/ovs/v1/device/{deviceId}                        | O   
            | -  전체 단말 리스트 조회                           | GET       | /api/ovs/v1/devices                                  | O   
            | -  특정 Service Type에 속하는 단말의 수 조회       | GET       | /api/ovs/v1/devices/cnt                              | O   
            | -  소유한 전체 단말 수	                         | GET       | /api/ovs/v1/devices/owned/cnt                        | O   
            | -  단말별 메시지 전달                              | POST      | /api/ovs/v1/device/{deviceId}/message                | O   
---------  ---------------------------------------------------  -----------  -----------------------------------------------------  ------
 Stats      | -  특정 회사 모든 단말의 기간별 이벤트 통계        | GET       | /api/ovs/v1/company/{companyId}/statistics/event     | O   
            | -  특정 단말 기간별 이벤트 통계                    | GET       | /api/ovs/v1/device/{deviceId}/statistics/event       | O   
=========  ===================================================  ===========  =====================================================  ======

.. rst-class:: text-align-justify

SA: System Admin
CA: Company Admin
D: Director

.. _api-specification_entity-registration:

Entity Model and Registration
-----------------------------------

.. rst-class:: text-align-justify

REST API에서는 다음과 같은 Entity들이 정의되어 있으며, 세부 데이터 모델과 등록 방법은 
:ref:`5. 구성요소(Entity) 등록 <entity-registration>`__ 내용을 참고하시기 바랍니다.

-  Company

-  Device

-  Director

.. _api-specification_authentication:

인증 Authentication API
-----------------------------------
.. rst-class:: text-align-justify

OVSE Northbound API 사용시 해당 API에 맞는 authentication API를 통해 token을 부여받고,
이를 header에 포함하여야 합니다. 

[표 추가: company admin과 director간 호출가능한 API 분류 - 혹은 표에 추가]
[ 혹은 company admin만 호출가능한 API 명시]

token을 받기 위한 authentication API는 아래와 같습니다.

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+------------------------------------+
| **POST**   | `/api/auth/login <https://TBD>`__  |
+------------+------------------------------------+

- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+--------------+--------+------------------+--------------+
| option       | Type   | Default          | Description  |
+==============+========+==================+==============+
| Content-Type | string | application/json | content type |
+--------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| username | string | 로그인할 아이디(이메일) |
+----------+--------+-------------------------+
| password | string | 패스워드                |
+----------+--------+-------------------------+

.. role:: underline
        :class: underline

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"

    {
        "username":"example@example.com",
        "password":"1234"
    }

:underline:`Response (code: 200)`

.. code-block:: json

    {
        "token":"eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…",
        "refreshToken": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1…"
    }

.. rst-class:: text-align-justify

요청이 성공하면(code:200) Response에서 인증 토큰으로 사용할 token 필드를 얻을 수 있습니다. Token 필드는 HTTP Header에 “X-Authorization"의 값으로 사용되며 로그인할 때마다 변경됩니다. 토큰이 있으면 해당 계정에 접근할 수 있으므로 외부 유출이 안되도록 주의해야 합니다.
|br|
토큰을 얻었으면 회사 정보 등록 API를 통해 서비스를 등록합니다.
|br|

.. _api-specification_information:

정보 조회 API
------------------------

.. _api-specification_company-information:

회사 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

등록된 회사의 정보를 조회하는 API 입니다. 회사정보를 조회하기 위해서는 회사 Admin 계정으로 인증받은 token이 필요합니다. 
관리자(Director) 계정으로는 회사 정보를 조회할 수 없습니다.


.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+----------------------------------------------------+
| **GET**   | `/api/ovs/v1/company/{companyId} <https://TBD>`__  |
+------------+----------------------------------------------------+

- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string |                  | auth token   |
+-----------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

.. role:: underline
        :class: underline

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"


:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "4813f210-73ab-11ea-ac0c-d950be57c747"
        },
        "createdTime": 1585699007148,
        "name": "test_companyname_ovse2",
        "serviceType": "test_servicetype_ovse2",
        "picPasswd": "null",
        "picName": "createcompanynam2e",
        "picPhone": "010-1111-1234",
        "picEmail": "test_servicetype_ovse2@sktint.com",
        "picDivision": "team1",
        "description": "additional description",
        "tokenPrefix": "enh03"
    }

.. rst-class:: text-align-justify

회사ID가 등록되어있고, token이 유효한 경우 정상적으로 조회할 수 있습니다. 
나의 소속 회사 ID를 모르는 경우, 소속 회사 조회 API로 검색 가능합니다. 
|br|


.. _api-specification_my-company-information:

내 회사 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

나의 계정정보와 내가 속한 회사의 Company ID를 조회하는 API 입니다. 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+------------------------------------------+
| **GET**   | `/api/ovs/v1/company/me <https://TBD>`__  |
+------------+------------------------------------------+

- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string |                  | auth token   |
+-----------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

.. role:: underline
        :class: underline

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

:underline:`Response (code: 200)`

.. code-block:: json
    {
        "id": {
            "id": "4823f7a0-73ab-11ea-ac0c-d950be57c747"
        },
        "createdTime": 1585699007493,
        "companyId": {
            "id": "4813f210-73ab-11ea-ac0c-d950be57c747"
        },
        "name": "createcompanynam2e",
        "phone": "010-1111-1234",
        "email": "test_servicetype_ovse2@sktint.com",
        "authority": "COMPANY_ADMIN",
        "password": null,
        "additionalInfo": null,
        "passwordUpdatedTime": 1585699007493
    }

.. rst-class:: text-align-justify

token이 유효한 경우 정상적으로 조회할 수 있습니다. 

|br|


.. _api-specification_my-company-information:

회사 관리자(Director) 리스트 조회 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. _api-specification_my-company-information:

단말 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

단말 ID를 통해 단말 정보를 조회하는 API 입니다. 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+-------------------------------------------------+
| **GET**   | `/api/ovs/v1/device/{deviceId} <https://TBD>`__  |
+------------+-------------------------------------------------+

- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string |                  | auth token   |
+-----------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

.. role:: underline
        :class: underline

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

:underline:`Response (code: 200)`

.. code-block:: json
    {
        "id": {
            "id": "4823f7a0-73ab-11ea-ac0c-d950be57c747"
        },
        "createdTime": 1585699007493,
        "companyId": {
            "id": "4813f210-73ab-11ea-ac0c-d950be57c747"
        },
        "name": "createcompanynam2e",
        "phone": "010-1111-1234",
        "email": "test_servicetype_ovse2@sktint.com",
        "authority": "COMPANY_ADMIN",
        "password": null,
        "additionalInfo": null,
        "passwordUpdatedTime": 1585699007493
    }

.. rst-class:: text-align-justify

token이 유효한 경우 정상적으로 조회할 수 있습니다. 

.. _api-specification_information_modification:

정보 수정 API
------------------------

.. _api-specification_company-information-modification:

회사 정보 수정
~~~~~~~~~~~~~~~~~~

.. _api-specification_device-information-modification:

단말 정보 수정
~~~~~~~~~~~~~~~~~~

.. _api-specification_director-information-modification:

관리자 정보 수정
~~~~~~~~~~~~~~~~~~

.. _api-specification_statistics:

이벤트 통계 API
------------------------

.. _api-specification_statistics1:

통계1
~~~~~~~~~~~~~~~~~~

.. _api-specification_statistics2:

통계2
~~~~~~~~~~~~~~~~~~

.. _api-specification_statistics3:

통계3
~~~~~~~~~~~~~~~~~~

