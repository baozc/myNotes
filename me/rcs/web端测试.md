### 群聊

> 1. 发起通话后RingingMessage无法确定发起人、发起的是音频还是视频通话；另外的人员也会收到RingingMessage
> 2. [x]被叫接通后，主叫只会收到一条AcceptMessage（被叫有多人）
> 3. [ ] 挂断，主/被叫收到的挂断消息不同，有问题和BUG
> 4. join不可用，主动加入
> 5. 被动加入方法没有
> 6. 官网给的command说明和demo里的不符，只有个别有，像那些通用的Key如：senderUserId、targetId、receivedTime等可以根据字面意思理解，剩下的key有无说明？

1. 主叫发起通话邀请成功后会收到 RingingMessage 类型消息，此时可进行响铃（等待被叫接听铃声）。**可以监听**
  1. 拨打音视频通话，没有主叫人帐号，无法区分是谁发起的？
    ```json
    {
        "content": {
          "messageName": "RingingMessage",
          "callId": "3_lipc0113_669" //发起人 Id
        },
        "conversationType": 3,
        "objectName": "RC:VCRinging",
        "messageDirection": 2,
        "messageId": "3_14655485",
        "receivedStatus": 1,
        "receivedTime": 1516003560731,
        "senderUserId": "lipengcheng2",//被叫人帐号
        "sentTime": 1516003560711,
        "targetId": "lipc0113",
        "messageType": "RingingMessage",
        "messageUId": "5GFH-MKD0-SD51-BA0J",
        "offLineMessage": false
    }
    ```
  2. A B C三人群聊，A发起通话请求，A会收到两条`RingingMessage`消息，分别对应B C
  3. B会收到一条`RingingMessage`消息，对应C，B是被叫方，不应该收到这条消息
2. 被叫收到通话邀请会收到 InviteMessage 类型消息，此时可进行已方响铃，显示接听、挂断按钮等。 **可以监听**
  1. 收到音视频通话
    ```json
    {
        "content": {
          "messageName": "InviteMessage",
          "callId": "3_lipc0113_669",//房间 Id
          "engineType": 3,//引擎类型，默认使用 1
          "channelInfo": {//每个用户对应一个 Id，每次音视频连接对应一个唯一的 Key
            "Key": "",//通道 Id
            "Id": "3_lipc0113_669"//通道 Key
          },
          "mediaType": 2,//通话类型
          "inviteUserIds": [//被邀请人列表
            "lipengcheng2",
            "lipengcheng3"
          ]
        },
        "conversationType": 3,
        "objectName": "RC:VCInvite",
        "messageDirection": 2,
        "messageId": "3_10189187",
        "receivedStatus": 1,
        "receivedTime": 1516003560713,
        "senderUserId": "lipengcheng1",//主叫人
        "sentTime": 1516003560690,
        "targetId": "lipc0113",
        "messageType": "InviteMessage",
        "messageUId": "5GFH-MKCU-8D51-BA0J",
        "offLineMessage": false
    }
    ```
3. 主叫发送通话邀请，被叫接听后会收到 AcceptMessage 类型消息，**可以监听**
  1. 对方接听音视频通话，B接通后会收到消息，**C接通后没有消息**
    ```json
      {
        "content": {
          "messageName": "AcceptMessage",
          "callId": "3_lipc0113_30",
          "mediaType": 2
        },
        "conversationType": 3,
        "objectName": "RC:VCAccept",
        "messageDirection": 2,
        "messageId": "3_1480813",
        "receivedStatus": 1,
        "receivedTime": 1516005970003,
        "senderUserId": "lipengcheng2",
        "sentTime": 1516005969958,
        "targetId": "lipc0113",
        "messageType": "AcceptMessage",
        "messageUId": "5GFH-VQG4-OD51-BA0J",
        "offLineMessage": false,
        "callInfo": {
          "mediaType": 2,
          "status": 4
        }
      }
    ```
