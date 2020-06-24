.. |br| raw:: html

.. _get-started:

Get Started 
=================

본 섹션은 OVSE 서비스와 단말을 처음 사용하시는 사용자와 개발자를 위한 가이드 입니다. 


  : 사용자가 그대로 따라하면 동작하도록 설명 
  : 지금 device simulator git 에는 설정값이 없다. broker 주소, device serialNo 등 넣어둘 것 
  : 기등록된 가상의 회사와 단말 정보 포함 
  : 가상이벤트 알람 추가 

.. _get-started-SKOA:

SK Open API 등록 및 key 생성
-----------------------------------

서비스 사용을 위해서는 SK Open API 가입 및 key 생성이 필요합니다. 자세한 절차는 :ref:`4. 서비스 세부 절차<service-procedure>` 를 참고하시기 바랍니다. 

아래는 샘플로 사용가능한 등록정보 정보입니다. 

+--------------+-----------------------------+----------------------------------------------------------------+
| Key          | Description                 | 샘플값                                                         |
+==============+=============================+================================================================+
| API key      | OVSE API 호출을 위한 token  |                                                                |
+--------------+-----------------------------+----------------------------------------------------------------+
| username     | 단말 serialNo               | csx13123451234500001, csx13123451234500002                     |
+--------------+-----------------------------+----------------------------------------------------------------+
| password     | 단말 credentialsId          | csx13123451234500001, csx13123451234500002                     |
+--------------+-----------------------------+----------------------------------------------------------------+
| deviceType   | 디바이스 타입               | OVC-G                                                          | 
+--------------+-----------------------------+----------------------------------------------------------------+


.. _get-started-device-simulator:

Device Simulator 사용
-----------------------------------
OVS device simulator의 자세한 동작은 :ref:`9. Device Simulator<device-simulator>` 를 참고하시기 바랍니다. 

SK open API 포털 등록 이전에는 아래와 같은 절차로 시험할 수 있습니다. 

.. code-block:: none

    $ git clone https://github.com/ovseadmin/ovse_device_simulator.git 
    $ cd ovse_device_simulator/ovsClient_nodeJS
    $ npm install
    $ node device.js


.. _get-started-statistics:

API 사용 및 통계
-----------------------------------

.. code-block:: none

    // 내 회사 조회 
    $ curl --location --request GET http://223.39.121.188:8080/api/ovs/v1/company/me --header 'Content-Type: application/json' --header 'X-Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0Y29tcGFueTFAc2tvcGVuYXBpLmNvbSIsInNjb3BlcyI6WyJPVlNfQ09NUEFOWV9BRE1JTiJdLCJ1c2VySWQiOiJlNzQ0MzU5MC1iNWQ1LTExZWEtOGYwMC02NzMwZThlZjFhOWUiLCJlbmFibGVkIjp0cnVlLCJpc1B1YmxpYyI6ZmFsc2UsInRlbmFudElkIjoiZTczZDdlZDAtYjVkNS0xMWVhLThmMDAtNjczMGU4ZWYxYTllIiwiY3VzdG9tZXJJZCI6IjEzODE0MDAwLTFkZDItMTFiMi04MDgwLTgwODA4MDgwODA4MCIsInNlcnZpY2VUeXBlIjoic2tvYV90ZXN0Y29tcGFueTEiLCJpc3MiOiJUIFJlbW90RXllLlNLIFRlbGVjb20iLCJpYXQiOjE1OTI5NzQwOTAsImV4cCI6NDEwMjMyNjAwMH0.15p2NCfzAe41BleJhiMgPJKenM3wPbdk7MY3ohatGNRG8J3pZUSaILfXuAta62UsoBKFMDn7J0I3cjzo1epfgg' -d ''
    // 단말 조회 
    // 서비스 통계 조회



