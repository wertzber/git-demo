CQM
===

![Project Status][1]
[![Build Status][3]][2]
[![tests][5]][4]
[![coverage][7]][6]
  
[1]: https://ctvr-ci:8443/?label=Project%20Status&value=Live&color=blue ("Project Status")
[2]: http://teamcity/viewType.html?buildTypeId=Ams_Cqm_CqmMaster
[3]: https://ctvr-ci:8443/?build=Ams_Cqm_CqmMaster (Build Status)
[4]: http://teamcity/viewType.html?buildTypeId=Ams_Cqm_CqmMaster
[5]: https://ctvr-ci:8443/?build=Ams_Cqm_CqmMaster&type=test ("Tests")
[6]: http://teamcity/viewType.html?buildTypeId=Ams_Cqm_CqmMaster
[7]: https://ctvr-ci:8443/?build=Ams_Cqm_CqmMaster&type=cov ("Coverage")


API
---
TBD

QA Environment
--------------
- NGinx: qtvr-wng1005
- Jetty: qtvr-jet1004

Alpha Environment
-----------------
TBD

How to Build It
---------------
Prerequisies: Maven 3.X, JDK 1.8
Then run:
```sh
> git clone https://lpgithub.dev.lprnd.net/AMS/cqm.git
> cd cqm
> mvn -DskipTests package
```
Local build in windows, starts ums-mock in background. 
In case that ums-mock fails to start because of "addess already in used", stop java process via Windows Task Manager.

How to Run It
-------------
- Main class: com.liveperson.cqm.application.CqmApplication
- VM options: -Dlogback.configurationFile="(full path of cqm)/common/common-config/src/main/resources/logback.xml"
- Program arguments: server common/common-config/src/main/resources/cqm.yml
- Working directory: cmq (full path to root cqm directory)

How run UMS Mock (independently)
--------------------------------
- Main class: com.liveperson.cqm.umsmock.application.UmsMockApplication
- Program arguments: server mock-configuration.yml
- Working directory: cmq <project.home>/test-utils/test-mock-ums/src/main/resources

How run CQM with embedded UMS Mock
----------------------------------
CQM-Application-With-Mocks is an application almost identical to CQM-Application except that it embeds the UMS-Mock 
- Main class: com.liveperson.cqm.application.with.mocks.CqmApplicationWithMocks
- VM options: -Dlogback.configurationFile=application/application-cqm-with-mocks/src/main/resources/logback.xml
- Program arguments: server application/application-cqm-with-mocks/src/main/resources/cqm-with-mocks.yml
- Working directory: cmq (full path to root cqm directory)


Working with the API from Command Line
--------------------------------------
TBD

Tests - Debug test using H2
---------------------------
Added static method called:
startConsoleH2AndSleep(dbUrl, 3000000);
dbUrl - take from base(H2DaoBaseTest)
time - depend how long do you want to keep the thread.

CQM HealthCheck -
------------------------------------------
The Nginx server passes requests to the app if the app healthcheck returns 200 OK.
In Order to check the Cqm app HealthCheck, send the following GET rest request to the relevant jetty server:

- QA - http://qtvr-app08:8080/healthcheck
- localhost - http://localhost:9002/healthcheck

ReportingResource - Get Agent/AgentManager Message Statistics
------------------------------------------
- GET http request
- Header: key - Authorization, value -  Bearer BEARER_VALUE
- Agent Manager statistics: http://localhost:9002/api/messaging/rest/reporting/brands/BRAND_ID/msgstatistics
- Agent manager Statistics by AgentIds: http://localhost:9002/api/messaging/rest/reporting/brands/BRAND_ID/msgstatistics?agentIds=AGENT_ID_1,AGENT_ID_2
- Agent Statistics: http://localhost:9002/api/messaging/rest/reporting/brands/BRAND_ID/agents/AGENT_ID/msgstatistics

Sample headers:
header:Authorization, value: Bearer ac066e09eafea1388690550ad8839f3e107829a7bd978920036cfe0cb0c05f83

How to use Test Page:
---------------------
> Add your IP address to 'locations' file in nginx machine:
file location: /liveperson/code/server_openresty/nginx/conf/conf.d/locations.include
Add your IP address as following: allow "192.168.XX.XXX/32";
Reload service: "service LPnginx reload"

- Open test page (http://qtvr-wng1004)
- Select <b>version:</b> 2

- <b>Token</b> (server jwt): 
eyJraWQiOiIwMDAwMSIsInR5cCI6IkpXVCIsImFsZyI6IlJTMjU2In0.eyJhdF9oYXNoIjoiam5lcjkzNGJoZ3JlODkzNGludWRmZGZzZ3MiLCJzdWIiOiJhYmMtYWJjLWRlZi1kZWYtMTIzIiwiYXVkIjoidW1zIiwicm9sZSI6InNlcnZlciIsImF6cCI6ImlkcCIsImlzcyI6ImxpdmVwZXJzb24ubmV0IiwiZXhwIjoxNTQyODkxNTMzLCJpYXQiOjE0ODI0MTE1MzMsImp0aSI6IjZmODE5MmQxLTY3ZWItNGE0Ni1iZWEwLTc5YmQxMTU5NjExYyJ9.D_bRey7QWiaaUYFWpliULxdu0lMUNkbIyNOi_zT9lbEbdHpYftjM5FDIldy-ada8cYqsXyFkV5UXJErHMnWOGwswIRfcSnm9E2NJbLtV0YRoi5iu2_etN9-kyTjT_DV7FZNNmQrfscQ2Up7ZJEgGf30OrmJgbe8AHeqkSQBgWEE

- <b>Connect URL</b>:
ws://tlvcivoice1/ws_cqm/cqm/messaging/eyJraWQiOiIwMDAwMSIsInR5cCI6IkpXVCIsImFsZyI6IlJTMjU2In0.eyJhdF9oYXNoIjoiam5lcjkzNGJoZ3JlODkzNGludWRmZGZzZ3MiLCJzdWIiOiJhYmMtYWJjLWRlZi1kZWYtMTIzIiwiYXVkIjoidW1zIiwicm9sZSI6InNlcnZlciIsImF6cCI6ImlkcCIsImlzcyI6ImxpdmVwZXJzb24ubmV0IiwiZXhwIjoxNTQyODkxNTMzLCJpYXQiOjE0ODI0MTE1MzMsImp0aSI6IjZmODE5MmQxLTY3ZWItNGE0Ni1iZWEwLTc5YmQxMTU5NjExYyJ9.D_bRey7QWiaaUYFWpliULxdu0lMUNkbIyNOi_zT9lbEbdHpYftjM5FDIldy-ada8cYqsXyFkV5UXJErHMnWOGwswIRfcSnm9E2NJbLtV0YRoi5iu2_etN9-kyTjT_DV7FZNNmQrfscQ2Up7ZJEgGf30OrmJgbe8AHeqkSQBgWEE?v=2

- <b>SubscribeExConversations API</b>:
{
  "kind": "req",
  "id": 1,
  "type": ".ams.aam.SubscribeExConversations",
  "body": {
    "maxLastUpdatedTime": null,
    "minLastUpdatedTime": null,
    "consumerId": "",
    "brandId": null,
    "maxETTR": null,
    "agentIds": [
      ""
    ],
    "convState": [
      "OPEN",
      "LOCKED",
      "CLOSE"
    ]
  }}
