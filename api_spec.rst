API Specification 
=======================================

About API Specification
--------------------------------

-OVS API Spec 소개
OVS 제공하는 API 종류 및 프로토콜 Spec에 대한 설명


REST API
-----------


OVS northbound는 다음과 같은 REST API를 제공합니다. 상세한 내용은 `OVS Document <https://ovs-document.readthedocs.io/en/latest/index.html>`__ 내용을 참고하시기 바랍니다.

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify


=========  ===================================================  ===========  =====================================================
 구분       |  설명                                              | Method    | URL
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
            | -  특정 회사 모든 단말의 기간별 이벤트 통계        | GET       | /api/ovs/v1/company/{companyId}/statistics/event
            | -  특정 회사 모든 단말에 메시지 전달               | POST      | /api/ovs/v1/company/{companyId}/message
            |                                                    |  
---------  ---------------------------------------------------  -----------  -----------------------------------------------------
 Device     | -  단말 등록                                       | POST      | /api/ovs/v1/device
            | -  SerialNo로 단말 조회                            | GET    
            | -  단말 정보 조회                                  | GET    
            | -  단말 정보 수정                                  | PUT    
            | -  단말 삭제                                       | DELETE 
            | -  전체 단말 리스트 조회                           | GET    
            | -  특정 Service Type에 속하는 단말의 수 조회       | GET    
            | -  소유한 전체 단말 수	                         | GET    
            | -  특정 단말 기간별 이벤트 통계                    | GET    
            | -  단말별 메시지 전달                              | POST   
            |                                                    |  
=========  ===================================================  ===========  =====================================================


REST API20
-----------

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify



=======  ==============================================   =========  ==
구분     설명                                              Method     a
=======  ==============================================   =========  ==
Auth     -  로그인                                         POST       a

         -  토큰 갱신                                      POST       a 

         -  비밀번호 변경                                  POST       a 

         -  임시비밀번호 변경                              POST       a 

         -  사용자 이메일 검색                             POST       a 
 
-------  ----------------------------------------------   ---------  --
Company  -  회사 정보 등록                                 POST       a 
         -  회사 정보 조회                                 GET        a 
         -  회사 정보 수정                                 PUT        a 
         -  회사 삭제                                      DELETE     a 
         -  내 회사 정보 조회                              GET        a 
         -  특정 Service Type에 속하는 회사 리스트 조회    GET        a 
         -  등록된 전체 회사 수 조회                       GET        a 
         -  특정 Service Type에 속하는 회사의 수 조회      GET        a 
         -  회사 관리자 등록                               POST       a 
         -  회사 관리자 수정                               PUT        a 
         -  회사 관리자 삭제                               DELETE     a 
         -  회사 관리자 리스트 조회                        GET        a 
         -  소유한 단말 리스트 조회                        GET        a 
         -  특정 회사 모든 단말의 기간별 이벤트 통계       GET        a 
         -  특정 회사 모든 단말에 메시지 전달              POST       a 
 
-------  ----------------------------------------------   ---------  --
Device   -  단말 등록                                      POST       a
         -  SerialNo로 단말 조회                           GET        a 
         -  단말 정보 조회                                 GET        a 
         -  단말 정보 수정                                 PUT        a 
         -  단말 삭제                                      DELETE     a 
         -  전체 단말 리스트 조회                          GET        a 
         -  특정 Service Type에 속하는 단말의 수 조회      GET        a 
         -  소유한 전체 단말 수                            GET        a 
         -  특정 단말 기간별 이벤트 통계                   GET        a 
         -  단말별 메시지 전달                             POST       a 
 
=======  ==============================================   =========  ==


REST API21
------------

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+


REST API22
------------

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
-----  -----  -------
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======


REST API3
------------

.. rst-class:: table-width-fix
.. rst-class:: text-align-justify

+----------+----------------------------------------------------+-------+
| 구분     | APIs                                               |Method |
+==========+====================================================+=======+
| Auth     | -  로그인                                          |POST   |
|          | -  토큰 갱신                                       |POST   |
|          |                                                    |       |
|          | -  비밀번호 변경                                   |POST   |
|          |                                                    |       |
|          | -  임시비밀번호 변경                               |POST   |
|          |                                                    |       |
|          | -  사용자 이메일 검색                              |POST   |
+----------+----------------------------------------------------+-------+
| Company  | -  회사 정보 등록                                  |POST   |
|          |                                                    |       |
|          | -  회사 정보 조회                                  |GET    |
|          |                                                    |       |
|          | -  회사 정보 수정                                  |PUT    |
|          |                                                    |       |
|          | -  회사 삭제                                       |DELETE |
|          |                                                    |       |
|          | -  내 회사 정보 조회                               |GET    |
|          |                                                    |       |
|          | -  특정 Service Type에 속하는 회사 리스트 조회     |GET    |
|          |                                                    |       |
|          | -  등록된 전체 회사 수 조회                        |GET    |
|          |                                                    |       |
|          | -  특정 Service Type에 속하는 회사의 수 조회       |GET    |
|          |                                                    |       |
|          | -  회사 관리자 등록                                |POST   |
|          |                                                    |       |
|          | -  회사 관리자 수정                                |PUT    |
|          |                                                    |       |
|          | -  회사 관리자 삭제                                |DELETE |
|          |                                                    |       |
|          | -  회사 관리자 리스트 조회                         |GET    |
|          |                                                    |       |
|          | -  소유한 단말 리스트 조회                         |GET    |
|          |                                                    |       |
|          | -  특정 회사 모든 단말의 기간별 이벤트 통계        |GET    |
|          |                                                    |       |
|          | -  특정 회사 모든 단말에 메시지 전달               |POST   |
|          |                                                    |       |
+----------+----------------------------------------------------+-------+
| Device   | -  단말 등록                                       |POST   |
|          |                                                    |       |
|          | -  SerialNo로 단말 조회                            |GET    |
|          |                                                    |       |
|          | -  단말 정보 조회                                  |GET    |
|          |                                                    |       |
|          | -  단말 정보 수정                                  |PUT    |
|          |                                                    |       |
|          | -  단말 삭제                                       |DELETE |
|          |                                                    |       |
|          | -  전체 단말 리스트 조회                           |GET    |
|          |                                                    |       |
|          | -  특정 Service Type에 속하는 단말의 수 조회       |GET    |
|          |                                                    |       |
|          | -  소유한 전체 단말 수	                        |GET    |
|          |                                                    |       |
|          | -  특정 단말 기간별 이벤트 통계                    |GET    |
|          |                                                    |       |
|          | -  단말별 메시지 전달                              |POST   |
|          |                                                    |       |
+----------+----------------------------------------------------+-------+


Entity Model
------------------------

.. rst-class:: text-align-justify

REST API에서는 다음과 같은 Entity들이 정의되어 있으며, 세부 데이터 모델 내용은 `OVS Document <https://ovs-document.readthedocs.io/en/latest/index.html>`__ 내용을 참고하시기 바랍니다.

-  Company

-  Device




