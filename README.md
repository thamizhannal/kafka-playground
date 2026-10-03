
# Apache Kafka Setup Guide (Docker)

A quickstart guide for running Apache Kafka locally using the official Apache Docker image, creating topics, producing events from a local file, and consuming events via PowerShell/Bash.

---

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.
- [PowerShell](https://learn.microsoft.com/en-us/powershell/) (for Windows environments) or standard Unix Terminal.

---

## Setup & Running Kafka

### 1. Pull the Docker Image

Fetch the official Apache Kafka Docker image:

```bash
docker pull apache/kafka-native:4.3.1

```

> **Note:** Kafka binaries inside this image are located at `/opt/kafka/`.

### 2. Start the Kafka Container

Run the Kafka container in the background and map port `9092`:

```bash
docker run -d --name kafka-local -p 9092:9092 apache/kafka-native:4.3.1

```

---

## Usage Guide

### 3. Create a Topic

Create a new topic named `quickstart-events`:

```powershell
docker exec -it kafka-local /opt/kafka/bin/kafka-topics.sh `
  --create `
  --topic quickstart-events `
  --bootstrap-server localhost:9092

```

### 4. Verify / Describe the Topic

Inspect the topic details:

```powershell
docker exec -it kafka-local /opt/kafka/bin/kafka-topics.sh `
  --describe `
  --topic quickstart-events `
  --bootstrap-server localhost:9092

```

---

## Producing & Consuming Events

### 5. Produce Events from a Local File

Stream payload data (e.g., JSON telemetry) into the topic using PowerShell:

```powershell
Get-Content C:\path\to\your\scapy_telemetry.json | docker exec -i kafka-local /opt/kafka/bin/kafka-console-producer.sh `
  --topic quickstart-events `
  --bootstrap-server localhost:9092

```

### 6. Read / Consume Events

Open a new terminal window to consume all messages from the beginning:

```powershell
docker exec -it kafka-local /opt/kafka/bin/kafka-console-consumer.sh `
  --topic quickstart-events `
  --from-beginning `
  --bootstrap-server localhost:9092

```

#### Example Output

```json
[
  {
    "time": 1789659652.105462,
    "frame_length": 91,
    "payload_size": 0,
    "direction": "TX",
    "cumulative_metrics": {
      "tx_bytes": 91,
      "rx_bytes": 0,
      "tx_packets": 1,
      "rx_packets": 0
    },
    "ip": {
      "src": "192.168.0.4",
      "dst": "192.168.0.1",
      "ttl": 64,
      "proto": 17,
      "id": 28581,
      "flags": ""
    },
    "udp": {
      "srcport": 64754,
      "dstport": 53,
      "len": 57
    },
    "dns": {
      "qname": "signaler-pa.clients6.google.com.",
      "qtype": 65
    }
  }
]

```

