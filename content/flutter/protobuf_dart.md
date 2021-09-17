## 1.引入protobuf库
```
# pubspec.yaml文件引入
protobuf: 2.0.0
```

## 2.编写`proto`文件
`socket.message.proto`文件
```
syntax = "proto3";
package socket;

// 发送聊天信息
message Message {
  string eventId = 1;
  string from = 2;
  string to = 3;
  string createAt = 4;
  string type = 5;
  string body = 6;
}

// 收到聊天消息
message AckMessage {
  string eventId = 1;
}
```

## 3.生成`proto`相关`Model`
配置好插件，使用cmd中的命令自动生成`proto`文件对应的`pb文件`
```
protoc --dart_out=. socket.message.proto
```
## 4.编码、发消息
```
# 准备protobuf对象
Message message = Message();
message.eventId = '####';
message.type = 'text';
message.body = 'hello world';

# ProtobufUtil编码
const MESSAGE_HEADER_LEN = 2;
/// 数据编码
static List<int> encode(int type, var content) {
    ByteData data = ByteData(MESSAGE_HEADER_LEN);
    data.setUint16(0, type, Endian.little);
    List<int> msg = data.buffer.asUint8List() + content.writeToBuffer().buffer.asUint8List();
    return msg;
}

# 发消息
/// 发送
sendSocket(int type, var content) async {
    IOWebSocketChannel channel = await SocketService.getInstance().getChannel();
    if (channel == null) return;
    List<int> msg = ProtobufUtil.encode(type, content);
    channel.sink.add(msg);
}
sendSocket(11, message)

```

## 5.收消息、解码
```
# 解码
/// 数据解码
  static DecodedMsg decode(data) {
    Int8List int8Data = Int8List.fromList(data);
    Int8List contentTypeInt8Data = int8Data.sublist(0, MESSAGE_HEADER_LEN);
    Int8List contentInt8Data = int8Data.sublist(MESSAGE_HEADER_LEN, int8Data.length);
    int contentType = contentTypeInt8Data.elementAt(0);


    GeneratedMessage content;
    switch (contentType) {
      case 10:
        content = AckMessage.fromBuffer(contentInt8Data);
        break;
      case 11:
        content = Message.fromBuffer(contentInt8Data);
        break;
    }

    DecodedMsg decodedMsg;
    if (contentType != null && content != null) {
      decodedMsg = DecodedMsg(
        contentType: contentType,
        content: content,
      );
    }
    return decodedMsg;
  }


  # 收消息
  channel.stream.listen((data) {
    DecodedMsg msg = ProtobufUtil.decode(data);
  }
```