# eBPF Process Monitor

## 📝 Overview
A kernel-level process monitoring tool built using eBPF (Extended Berkeley Packet Filter) that tracks process creation events in real-time by intercepting `sys_clone` system calls.

## 🔍 How It Works
```ascii
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   Process    │         │  eBPF Probe  │         │    Kernel    │
│  Execution   │─────────▶   (BPF)      │◀────────▶   Space      │
└──────────────┘         └──────────────┘         └──────────────┘
                              │
                              ▼
                        ┌──────────────┐
                        │   Python     │
                        │   Frontend   │
                        └──────────────┘
```

## 🛠 Components
1. **ebpf-probe.c**: Kernel-space eBPF program
   - Hooks into `sys_clone` system calls
   - Collects process information (PID, PPID, command name)
   - Sends data through perf buffer

2. **ebpf-runner.py**: User-space control program
   - Loads and manages the eBPF program
   - Processes events from the kernel
   - Displays real-time process creation information

## 📋 Requirements
- Linux Kernel 4.x or later
- Python 3.6+
- BCC (BPF Compiler Collection)
- Root privileges

## 🚀 Installation
```bash
# Install dependencies
sudo apt-get update
sudo apt-get install -y bpfcc-tools python3-bcc

# Clone the repository
git clone <repository-url>
cd <repository-name>
```

## 💻 Usage
```bash
# Run the monitor (requires root privileges)
sudo python3 ebpf-runner.py
```

## 📊 Output Format
The program outputs process creation events in the following format:
```
Process <command_name> (PID:<process_id>, PPID: <parent_process_id>) called sys_clone
```

## 🔒 Security Considerations
- Requires root privileges to run
- Has access to kernel-level system calls
- Use with caution in production environments


## ⚠️ Disclaimer
This tool interacts with kernel-level operations. Use at your own risk and ensure you understand the implications of running eBPF programs in your environment.
