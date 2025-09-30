# MCP HelloWorld for Code Exchange – integrated MCP Inspector Demo
MCP HelloWorld for Code Exchange with web MCP Inspector. This repo contains a tiny FastMCP server and a simple demo that you can run locally or instantly in Cisco DevNet DevEnv. It showcases three small tools that are handy for quick testing and network workflows: `roll_dice` (random dice), `nslookup` (DNS lookup wrapper), and `cidr_info_ipv4` (IPv4 subnet facts). Use the "Run MCP Inspector IDE in Cloud" button to launch a preconfigured environment, then open the included MCP Inspector to invoke the tools. A short internal demo recording is linked below for a quick walkthrough.

Cisco-internal demo: https://app.vidcast.io/share/0a514521-2c77-4580-a2dd-69532c8bfeb3

**Note: Demo video instructions are based on older MCP Inspector IDE, now need fewer steps(please refer next section for updated steps)**

## Try it in Cisco DevNet DevEnv (no local setup)
Click to launch a browser-based environment with this repo pre-cloned and FastMCP preinstalled:

[![Run MCP Inspector IDE in Cloud](assets/run-in-cloud-ide.svg)](https://testing-developer.cisco.com/devenv/?id=devenv-base-mcp-inspector&GITHUB_SOURCE_REPO=https://github.com/npateriya/mcp-helloworld-code-exch)

Note: This DevEnv link is Cisco-internal.

Once the DevEnv opens:
- Change working dir `cd mcp-helloworld-code-exch`
- Install requirements `pip install -r requirements.txt`
- Start the server (HTTP on 9000) if needed: `python demo_mcp.py`
  - MCP server endpoint: `http://127.0.0.1:9000/mcp` (transport: streamable-http)
- On right side of IDE in MCP Inspector, configure below settings and `Connect` to local MCP server.
  - Transport Type : Streamable HTTP
  - URL : http://127.0.0.1:9000/mcp
- Go to tools tab and after listing tool, explore and try out these tools
  - _roll_dice
  - _nslookup
  - _cidr_info_ipv4

### Local run (quick)
If you prefer local instead of DevEnv:
```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python demo_mcp.py
```



## What this demo provides
- **roll_dice(sides=6, rolls=1)**: Returns individual dice results and total.
- **nslookup(name, record?, server?, timeout=5.0)**: Wraps the system `nslookup` for quick DNS checks. Returns `ok`, `exit_code`, `command`, `stdout`, `stderr`.
- **cidr_info_ipv4(cidr)**: IPv4-only CIDR facts like netmask, wildcard, first/last IP, and total addresses.

## Requirements
- `nslookup` available on your system (macOS has it by default). Verify with: `which nslookup`.

## Quick start (venv)
```bash
cd mcp-helloworld-code-exch
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -U pip wheel setuptools
python -m pip install -r requirements.txt
```

## Run as HTTP (Streamable) on port 9000
The script is configured to start an HTTP MCP server on `http://127.0.0.1:9000/mcp`.

```bash
source .venv/bin/activate
python demo_mcp.py
```

Health checks / quick probes:
- Base server: `curl -i http://127.0.0.1:9000/`
- MCP endpoint (expects MCP client payloads): `curl -i http://127.0.0.1:9000/mcp` (GET may return 406; that still confirms the endpoint is up.)


## Tool reference
### cidr_info_ipv4
Input:
```json
{ "cidr": "10.0.0.0/24" }
```
Output:
```json
{
  "cidr": "10.0.0.0/24",
  "netmask": "255.255.255.0",
  "wildcard": "0.0.0.255",
  "first_ip": "10.0.0.0",
  "last_ip": "10.0.0.255",
  "total_hosts": 256
}
```

### nslookup
Inputs (examples):
```json
{ "name": "example.com" }
{ "name": "example.com", "record": "AAAA" }
{ "name": "example.com", "record": "A", "server": "8.8.8.8" }
```

### roll_dice
Input:
```json
{ "rolls": 3 }
```
Output:
```json
{ "sides": 6, "rolls": 3, "results": [2, 6, 5], "total": 13 }
```

