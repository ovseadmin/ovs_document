.. |br| raw:: html

.. _api-specification:

Northbound API Specification 
=======================================

About Northbound API Specification
---------------------------------------

OVSE 플랫폼은 정보 조회, 단말 관리, 통계를 위한 Northbound API를 제공합니다. 


.. _api-specification_rest-api:

REST API
-----------

OVSE northbound는 다음과 같은 REST API를 제공합니다. 상세한 내용은 `OVSE Document <https://ovs-document.readthedocs.io/en/latest/index.html>`__ 내용을 참고하시기 바랍니다.

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify


=========  ===============================================  ===========  =====================================================
| 구분      |  설명                                          | Method    | URL                                                
=========  ===============================================  ===========  =====================================================
 Company    | -  회사 정보 조회                              | GET       | /api/ovs/v1/company/{companyId}                    
            | -  내 회사 정보 조회                           | GET       | /api/ovs/v1/company/me                             
            | -  회사의 모든 단말에 메시지 전달              | POST      | /api/ovs/v1/company/{companyId}/message            
---------  -----------------------------------------------  -----------  -----------------------------------------------------
 Device     | -  단말 등록                                   | POST      | /api/ovs/v1/device                                 
            | -  단말 정보 조회                              | GET       | /api/ovs/v1/device/{deviceId}                      
            | -  단말 정보 수정                              | PUT       | /api/ovs/v1/device/{deviceId}                      
            | -  단말 삭제                                   | DELETE    | /api/ovs/v1/device/{deviceId}                      
            | -  전체 단말 정보 조회                         | GET       | /api/ovs/v1/devices                                
            | -  회사 전체 단말 수 조회                      | GET       | /api/ovs/v1/devices/owned/cnt                      
            | -  단말별 메시지 전달                          | POST      | /api/ovs/v1/device/{deviceId}/message              
---------  -----------------------------------------------  -----------  -----------------------------------------------------
 Stats      | -  회사 모든 단말의 기간별 이벤트 통계         | GET       | /api/ovs/v1/company/{companyId}/statistics/event   
            | -  특정 단말 기간별 이벤트 통계                | GET       | /api/ovs/v1/device/{deviceId}/statistics/event     
=========  ===============================================  ===========  =====================================================
.. rst-class:: text-align-justify

.. _api-specification_information:

정보 조회 API
------------------------

.. _api-specification_company-information:

회사 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

등록된 회사의 정보를 조회하는 API 입니다. 회사정보를 조회하기 위해서는 인증받은 token과 회사ID(companyId) 가 필요합니다. 
token은 SK Open API 홈페이지에서, 회사ID(companyId)는 "내 회사 정보 조회" API로 확인할 수 있습니다. 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

- Request API URL
+------------+----------------------------------------------------+
| **GET**    | `/api/ovs/v1/company/{companyId} <https://TBD>`__  |
+------------+----------------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+-----------------------------+
| option          | Type   | Default          | Description                 |
+=================+========+==================+=============================+
| Content-Type    | string | application/json | content type                |
+-----------------+--------+------------------+-----------------------------+
| X-authorization | string | {{authToken}}    | auth token of each company  |
+-----------------+--------+------------------+-----------------------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

- Response fields

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+----------------------------------------------------+
| Field           | Description                                        |
+=================+====================================================+
| id              | ID of my company                                   |
+-----------------+----------------------------------------------------+
| name            | service name(automatically generated)              |
+-----------------+----------------------------------------------------+
| serviceType     | service type(automatically generated)              |
+-----------------+----------------------------------------------------+
| tokenPrefix     | company prefix for serialNo and credentialsId      |
+-----------------+----------------------------------------------------+

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
            "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
        },
        "createdTime": 1590654831577,
        "name": "skoa_l7xx73e3323ea2124bd89d5ce708bcb26fd8",
        "serviceType": "skoa_l7xx73e3323ea2124bd89d5ce708bcb26fd8",
        "master": true,
        "masterId": {
            "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
        },
        "picPasswd": null,
        "picName": "skoa_l7xx73e3323ea2124bd89d5ce708bcb26fd8",
        "picPhone": null,
        "picEmail": "l7xx73e3323ea2124bd89d5ce708bcb26fd8@skopenapi.com",
        "picDivision": null,
        "sktManagerName": null,
        "sktManagerEmail": null,
        "cooperationTask": null,
        "description": null,
        "notifyHost": null,
        "notifyMsgType": null,
        "notifyErrIdleMin": 0,
        "pwAccess": null,
        "dataAnalytics": null,
        "bcn": null,
        "tokenPrefix": "uio35",
        "ovs": true,
        "tokenExpr": -1
    }

.. rst-class:: text-align-justify



