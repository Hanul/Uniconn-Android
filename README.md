# Unicorn-Android
UPPERCASE Android 클라이언트 모듈입니다.

MODEL 처리
```java
Log.i(TAG, "접속을 시도합니다.");

RoomConnector connector = new RoomConnector("192.168.0.6", 9126, new ConnectedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속되었습니다.");
    }

}, new ConnectionFailedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속에 실패하였습니다.");
    }

}, new DisconnectedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속이 끊어졌습니다.");
    }
});

final Model testBox = new Model(roomConnector, "TestBox", "Test");

JSONObject data = new JSONObject();
try {
    data.put("name", "YJ Sim");
    data.put("isMan", true);
} catch (JSONException e) {
    e.printStackTrace();
}

testBox.create(data, new CreateHandler() {

    @Override
    public void notValid(JSONObject validErrors) {
        Log.i(TAG, validErrors.toString());
    }

    @Override
    public void success(JSONObject savedData) {
        Log.i(TAG, savedData.toString());
    }
});

testBox.find(new FindHandler() {

    @Override
    public void success(List<JSONObject> savedDataSet) {
        Log.i(TAG, savedDataSet.toString());
    }
});

testBox.get("test", new GetHandler() {

    @Override
    public void success(JSONObject savedData) {
        Log.i(TAG, savedData.toString());
    }
});

testBox.onNew(new OnNewHandler() {

    @Override
    public void handle(JSONObject savedData) {
        Log.i(TAG, "ON NEW: " + savedData.toString());
    }
});

JSONObject properties = new JSONObject();
try {
    properties.put("age", 27);
} catch (JSONException e) {
    e.printStackTrace();
}

testBox.onNew(properties, new OnNewHandler() {

    @Override
    public void handle(JSONObject savedData) {
        Log.i(TAG, "ON NEW when age is 27: " + savedData.toString());
    }
});

JSONObject data2 = new JSONObject();

testBox.create(data2, new CreateHandler() {

    @Override
    public void success(JSONObject savedData) {
        Log.i(TAG, savedData.toString());
    }
});

JSONObject data3 = new JSONObject();
try {
    data3.put("name", "YJ Sim");
    data3.put("age", 27);
    data3.put("isMan", true);
} catch (JSONException e) {
    e.printStackTrace();
}

testBox.create(data3);

testBox.create(data3, new CreateHandler() {

    @Override
    public void success(JSONObject savedData) {

        Log.i(TAG, "CREATE: " + savedData.toString());

        try {
            testBox.get(savedData.getString("id"), new GetHandler() {

                @Override
                public void success(JSONObject savedData) {
                    Log.i(TAG, "GET: " + savedData.toString());

                    JSONObject data = new JSONObject();
                    try {
                        data.put("id", savedData.getString("id"));
                        data.put("name", "TEST!!!" + new Date().getTime());
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }

                    testBox.update(data, new UpdateHandler() {

                        @Override
                        public void success(JSONObject savedData, JSONObject originData) {
                            Log.i(TAG, "UPDATE: " + savedData.toString());

                            try {
                                testBox.remove(savedData.getString("id"), new RemoveHandler() {

                                    @Override
                                    public void notAuthed() {
                                        Log.i(TAG, "not authed!!");
                                    }

                                    @Override
                                    public void success(JSONObject originData) {
                                        Log.i(TAG, "REMOVE: " + originData.toString());
                                    }
                                });

                            } catch (JSONException e) {
                                e.printStackTrace();
                            }
                        }
                    });
                }
            });

        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
});
```

룸 서버 접속
```java
Log.i(TAG, "접속을 시도합니다.");

RoomConnector connector = new RoomConnector("192.168.0.6", 9126, new ConnectedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속되었습니다.");
    }

}, new ConnectionFailedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속에 실패하였습니다.");
    }

}, new DisconnectedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속이 끊어졌습니다.");
    }
});

Room room1 = new Room(roomConnector, "TestBox", "testRoom");
Room room2 = new Room(roomConnector, "TestBox", "testRoom");

room1.send("msg", "Hello, test1!");

room1.on("msg", new MethodHandler() {
    @Override
    public void handle(Object data) {
        Log.i(TAG, data.toString());
    }
});

room2.send("msg", "Hello, test2!");

room2.on("msg", new MethodHandler() {
    @Override
    public void handle(Object data) {
        Log.i(TAG, data.toString());
    }
});

room1.exit();

room1.send("msg", "Hello, test3!");
room2.send("msg", "Hello, test4!");
```

소켓 서버 접속
```java
Log.i(TAG, "접속을 시도합니다.");

SocketServerConnector connector = new SocketServerConnector("192.168.0.6", 8124, new ConnectedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속되었습니다.");
    }

}, new ConnectionFailedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속에 실패하였습니다.");
    }

}, new DisconnectedHandler() {

    @Override
    public void handle() {
        Log.i(TAG, "접속이 끊어졌습니다.");
    }
});

connector.on("message", new MethodHandler() {

    @Override
    public void handle(Object data) {
        Log.i(TAG, data.toString());
    }
});

JSONObject data = new JSONObject();
try {
    data.put("msg", "message from client.");

    connector.send("message", data, new MethodHandler() {
        @Override
        public void handle(Object data) {
            Log.i(TAG, data.toString());
        }
    });

} catch (JSONException e) {
    e.printStackTrace();
}
```

## 라이센스
[MIT](LICENSE)

## 작성자
[Young Jae Sim](https://github.com/Hanul)

## 문의하기
* [UPPERCASE 페이스북 그룹](https://www.facebook.com/groups/uppercase/)
* [GitHub Issues](https://github.com/Hanul/Unicorn-Android/issues)