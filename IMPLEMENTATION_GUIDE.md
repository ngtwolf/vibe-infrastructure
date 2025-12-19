# Vibe Infrastructure: Implementation Guide

**Version 1.0** | **Companion to OVERVIEW.md**

This document provides the technical specifications needed to implement a Vibe Infrastructure system. An AI assistant reading this document should be able to set up the initial system and ask appropriate questions to complete the implementation.

## Prerequisites

### Required Capabilities

The AI assistant must have:
- File read/write access
- Command execution (local and remote SSH)
- Network access (HTTP, SSH)
- Version control (Git) for history (recommended)

### User Requirements

The user must have:
- SSH access to network devices (if AI will manage them)
- Basic understanding of their network infrastructure
- AI assistant with the capabilities listed above

## Directory Structure

Create the following directory structure. This structure is required—the AI expects these paths:

```
HomeNetwork/
├── docs/
│   ├── network/          # Network topology, VLANs, routing
│   │   └── configs/      # Backup configuration files
│   ├── devices/          # Physical devices (Router, Switches, APs)
│   ├── servers/          # NAS, Home Server, compute hosts
│   └── services/         # Software services (Plex, Home Assistant, etc.)
├── inventory/            # Structured data (YAML files)
│   ├── devices.yaml      # Device inventory
│   ├── servers.yaml      # Server inventory
│   └── services.yaml     # Service inventory
├── scripts/              # Automation scripts
├── diagnostics/          # Troubleshooting notes (created as needed)
└── .cursor/              # AI Configuration (if using Cursor)
    └── rules/            # AI team protocols
```

**Note**: IP addresses shown in examples throughout this guide (e.g., 192.168.1.1, 192.168.1.200) are placeholder values from the standard private IP range. Replace them with your actual network configuration.


## AI Team Protocol Definition