4. **挂断**，挂断和被挂断收到的消息类型不同，被挂断会收到`HungupMessage`，挂断会收到`SummaryMessage`。   如A Ｂ C三方通话，A发起通话。**_主叫挂断时收到的消息，没有判断通话是否接通_**
  - B C不接通，自动挂断，A不会收到挂断消息
  - 然后A再发起通话，B C接通时报错
    ```js
    Uncaught TypeError: Cannot set property 'status' of undefined
    at rong-calllib.js:1061
    at Object.onSuccess (rong-calllib-sendmsg.js:55)
    at RongIMLib-2.3.0.js:8560
    ```
  - C挂断
    - C收到的消息，`SummaryMessage`，状态不正确，A为主叫
      ```json
      {
        "conversationType": 3,
        "targetId": "lipc0113",
        "messageDirection": 2,
        "content": {
          "caller": "lipengcheng1",//发起人
          "inviter": "lipengcheng1",//被邀请人
          "mediaType": 2,//通话类型
          "startTime": 1516005973950,//通话开始时间
          "duration": 173781,//通话时长(s)
          "status": 3,//主叫挂断
          "memberIdList": [//成员列表
            "lipengcheng2",
            "lipengcheng3"
          ]
        },
        "senderUserId": "lipengcheng1",
        "messageType": "SummaryMessage"
      }
      ```
    - B收到的消息
      ```json
      {
        "content": {
          "messageName": "HungupMessage",
          "callId": "3_lipc0113_821",
          "reason": 3
        },
        "conversationType": 3,
        "objectName": "RC:VCHangup",
        "messageDirection": 2,
        "messageId": "3_901435",
        "receivedStatus": 1,
        "receivedTime": 1516006945363,
        "senderUserId": "lipengcheng3",
        "sentTime": 1516006945326,
        "targetId": "lipc0113",
        "messageType": "HungupMessage",
        "messageUId": "5GFI-3HI5-OD51-BA0J",
        "offLineMessage": false,
        "callInfo": {
          "mediaType": 2,
          "status": 5
        }
      }
      ```
    - A收到的消息
      ```json
      {
        "content": {
          "messageName": "HungupMessage",
          "callId": "3_lipc0113_30",
          "reason": 3
        },
        "conversationType": 3,
        "objectName": "RC:VCHangup",
        "messageDirection": 2,
        "messageId": "3_163338",
        "receivedStatus": 1,
        "receivedTime": 1516006147618,
        "senderUserId": "lipengcheng3",
        "sentTime": 1516006147587,
        "targetId": "lipc0113",
        "messageType": "HungupMessage",
        "messageUId": "5GFI-0G60-CD51-BA0J",
        "offLineMessage": false,
        "callInfo": {
          "mediaType": 2,
          "status": 5
        }
      }
      ```
  - B挂断
    - B收到的消息，状态不正确，A为主叫
      ```json
      {
        "conversationType": 3,
        "targetId": "lipc0113",
        "messageDirection": 2,
        "content": {
          "caller": "lipengcheng1",
          "inviter": "lipengcheng1",
          "mediaType": 2,
          "startTime": 1516005970002,
          "duration": 435241,
          "status": 3,
          "memberIdList": [
            "lipengcheng2",
            "lipengcheng3"
          ]
        },
        "senderUserId": "lipengcheng1",
        "messageType": "SummaryMessage"
      }
      ```
    - A收到的消息
      ```json
      {
        "content": {
          "messageName": "HungupMessage",
          "callId": "3_lipc0113_821",
          "reason": 3
        },
        "conversationType": 3,
        "objectName": "RC:VCHangup",
        "messageDirection": 2,
        "messageId": "3_2420074",
        "receivedStatus": 1,
        "receivedTime": 1516007006267,
        "senderUserId": "lipengcheng2",
        "sentTime": 1516007006196,
        "targetId": "lipc0113",
        "messageType": "HungupMessage",
        "messageUId": "5GFI-3OVU-GD51-BA0J",
        "offLineMessage": false,
        "callInfo": {
          "mediaType": 2,
          "status": 5
        }
      }
      ```
5. join不管用，三人群组，指定A呼叫B
  - 在B接通时C会输出错误日志
    ```js
    rong-calllib.js:601 Uncaught TypeError: Cannot read property 'already' of undefined
    at AcceptMessage (rong-calllib.js:601)
    at Array.<anonymous> (rong-calllib.js:721)
    at ObserverList.notify (rong-calllib-util.js:49)
    at Array.<anonymous> (rong-calllib.js:44)
    at ObserverList.notify (rong-calllib-util.js:49)
    at Object.onReceived (rong-calllib-sendmsg.js:208)
    at MessageHandler.onReceived (RongIMLib-2.3.0.js:4757)
    at Array.MessageHandler.handleMessage (RongIMLib-2.3.0.js:4789)
    at Socket.fire (RongIMLib-2.3.0.js:4178)
    at Socket.onMessage (RongIMLib-2.3.0.js:4148)
    ```
  - C主动加入群聊，调用join，出错
    ```js
    Uncaught Error: undefined: Not call yet
    at errorHandler (rong-calllib.js:993)
    at checkSession (rong-calllib.js:998)
    at sendAccept (rong-calllib.js:1013)
    at Object.join (rong-calllib.js:1074)
    at joinVideo (demo.js:91)
    at HTMLInputElement.onclick (group.html?peer2:64)
    ```
