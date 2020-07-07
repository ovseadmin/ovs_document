.. |br| raw:: html

.. _get-started:

Get Started 
=================

본 섹션은 OVSE 서비스와 단말을 처음 사용하시는 사용자와 개발자를 위한 가이드 입니다. 

.. _get-started-SKOA:

SK open API 등록 및 token 생성
-----------------------------------

서비스 사용을 위해서는 SK open API 가입 및 toekn 생성이 필요합니다. 자세한 절차는 :ref:`4. 서비스 세부 절차<service-procedure>` 를 참고하시기 바랍니다. 

아래는 기등록되어 샘플로 사용가능한 계정 및 단말 정보이며, SK open API 포털의 OVSE API상품을 이용하여 사용자가 직접 생성할 수 있습니다. 

+--------------+-----------------------------+-------------------------------------------------------------------+
| Key          | Description                 | 샘플값                                                            |
+==============+=============================+===================================================================+
| API token    | OVSE API 호출을 위한 token  | eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLm   |
|              |                             | NvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlN   |
|              |                             | zQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0   |
|              |                             | cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0   |
|              |                             | xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLT   |
|              |                             | FkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvY   |
|              |                             | V90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJp   |
|              |                             | YXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiM   |
|              |                             | gPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1e   |
|              |                             | pfgg                                                              |
+--------------+-----------------------------+-------------------------------------------------------------------+
| username     | 단말 serialNo               | csx13123451234500001,                                             |
|              |                             | csx13123451234500002                                              |
+--------------+-----------------------------+-------------------------------------------------------------------+
| password     | 단말 credentialsId          | csx13123451234500001,                                             |
|              |                             | csx13123451234500002                                              |
+--------------+-----------------------------+-------------------------------------------------------------------+
| companyId    | 회사 ID                     | e73d7ed0-b5d5-11ea-8f00-6730e8ef1a9e                              |
+--------------+-----------------------------+-------------------------------------------------------------------+
| deviceType   | 디바이스 타입               | OVC-G                                                             | 
+--------------+-----------------------------+-------------------------------------------------------------------+

.. _get-started-device-simulator:

Device Simulator 사용
-----------------------------------
OVS device simulator의 자세한 동작은 :ref:`9. Device Simulator<device-simulator>` 를 참고하시기 바랍니다. 

.. code-block:: none

    $ git clone https://github.com/ovseadmin/ovse_device_simulator.git 
    $ cd ovse_device_simulator/ovsClient_nodeJS
    $ npm install express pem
    $ node device.js


.. _get-started-statistics:

API 사용 및 통계
-----------------------------------

API 사용은 postman 과 같은 API 시험툴이나 shell 상에서 curl을 이용하여 확인할 수 있습니다. 
SK open API 포털에서 샘플 API 호출가능하며, 아래의 샘플 값으로도 시험할 수 있습니다. 

.. code-block:: none

    // 내 회사 정보 조회 
    $ curl --location --request GET http://{SK_Open_API_Portal}/api/ovs/v1/company/me --header 'Content-Type: application/json' --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLmNvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlNzQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLTFkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvYV90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJpYXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiMgPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1epfgg' -d ''
    // 단말 수 조회
    $ curl --location --request GET http://{SK_Open_API_Portal}/api/ovs/v1/devices/owned/cnt --header 'Content-Type: application/json' --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLmNvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlNzQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLTFkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvYV90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJpYXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiMgPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1epfgg' -d '' 
    // 단말 정보 조회
    $ curl --location --request GET http://{SK_Open_API_Portal}/api/ovs/v1/devices?limit=10 --header 'Content-Type: application/json' --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLmNvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlNzQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLTFkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvYV90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJpYXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiMgPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1epfgg' -d '' 
    // 서비스 통계 조회-단말별
    $ curl --location --request GET http://{SK_Open_API_Portal}/api/ovs/v1/device/csx13123451234500001/statistics/event --header 'Content-Type: application/json' --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLmNvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlNzQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLTFkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvYV90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJpYXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiMgPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1epfgg' -d '' 
    // 서비스 통계 조회-회사전체
    $ curl --location --request GET http://{SK_Open_API_Portal}/api/ovs/v1/company/e73d7ed0-b5d5-11ea-8f00-6730e8ef1a9e/statistics/event --header 'Content-Type: application/json' --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLmNvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlNzQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLTFkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvYV90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJpYXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiMgPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1epfgg' -d '' 





