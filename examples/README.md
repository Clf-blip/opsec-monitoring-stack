# Example Configurations

This directory contains specialized OPSEC monitoring configurations for different use cases.

## Available Configurations

### 1. minimal.json
Lightweight configuration for low-resource systems or getting started.
- 4 essential monitors
- Low CPU/memory footprint
- Basic network and security coverage

**Use case**: Raspberry Pi, VPS, or learning the system

```bash
./run_opsec.py --config examples/minimal.json
```

### 2. network-heavy.json  
Deep network traffic analysis with comprehensive packet capture.
- 8+ network-focused monitors
- Full packet capture with rotation
- DNS, TLS, HTTP deep inspection
- NetFlow statistics
- Bandwidth breakdown by IP/port

**Use case**: Network security monitoring, SOC operations

```bash
INTERFACE=eth0 sudo ./run_opsec.py --config examples/network-heavy.json
```

### 3. security-heavy.json
Maximum security visibility for threat hunting and incident response.
- 10+ security-focused monitors
- Suricata IDS alerts
- File integrity monitoring
- Rootkit checks
- Kernel module tracking
- Authentication event tracking

**Use case**: Security operations, penetration testing, incident response

```bash
sudo ./run_opsec.py --config examples/security-heavy.json
```

### 4. cloud-containers.json
Container and cloud infrastructure monitoring.
- Docker stats and events
- Kubernetes pod monitoring
- Container resource tracking
- Image pull monitoring
- K8s events stream

**Use case**: Container orchestration, microservices, cloud-native apps

```bash
sudo ./run_opsec.py --config examples/cloud-containers.json
```

### 5. forensics.json
Incident response and forensic data collection.
- Memory snapshots
- Process tree captures
- Open file tracking
- Network state snapshots
- Timeline events
- Command audit trails

**Use case**: Digital forensics, incident investigation, evidence collection

```bash
sudo ./run_opsec.py --config examples/forensics.json --log-retention 90
```

---

## Creating Custom Configurations

Copy any example and customize:

```bash
cp examples/network-heavy.json my-config.json
nano my-config.json  # Edit as needed
./run_opsec.py --config my-config.json
```

### Configuration Schema

```json
{
  "version": "1.0.0",
  "metadata": {
    "description": "Your custom configuration",
    "profile": "custom"
  },
  "commands": [
    {
      "name": "unique_name",
      "category": "network|security|system|containers|forensics|custom",
      "description": "What this monitors",
      "cmd": "command {{INTERFACE}} --args",
      "restart_delay": 5
    }
  ]
}
```

---

## Combining Configurations

Run multiple sessions for different purposes:

```bash
# Terminal 1: Network monitoring
INTERFACE=eth0 sudo ./run_opsec.py --session opsec-net --config examples/network-heavy.json

# Terminal 2: Security monitoring  
sudo ./run_opsec.py --session opsec-sec --config examples/security-heavy.json

# Terminal 3: Container monitoring
sudo ./run_opsec.py --session opsec-containers --config examples/cloud-containers.json
```

---

## Notes

- **All examples require the complete files** from Claude artifacts
- Copy the example configurations from the "Example Configuration Files" artifact
- Some commands may require additional tools (install with `setup.sh`)
- Always test with `--dry-run` first
- Adjust `restart_delay` based on your needs

---

## File Locations

Get the complete example files from the Claude chat artifacts:
- Look for the "Example Configuration Files" artifact
- Copy each configuration (separated by `---SEPARATOR---`)
- Save as the corresponding filename in this directory
