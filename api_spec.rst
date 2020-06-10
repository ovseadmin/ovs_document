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


=========  ===================================================  ===========  =====================================================
| 구분      |  설명                                              | Method    | URL                                                
=========  ===================================================  ===========  =====================================================
 Company    | -  회사 정보 조회                                  | GET       | /api/ovs/v1/company/{companyId}                    
            | -  내 회사 정보 조회                               | GET       | /api/ovs/v1/company/me                             
            | -  특정 회사 모든 단말에 메시지 전달               | POST      | /api/ovs/v1/company/{companyId}/message            
---------  ---------------------------------------------------  -----------  -----------------------------------------------------
 Device     | -  단말 등록                                       | POST      | /api/ovs/v1/device                                 
            | -  단말 정보 조회                                  | GET       | /api/ovs/v1/device/{deviceId}                      
            | -  단말 정보 수정                                  | PUT       | /api/ovs/v1/device/{deviceId}                      
            | -  단말 삭제                                       | DELETE    | /api/ovs/v1/device/{deviceId}                      
            | -  전체 단말 리스트 조회                           | GET       | /api/ovs/v1/devices                                
            | -  특정 Service Type에 속하는 단말의 수 조회       | GET       | /api/ovs/v1/devices/cnt                            
            | -  소유한 전체 단말 수	                         | GET       | /api/ovs/v1/devices/owned/cnt                      
            | -  단말별 메시지 전달                              | POST      | /api/ovs/v1/device/{deviceId}/message              
---------  ---------------------------------------------------  -----------  -----------------------------------------------------
 Stats      | -  특정 회사 모든 단말의 기간별 이벤트 통계        | GET       | /api/ovs/v1/company/{companyId}/statistics/event   
            | -  특정 단말 기간별 이벤트 통계                    | GET       | /api/ovs/v1/device/{deviceId}/statistics/event     
=========  ===================================================  ===========  =====================================================
.. rst-class:: text-align-justify

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


.. _api-specification_my-company-information:

내 회사 정보 조회
~~~~~~~~~~~~~~~~~~

.. rst-class:: text-align-justify

내가 속한 회사의 Company ID를 조회하는 API 입니다. 

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

.. rst-class:: text-align-justify

나의 계정정보와 내가 속한 회사의 Company ID를 수정하는 API 입니다. 회사 admin만 사용가능합니다. (director는 사용불가) 

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------+---------------------------------------------------+
| **PUT**   | `/api/ovs/v1/company/{companyId} <https://TBD>`__  |
+------------+---------------------------------------------------+

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

