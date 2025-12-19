# Vibe Infrastructure

> **Conversational AI for Home Network Management**  
> Manage your network through natural language while AI maintains comprehensive documentation as the source of truth.

**Version 1.0** | Released December 19, 2025

---

## What Is This?

Vibe Infrastructure is a system that lets you manage your home network by talking to AI in plain English. The AI maintains detailed documentation about your network, and that documentation becomes the "infrastructure"—always up-to-date, always accurate.

Instead of remembering IP addresses, hunting for configuration files, or googling error messages, you just ask:
- "Why is my media server slow?"
- "Update all my Docker containers"
- "Can't access file shares from Windows—what's wrong?"

The AI figures it out, fixes it, documents it, and learns from it.

**Based on 6+ months of real-world use** managing a complex home network with multiple VLANs, 20+ Docker containers, and 50+ IoT devices.

---

## Quick Start

### Option 1: Give AI the GitHub URL (Easiest)

Modern AI assistants can read GitHub repositories directly:

```
You: "I want to implement Vibe Infrastructure from 
     https://github.com/yourusername/vibe-infrastructure
     Help me set it up for my home network."

AI: [reads the repo, asks about your setup, creates structure]
```

### Option 2: Manual Setup (More Control)

1. **Read the Overview** - Understand what you're building  
   → `OVERVIEW.md`

2. **Read the Implementation Guide** - Technical details  
   → `IMPLEMENTATION_GUIDE.md`

3. **Set up AI protocols** - Copy rules to your AI assistant  
   → `networkguide.mdc` and `networkteam.mdc`

4. **Start building** - Document your first device

**Time to working system: ~1 hour**

---

## What's In This Repo

| File | Purpose | Who Reads It |
|------|---------|--------------|
| **OVERVIEW.md** | What Vibe Infrastructure is, how it works, why it works | You (first), then optionally your AI |
| **IMPLEMENTATION_GUIDE.md** | Step-by-step technical implementation | You + AI (to build the system) |
| **networkguide.mdc** | AI interaction rules and protocols | AI only (core behavior) |
| **networkteam.mdc** | AI team structure and workflows | AI only (team coordination) |

---

## Installation

### For Cursor IDE

```bash
# In your project directory:
mkdir -p .cursor/rules
cp networkguide.mdc .cursor/rules/
cp networkteam.mdc .cursor/rules/

# Create documentation structure:
mkdir -p docs/{network/configs,devices,servers,services}
mkdir -p inventory scripts diagnostics
```

Then tell Cursor: *"Help me set up Vibe Infrastructure. I've loaded the protocol files."*

### For Claude.ai or Claude Desktop

1. Create a new project called "HomeNetwork" (or similar)
2. Upload `networkguide.mdc` and `networkteam.mdc` as project knowledge
3. In your first conversation, say: *"I want to implement Vibe Infrastructure using the protocols I've uploaded. I have a [router brand] at [IP] to start with."*

### For Other AI Assistants

- Provide both `.mdc` files as system instructions or custom instructions
- Ensure your AI has: file read/write, command execution, SSH access (if managing remote devices)
- Follow the Implementation Guide to create directory structure

---

## How It Works

### Documentation as Infrastructure

Every device, service, and configuration is documented in structured markdown and YAML files. This documentation IS your infrastructure—not a description of it.

```
HomeNetwork/
├── docs/
│   ├── network/          # Topology, VLANs, routing
│   │   └── configs/      # Configuration backups
│   ├── devices/          # Router, switches, APs
│   ├── servers/          # NAS, servers
│   └── services/         # Docker containers, apps
├── inventory/            # YAML: devices, IPs, services
└── scripts/              # Automation scripts
```

### AI Team Model

The AI operates as a professional IT services company with specialized roles:
- **Service Delivery Manager** - Coordinates all work
- **Senior Network Engineer** - Routing, switching, VLANs
- **Senior DevOps Engineer** - Containers, orchestration
- **Storage Systems Administrator** - NAS, storage
- **Network Security Engineer** - Firewalls, security
- **IoT & Automation Engineer** - Smart home devices
- *+ 3 more specialists*

The team coordinates **internally**—you just see results.

### Process Over Hardcoding

The AI never assumes. It always verifies actual state:

```python
# Traditional automation:
container_name = "plex"  # Hope this is right
docker restart plex

# Vibe Infrastructure:
containers = run("docker ps --format '{{.Names}}'")  # Check reality
actual_name = find_container(containers, service="plex")
if actual_name != documented_name:
    update_docs(actual_name)  # Self-correct
docker restart actual_name
```

This dual-check approach (documentation + reality) prevents failures when things drift.

---

## Real-World Example

**You:** "Can't access Plex—what's wrong?"

**AI:**  
```
[Checks documentation for Plex service]
[Verifies container status: docker ps]
[Reviews recent changes]

Found the issue - container is running but port mapping is incorrect.

[Updates docker-compose.yml]
[Recreates container]
[Tests accessibility]

✓ Plex accessible at https://plex.yourdomain.com
✓ Documentation updated
✓ Added to troubleshooting guide
```

All conversational. No scripts to write. No configuration to remember.

---

## What Makes This Different