.. _api-specification_my-company-information:

내 회사 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

내가 속한 회사의 Company ID를 조회하는 API 입니다. 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+------------------------------------------+
| **GET**    | `/api/ovs/v1/company/me <https://TBD>`__ |
+------------+------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | {{authToken}}    | auth token   |
+-----------------+--------+------------------+--------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

- Response fields

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+----------------------------------------------------+
| Field           | Description                                        |
+=================+====================================================+
| id              | ID of my company                                   |
+-----------------+----------------------------------------------------+
| name            | service name(automatically generated)              |
+-----------------+----------------------------------------------------+
| serviceType     | service type(automatically generated)              |
+-----------------+----------------------------------------------------+
| tokenPrefix     | company prefix for serialNo and credentialsId      |
+-----------------+----------------------------------------------------+


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
            "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
        },
        "createdTime": 1590654831577,
        "name": "skoa_l7xx73e3323ea2124bd89d5ce708bcb26fd8",
        "serviceType": "skoa_l7xx73e3323ea2124bd89d5ce708bcb26fd8",
        "master": true,
        "masterId": {
            "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
        },
        "picPasswd": null,
        "picName": "skoa_l7xx73e3323ea2124bd89d5ce708bcb26fd8",
        "picPhone": null,
        "picEmail": "l7xx73e3323ea2124bd89d5ce708bcb26fd8@skopenapi.com",
        "picDivision": null,
        "sktManagerName": null,
        "sktManagerEmail": null,
        "cooperationTask": null,
        "description": null,
        "notifyHost": null,
        "notifyMsgType": null,
        "notifyErrIdleMin": 0,
        "pwAccess": null,
        "dataAnalytics": null,
        "bcn": null,
        "tokenPrefix": "uio35",
        "ovs": true,
        "tokenExpr": -1
    }

.. rst-class:: text-align-justify

token이 유효한 경우 정상적으로 조회할 수 있습니다. 


.. _api-specification_device-information:

단말 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

단말 시리얼번호(serialNo)를 통해 단말 ID, 단말 형태 등 단말정보를 조회하는 API 입니다. token이 유효한 경우 정상적으로 조회할 수 있습니다. 


.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+-------------------------------------------------+
| **GET**    | `/api/ovs/v1/device/{serialNo} <https://TBD>`__ |
+------------+-------------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | {{authToken}}    | auth token   |
+-----------------+--------+------------------+--------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

- Response fields

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+----------------------------------------------------+
| Field           | Description                                        |
+=================+====================================================+
| id              | unique device id                                   |
+-----------------+----------------------------------------------------+
| companyId       | unique company id                                  |
+-----------------+----------------------------------------------------+
| type            | device type(OVC-G or OVC-M)                        |
+-----------------+----------------------------------------------------+


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
            "id": "37c6b060-a0be-11ea-a9b8-ff6a8104c32f"
        },
        "createdTime": 1590654942693,
        "companyId": {
            "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
        },
        "vendor": "SKT1",
        "type": "OVC-G",
        "additionalInfo": null,
        "activationRequired": false,
        "serialNo": "uio35fine1236",
        "credentialsId": null
    }

.. rst-class:: text-align-justify


.. _api-specification_device-list-information:

전체 단말 리스트 조회
~~~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

전체 단말 정보를 조회하는 API 입니다. token이 유효한 경우 정상적으로 조회할 수 있습니다. 


.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+-------------------------------------------------+
| **GET**    | `/api/ovs/v1/devices <https://TBD>`__           |
+------------+-------------------------------------------------+
| **GET**    | `/api/ovs/v1/devices?limit=10 <https://TBD>`__  |
+------------+-------------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | {{authToken}}    | auth token   |
+-----------------+--------+------------------+--------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

- Response fields

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+----------------------------------------------------+
| Field           | Type   | Description                                        |
+=================+========+====================================================+
| id              | string | unique device id                                   |
+-----------------+--------+----------------------------------------------------+
| companyId       | string | unique company id                                  |
+-----------------+--------+----------------------------------------------------+
| type            | string | device type(OVC-G or OVC-M)                        |
+-----------------+--------+----------------------------------------------------+


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
        "data": [
            {
                "id": {
                    "id": "37c6b060-a0be-11ea-a9b8-ff6a8104c32f"
                },
                "createdTime": 1590654942693,
                "companyId": {
                    "id": "f58ccd10-a0bd-11ea-a9b8-ff6a8104c32f"
                },
                "vendor": "SKT1",
                "type": "OVC-G",
                "additionalInfo": null,
                "activationRequired": false,
                "serialNo": "uio35fine1236",
                "credentialsId": null
            }
        ],
        "nextPageLink": null,
        "hasNext": false
    }

