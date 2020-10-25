# Kafka Cluster Failover Demo

Kafka cluster with Zookeeper ensemble, deployed in Docker.

## Content

This demo contains a docker-compose.yml file for creating a 3-node Kafka cluster, managed by a 3-node ZooKeeper ensemble, both with failover.

This project also includes a bash script to automate some common procedures.

## Steps to test Kafka broker failover

### Make the helper bash script executable

The helper script frees us from typing long Docker commands.

To make it executable, type:

```bash
chmod +x taskd
```

### Start the containers

```bash
./taskd up
```

This will spin up both the 3-node ZooKeeper ensemble and 3-node Kafka cluster.

The ZooKeeper servers are reachable at host ports 2181, 2182, and 2183. The Kafka brokers are reachable at host ports 8001, 8002, and 8003.

### View ZooKeeper Ensemble status (just FYI)

To view the ZooKeeper ensemble status, run:

```bash
./taskd zkStatus
```

This should indicate which instance is the leader (not to be confused with the Kafka leader that we will see later).

### Create a topic

To create a topic named `events`, open another terminal window and type:

```bash
./taskd create events
```

You can also create topics other than `events` by changing the topic name in the command.

### Subscribe to the topic

To subscribe to the topic we just created, run:

```bash
./taskd sub events
```

As before, you can also subscribe to another topic.

You can run multiple subscribers subscribing to the same or different topics.

### Publish to the topic

To publish to the `events` topic, open another terminal window and type:

```bash
./taskd pub events
```

Then type a message and press Enter.

If you have the subscriber from the previous step open in another terminal window, you should see the message there.

Similar to subscribers, you can run multiple publishers publishing to the same or different topics.

### Describe the topic

To view information related to the topic, open another terminal window and type:

```bash
./taskd describe events
```

Among other things, the returned information includes the leader broker of the topic (which should be a number from 1 to 3)

### Kill the leader broker of the topic

To verify that the topic can still work properly even after its leader broker dies, let's kill its leader broker.

Without closing the subscriber and publisher, run:

```bash
./taskd stop BROKER_ID
```

Where `BROKER_ID` is the ID of the leader broker we discovered from the last step.

For example, if the output of the previous step is this:

![Describe Topic Output](./describe_topic_output_before_kill.png)

Then the leader broker ID is 2. To kill it, we should run:

```bash
./taskd stop 2
```

### Test that the topic still works

Now, run `./taskd describe events` again to verify that a new leader has been elected for the topic.

A possible output is:

![Describe Topic Output After Killing Leader](./describe_topic_output_after_kill.png)

Try publishing to the topic. The subscriber should still be able to receive the messages.

### Stop and remove the containers

```bash
./taskd down
```
