# GetDeviceShadow {#reference_s4y_zrz_wdb .reference}

You can call this operation to obtain information about the shadow of the specified device.

## Request parameters {#section_nwb_hjb_ydb .section}

|Name|Type|Required|Request parameters|
|:---|:---|:-------|:-----------------|
|Action|String|Yes|The operation that you want to perform. Set the value to GetDeviceShadow.|
|ProductKey|String|Yes|The key of the product that the device belongs to.|
|DeviceName|String|Yes|Name of the device.|

## Response parameters {#section_sg4_mjb_ydb .section}

|Name|Type|Description|
|:---|:---|:----------|
|RequestId|String|The GUID generated by Alibaba Cloud for the request.|
|Success|Boolean|Indicates whether the call is successful. A value of true indicates that the call is successful. A value of false indicates that the call has failed. |
|ErrorMessage|String|The error message returned when the call fails.|
|ShawdowMessage|String| The shadow information returned when the call is successful.

 **Note:** The structure of the shadow information varies depending on the status of the device. For more information, see [Configure device shadows](../../../../intl.en-US/User Guide/Develop devices/Device shadows/Device shadows.md#).

 |

## Examples {#section_zrq_kkb_ydb .section}

**Request example**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=GetDeviceShadow
         &ProductKey=...
         &DeviceName=...
         &common_parameters
```

**Response example**

-   JSON format

    ```
    {
          "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
          "Success":true,
          "ShadowMessage":{...}
      }
    ```

-   XML format

    ```
    <? xml version='1.0' encoding='UTF-8'? >
      <GetDeviceShadowResponse>
          <RequestId>BB71E443-4447-4024-A000-EDE09922891E</RequestId>
          <Success>true</Success>
          <ShadowMessage>{...}</ShadowMessage>
      </GetDeviceShadowResponse>
    ```

