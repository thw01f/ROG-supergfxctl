# supergfxctl

A command-line utility (with daemon) for controlling GPU switching / hybrid graphics on ASUS laptops.  
Originally developed by the asus-linux project: [asus-linux/supergfxctl](https://gitlab.com/asus-linux/supergfxctl)  

---

## What it does

- Switches GPU modes (integrated / dedicated / hybrid / VFIO) on supported ASUS laptops. :contentReference[oaicite:2]{index=2}  
- Enables power-saving by disabling the discrete GPU when not needed. :contentReference[oaicite:3]{index=3}  
- Works with ASUS MUXed systems, external GPUs, and hybrid setups. :contentReference[oaicite:4]{index=4}  

---

## Key Features

- CLI interface: `supergfxctl --mode <MODE>` to switch modes :contentReference[oaicite:5]{index=5}  
- Daemon (supergfxd) for mode persistence and state tracking. :contentReference[oaicite:6]{index=6}  
- Modes supported include:  
  - `Integrated` — use iGPU only, disable dGPU  
  - `Hybrid` — offload mode: iGPU + dGPU as needed  
  - `VFIO` — bind dGPU for VM passthrough (if supported)  
  - Additional ASUS-specific modes such as `AsusEgpu`, `AsusMuxDgpu` :contentReference[oaicite:7]{index=7}  
- Configurable via `/etc/supergfxd.conf` (for example: hotplug type, logout timeout, always reboot setting) :contentReference[oaicite:8]{index=8}  

---

## Installation

Here’s a typical build & install flow:

```bash
# 1. Install necessary build dependencies (for your distro)
#   e.g., Debian/Ubuntu:
sudo apt update && sudo apt install curl git build-essential

# 2. Clone the repo
git clone https://gitlab.com/asus-linux/supergfxctl.git
cd supergfxctl

# 3. Build & install
make
sudo make install

# 4. Enable and start the daemon
sudo systemctl enable --now supergfxd.service