.. rst-class:: text-align-justify



.. _api-specification_device-count:

회사 전체 단말 수 조회
~~~~~~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

회사 소유의 전체 단말 수를 조회하는 API 입니다. token이 유효한 경우 정상적으로 조회할 수 있습니다. 


.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+-------------------------------------------------+
| **GET**    | `/api/ovs/v1/devices/owned/cnt <https://TBD>`__ |
+------------+-------------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | {{authToken}}    | auth token   |
+-----------------+--------+------------------+--------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+-------------------------+
| Key      | Type   | Description             |
+==========+========+=========================+
| N/A      | N/A    | N/A                     |
+----------+--------+-------------------------+

- Response fields

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+----------------------------------------------------+
| Field           | Description                                        |
+=================+====================================================+
| count           | number of my registered devices                    |
+-----------------+----------------------------------------------------+

.. role:: underline
        :class: underline

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"


:underline:`Response (code: 200)`

.. code-block:: json

    1

.. rst-class:: text-align-justify




.. _api-specification_device-management:

단말 관리 API
------------------------

.. _api-specification_device-registration:

단말 등록
~~~~~~~~~~~~~~~~~~

OVS 서비스를 이용할 신규 단말을 등록합니다. 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+---------------------------------------------------+
| **POST**   | `/api/ovs/v1/device              <https://TBD>`__ |
+------------+---------------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | {{authToken}}    | auth token   |
+-----------------+--------+------------------+--------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------------+--------+--------------------------------------------------------------------+
| Key            | Type   | Description                                                        |
+================+========+====================================================================+
| vendor         | string | company name                                                       |
+----------------+--------+--------------------------------------------------------------------+
| type           | string | device type(OVS-G or OVS-M)                                        |
+----------------+--------+--------------------------------------------------------------------+
| credentialsId  | string | device credentails (5 digit company prefix + 15 digit credentails) |
+----------------+--------+--------------------------------------------------------------------+
| serialNo       | string | device serialNo (5 digit company prefix + unique serial number)    |
+----------------+--------+--------------------------------------------------------------------+

.. role:: underline
        :class: underline

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

:underline:`Request` in curl format

.. code-block:: none

    curl --location --request POST 'http://openapi_gatweay:18080/api/ovs/v1/device' \
        --header 'Content-Type: application/json' \
        --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…' \
        --data-raw '{
            "vendor": "SKT",
            "type": "OVC-G",
            "credentialsId":"uio35123456789012345",
            "serialNo":"uio3512345678911234"
        }'


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





.. _api-specification_company-information-modification:

회사 정보 수정
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

나의 계정정보와 내가 속한 회사의 Company ID를 수정하는 API 입니다. 회사 admin만 사용가능합니다. (director는 사용불가) 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+---------------------------------------------------+
| **PUT**    | `/api/ovs/v1/company/{companyId} <https://TBD>`__ |
+------------+---------------------------------------------------+

- Request Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+-----------------+--------+------------------+--------------+
| option          | Type   | Default          | Description  |
+=================+========+==================+==============+
| Content-Type    | string | application/json | content type |
+-----------------+--------+------------------+--------------+
| X-authorization | string | {{authToken}}    | auth token   |
+-----------------+--------+------------------+--------------+

- Request Body

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+----------+--------+----------------------------+
| Key      | Type   | Description                |
+==========+========+============================+
| picPhone | string | data field(for example)    |
+----------+--------+----------------------------+
| ...      | ....   | any other field to change  |
+----------+--------+----------------------------+

.. role:: underline
        :class: underline

- Example Code

:underline:`Request`

.. code-block:: none

    content-type:"application/json"
    X-Authorization: "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeXNhZG1pbkB0aG…"
    {
        "picPhone": "010-1111-1235",
        "... any other field to change ..."
    }


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
        "phone": "010-1111-1235",
        "email": "test_servicetype_ovse2@sktint.com",
        "authority": "COMPANY_ADMIN",
        "password": null,
        "additionalInfo": null,
        "passwordUpdatedTime": 1585699007493
    }

.. rst-class:: text-align-justify

token이 유효한 경우 정상적으로 조회할 수 있습니다. 

.. _api-specification_device-information-modification:

단말 정보 수정
~~~~~~~~~~~~~~~~~~

.. _api-specification_director-information-modification:

To be added

관리자 정보 수정
~~~~~~~~~~~~~~~~~~

.. _api-specification_statistics:

To be added

이벤트 통계 API
------------------------

.. _api-specification_statistics1:

To be added

통계1
~~~~~~~~~~~~~~~~~~

.. _api-specification_statistics2:

To be added

통계2
~~~~~~~~~~~~~~~~~~

.. _api-specification_statistics3:

To be added

