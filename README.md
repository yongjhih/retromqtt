# retromqtt

## Usage

```java
@RetroMqtt(baseUrl = "tcp://test.mosquitto.org:1883")
public interface Mosquitto {
  @Topic("#")
  Observable<String> all();
  @Topic("message")
  Observable<Message> message();
}

Mosquitto mos = RetroMqtt.create(Mosquitto.class);
mos.message.subscribe(msg -> println(msg.message));
```
