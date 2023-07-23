# Using IBM Environmental Intelligence Suite (EIS) from SAP Business Technology Platform (BTP)

In this tutorial you will deploy an API connector for IBM EIS using the [Open Connectors](https://help.sap.com/docs/OPEN_CONNECTORS) capability of [SAP Integration Suite](https://www.sap.com/canada/products/technology-platform/integration-suite.html). You will then run a weather application built using the SAP Build App framework that uses the API served by SAP Integration Suite. Finally, you will use SAP Integration Suite to collect API usage information for the IBM EIS API.

Connectors built using the [Open Connectors](https://help.sap.com/docs/OPEN_CONNECTORS) provide an abstraction of the EIS API and provide them in a standard format to developers. Multiple instances of a connector can be instantiated, allowing a single EIS API key to be shared between multiple lines of business such that usage to be tracked per line of business.

There are three parts to the tutorial.

1. **Deploy API Connectors with SAP Integration Suite.** You will deploy an API connector for IBM EIS using SAP Integration Suite. Instances of this connector provide a REST API to obtain weather information. 
2. **Run an Example Weather Application.** The example weather application is built using the SAP Build Apps framework. It calls the API connector to get weather information.
3. **Collect API Usage Metrics.** You will use the SAP Open Connectors API to get usage information about individual connector instances.

In order to perform the tutorials you will need to have an EIS API key.

- IBMers can request EIS access [here](https://eistrialrequest.ideas.aha.io/portal_session/new).
- Non-IBMers can request a [30-day free trial](https://www.ibm.com/account/reg/us-en/signup?formid=urx-51911&_gl=1*9cen1r*_ga*NzczNTIyMDM3LjE2ODkxNzIwNjE.*_ga_FYECCCS21D*MTY4OTUzODI1My4yMi4xLjE2ODk1Mzg2MjEuMC4wLjA).

In addition, you will need access to the SAP Business Technology Platform (BTP). For this tutorial you can use a [trial account](https://developers.sap.com/tutorials/hcp-create-trial-account.html).

## Reference Architecture

![image](https://github.com/ibm-build-lab/ibm-eis-sap-btp/assets/49033907/20289270-98b9-4820-b4b6-ab83345eeebd)

## Part 1: Deploy API Connectors with SAP Integration Suite 

## Prerequisites

Install the SAP Integation Suite. You can use this [tutorial](https://developers.sap.com/tutorials/cp-starter-isuite-onboard-subscribe.html) to install it with a trial account. Once installed, click on the **Manage Capabilities** tile. This opens a window in which you can enable the **Open Connectors** capability. 

Click on **Create Connectors** to enter the Open Connectors GUI.

![image](https://media.github.ibm.com/user/24824/files/ee39bd02-d7e1-4005-8d7d-6477ef84bf3a)

**Tip:** You may occassionally see a message when loading the Open Connectors GUI that the session has expired. In such cases, clear cookies and site data in your browser.

### Step 1: Import the EIS connector

Inside the Open Connectors GUI, click on **Build New Connector**.

![image](https://media.github.ibm.com/user/24824/files/02230cff-6800-4c65-aaf7-2e5241e37032)

Click **Import**

![image](https://media.github.ibm.com/user/24824/files/2b8563d8-9e44-47d8-b4a2-43c9867ea7f2)

Import connectors from the cloned repo. These connectors are JSON files. For example, the connector for the EIS Standard Data Package uses the file:
```
eis-connector-for-sap/connector/eis-connector.json 
```

![image](https://media.github.ibm.com/user/24824/files/ecfb811e-ef7c-41f9-8cd2-7eabeee6c9a8)

Once completed you will be able to see the newly added private connectors.

![image](https://media.github.ibm.com/user/24824/files/94e90b8f-56b9-4865-89f6-c69465ccf4ab)

### Step 2: Create a Connector Instance

In order to use a connector, you must first create an instance of the connector. You will need an EIS API key, which will be associated with the instance. One or more instances can share the same API key, allowing you to share the key between different lines of business. This API key should have been sent to you by email when you registered for the EIS service.

Click on the **Instances** tab, and then click **Create Instance**.

![image](https://media.github.ibm.com/user/24824/files/851e46c9-ce54-46cd-9a47-8bec65272ed0)

Search for the newly added connector to add. Fill in the **Name** and **apiKey** fields, and then click **Create Instance**.

![image](https://media.github.ibm.com/user/24824/files/13175c22-f8d5-4331-bfa4-9a3e48a497be)

You have now created an instance of the connector, which can be viewed from the **Instances** tab.

### Step 3: Test the Connector

Go to the **Instances** tab and click **API Docs** for the instance you want to test.

![image](https://media.github.ibm.com/user/24824/files/04d86925-bdbd-4f51-9050-12d2c7cbb998)

From here, you can test the APIs exposed by the connector.

![image](https://media.github.ibm.com/user/24824/files/048c54bd-3bb8-486c-9dc7-d05ee192c28d)

You can use the EIS API documentation to help fill out fields in the API forms. 
- [Standard Data Package APIs](https://docs.google.com/document/d/14OK6NG5GRwezb6-5C1vQJoRdStrGnXUiXBDCmQP9T9s/edit)
- [Premium Data Package APIs](https://docs.google.com/document/d/16TJJVFvNxqmWR1T6wmpnHuV3XZIZ0krJVcJRprs85tc/edit)
- [Greenhouse Gas Emissions APIs](https://www.ibm.com/docs/en/environmental-intel-suite?topic=components-ghg-emissions-apis)

Note that the curl calls generated from this form can be run from any application with internet access. They are not restricted to run in a particular network.

## Part 2: Run an Example Weather Application

The example weather application has been developed using [SAP Build Apps](https://learning.sap.com/products/sap-build/build-apps). This is a framework for writing low-code/no-code web or mobile applications.

This application uses the EIS connector deployed above to fetch weather information from EIS.

### Prerequisites

Set up an instance of the EIS connector for the Standard Data Platform as shown in the previous section. Then, you need an instance of SAP Build Apps. 
- If you have a pay-as-you-go account you can follow the [Booster](https://help.sap.com/docs/build-apps/service-guide/booster-automatic-configuration) to set up the service. Otherwise,
- Use the free [SAP Build Apps Sandbox](https://groups.community.sap.com/t5/sap-builders-blog-posts/announcing-the-sap-build-apps-sandbox/ba-p/128821).

### Step 1: Install the weather application

Open SAP Build Apps. From the Lobby page, click **Import**. 

![image](https://media.github.ibm.com/user/24824/files/10b19438-c35b-4b81-b8dc-41aec721df1d)

Import the SAP Build Apps file that is stored in the repository:
```
eis-connector-for-sap/weatherapp/WeatherApp.mtar
```

### Step 2: Preview the application

Click on **WeatherApp**. This will open the application development GUI in a new window.

![image](https://media.github.ibm.com/user/24824/files/b7954af8-56c0-403d-9abe-45bd8cb503d5)

You can have a look around with this GUI window to see how the application is built, or to modify it. When you are ready, click **LAUNCH** and then follow the directions to open a preview of the application, either on the web or on a mobile device.

![image](https://media.github.ibm.com/user/24824/files/3676ad7c-167b-4a39-ac30-d9bd20a7c646)

### Step 3: Set the credentials in the application

When you first open the application, you will be on the Settings page. You need to fill the **Authentication Header** text box. This will be sent in headers of messages made to the EIS connector. 

![image](https://media.github.ibm.com/user/24824/files/b50729a0-87f0-4b3c-b079-c0c5ed5f6557)

To get the correct value for this open the SAP Integration Suite and, for the EIS connector instance you want to use, copy the value from the **Authorization** field from one of the endpoints.

![image](https://media.github.ibm.com/user/24824/files/a528144d-e74a-49f2-ac55-b8acff9d3f06)

At this point, the application is ready to use. 

## Part 3: Collect API Usage Metrics

Note that a single IBM EIS API key can be used by multiple connector instances. This can be useful for sharing access to the weather APIs across lines-of-business. In this section, you will see how to get the weather API usage for a single connector instance.

In order to track API usage, then Open Connectors GUI provides an **Activity** page.

![image](https://media.github.ibm.com/user/24824/files/d96b0d98-648d-4adc-b61f-a8d2fa348fc8)

Under the **API Logs** tab, you can filter on a particular connector instance to see all of the API calls made to that instance. 

However, note that this page includes all the calls made to the connector instance, including both successful and unsuccessful calls, as well as calls to the Open Connectors instance that do not generate a call to the IBM EIS service. These may be for example, calls to get the API documentation or usage metrics for the connector. 

Ideally, we would like to get the successful calls made to IBM EIS service itself. For this, we will employ the **usage** API call. The documentation for this call is available by clicking on **API Docs** at the top right of the window, and then clicking on the **Usage** tab.

![image](https://media.github.ibm.com/user/24824/files/6bc844fa-1957-4c1e-9930-b3b554ff6818)

Use the following command to get a JSON list of the successful calls made to a connector instance that involve calling IBM EIS. 
```
curl "https://api.openconnectors.trial.us10.ext.hana.ondemand.com/elements/api-v2/usage?status=SUCCESS&instanceIds%5B%5D=${INSTANCE_ID}" -H  "accept: application/json" -H  "Authorization: ${AUTH}" | jq '.[] | select(.vendor_resource != null)'
```
Where:
- `INSTANCE_ID` is the ID number of the connector instance, e.g. `4945151`.
- `AUTH` is the **Authorization** string that can be found by testing the Usage API in the GUI.

The `curl` command will return a list of calls made to the specified connector instance in JSON format. By filtering for only those elements with a `vendor_resource` field specified, we will get those calls that in turn triggered a call to EIS. The value for the field gives the EIS endpoint that is invoked.

Optionally, if you want to further restrict the range of records to particular dates, you can use the following command:
```
curl "https://api.openconnectors.trial.us10.ext.hana.ondemand.com/elements/api-v2/usage?status=SUCCESS&instanceIds%5B%5D=${INSTANCE_ID}&from=${START_TIME}&to=${END_TIME}" -H  "accept: application/json" -H  "Authorization: ${AUTH}" | jq '.[] | select(.vendor_resource != null)' 
```
Where:
- `START_TIME` is specified in ISO 8601 format, e.g. '2023-04-14T00:00:00-04:00'. An unspecified time zone defaults to UTC.
- `END_TIME` is specified in ISO 8601 format, e.g. '2023-05-14T00:00:00-04:00'. An unspecified time zone defaults to UTC.

It is straightforward to get the number of calls made to EIS from this returned JSON.

Below is an example of one of the records returned from this call.

```
{
  "request_status": "SUCCESS",
  "is_ce_mapped_resource": true,
  "user_name": "p2006779681@290865.70739.generated",
  "request_method": "GET",
  "process_time": 204,
  "traffic_id": "6481fe21e4b02d14bbf23c01",
  "vendor_process_time": 161,
  "company_external_id": "NA",
  "account_name": "CF_47db4bd7-a706-405b-91f1-b3840db5fa00",
  "vendor_request_method": "GET",
  "instance_tags": "EIS instance",
  "traffic_date": "2023-06-08T16:13:21.745Z",
  "request_ip": "24.50.184.192",
  "headers": "sec-fetch-mode: cors, x-request-id: ac3b3fa9-af1c-9ec1-8c9b-e4ef3c2e9a5f, referer: https://my.openconnectors.trial.us10.ext.hana.ondemand.com/, x-datadog-sampling-priority: 1, sec-fetch-site: same-site, x-forwarded-proto: http, origin: https://my.openconnectors.trial.us10.ext.hana.ondemand.com, x-forwarded-port: 443, x-datadog-trace-id: 5551818261594292888, x-envoy-attempt-count: 1, x-datadog-parent-id: 6454959658176001483, x-envoy-external-address: 10.129.10.119, x-forwarded-host: api.openconnectors.trial.us10.ext.hana.ondemand.com, x-forwarded-client-cert: By=spiffe://cluster.local/ns/cloudelements/sa/stage0-soba;Hash=adb1330183b9c0bac2320a61aa7c100f0b309767068a56046b1bee5a694e38ec;Subject=\"\";URI=spiffe://cluster.local/ns/istio-system/sa/istio-ingressgateway-1-14-5-service-account, sec-fetch-dest: empty",
  "instance_name": "EIS instance",
  "company_id": 70739,
  "response_status": 200,
  "vendor_resource": "/v3/wx/forecast/daily/10day",
  "element_tag": "EIS instance",
  "element_resource": "10day",
  "element_key": "ibmenvironmentalintelligencesuite(eis)",
  "request_uri": "/elements/api-v2/forecast/daily/10day",
  "account_id": 290865,
  "instance_id": 4916312,
  "platform_process_time": 43,
  "provision_name": "EIS instance",
  "user_id": 1164457,
  "company_name": "2a67083b-2dcf-4634-aaf2-ec36361b732b",
  "element_api": "avadaKedavra",
  "company_external_name": "NA",
  "request_id": "6481fe21e4b02d14bbf23c01",
  "element_name": "IBM Environmental Intelligence Suite (EIS)"
}
```

## Conclusion

In this tutorial, you have deployed an API using SAP Open Connectors. You have run a weather application that was created using the SAP Build Apps framework. This weather application uses an instance of the API connector to access weather info. Finally, you have employed the SAP Open Connectors API to get usage info about the weather API.
