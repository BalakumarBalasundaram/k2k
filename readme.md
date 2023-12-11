# QuickRead

https://docs.confluent.io/platform/current/multi-dc-deployments/cluster-linking/index.html

## Q) What is cluster linking?

A) Cluster linking is a feature in Confluent Platform that allows for the replication of data across multiple Apache Kafka clusters. It's designed to be lightweight and have minimal impact on performance. It's useful in scenarios such as multi-datacenter deployments, disaster recovery, and geo-local data presence.

## Q) How can I configure cluster linking in Confluent Platform?
   1. Configure the destination cluster:
      - Set 'confluent.cluster.links.enable' to 'true' in the destination cluster's properties file.

   2. Create a cluster link on the source cluster:
      - Use the 'kafka-cluster-links' command with the 'create' option.
      - Provide the necessary details like 'link-name', 'bootstrap.servers' of the destination cluster.

   3. Create a MirrorMaker 2.0 replication flow on the source cluster:
      - Use the 'kafka-mirrormaker' command with the 'create' option.
      - Provide the necessary details like 'source-cluster', 'target-cluster', 'topics-include'.


   Here is a brief pseudocode:

      1. Configure the destination cluster:
         - Set 'confluent.cluster.links.enable' to 'true' in the destination cluster's properties file.

      2. Create a cluster link on the source cluster:
         - Use the 'kafka-cluster-links' command with the 'create' option.
         - Provide the necessary details like 'link-name', 'bootstrap.servers' of the destination cluster.

      3. Create a MirrorMaker 2.0 replication flow on the source cluster:
         - Use the 'kafka-mirrormaker' command with the 'create' option.
         - Provide the necessary details like 'source-cluster', 'target-cluster', 'topics-include'.

   Here is the corresponding shell commands:

      # Step 1: Configure the destination cluster
      # Add the following line to the destination cluster's properties file
      confluent.cluster.links.enable=true

      # Step 2: Create a cluster link on the source cluster
      # Replace 'link-name' and 'bootstrap.servers' with your values
      confluent kafka cluster-links create --config bootstrap.servers=<bootstrap.servers> --link-name <link-name>

      # Step 3: Create a MirrorMaker 2.0 replication flow on the source cluster
      # Replace 'source-cluster', 'target-cluster', 'topics-include' with your values
      confluent kafka mirrormaker create --source-cluster <source-cluster> --target-cluster <target-cluster> --topics-include <topics-include>

      Please replace <bootstrap.servers>, <link-name>, <source-cluster>, <target-cluster>, and <topics-include> with your actual values.

      Next, you might want to verify the cluster linking or start producing and consuming data to test the setup.

## Q) How can I verify the status of cluster linking in Confluent Platform?

To verify the status of cluster linking in Confluent Platform, you can use the describe command with the kafka-cluster-links tool. This will provide information about the cluster link, including its status.

   Here is the pseudocode:

      1. Use the 'kafka-cluster-links' command with the 'describe' option.
      2. Provide the 'link-name' as an argument.

   
   Here is the corresponding shell command:

      # Replace 'link-name' with your actual link name
      confluent kafka cluster-links describe --link-name <link-name>

>
This command will output the details of the cluster link, including its status. If the link is working correctly, the status should be 'ACTIVE'.

Next, you might want to learn how to troubleshoot if the cluster link is not active or explore other commands related to cluster linking.

### view the installation 
https://www.confluent.io/installation/


## Q) How can I troubleshoot cluster linking issues in Confluent Platform?

Troubleshooting cluster linking issues in Confluent Platform can involve several steps. Here's a general approach:

Check the status of the cluster link using the describe command.
Check the logs of the source and destination clusters for any error messages.
Verify the network connectivity between the source and destination clusters.
Ensure that the configurations of the source and destination clusters are correct.

Here is the pseudocode:

   1. Use the 'kafka-cluster-links' command with the 'describe' option and the 'link-name' as an argument.
   2. Check the logs of the source and destination clusters.
   3. Verify the network connectivity between the source and destination clusters.
   4. Check the configurations of the source and destination clusters.

Here is the corresponding shell command for the first step:

### Replace 'link-name' with your actual link name
confluent kafka cluster-links describe --link-name <link-name>

