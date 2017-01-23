# retromqtt

## Usage

Before:

```java
try {
    final MqttClient client = new MqttClient(
            "tcp://test.mosquitto.org:1883",
            MqttClient.generateClientId(),
            new MemoryPersistence());

    client.setCallback(new MqttCallback() {
        @Override
        public void connectionLost(Throwable e) {
        }

        @Override
        public void messageArrived(String topic, MqttMessage message) throws Exception {
            println(Gson.fromJson(new String(message.getPayload()), Message.class).message)
        }

        @Override
        public void deliveryComplete(IMqttDeliveryToken token) {
            finalClient.subscribe("lobby");
        }
    });

    client.connect(new MqttConnectOptions());
} catch (MqttException e) {
    e.printStackTrace();
}
```

Before, using [yongjhih/rx-mqtt](https://github.com/yongjhih/rx-mqtt) for android:

```java
MqttObservable.message(new MqttAndroidClient(
        context,
        "tcp://test.mosquitto.org:1883",
        MqttClient.generateClientId()),
        "lobby")
  .subscribe(System.out::println);
```

After:

```java
@RetroMqtt(baseUrl = "tcp://test.mosquitto.org:1883")
public interface Mosquitto {
  @Topic("#")
  Observable<String> all();
  @Topic("message")
  Observable<Message> message();
}

Mosquitto mos = RetroMqtt.create(Mosquitto.class);
mos.message().subscribe(System.out::println);
```
