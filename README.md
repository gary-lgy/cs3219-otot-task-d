# Kafka cluster demo

Kafka cluster with Zookeeper ensemble, deployed in Docker.

## Running the demo

### Make the helper script executable

The helper script frees us from typing long Docker commands.

To make it executable, type:

```bash
chmod +x taskd
```

### Start the containers

In another terminal window, type:

```bash
./taskd up
```

### Create a topic

To create a topic named `events`, type (in yet another terminal window):

```bash
./taskd create events
```

You can also create topics other than `events` by changing the topic name in the command.

### Subscribe to a topic

To subscribe to a topic named `events`, type:

```bash
./taskd sub events
```

As before, you can also subscribe to another topic.

You can run multiple subscribers subscribing to the same or different topics.

### Publish to a topic

To publish to a topic named `events`, open another terminal window and type:

```bash
./taskd pub events
```

Then type a message and press Enter.

If you have the subscriber from the previous step open in another terminal window, you should see the message there.

Similar to subscribers, you can run multiple publishers publishing to the same or different topics.

### Stop and remove the containers

```bash
./taskd down
```