The AI must be configured with a team protocol that defines roles, workflows, and standards. This protocol should be set up as rules: stored in `.cursor/rules/networkguide.mdc` (for Cursor) or in the appropriate rules directory for other AI assistants (check your AI assistant's documentation for the exact location).

### Complete Protocol Files

The complete AI team protocol is defined in two files:

1. **networkguide.mdc** - Interaction guidelines, documentation protocols, verification standards, troubleshooting procedures
2. **networkteam.mdc** - Team structure, expert roles, coordination workflows, SLA definitions

These files contain the detailed rules that govern how the AI operates. They should be:
- Placed in `.cursor/rules/` (for Cursor IDE)
- Set up as rules in the appropriate rules directory for other AI assistants (check your AI assistant's documentation for the exact location)
- Treated as the "operating manual" for the AI team

**Note**: These protocol files are extensive (2000+ lines combined) and define every aspect of the team's behavior. The specifications in this Implementation Guide summarize the key requirements, but the full protocols provide comprehensive details including:
- Detailed troubleshooting decision trees
- Common mistakes and prevention strategies
- Session summary templates
- Emergency response protocols
- Learning and improvement processes

**Obtaining the protocols**:

**Option 1 - This Repository**: The complete protocol files are included in this repository. Copy `networkguide.mdc` and `networkteam.mdc` from the root directory to your `.cursor/rules/` directory (for Cursor IDE) or set them up as rules in the appropriate rules directory for other AI assistants (check your AI assistant's documentation for the exact location).

**Option 2 - Create Your Own**: Use the specifications in this Implementation Guide to create your own protocol files. The key sections are documented above (Team Structure, Core Workflow, Documentation Standards, etc.), and you can customize them for your specific needs.

For Cursor IDE, protocol files should be placed in `.cursor/rules/` directory. For other AI assistants, they should be set up as rules (not as project knowledge or general context).

### Required Protocol Components

#### 1. Team Structure

Define specialized expert roles:
- **Service Delivery Manager**: Coordinates all work, assesses priorities, ensures quality
- **Senior Network Engineer**: Routing, switching, VLANs, Wi-Fi
- **Senior DevOps Engineer**: Docker, containers, orchestration
- **Storage Systems Administrator**: NAS, storage, backups
- **Network Security Engineer**: Firewalls, access control, security
- **IoT & Automation Engineer**: Smart home devices, automation
- **Backup & Recovery Specialist**: Backup strategies, recovery procedures
- **Technical Documentation Specialist**: Documentation maintenance

The team coordinates internally. Users see results, not coordination.

#### 2. Core Workflow

For every user request, the AI must follow:
1. **Verify**: Check actual state (e.g., `docker ps`, `ping`) before assuming
2. **Research**: Read existing files in `docs/` and `inventory/`
3. **Plan**: Decide on fix or change, assess impact
4. **Execute**: Provide commands or perform edits
5. **Document**: Update relevant `.md` files in `docs/` to reflect new state

#### 3. Documentation Standards

The AI is responsible for maintaining the "Single Source of Truth" in the `docs/` folder:
- **Topology**: Keep `docs/network/topology.md` updated
- **Devices**: Create/update `docs/devices/{device}.md`
- **Servers**: Create/update `docs/servers/{server}.md`
- **Services**: Create/update `docs/services/{service}.md`
- **Inventory**: Maintain YAML files in `inventory/`

#### 4. Service Level Agreements (Priorities)

Assess urgency internally:
- **CRITICAL**: Network outage, security breach, data loss risk (immediate action)
- **URGENT**: Major service down, affecting multiple users/services (within 1 hour)
- **HIGH**: Service degraded, single user impact (within 4 hours)
- **MEDIUM**: Configuration changes, non-urgent updates (within 24 hours)
- **LOW**: Documentation updates, optimization (best effort)

#### 5. Change Management

All changes require:
- Documentation review before starting
- Backup verification (check existing or create new)
- Impact assessment (which services/devices affected)
- Rollback plan identified
- Post-change verification steps
- Documentation updates after completion

Major changes additionally require:
- User notification of potential impact
- Extended verification period
- Clear rollback procedures documented

#### 6. Verification Requirements

The AI must never assume. Always verify:
- Container names: `docker ps --format "{{.Names}}"`
- SSH users: Check docs, then verify with `whoami`
- Service status: Check reality before troubleshooting
- File locations: Verify with `find` or check documentation
- Network configuration: Compare actual vs. documented state

#### 7. Communication Style

- Professional but conversational
- Action-oriented: "I checked X, found Y, fixed it by doing Z"
- Always show verification: "✓ Service is responding"
- No internal process explanation: Don't list which experts were consulted

## Initial Setup Process

When setting up a new Vibe Infrastructure system, the AI should:

1. **Create directory structure** as specified above
2. **Create initial template files**:
   - `docs/network/topology.md` (basic template)
   - `inventory/devices.yaml` (YAML structure template)
   - `inventory/servers.yaml` (YAML structure template)
   - `inventory/services.yaml` (YAML structure template)
   - `README.md` (project overview)
3. **Set up AI team protocol** in `.cursor/rules/networkguide.mdc` (or equivalent)
4. **Ask user for initial information**:
   - Router model and IP address
   - Access method (SSH user, web UI URL)
   - Any existing network documentation
   - Key devices or services to document first

## Documentation File Formats

### Device Documentation (`docs/devices/{device}.md`)

Required sections:
- Basic Information (type, model, location, purpose)
- Network Configuration (IP addresses, interfaces, VLANs)
- Authentication (access methods, credentials location)
- Services/Features (what the device provides)
- Configuration Details (important settings, custom configs)
- Troubleshooting (common issues and solutions)
- Last Updated timestamp

**Example Template**:

```markdown
# EdgeRouter X

## Basic Information
- **Type**: Router
- **Model**: EdgeRouter X 5-Port
- **Location**: Network closet
- **Purpose**: Primary router and firewall

## Network Configuration
- **WAN Interface**: eth0 (DHCP from ISP)
- **LAN Interface**: switch0 (192.168.1.1/24)
- **Management**: 192.168.1.1

## Authentication
- **SSH**: `ssh admin@192.168.1.1`
- **Web UI**: https://192.168.1.1
- **Credentials**: Stored in password manager

## Services/Features
- NAT
- DHCP Server (192.168.1.100-192.168.1.200)
- DNS Forwarding
- Firewall

## Configuration Details
- DHCP enabled on switch0
- NAT masquerading on eth0
- Hardware offloading enabled

## Troubleshooting
(To be populated as issues are encountered)

**Last Updated**: 2025-01-19
```

### Server Documentation (`docs/servers/{server}.md`)

Required sections:
- Basic Information (type, model, location, OS)
- Network Configuration (IP addresses, interfaces)
- Authentication (SSH user, access methods)
- Services Running (what services are hosted)
- Storage Configuration (if applicable)
- Docker/Container Setup (if applicable)
- Backup Information
- Last Updated timestamp

**Example Template**:

```markdown
# QNAP NAS

## Basic Information
- **Type**: NAS
- **Model**: QNAP TS-451
- **Location**: Network closet
- **Operating System**: QTS 5.2.4

## Network Configuration
- **Primary IP**: 192.168.1.200
- **Hostname**: NAS
- **Interfaces**: eth0 (primary), eth1 (secondary)

## Authentication
- **SSH**: `ssh admin@192.168.1.200`
- **Web UI**: https://192.168.1.200
- **Credentials**: Stored in password manager

## Services Running
- SMB/CIFS file sharing
- NFS file sharing
- WireGuard VPN server
- Transmission (torrent client)

## Storage Configuration
- **Total Storage**: 16TB
- **Used**: 8.3TB
- **Available**: 7.6TB
- **RAID**: RAID 5

## Docker/Container Setup
- Docker available via Container Station
- Containers use bridge network by default

## Backup Information
- Configuration backups: `backups/nas_configs_raw_YYYY-MM-DD.zip`
- Automated backup schedule: Weekly

**Last Updated**: 2025-01-19
```

### Service Documentation (`docs/services/{service}.md`)

Required sections:
- Description
- Network Configuration (IP, ports, domains)
- Docker Compose File Location (if containerized)
- Persistent Data Paths
- Special Notes (GPU, special configs, dependencies)
- Troubleshooting (common issues and solutions)
- Last Updated timestamp

**Example Template**:

```markdown
# Plex Media Server

## Description
Media streaming server for video, music, and photos.

## Network Configuration
- **IP Address**: 192.168.1.50 (macvlan)
- **Port**: 32400
- **Domain**: plex.example.com (via reverse proxy)

## Docker Compose File Location
- `~/dockers/plex/docker-compose.yml`

## Persistent Data Paths
- **Configuration**: `/srv/docker-data/plex/config` → `/config`
- **Transcode Cache**: `/srv/docker-data/plex/transcode` → `/transcode`
- **Media Library**: `/mnt/nas/videos` → `/videos`

## Special Notes
- GPU transcoding enabled (NVIDIA RTX 3060)
- USB device access configured to prevent libusb_init errors

## Troubleshooting
### Container Exits with "libusb_init failed" Error
**Symptom**: Container stops immediately with error "Critical: libusb_init failed"
**Solution**: Ensure USB device access is configured in docker-compose.yml:
```yaml
devices:
  - /dev/bus/usb:/dev/bus/usb
```

**Last Updated**: 2025-01-29
```

### Network Topology (`docs/network/topology.md`)

Required sections:
- Core Devices and Connections (table format)
- Logical Networks and VLANs
- IP Addressing Scheme
- Wireless Access Points (if applicable)
- SSIDs and VLAN Mapping (if applicable)

## Inventory YAML Structure

### `inventory/devices.yaml`

```yaml
---
metadata:
  last_updated: "YYYY-MM-DD"
  version: 1
  
devices:
  - name: "DeviceName"
    type: "Router|Switch|AP|etc"
    model: "Model Name"
    manufacturer: "Manufacturer"
    ip_address: "192.168.1.1"
    mac_address: "AA:BB:CC:DD:EE:FF"
    hostname: "hostname"
    location: "Physical location"
    documentation: "../docs/devices/devicename.md"
    services:
      - "Service 1"
      - "Service 2"
```

### `inventory/servers.yaml`

```yaml
---
metadata:
  last_updated: "YYYY-MM-DD"
  version: 1
  
servers:
  - name: "ServerName"
    type: "NAS|Server|VM"
    model: "Model Name"
    ip_address: "192.168.1.200"
    hostname: "hostname"
    location: "Physical location"
    documentation: "../docs/servers/servername.md"
    operating_system: "OS Version"
    services:
      - "Service 1"
      - "Service 2"
```

### `inventory/services.yaml`

```yaml
---
metadata:
  last_updated: "YYYY-MM-DD"
  version: 1
  
services:
  - name: "ServiceName"
    type: "Docker|Native|QPKG"
    host: "ServerName"
    url: "http://192.168.1.50:port"
    port: 32400
    documentation: "../docs/services/servicename.md"
    description: "What the service does"
```

## Verification Commands

The AI must use these commands to verify actual state:

### Container Verification
```bash
docker ps --format "{{.Names}}\t{{.Status}}\t{{.Ports}}"
docker ps -a --filter "name=<container_name>"
docker logs --tail 50 <container_name>
docker inspect <container_name> | grep -A 5 IPAddress
```

### Service Status
```bash
systemctl status <service_name>
curl -f http://<service_ip>:<port>
ping <device_ip>
```

### SSH Access
```bash
ssh user@host "whoami"
ssh user@host "pwd"
```

### File Locations
```bash
find ~/dockers -name "docker-compose.yml" -type f
find /opt -name "docker-compose.yml" -type f
```

## Common Patterns

### Troubleshooting Pattern

When user reports an issue:
1. Check documentation for the affected service/device
2. Verify actual state (container status, service status, connectivity)
3. Review recent changes in documentation
4. Check for similar past issues in troubleshooting sections
5. Diagnose based on actual state vs. documented state
6. Fix the issue
7. Verify the fix
8. Document the solution

### Documentation Update Pattern

When making changes:
1. Identify all files that need updating (device, server, service, topology, inventory)
2. Create backup if needed
3. Make the change
4. Verify the change
5. Update all relevant documentation files
6. Update inventory if IPs, roles, or details changed

### Learning Pattern

When encountering issues:
- **1st occurrence**: Document in service notes with date
- **2nd occurrence**: Add to troubleshooting section
- **3rd occurrence**: Add to quick diagnosis patterns in protocol

## Troubleshooting the Vibe Infrastructure System

If the AI is not behaving as expected:

### AI Not Following Protocols
- **Check**: Are the protocol files properly set up as rules?
- **Verify**: In Cursor, check `.cursor/rules/` exists and contains `networkguide.mdc` and `networkteam.mdc`
- **Verify**: For other AI assistants, check that the files are in the rules directory (not project knowledge)
- **Test**: Ask AI "What are your core workflow steps?" - it should respond with Verify → Research → Plan → Execute → Document

### Documentation Not Updating
- **Check**: Does AI have write access to `docs/` directory?
- **Verify**: Git status shows uncommitted changes after AI makes updates
- **Fix**: Ensure file permissions allow writing

### AI Not Verifying Actual State
- **Check**: Does AI have command execution capabilities?
- **Test**: Ask AI "What containers are running?" - it should run `docker ps`, not guess
- **Fix**: Enable command execution in AI assistant settings

### AI Assuming Instead of Checking
- **Remind**: Include in your request "Verify the actual state first"
- **Protocol Update**: Add emphasis to verification requirements in protocol
- **Check**: Review verification commands are accessible to AI

### Documentation Structure Wrong
- **Verify**: Directory structure matches the specification exactly
- **Check**: Paths in inventory YAML files are correct
- **Fix**: Recreate directory structure if needed

## Customization Points

The system can be customized by modifying:

1. **Team Roles**: Add or remove expert roles based on infrastructure
2. **Priority Definitions**: Adjust SLA priorities to match user needs
3. **Documentation Structure**: Modify file organization if needed
4. **Verification Commands**: Add domain-specific verification steps
5. **Workflow Steps**: Adjust the core workflow if preferred

## Questions to Ask During Initial Setup

When setting up a new system, the AI should ask:

1. **Router Information**:
   - What router model do you have?
   - What is the router's IP address?
   - How do you access it? (SSH user, web UI URL)
   - Do you have SSH keys set up?

2. **Network Structure**:
   - What is your main network IP range? (e.g., 192.168.1.0/24)
   - Do you use VLANs? If so, what VLANs and their purposes?
   - What devices are on your network?

3. **Servers**:
   - Do you have a NAS or home server?
   - What services are running? (Plex, Home Assistant, etc.)
   - Are services containerized? (Docker)

4. **Access Methods**:
   - Can you SSH to your devices?
   - Do you have SSH keys configured?
   - Are there any devices that require special access methods?

5. **Existing Documentation**:
   - Do you have any existing network documentation?
   - Are there configuration backups I should know about?

## Implementation Checklist

When implementing, ensure:

- [ ] Directory structure created
- [ ] AI team protocol configured
- [ ] Initial template files created
- [ ] Router documented (first device)
- [ ] Network topology started
- [ ] Inventory files initialized
- [ ] User understands the system is ready for first task

The system builds itself over time. Start with basics—the AI will add details as you work together.

## After Initial Setup: First Tasks

Once the initial setup is complete, start with simple documentation tasks:

### Good First Tasks
- "Document my NAS at 192.168.1.200"
- "Check if all services are running"
- "Update the network topology with my switch"
- "Add documentation for my Plex server"

### Avoid as First Tasks
- Complex migrations or reconfigurations
- Emergency troubleshooting
- Large-scale infrastructure changes

**Why**: First tasks build the documentation foundation. The system becomes more effective as it learns your infrastructure through these initial documentation tasks.

### Moving to Operational Use

After 3-5 devices/services are documented, the system is ready for:
- Troubleshooting issues
- Making configuration changes
- Updating services
- Complex tasks requiring context

The more documented, the more capable the system becomes.
