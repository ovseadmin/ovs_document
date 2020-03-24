.. |br| raw:: html

   <br />

.. _entity-registration:

구성요소(Entity) 등록
=======================================

이 매뉴얼은 OVS 플랫폼을 사용하기 원하는 파트너들에게 플랫폼 사용을 위한 절차를 설명하기 위한 것입니다.

Device 와 플랫폼을 연동하는 방법은 :ref:`5. Device 연동 절차 <device-interface>` 와 :ref:`8. 메시지 포맷 <message-format>` 을 참고하십시오. App 개발자는 :ref:`6. API 규격 <api-specification>` 과 :ref:`7. SDK <platform-sdk>` 를 참고하십시오.

구성요소를 등록하는 방법은 2가지가 제공되고 있습니다.

1. `OVS 포털' - ** 현재 제공 계획 없음 - 삭제 ** 
2. `OVS Open API <https://openapi.sk.com>`__ : OVS 플랫폼에서 제공하는 HTTP 기반 Rest API를 이용하는 방법

각 등록 절차마다 두 가지 방법이 설명되어 있으니 참고하시기 바랍니다.

서비스 등록 (Service Registration)
-----------------------------------

API를 활용한 등록
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

SK Open API 가입승인후 OVSE 플랫폼용 JSON Web Token을 발급받을 수 있습니다. 
REST API로 정상적으로 등록된 Token을 확인하는 방법은 다음과 같습니다.

로그인 요청 정보 API
^^^^^^^^^^^^^^^^^^^^

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+------------------------------------+
| **POST**   | `/api/auth/login <https://TBD>`__                           |
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
요청 파라미터를 입력할 때 ServiceType이 중복되지 않도록 해야 합니다. ServiceType은 Unique 값으로 하나의 ServiceType에 한 운영사만 등록할 수 있습니다.

.. _company-registration-api:

회사 정보 등록 API
^^^^^^^^^^^^^^^^^^

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+--------------------------------------+
| **POST**   | `/api/tre/v1/company <https://app.sw |
|            | aggerhub.com/apis/tremoteye/tremotey |
|            | eapi/1.0.0#/Company/post_api_tre_v1_ |
|            | company>`__                          |
+------------+--------------------------------------+

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

+-------------------+---------+-----------------------------------------+
| Key               | Type    | Description                             |
+===================+=========+=========================================+
| name              | string  | 등록할 회사 명칭                        |
+-------------------+---------+-----------------------------------------+
| region            | string  | 등록할 회사 지역                        |
+-------------------+---------+-----------------------------------------+
| serviceType       | string  | 운용하는 사업 명칭                      |
+-------------------+---------+-----------------------------------------+
| picName           | string  | 등록할 COMPANY_ADMIN 이름               |
+-------------------+---------+-----------------------------------------+
| picPhone          | string  | 등록할 COMPANY_ADMIN 연락처             |
+-------------------+---------+-----------------------------------------+
| picEmail          | string  | 등록할 COMPANY_ADMIN 이메일             |
+-------------------+---------+-----------------------------------------+
| picPasswd         | string  | 등록할 COMPANY_ADMIN 패스워드           |
+-------------------+---------+-----------------------------------------+
| picDivision       | string  | 등록할 COMPANY_ADMIN 소속 부서          |
+-------------------+---------+-----------------------------------------+
| sktManagerName    | string  | SKT 담당 매니저 이름                    |
+-------------------+---------+-----------------------------------------+
| sktManagerEmail   | string  | SKT 담당 매니저 이메일                  |
+-------------------+---------+-----------------------------------------+
| cooperationTask   | string  | 협력사 정보, 협력사 업무에 대해 기술    |
+-------------------+---------+-----------------------------------------+
| description       | string  | 추가 정보                               |
+-------------------+---------+-----------------------------------------+
| notifyHost        | string  | Push 메시지 수신 서버 경로 정보         | 
+-------------------+---------+-----------------------------------------+
| notifyMsgType     | string  | 수신하고자 하는 Push 메시지 타입 정보   |
+-------------------+---------+-----------------------------------------+

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

    {
        "name":"운영사A",
        "region":"대한민국",
        "serviceType":"example",
        "picName":"김담당자",
        "picEmail":"companya@example.com",
        "picPhone":"010-0000-0000",
        "picPasswd":"1234",
        "picDivision":"사업1팀",
        "sktManagerName":"박매니저",
        "sktManagerEmail":"manager@skt.com",
        "cooperationTask":"수리",
        "description":"additional description",
        "notifyHost":"http://192.168.0.100:9090/noti",
        "notifyMsgType":"0f"
    }

:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "c7fc12a0-beea-11e7-8bdf-af923035d741"
        },
        "createdTime": 1509530124485,
        "name": "운영사A",
        "serviceType": "example",
        "master": true,
        "masterId": {
            "id": "c7fc12a0-beea-11e7-8bdf-af923035d741"
        },
        "picPasswd": "1234",
        "picName": "김담당자",
        "picPhone": "010-0000-0000",
        "picEmail": "companya@example.com",
        "picDivision": "사업1팀",
        "sktManagerName": "박매니저",
        "sktManagerEmail": "manager@skt.com",
        "cooperationTask": "수리",
        "description": "additional description",
        "notifyHost": "http://192.168.0.100:9090/noti",
        "notifyMsgType": "0f"
    }

.. rst-class:: text-align-justify

정상적으로 등록하면(code:200) 위와 같이 생성된 회사 정보를 Response 값으로 확인할 수 있습니다.

운영사는 master 필드가 true로 출력되므로 master 필드를 통해 이 회사가 운영사로 등록됐는지 구분할 수 있습니다. 생성한 회사 계정으로 처음 로그인할 때 입력한 picEmail를 아이디, picPasswd를 패스워드로 사용합니다. 예시로 보면 아이디는 "companya@example.com", 패스워드는 "123가"입니다. 패스워드는 로그인 후에 변경할 수 있습니다.

REST API를 사용할 때 입력하는 Company ID는 Response 데이터에 있는 id 필드입니다. 예시에 있는 "c7fc12a0-beea-11e7-8bdf-af923035d741"이 Company ID입니다.

회사(협력사) 등록 (Company (Partner) Registration)
----------------------------------------------------

.. _company-registration-portal:

OVS 포털을 통한 등록
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

(제공 계획 없으므로 삭제) 

API를 활용한 등록
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

운영사 계정을 통해서 협력사를 생성할 수 있습니다. 협력사를 등록하기 전에 운영사 계정으로 로그인하여 토큰 데이터를 얻습니다. :ref:`4.1.2.2. 회사 정보 등록 API <company-registration-api>` 와 비교하면 계정이 가진 권한에 차이가 있을 뿐 등록 절차는 동일합니다.

로그인 요청 정보 API
^^^^^^^^^^^^^^^^^^^^

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+----------------------------------------+
| **POST**   | `/api/auth/login  <https://app.swagger |
|            | hub.com/apis/tremoteye/tremoteyeap     |
|            | i/1.0.0#/Auth/post_api_auth_logi       |
|            | n>`__                                  |
+------------+----------------------------------------+

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

- Example Code

:underline:`Request`

.. code-block:: none

        content-type:"application/json"

    {
        "username":"companya@example.com",
        "password":"1234"
    }

:underline:`Response (code: 200)`

.. code-block:: json

    {
        "token":"eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…",
        "refreshToken": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1…"
    }

.. rst-class:: text-align-justify

요청 파라미터를 입력할 때 협력사 ServiceType에는 운영사와 동일한 ServiceType을 기입합니다. 요청이 성공하면(code:200) Response에서 인증 토큰으로 사용할 token 필드를 얻을 수 있습니다. 토큰을 얻었으면 회사 정보 등록 API를 통해 서비스를 등록합니다.

회사 정보 등록 API
^^^^^^^^^^^^^^^^^^

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+-------------+-----------------------------------------------+
|  **POST**   | `api/tre/v1/company <https://app.swaggerhub.c |
|             | om/apis/tremoteye/tremoteyeapi/1.0.0#/Company |
|             | /post_api_tre_v1_comapany>`__                 |
+-------------+-----------------------------------------------+


- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | application/json | auth token   |
+-----------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-------------------+---------+-----------------------------------------+
| Key               | Type    | Description                             |
+===================+=========+=========================================+
| name              | string  | 등록할 회사 명칭                        |
+-------------------+---------+-----------------------------------------+
| region            | string  | 등록할 회사 지역                        |
+-------------------+---------+-----------------------------------------+
| serviceType       | string  | 운용하는 사업 명칭                      |
+-------------------+---------+-----------------------------------------+
| picName           | string  | 등록할 COMPANY_ADMIN 이름               |
+-------------------+---------+-----------------------------------------+
| picPhone          | string  | 등록할 COMPANY_ADMIN 연락처             |
+-------------------+---------+-----------------------------------------+
| picEmail          | string  | 등록할 COMPANY_ADMIN 이메일             |
+-------------------+---------+-----------------------------------------+
| picPasswd         | string  | 등록할 COMPANY_ADMIN 패스워드           |
+-------------------+---------+-----------------------------------------+
| picDivision       | string  | 등록할 COMPANY_ADMIN 소속 부서          |
+-------------------+---------+-----------------------------------------+
| sktManagerName    | string  | SKT 담당 매니저 이름                    |
+-------------------+---------+-----------------------------------------+
| sktManagerEmail   | string  | SKT 담당 매니저 이메일                  |
+-------------------+---------+-----------------------------------------+
| cooperationTask   | string  | 협력사 정보, 협력사 업무에 대해 기술    |
+-------------------+---------+-----------------------------------------+
| description       | string  | 추가 정보                               |
+-------------------+---------+-----------------------------------------+
| rpcNotifyHost     | string  | RPC 결과를 전송받기 위한 서버 호스트    |
+-------------------+---------+-----------------------------------------+
| rpcNotifyPort     | integer | RPC 결과를 전송받기 위한 서버 포트      |
+-------------------+---------+-----------------------------------------+
| rpcNotifyBasePath | string  | RPC 결과를 전송받기 위한 서버 기본 경로 |
+-------------------+---------+-----------------------------------------+

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

    {
        "name":"협력사B",
        "region":"대한민국",
        "serviceType":"example",
        "picName":"김담당자",
        "picEmail":"companyb@example.com",
        "picPhone":"010-0000-0000",
        "picPasswd":"1234",
        "picDivision":"사업1팀",
        "sktManagerName":"박매니저",
        "sktManagerEmail":"manager@skt.com",
        "cooperationTask":"수리",
        "description":"additional description",
        "rpcNotifyHost":"localhost",
        "rpcNotifyPort":9000,
        "rpcNotifyBasePath":"/rpc_noti"
    }


:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "3820ea50-beec-11e7-8bdf-af923035d741"
        },
        "createdTime": 1509530742131,
        "name": "협력사A",
        "serviceType": "example",
        "master": false,
        "masterId": {
            "id": "c7fc12a0-beea-11e7-8bdf-af923035d741"
        },
        "picPasswd": "1234",
        "picName": "김담당자",
        "picPhone": "010-0000-1111",
        "picEmail": "companya@example.com",
        "picDivision": "사업1팀",
        "sktManagerName": "박매니저",
        "sktManagerEmail": "manager@skt.com",
        "cooperationTask": "수리",
        "description": "additional description",
        "rpcNotifyHost": "localhost",
        "rpcNotifyPort": 9000,
        "rpcNotifyBasePath": "/rpc_noti"
    }

.. rst-class:: text-align-justify

정상적으로 등록하면(code:200) 위와 같이 생성된 회사 정보를 Response 값으로 확인할 수 있습니다.

협력사는 Master 필드가 False로 출력되므로 Master 필드를 통해 이 회사가 협력사로 등록됐는지 구분할 수 있습니다. 생성한 회사 계정으로 처음 로그인할 때 입력한 picEmail를 아이디로, picPasswd를 패스워드로 사용합니다. 예시로 보면 아이디는 "companyb@example.com", 패스워드는 "1234"입니다. 패스워드는 변경할 수 있습니다.

REST API를 사용할 때 입력하는 Company ID는 Response 데이터에 있는 id입니다. 예시에 있는 "3820ea50-beec-11e7-8bdf-af923035d741"이 Company ID입니다.


.. _device-registration:

단말 등록 (Device Registration)
-------------------------------

.. _device-registration-portal:

OVS 포털을 통한 등록
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

(삭제) 

API를 활용한 등록
~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

단말은 COMPANY_ADMIN 권한을 가진 회사 계정으로만 등록할 수 있습니다.

단말 등록 API
^^^^^^^^^^^^^

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+--------------------------------------------+
| **POST**   | `/api/ovs/v1/device <https://app.swaggerh  |
|            | ub.com/apis/tremoteye/tremoteyeapi/1.0.0#/ |
|            | Sensor/post_api_tre_v1_sensor>`__          |
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
| X-authorization | string |                  | auth token   |
+-----------------+--------+------------------+--------------+

- Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+--------------------+---------+-----------+---------------------------------+
| Key                | Type    | Enum      | Description                     |
+====================+=========+===========+=================================+
| serialNo           | string  |           | 단말 Serial No.                 |
+--------------------+---------+-----------+---------------------------------+
| credentialsId      | string  |           | Access Token                    |
+--------------------+---------+-----------+---------------------------------+
| vendor             | string  |           | 제조사                          |
+--------------------+---------+-----------+---------------------------------+
| type               | string  | OBD2 |br| | 단말 타입                       |
|                    |         | ADAS      |                                 |
+--------------------+---------+-----------+---------------------------------+
| activationRequired | boolean |           | RPC로 단말 활성화 필요한지 여부 |
+--------------------+---------+-----------+---------------------------------+
| missionType        | string  |           | 변속기 타입                     |
+--------------------+---------+-----------+---------------------------------+
| additionalInfo     | string  |           | 추가 정보                       |
+--------------------+---------+-----------+---------------------------------+

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

    {
        "serialNo": "A1",
        "credentialsId": "00000000000000000002",
        "vendor": "sk",
        "type": "OBD2",
        "activationRequired": true,
        "additionalInfo": "string"
    }


:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "05a55bc0-bf63-11e7-8bdf-af923035d741"
        },
        "createdTime": 1509581767542,
        "vehicleId": {
            "id": "13814000-1dd2-11b2-8080-808080808080"
        },
        "companyId": {
            "id": "c7fc12a0-beea-11e7-8bdf-af923035d741"
        },
        "directorId": {
            "id": "13814000-1dd2-11b2-8080-808080808080"
        },
        "status": "DEACTIVATED",
        "vendor": "sk",
        "type": "OBD2",
        "additionalInfo": "string",
        "lastTripMsgType": null,
        "activationRequired": true,
        "vehicleNo": null,
        "serialNo": "A1",
        "credentialsId": "00000000000000000002"
    }

.. rst-class:: text-align-justify

요청이 성공하면(code:200) Response에서 Device ID를 얻을 수 있습니다. Device ID는 Response 데이터에 있는 id 필드 내의 id 값입니다. 예시에 있는 45f8a100-bef0-11e7-8bdf-af923035d741이 Device ID입니다.
|br|
처음 등록할 때 단말은 DEACTIVATED 상태로 설정됩니다. 

디렉터 등록 (Director Registration)
-----------------------------------

.. _director-registration-portal:

OVS 포털을 통한 등록
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

(삭제)

API를 활용한 등록
~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

디렉터는 COMPANY_ADMIN 권한을 가진 회사 계정으로만 등록할 수 있습니다. 특정 차량들에 대해 관리자로 지정되어 관리하거나, 타 회사의 차량을 위임받아서 모니터링 할 수 있습니다.

디렉터 정보 등록 API
^^^^^^^^^^^^^^^^^^^^

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+----------------------------------------------+
| **POST**   | `/api/tre/v1/director <https://app.swaggerhub|
|            | .com/apis/tremoteye/tremoteyeapi/            |
|            | 1.0.0#/Director/post_api_tre_v1_d            |
|            | irector>`__                                  |
+------------+----------------------------------------------+

-   Header

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

+----------+--------+-------------+
| Key      | Type   | Description |
+==========+========+=============+
| name     | string | 디렉터 이름 |
+----------+--------+-------------+
| email    | string | 이메일      |
+----------+--------+-------------+
| phone    | string | 연락처      |
+----------+--------+-------------+
| password | string | 패스워드    |
+----------+--------+-------------+

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"

    {
        "name": "디렉터C",
        "email": "directorc@example.com",
        "phone": "010-0000-0000",
        "password": "1234",
    }


:underline:`Response (code: 200)`

.. code-block:: json

    {
        "id": {
            "id": "8e904530-c06c-11e7-8bdf-af923035d741"
        },
        "createdTime": 1509695813887,
        "companyId": {
            "id": "c7fc12a0-beea-11e7-8bdf-af923035d741"
        },
        "name": "디렉터C",
        "phone": "010-0000-0000",
        "vehicleId": null,
        "latestTripId": {
            "id": "13814000-1dd2-11b2-8080-808080808080"
        },
        "email": "directorc@example.com",
        "authority": "DIRECTOR",
        "password": null,
        "additionalInfo": null,
        "passwordUpdatedTime": 1509695813887
    }

.. rst-class:: text-align-justify

등록할 때 입력한 email이 아이디입니다. Example Code에서 아이디는 directorc@example.com 이고, 패스워드는 1234 입니다. Authority 필드를 통해 해당 계정이 DIRECTOR 계정인지 DRIVER 계정인지 구분할 수 있습니다.

