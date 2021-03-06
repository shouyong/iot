# 自定义Topic通信 {#concept_enx_wbv_hfb .concept}

Android SDK 提供了与云端长链接的基础能力接口，用户可以直接使用这些接口完成自定义 Topic 相关的功能。提供的基础能力包括：发布、订阅、取消订阅、RRPC、订阅下行。如果不想使用物模型，可以通过这部分接口实现云端数据的上下行。

## 上行接口请求 {#section_mbw_1cv_hfb .section}

调用上行请求接口，SDK封装了上行发布、订阅和取消订阅等接口。

```
/**
 * 发布
 *
 * @param request  发布请求
 * @param listener 监听器
 */
void publish(ARequest request, IConnectSendListener listener);

/**
 * 订阅
 *
 * @param request  订阅请求
 * @param listener 监听器
 */
void subscribe(ARequest request, IConnectSubscribeListener listener);

/**
 * 取消订阅
 *
 * @param request  取消订阅请求
 * @param listener 监听器
 */
void unsubscribe(ARequest request, IConnectUnscribeListener listener);
```

调用示例：

```
// 发布
MqttPublishRequest request = new MqttPublishRequest();
request.isRPC = false;
request.topic = topic;
request.payloadObj = data;
LinkKit.getInstance().publish(request, new IConnectSendListener() {
    @Override
    public void onResponse(ARequest aRequest, AResponse aResponse) {
        // 发布成功
    }

    @Override
    public void onFailure(ARequest aRequest, AError aError) {
        // 发布失败
    }
});
// 订阅
MqttSubscribeRequest subscribeRequest = new MqttSubscribeRequest();
subscribeRequest.topic = subTopic;
subscribeRequest.isSubscribe = true;
LinkKit.getInstance().subscribe(subscribeRequest, new IConnectSubscribeListener() {
    @Override
    public void onSuccess() {
        // 订阅成功
    }

    @Override
    public void onFailure(AError aError) {
        // 订阅失败
    }
});
// 取消订阅
MqttSubscribeRequest unsubRequest = new MqttSubscribeRequest();
unsubRequest.topic = unSubTopic;
unsubRequest.isSubscribe = false;
LinkKit.getInstance().unsubscribe(unsubRequest, new IConnectUnscribeListener() {
    @Override
    public void onSuccess() {
        // 取消订阅成功
    }

    @Override
    public void onFailure(AError aError) {
        // 取消订阅失败
    }
});
```

## 下行数据监听 {#section_gc4_wsr_z2b .section}

下行数据监听可以通过 RRPC 方式或者注册一个下行数据监听器实现。

```
/**
 * RRPC 接口
 *
 * @param request  RRPC 请求
 * @param listener 监听器
 */
void subscribeRRPC(ARequest request, IConnectRrpcListener listener);

/**
 * 注册下行数据监听器
 *
 * @param listener 监听器
 */
void registerOnPushListener(IConnectNotifyListener listener);

/**
 * 取消注册下行监听器
 *
 * @param listener 监听器
 */
void unRegisterOnPushListener(IConnectNotifyListener listener);
```

调用示例（数据监听）：

```
// 下行数据监听
IConnectNotifyListener onPushListener = new IConnectNotifyListener() {
    @Override
    public void onNotify(String connectId, String topic, AMessage aMessage) {
        // 下行数据通知
    }

    @Override
    public boolean shouldHandle(String connectId, String topic) {
        return true; // 是否需要处理 该 topic
    }

    @Override
    public void onConnectStateChange(String connectId, ConnectState connectState) {
        // 连接状态变化
    }
};
// 注册
LinkKit.getInstance().registerOnPushListener(onPushListener);
// 取消注册
LinkKit.getInstance().unRegisterOnPushListener(onPushListener);
```

调用示例（RRPC ）：

```
final MqttRrpcRegisterRequest registerRequest = new MqttRrpcRegisterRequest();
registerRequest.topic = rrpcTopic;
registerRequest.replyTopic = rrpcReplyTopic;
registerRequest.payloadObj = payload;
// 先订阅回复的 replyTopic
// 云端发布消息到 replyTopic
// 收到下行数据 回复云端 具体可参考 Demo 同步服务调用
LinkKit.getInstance().subscribeRRPC(registerRequest, new IConnectRrpcListener() {
    @Override
    public void onSubscribeSuccess(ARequest aRequest) {
        // 订阅成功
    }

    @Override
    public void onSubscribeFailed(ARequest aRequest, AError aError) {
        // 订阅失败
    }

    @Override
    public void onReceived(ARequest aRequest, IConnectRrpcHandle iConnectRrpcHandle) {
        // 收到云端下行
        AResponse response = new AResponse();
        response.data = responseData;
        iConnectRrpcHandle.onRrpcResponse(registerRequest.topic, response);
    }

    @Override
    public void onResponseSuccess(ARequest aRequest) {
        // RRPC 响应成功
    }

    @Override
    public void onResponseFailed(ARequest aRequest, AError aError) {
        // RRPC 响应失败
    }
});
```

