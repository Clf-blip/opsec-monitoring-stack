# Installation Instructions

## 📥 Getting the Complete Files

Due to GitHub API file size limitations, the complete production-ready files are provided as artifacts in the Claude chat conversation. Here's how to get them:

### Method 1: Copy from Claude Artifacts (Recommended)

1. **In the Claude chat above**, you'll see several artifacts:
   - `run_opsec.py` - Main launcher script
   - `opsec.json - Monitoring Configuration (Part 1)` - Configuration file
   - `setup.sh` - Installation helper
   - `CATEGORIES.md` - Complete category reference
   - `Example Configuration Files` - 5 specialized configs

2. **Click each artifact** and copy the complete content

3. **Create the files** in your local repository:
   ```bash
   # Clone this repo
   git clone https://github.com/Clf-blip/opsec-monitoring-stack.git
   cd opsec-monitoring-stack
   
   # Create each file and paste the content from artifacts
   nano run_opsec.py       # Paste content, save
   nano opsec.json         # Paste content, save
   nano setup.sh           # Paste content, save
   nano CATEGORIES.md      # Paste content, save
   ```

4. **Make scripts executable**:
   ```bash
   chmod +x run_opsec.py setup.sh
   ```

### Method 2: Download from GitHub Releases (Coming Soon)

We'll create a release with all complete files packaged together.

---

## 🚀 Quick Setup

Once you have the complete files:

```bash
# 1. Run the setup script (installs dependencies)
sudo ./setup.sh

# 2. Review the configuration
cat opsec.json

# 3. Set your network interface
export INTERFACE=eth0  # Change to your interface

# 4. Do a dry run first
./run_opsec.py --dry-run --list

# 5. Launch the monitoring stack
sudo ./run_opsec.py
```

---

## 📋 File Descriptions

### Core Files

- **`run_opsec.py`** (500+ lines)
  - Full Python launcher with all features
  - Tmux orchestration
  - Auto-restart mechanism
  - Logging and error handling
  - Category filtering
  - Environment variable substitution

- **`opsec.json`** (40+ commands)
  - Complete monitoring configuration
  - Network, security, system, container monitoring
  - Pre-configured with sensible defaults
  - Easy to extend

- **`setup.sh`** (300+ lines)
  - Automated dependency installation
  - Distribution detection (Ubuntu/Debian/RHEL/CentOS)
  - Tool installation
  - Directory setup
  - Systemd service creation

### Documentation

- **`CATEGORIES.md`**
  - Comprehensive reference for all monitoring categories
  - Based on NSA-grade inventory
  - Tool recommendations
  - Visualization suggestions

- **`README.md`**
  - Main documentation
  - Quick start guide
  - Configuration reference
  - Security considerations

### Examples

- **`examples/minimal.json`** - Lightweight config for constrained systems
- **`examples/network-heavy.json`** - Deep network traffic analysis
- **`examples/security-heavy.json`** - Maximum security visibility
- **`examples/cloud-containers.json`** - Kubernetes & Docker focus
- **`examples/forensics.json`** - Incident response ready

---

## 🔍 What Makes These Files Special

### Advanced Features in `run_opsec.py`:

- **Environment variable substitution**: `{{INTERFACE}}` automatically replaced
- **Auto-restart with delays**: Configurable per-command
- **Log rotation**: Automatic cleanup based on retention policy
- **Category filtering**: Run only network, security, or custom categories
- **Multiple layouts**: Grid, tiled, even-vertical, or windows mode
- **Dry run mode**: See what will execute without running
- **Colorized output**: Beautiful ANSI headers for each pane
- **Error handling**: Graceful failures with warnings
- **Command validation**: Checks if tools exist before running

### Comprehensive Monitoring in `opsec.json`:

- **Network**: 15+ commands for packet capture, flows, DNS, TLS, HTTP
- **Security**: 12+ commands for auth, intrusion detection, file integrity
- **System**: 10+ commands for CPU, memory, disk, temperature
- **Containers**: Docker and Kubernetes monitoring
- **All extensible**: Add your own commands easily

---

## ⚠️ Important Notes

1. **Root privileges required** for most monitoring commands
2. **Review configuration** before running in production
3. **Test with dry-run** first: `./run_opsec.py --dry-run`
4. **Check tool availability**: Run `./setup.sh` or install manually
5. **Secure logs directory**: `chmod 700 logs/`

---

## 🆘 Troubleshooting

### "tmux not found"
```bash
sudo apt-get install tmux  # Debian/Ubuntu
sudo yum install tmux      # RHEL/CentOS
```

### "Command not found" warnings
```bash
# Install missing tools
sudo ./setup.sh

# Or install specific tools
sudo apt-get install nethogs iftop tcpdump
```

### "Config file not found"
```bash
# Make sure opsec.json is in the same directory
ls -la opsec.json

# Or specify path
./run_opsec.py --config /path/to/opsec.json
```

### "Permission denied"
```bash
# Make scripts executable
chmod +x run_opsec.py setup.sh

# Run with sudo for privileged commands
sudo ./run_opsec.py
```

---

## 📞 Support

- **Issues**: Check the Claude conversation for the complete files
- **Questions**: Review CATEGORIES.md for detailed monitoring info
- **Customization**: Edit opsec.json to add/remove commands

---

## ✅ Verification

To verify you have all the files correctly:

```bash
# Check file sizes (approximate)
ls -lh run_opsec.py     # Should be ~25KB
ls -lh opsec.json       # Should be ~10-15KB
ls -lh setup.sh         # Should be ~12KB
ls -lh CATEGORIES.md    # Should be ~15KB

# Check executability
file run_opsec.py setup.sh

# Test basic functionality
./run_opsec.py --help
```

---

**Once you have all files, you're ready to deploy your NSA-grade monitoring stack!** 🚀
