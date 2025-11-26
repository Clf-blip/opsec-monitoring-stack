# OPSEC Monitoring Stack 🛡️

**Military-grade monitoring orchestrator** for comprehensive security, network, and system telemetry. Single-file configuration drives hundreds of monitoring commands through a robust tmux-based launcher.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.7+-blue.svg)](https://python.org)
[![Platform](https://img.shields.io/badge/platform-linux-lightgrey.svg)](https://kernel.org)

---

## 🚨 IMPORTANT: Complete Files in Claude Chat

Due to file size limitations, the **complete, production-ready files** are available in the Claude chat artifacts above. Please copy them from the artifacts to get the full implementation:

### Files to Copy from Artifacts:

1. **`run_opsec.py`** - Full launcher script (~500 lines)
2. **`opsec.json`** - Complete monitoring configuration (40+ commands)
3. **`setup.sh`** - Automated installation script
4. **`CATEGORIES.md`** - Comprehensive category reference
5. **Example configurations** - 5 specialized config files

---

## 🎯 Purpose

This project provides a **unified monitoring foundation** that can launch and manage hundreds of security, network, system, and forensic monitoring tools simultaneously. It's designed as the data collection layer for a future NSA-grade visualization UI.

### Key Features

- ✅ **Single JSON Configuration** - All monitoring commands defined in `opsec.json`
- ✅ **Auto-Restart** - Commands automatically restart on failure with configurable delays
- ✅ **Persistent Logging** - All output logged with timestamps and automatic rotation
- ✅ **Category Filtering** - Run only specific monitoring categories
- ✅ **Environment Variable Substitution** - Dynamic configuration via env vars
- ✅ **Tmux Orchestration** - Professional terminal multiplexing with multiple layout options
- ✅ **Dry Run Mode** - Preview what would execute without running commands
- ✅ **Unlimited Scalability** - Add monitoring commands without touching launcher code

---

## 🚀 Quick Start

### Prerequisites

```bash
# Install tmux
sudo apt-get install tmux  # Debian/Ubuntu
sudo yum install tmux      # RHEL/CentOS
brew install tmux          # macOS

# Optional but recommended monitoring tools
sudo apt-get install ifstat nethogs iftop tcpdump htop iostat smartmontools
```

### Basic Usage

```bash
# Make launcher executable
chmod +x run_opsec.py

# Launch full monitoring stack
INTERFACE=eth0 sudo ./run_opsec.py

# Launch specific categories only
INTERFACE=eth0 sudo ./run_opsec.py --categories network,security

# List all available commands
./run_opsec.py --list

# Dry run to see what would execute
INTERFACE=eth0 ./run_opsec.py --dry-run --verbose

# Kill existing session
./run_opsec.py --kill
```

---

## 📋 Configuration

### Environment Variables

The launcher supports dynamic configuration through environment variables:

| Variable | Description | Example |
|----------|-------------|---------|
| `INTERFACE` | Network interface to monitor | `eth0`, `wlan0`, `enp0s3` |
| `PORTS` | Comma-separated ports | `22,80,443,8080` |
| `TARGET_IP` | Specific target for monitoring | `192.168.1.1` |
| `OUTPUT_DIR` | Directory for output files | `/var/opsec/captures` |

### Command Line Options

```
Options:
  --config PATH           Path to opsec.json (default: ./opsec.json)
  --session NAME          Tmux session name (default: opsec)
  --layout LAYOUT         Tmux layout: grid|tiled|even|windows (default: grid)
  --log-dir PATH          Log directory (default: ./logs)
  --log-retention DAYS    Log retention in days (default: 7)
  --no-restart            Disable auto-restart on command failure
  --categories CAT,...    Only run specific categories
  --dry-run              Show what would be executed
  --kill                 Kill existing session and exit
  --list                 List all configured commands
  --verbose              Enable verbose output
```

---

## 📁 Project Structure

```
opsec-monitoring-stack/
├── run_opsec.py           # Main launcher script
├── opsec.json             # Monitoring command configuration
├── setup.sh               # Automated setup script
├── logs/                  # Auto-created log directory
│   ├── interface_throughput.log
│   ├── auth_events.log
│   └── ...
├── README.md              # This file
├── CATEGORIES.md          # Detailed category reference
└── examples/              # Example configurations
    ├── minimal.json
    ├── network-heavy.json
    ├── security-heavy.json
    ├── cloud-containers.json
    └── forensics.json
```

---

## 🔧 Adding New Monitoring Commands

Simply edit `opsec.json` and add new command entries:

```json
{
  "name": "my_custom_monitor",
  "category": "custom",
  "description": "Description of what this monitors",
  "cmd": "your_command_here --with-args {{INTERFACE}}",
  "restart_delay": 5
}
```

The launcher will automatically pick up the new command on next run. No code changes needed!

### Command Schema

```json
{
  "name": "string",              // Unique identifier
  "category": "string",          // Group: network, security, system, etc.
  "description": "string",       // Human-readable description
  "cmd": "string",              // Shell command with {{VAR}} placeholders
  "restart_delay": 5,           // Seconds before restart (default: 5)
  "check_exists": true          // Verify command exists (default: true)
}
```

---

## 📊 Monitoring Categories

The stack organizes commands into logical categories:

### Network Monitoring
- Interface throughput & bandwidth
- Top talkers & flow analysis
- NetFlow/IPFIX exports
- Packet captures (full & live)
- Connection states & tracking
- DNS, TLS, HTTP traffic
- Latency & retransmission monitoring

### Security & Intrusion Detection
- Authentication events
- Failed login tracking
- Port scan detection
- IDS/IPS alerts
- File integrity monitoring
- Suspicious process detection
- SUID binary tracking
- Kernel module changes

### System Health
- CPU, memory, disk I/O
- Filesystem & inode usage
- SMART disk health
- Temperature sensors
- Entropy pool monitoring
- Process & file descriptor tracking

### Containers & Cloud
- Docker stats & events
- Kubernetes pod monitoring
- Container resource usage

**For complete category details, see [CATEGORIES.md](CATEGORIES.md)**

---

## 🎨 Tmux Layouts

The launcher supports multiple layout strategies:

### Grid (Default)
```bash
./run_opsec.py --layout grid
```
Automatically tiles all panes in a grid pattern.

### Windows Mode
```bash
./run_opsec.py --layout windows
```
Each command in its own tmux window (easier navigation for many commands).

### Even Vertical
```bash
./run_opsec.py --layout even
```
Vertical split with equal-sized panes.

---

## 🔐 Security Considerations

### Root Privileges

Many monitoring commands require root:
- Packet capture (tcpdump)
- Network statistics (ss, netstat)
- File system monitoring (inotifywait)
- Hardware sensors (smartctl, sensors)

Always audit commands in `opsec.json` before running with sudo.

### Ethical & Legal Guardrails

⚠️ **CRITICAL**: Only monitor systems you own or have explicit permission to monitor.

- Deep packet inspection may be subject to legal restrictions
- Ensure compliance with data protection laws (GDPR, etc.)
- Treat all monitoring data as highly sensitive
- Implement proper access controls on log directories
- Consider data retention policies and regulations

### Log Security

```bash
# Secure log directory
chmod 700 logs/
chown root:root logs/

# Rotate and compress old logs
./run_opsec.py --log-retention 7
```

---

## 🎯 Use Cases

1. **Network Security Operations Center (SOC)** - Deploy as data collection layer for security monitoring
2. **Incident Response** - Rapid deployment during security incidents for comprehensive visibility
3. **Performance Monitoring** - System administrators tracking resource utilization and network health
4. **Penetration Testing** - Red teams monitoring their own infrastructure during engagements
5. **Research & Development** - Security researchers building custom monitoring solutions

---

## 🛠️ Advanced Usage

### Custom Configuration Files

```bash
# Use production config
./run_opsec.py --config /etc/opsec/production.json

# Use lightweight config for low-resource systems
./run_opsec.py --config examples/minimal.json
```

### Multiple Simultaneous Sessions

```bash
# Run separate sessions for different purposes
INTERFACE=eth0 ./run_opsec.py --session opsec-network --categories network
INTERFACE=eth0 ./run_opsec.py --session opsec-security --categories security
```

### Integration with Other Tools

```bash
# Export to Prometheus
# Add commands that expose metrics on HTTP endpoints

# Forward to SIEM
# Configure commands to send logs to syslog/rsyslog
# Have rsyslog forward to your SIEM
```

---

## 📈 Roadmap

### Phase 1: Foundation (✅ Current)
- [x] Single-file launcher
- [x] JSON-driven configuration
- [x] Auto-restart mechanism
- [x] Logging & retention
- [x] Category filtering

### Phase 2: Enhanced Telemetry
- [ ] eBPF/XDP probe integration
- [ ] Full packet reconstruction
- [ ] GeoIP enrichment
- [ ] Certificate transparency monitoring
- [ ] Cloud provider integrations (AWS, Azure, GCP)

### Phase 3: NSA-Grade UI
- [ ] Real-time web dashboard
- [ ] Interactive packet analysis
- [ ] Flow visualization (Sankey diagrams)
- [ ] Geographic heatmaps
- [ ] Anomaly detection ML models
- [ ] Alert correlation engine

### Phase 4: Automation & Response
- [ ] Automated threat response
- [ ] Integration with firewalls/IDS
- [ ] Policy enforcement hooks
- [ ] Orchestration API

---

## 🤝 Contributing

Contributions welcome! Areas of interest:

- Additional monitoring command definitions
- Platform-specific command variants (BSD, macOS)
- Integration guides for specific tools
- Performance optimizations
- Documentation improvements

---

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details.

---

## ⚠️ Disclaimer

This tool is for authorized security monitoring only. Users are responsible for:
- Obtaining proper authorization before deployment
- Complying with all applicable laws and regulations
- Securing monitoring data appropriately
- Using the tool ethically and responsibly

The authors assume no liability for misuse.

---

## 🙏 Acknowledgments

Built on the shoulders of giants:
- tmux - Terminal multiplexer
- tcpdump/Wireshark - Packet analysis
- Suricata/Snort - IDS/IPS
- Prometheus - Metrics collection
- The entire open-source security community

---

**Made with 🔐 for security professionals, by security professionals**
