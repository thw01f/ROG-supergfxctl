# supergfxctl

## https://asus-linux.org/ : A control daemon, CLI tools, and a collection of crates for interacting with ASUS ROG laptops

A command-line utility (with daemon) for controlling GPU switching / hybrid graphics on ASUS laptops.  
Originally developed by the asus-linux project: [asus-linux/supergfxctl](https://gitlab.com/asus-linux/supergfxctl)  



## What it does

- Switches GPU modes (integrated / dedicated / hybrid / VFIO) on supported ASUS laptops. :contentReference[oaicite:2]{index=2}  
- Enables power-saving by disabling the discrete GPU when not needed. :contentReference[oaicite:3]{index=3}  
- Works with ASUS MUXed systems, external GPUs, and hybrid setups. :contentReference[oaicite:4]{index=4}  



## Key Features

- CLI interface: `supergfxctl --mode <MODE>` to switch modes :contentReference[oaicite:5]{index=5}  
- Daemon (supergfxd) for mode persistence and state tracking. :contentReference[oaicite:6]{index=6}  
- Modes supported include:  
  - `Integrated` — use iGPU only, disable dGPU  
  - `Hybrid` — offload mode: iGPU + dGPU as needed  
  - `VFIO` — bind dGPU for VM passthrough (if supported)  
  - Additional ASUS-specific modes such as `AsusEgpu`, `AsusMuxDgpu` :contentReference[oaicite:7]{index=7}  
- Configurable via `/etc/supergfxd.conf` (for example: hotplug type, logout timeout, always reboot setting) :contentReference[oaicite:8]{index=8}  



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
```


# Usage

## General Help

```bash
supergfxctl --help
```

Display all available commands, flags, and supported operations.



## Check Current GPU Mode

```bash
supergfxctl --get
```

Shows the currently active GPU mode (Integrated, Hybrid, VFIO, etc.).



## List Supported Modes

```bash
supergfxctl --supported
```

Displays all GPU modes available on your hardware.


## Switch GPU Mode

```bash
sudo supergfxctl --mode Hybrid
sudo supergfxctl --mode Integrated
sudo supergfxctl --mode AsusMuxDgpu
sudo supergfxctl --mode VFIO
```

Mode names depend on your machine's hardware capabilities.


## GPU Modes Explained (Full)

### **Integrated Mode**

* Only the **iGPU** is active
* **dGPU is powered off**
* Best for battery life
* May require **logout or reboot**, depending on hardware



### **Hybrid Mode**

* **iGPU** drives the internal display
* **dGPU** is available for offloading via PRIME
* Balanced performance + battery
* Usually **no reboot required**



### **AsusMuxHybrid**

* Requires an ASUS laptop with **hardware MUX**
* Forces MUX to route display through the **iGPU**
* dGPU still available for offload
* May require a **reboot**



### **AsusMuxDgpu**

* For machines with ASUS hardware MUX
* Internal display is driven directly by the **dGPU**
* Maximum performance mode
* Higher power usage
* Typically **requires a reboot**



### **AsusEgpu**

* Designed for systems using **external GPU (eGPU) enclosures**
* Behavior varies depending on ASUS EC firmware
* Often requires **reboots** to switch



### **VFIO Mode**

* dGPU is **unbound** from the standard driver
* Rebound to the `vfio-pci` driver
* Used for **GPU passthrough** to virtual machines
* Requirements:

  * Kernel modules for VFIO loaded separately
  * dGPU cannot be driving any display
* Usually **requires a reboot**


## View GPU Status

```bash
supergfxctl --status
```

Shows the runtime state of GPUs, MUX switch, power status, and current daemon mode.



## Reload Daemon Configuration

```bash
sudo supergfxctl --reload
```

Reloads `/etc/supergfxd.conf` without restarting the service.


## System Service Management (supergfxd)

Start the daemon:

```bash
sudo systemctl start supergfxd
```

Stop the daemon:

```bash
sudo systemctl stop supergfxd
```

Check daemon status:

```bash
sudo systemctl status supergfxd
```

View logs:

```bash
journalctl -u supergfxd -f
```

---