| Traditional | Vibe Infrastructure |
|-------------|---------------------|
| Documentation gets outdated | Documentation is always current (AI maintains it) |
| Scripts/automation break when things change | Self-corrects by verifying actual state |
| Knowledge lives in your head | Knowledge lives in documentation, builds over time |
| Complex tools for simple tasks | Natural language for everything |
| Always-on monitoring overhead | On-demand, zero overhead when idle |

---

## Key Features

✅ **Conversational Interface** - Plain English, no special syntax  
✅ **Self-Documenting** - AI maintains docs as it works  
✅ **Self-Correcting** - Detects drift between docs and reality  
✅ **Self-Improving** - Learns patterns, gets better over time  
✅ **On-Demand** - Activates when needed, zero overhead otherwise  
✅ **Professional Standards** - IT service company workflows at home scale  
✅ **Change Management** - Backups, verification, rollback plans  
✅ **Multi-Specialist** - 8 specialized AI experts coordinate internally  

---

## Requirements

**Your Network:**
- At least a router (to document)
- SSH access to devices (optional, for remote management)
- Docker (optional, for container management)

**Your AI Assistant:**
- File read/write capabilities
- Command execution (bash, SSH)
- 100K+ token context window (most modern AIs)

**You:**
- Basic understanding of your network
- Willingness to answer questions during setup
- Patience as system builds knowledge (first 5-10 tasks)

---

## Example Use Cases

### Simple Status Checks
```
"Is the media server running?"
"Check if all containers are healthy"
"What's using all the disk space?"
```

### Troubleshooting
```
"Can't access file shares from Windows"
"Why is the network slow?"
"Camera keeps going offline"
```

### Configuration Changes
```
"Update all Docker containers"
"Add a new VLAN for IoT devices"
"Change the WiFi password"
```

### Documentation Tasks
```
"Document my NAS at 192.168.1.200"
"Create network topology diagram"
"What services are running on the NAS?"
```

---

## Customization

The protocol files (`networkguide.mdc` and `networkteam.mdc`) are designed to work out-of-the-box but can be customized:

**Safe to customize:**
- Priority levels (SLA timeframes)
- Team roles (add/remove specialists)
- Documentation structure
- Verification commands
- Quick diagnosis patterns

**Keep intact (core functionality):**
- Verification-first principles
- Change management discipline
- Response structure
- Learning mechanisms

As you use the system, it naturally documents YOUR specific hardware—your router model, NAS brand, IoT platform, services, and configurations.

---

## Troubleshooting

**AI not following protocols?**
- Verify protocol files are loaded in `.cursor/rules/` (Cursor)
- Check that AI has access to uploaded files (Claude projects)
- Ask: "What are your core workflow steps?" (Should respond: Verify → Research → Plan → Execute → Document)

**Documentation not updating?**
- Check file write permissions
- Verify `docs/` directory exists
- Review recent Git commits to see if changes are being tracked

**AI not verifying actual state?**
- Confirm AI has command execution enabled
- Test: "What containers are running?" (Should run `docker ps`, not guess)

See `IMPLEMENTATION_GUIDE.md` for complete troubleshooting section.

---

## Limitations

**This is NOT:**
- ❌ A monitoring system (no 24/7 alerts)
- ❌ A backup solution (enforces backups, doesn't create them)
- ❌ Magic (requires accurate initial documentation)
- ❌ Fully autonomous (you remain in control)
- ❌ Just documentation (actively manages infrastructure)

**This IS:**
- ✅ On-demand intelligence
- ✅ Conversational management
- ✅ Self-maintaining documentation
- ✅ Professional workflows at home scale

---

## Philosophy

### Documentation as Infrastructure
Documentation isn't a description of infrastructure—it IS the infrastructure. The AI reads it to understand your network and updates it as things change.

### Process Over Hardcoding  
Never assume, always verify. Compare documentation against actual state, self-correct when drift is detected.

### Institutional Knowledge
Every task builds knowledge:
- 1st occurrence → Documented in notes
- 2nd occurrence → Added to troubleshooting
- 3rd occurrence → Quick diagnosis pattern

Knowledge accumulates, making the system more effective over time.

---

## Contributing

Found improvements while using Vibe Infrastructure? Share them!

- Document what you changed in your protocols
- Note how it improved the system
- Open an issue or PR with patterns that might help others

This system gets better as people use it and share learnings.

---

## Credits

Vibe Infrastructure combines:
- Modern AI capabilities (large context windows, function calling, reasoning)
- Professional IT service company workflows
- Documentation-as-code principles  
- Vibe coding philosophy ([Andrej Karpathy, 2025](https://x.com/karpathy/status/1878176549316649299))

---

## License

The Unlicense (Public Domain) - See LICENSE file

---

## Getting Started

1. **Read `OVERVIEW.md`** - Understand the concept (10 min)
2. **Skim `IMPLEMENTATION_GUIDE.md`** - Technical details (20 min)
3. **Set up AI protocols** - Load the `.mdc` files (5 min)
4. **Create directory structure** - `docs/`, `inventory/`, etc. (2 min)
5. **Document first device** - Your router (15 min)

**Total: ~1 hour to working system**

After that, the system builds itself through your interactions. Each task adds knowledge, making future tasks faster and more effective.

---

## Support

- **Issues**: Open an issue for bugs or questions
- **Discussions**: Share your setup, ask for advice
- **Wiki**: Community tips and advanced configurations

---

**Ready to manage your network through conversation?**

Clone this repo, load the protocols, and start talking to your AI about your network. It's that simple.
