# Arduino Pusher Client, a Pusher client developed for use on Arduino devices
Blog: [World Domination Using Arduinos And Websockets](http://kevinrohling.wordpress.com/2011/09/14/world-domination-using-arduinos-and-websockets)

[Pusher] (http://www.pusherapp.com) is a Push Notification service that uses Websockets for relaying messages back and forth between clients.  This allows real time messaging between a diverse range of applications running on Web browsers, mobile devices and now Arduinos.  It is my hope that allowing devices to easily send information about themselves as well as respond to messages received from applications and other devices will result in some interesting applications.

## Installation instructions

Once you've cloned this repo locally, copy the ArduinoPusherClient directory into your Arduino Sketchbook directory under Libraries then restart the Arduino IDE so that it notices the new library.  Now, under File\Examples you should see ArduinoPusherClient.  To use the library in your app, select Sketch\Import Library\ArduinoPusherClient.

## Examples

Included with this library is an example, called RobotExample, that uses Pusher events to drive two Servos.  This example connects to a channel named "robot_channel" and binds to 5 events: forward, backward, turn_left, turn_right, and stop.  When the events are received the appropriate method gets called and adjusts the angle of the servo motors, driving the robot.

## How To Use This Library

### Connecting to Pusher

```
PusherClient client;

if(client.connect("your-api-key-here")) {
  //Connected!
}
else {
  //Uh oh.
}

void loop() {
  client.monitor();  //Must have a call to monitor() inside loop()
}
```


### Channels

```
//Subscribing to a Public Channel
client.subscribe("my-channel");  

//Subscribing to a Private Channel
client.subscribe("private-my-channel", "my-auth-token");  

//Subscribing to a Presence Channel
client.subscribe("presence-my-channel", "my-auth-token", "my-user-id"); 

//Unsubscribing to a Channel
client.unsubscribe("my-channel");  

```

### Triggering Events

```
client.triggerEvent("my-event", "some data about my-event");
```

### Binding to Events

```
client.bind("my-event", handleMyEvent);

void handleMyEvent(String data) {
   //Do stuff here
}
```

For subsequent authentication it is possible to obtain the `socket_id` by binding to the `pusher:connection_established` event. Then the `socket_id` can be used for authentication locally, e.g. using an Sha256 library or remotly by passing the `socket_id` and `channel_name` to an authentication server. Read more in the [Authentication Signatures](http://pusher.com/docs/auth_signatures) documentation.


### Binding to all Events

```
client.bindAll(handleAllEvents);

void handleAllEvents(String data) {
   //Do stuff here
}
```

## Credits

Arduino Pusher Client uses the [HashMap](http://www.arduino.cc/playground/Code/HashMap) library developed by Alexander Brevig.