For the remaining steps, you would need to manually check the logs, verify network connectivity, and check the configurations.

Next, you might want to learn about common issues and their solutions or explore more advanced troubleshooting techniques.

## Q) How can I monitor the performance of cluster linking in Confluent Platform?

Monitoring the performance of cluster linking in Confluent Platform can be done using Confluent Control Center or JMX metrics.

Confluent Control Center provides a user-friendly interface to monitor the health and performance of your Kafka clusters. It provides metrics like throughput, latency, and error rates for your cluster links.

JMX (Java Management Extensions) metrics can be used for more detailed monitoring. Confluent Platform exposes several JMX metrics related to cluster linking that can be monitored using JMX monitoring tools.

Here is the pseudocode:
1. Open Confluent Control Center.
2. Navigate to the 'Cluster Linking' section.
3. Monitor the metrics like throughput, latency, and error rates for your cluster links.

.
For JMX metrics:
1. Enable JMX in your Confluent Platform configuration.
2. Use a JMX monitoring tool to monitor the JMX metrics related to cluster linking.
   
Next, you might want to learn how to interpret these metrics or how to set up alerts based on these metrics.

How can I set up alerts based on the performance metrics of cluster linking in Confluent Platform?

Setting up alerts based on the performance metrics of cluster linking in Confluent Platform can be done using Confluent Control Center. It allows you to create alert rules based on various metrics.

Here is the pseudocode:
1. Open Confluent Control Center.
2. Navigate to the 'Alerts' section.
3. Click on 'Add Alert'.
4. Select the metric for which you want to create an alert.
5. Set the condition and threshold for the alert.
6. Save the alert.
.
Next, you might want to learn how to manage these alerts or how to integrate them with external alerting systems.

## Q) How can I manage alerts based on the performance metrics of cluster linking in Confluent Platform?
//TODO


## Testing

### Find latency of cluser linking

* This test helps to measure average end-to-end latency of a messages sent from one Cluster via Cluster Linking to another Kafka Cluster.

* Producer --> Cluster1 ---cluster-link---> Cluster2 --> Consumer

* Each message is given a timestamp by the producer. Latency is computed by the consumer by subtracting the producer timestamp from the current time.

      /**
         * Before the test, we need to set up a fresh topic in the source cluster, and a fresh mirror of the topic
         * in the destination cluster:
         *
         * kafka-topics --create --topic demo --bootstrap-server localhost:9092
         * kafka-cluster-links --bootstrap-server localhost:9093 \
         *   --create --link demo-link --config bootstrap.servers=localhost:9092
         * kafka-mirrors --create \
         *   --mirror-topic demo \
         *   --link demo-link \
         *   --bootstrap-server localhost:9093
         *
         * Delete the topics after the test:
         *
         * Cleanup:
         * kafka-topics --delete \
         *  --topic demo \s
         *  --bootstrap-server localhost:9092
         *
         * kafka-topics --delete \
         *  --topic demo \
         *  --bootstrap-server localhost:9093
         **/

# Test Plan

Cluster Linking is used to asyncronously share Kafka topics between two data centers. This can be for simple data sharing between two (or more) active data centers (e.g. data mesh) or disaster recovery scenarios.

In this demo, we will set up a Cluster Link between Primary and Secondary data centers. Source data will be produced using a Kafka command line tool that will read mock data generated by script.

We will also demonstrate ksqlDB to create streams and tables in order to use push and pull queries of the Employee data.

More information on Cluster Linking and another demo can be found [here](https://docs.confluent.io/platform/6.0.0/multi-dc-deployments/cluster-linking/docker-quickstart.html).

<br>

### Table of Contents
* [Setup](#setup)
* [Running](#running)
* [Produce Source Data](#produce-employee-data)
* [Link Primary to Secondary Clusters](#link-primary-to-secondary-clusters)
* [Using ksqlDB](#using-ksqlDB)

<br>

## Further Reading

- https://docs.confluent.io/platform/current/multi-dc-deployments/cluster-linking/migrate-cp.html
- https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html#share-data-with-cluster-linking-on-ccloud
- https://docs.confluent.io/platform/current/multi-dc-deployments/cluster-linking/mirror-topics-cp.html#mirror-topics-for-cluster-linking