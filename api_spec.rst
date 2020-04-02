.. |br| raw:: html

.. _api-specification:

Northbound API Specification 
=======================================

About Northbound API Specification
--------------------------------

OVSE 플랫폼은 인증, 회사 및 단말 관리, 통계를 위한 Northbound API를 제공합니다. 


REST API
-----------

OVSE northbound는 다음과 같은 REST API를 제공합니다. 상세한 내용은 `OVSE Document <https://ovs-document.readthedocs.io/en/latest/index.html>`__ 내용을 참고하시기 바랍니다.

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify


=========  ===================================================  ===========  =====================================================
| 구분      |  설명                                              | Method    | URL
=========  ===================================================  ===========  =====================================================
 Auth       | -  로그인                                          | POST      | /api/auth/login
            | -  토큰 갱신                                       | POST      | /api/auth/token
            | -  비밀번호 변경                                   | POST      | /api/auth/changePassword
            | -  임시비밀번호 변경                               | POST      | /api/auth/resetPasswordByEmail
            | -  사용자 이메일 검색                              | POST      | /api/auth/findUserEmail
---------  ---------------------------------------------------  -----------  -----------------------------------------------------
 Company    | -  회사 정보 등록                                  | POST      | /api/ovs/v1/company	
            | -  회사 정보 조회                                  | GET       | /api/ovs/v1/company/{companyId}
            | -  회사 정보 수정                                  | PUT       | /api/ovs/v1/company/{companyId} 
            | -  회사 삭제                                       | DELETE    | /api/ovs/v1/company/{companyId}	 
            | -  내 회사 정보 조회                               | GET       | /api/ovs/v1/company/me 
            | -  특정 Service Type에 속하는 회사 리스트 조회     | GET       | /api/ovs/v1/companies 
            | -  등록된 전체 회사 수 조회                        | GET       | /api/ovs/v1/companies/all
            | -  특정 Service Type에 속하는 회사의 수 조회       | GET       | /api/ovs/v1/companies/cnt
            | -  회사 관리자 등록                                | POST      | /api/ovs/v1/company/{companyId}/admin
            | -  회사 관리자 수정                                | PUT       | /api/ovs/v1/company/{companyId}/admin/{adminId}
            | -  회사 관리자 삭제                                | DELETE    | /api/ovs/v1/company/{companyId}/admin/{adminId}
            | -  회사 관리자 리스트 조회                         | GET       | /api/ovs/v1/company/{companyId}/admins
            | -  소유한 단말 리스트 조회                         | GET       | /api/ovs/v1/company/{companyId}/devices
            | -  특정 회사 모든 단말에 메시지 전달               | POST      | /api/ovs/v1/company/{companyId}/message
---------  ---------------------------------------------------  -----------  -----------------------------------------------------
 Device     | -  단말 등록                                       | POST      | /api/ovs/v1/device
            | -  SerialNo로 단말 조회                            | GET       | /api/ovs/v1/device
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


Entity Model and Registration
------------------------

.. rst-class:: text-align-justify

REST API에서는 다음과 같은 Entity들이 정의되어 있으며, 세부 데이터 모델과 등록 방법은 
:ref:`5. 구성요소(Entity) 등록 <entity-registration>`__ 내용을 참고하시기 바랍니다.

-  Company

-  Device

-  Director


Entity Lookup
------------------------










