# Process Exporter

A Prometheus exporter for per-process and system-level Linux metrics. It exposes
detailed CPU, memory, I/O, file descriptor, page fault, context switch, OOM,
network, and TCP/UDP connection metrics for monitored processes, plus
system-wide Pressure Stall Information (PSI), over HTTP for Prometheus to scrape.

Almost every metric can be toggled individually via flags (all enabled by
default), so you can keep the exported set as lean or as detailed as you need.

---

## Features

- **Per-process metrics**
  - CPU: percentage, user time, system time
  - Memory: RSS, VMS, swap, percentage, heap, stack
  - I/O: read/write bytes and counts, block I/O wait ticks
  - File descriptors and thread counts
  - Context switches: voluntary and involuntary
  - Page faults: minor, major, child minor, child major
  - Scheduling: nice value
  - OOM: oom_score, oom_score_adj, oom_adj
  - Heap start address / start_brk
- **Network & connection metrics (per process)**
  - Connection counts by state: ESTABLISHED, LISTEN, TIME_WAIT, CLOSE_WAIT,
    SYN_SENT, SYN_RECV
  - TCP, TCP6, UDP, UDP6, and total connection counts
  - TCP RX/TX queue bytes
  - Network in/out octets
  - TCP internals: retransmits (fast/lost), timeouts, SACK recovery, delivered,
    original data sent, out-of-order queue, receive coalesce, fast-path hits,
    delayed ACKs / delayed ACK lost, abort-on-close, abort-on-data
- **System-level metrics (always enabled)**
  - Pressure Stall Information (PSI) for CPU, I/O, and memory
- Optional HTTP basic authentication
- Configurable listening port

---

## Requirements

- Linux (amd64 build provided: `process_exporter_amd64`)
- **Linux kernel 4.20+** for PSI (Pressure Stall Information) metrics
- Prometheus, to scrape the exporter

---

## Installation

```bash
chmod +x process_exporter_amd64
./process_exporter_amd64
```

The exporter listens on port `9414` by default and serves metrics at `/metrics`.

```bash
curl http://localhost:9414/metrics
```

> Note: the metrics path is `/metrics` by Prometheus convention. Adjust if your
> build differs.

---

## Usage

```bash
./process_exporter_amd64 [options]
```

> Note: this build does not support a `-version` flag. Run without arguments to
> start with defaults, or `-h` / `--help` to list all flags.

### Common examples

Run with defaults (all metrics enabled, port 9414):

```bash
./process_exporter_amd64
```

Monitor specific processes and run on a custom port:

```bash
./process_exporter_amd64 -cmdname /etc/process-exporter/cmdname.txt -port 9500
```

With basic auth:

```bash
./process_exporter_amd64 -authfile /etc/process-exporter/auth.txt
```

Trim the exported set (example: disable the detailed TCP-internals metrics):

```bash
./process_exporter_amd64 \
  -enable-net-tcp-fast-retrans=false \
  -enable-net-tcp-lost-retransmit=false \
  -enable-net-tcp-sack-recovery=false \
  -enable-net-tcp-ofo-queue=false
```

---

## Configuration

### General

| Flag | Default | Description |
|------|---------|-------------|
| `-port int` | `9414` | Port to run the HTTP server on |
| `-authfile string` | `auth.txt` | File containing `username:password` for basic auth |
| `-cmdname string` | `cmdname.txt` | File listing process names to monitor (optional) |

### Process name file (`-cmdname`)

A plain text file listing the process names to monitor, one per line:

```
nginx
asterisk
postgres
```

The names must match the process **command name** as reported by the kernel in
`/proc/<PID>/comm`. To find the correct name for a running process, read that
file using its PID:

```bash
cat /proc/<PID>/comm
```

For example:

```bash
$ cat /proc/1017662/comm
asterisk
```

The value printed (`asterisk` here) is exactly what you put in `cmdname.txt`.
Add one name per line for every process you want to monitor:

```
name1
name2
...
nameN
```

Tip — if you know the binary but not the PID, you can look up the comm name
directly:

```bash
# get the PID(s) first
pgrep nginx

# then read comm for one of them
cat /proc/$(pgrep -n nginx)/comm
```

Note: `comm` is limited by the kernel to 15 characters, so long process names
appear truncated there — use the truncated value as it appears in
`/proc/<PID>/comm`.

### CPU metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-cpu-percent` | `true` | CPU percentage |
| `-enable-cpu-user` | `true` | CPU user time |
| `-enable-cpu-system` | `true` | CPU system time |

### Memory metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-memory-rss` | `true` | Memory RSS bytes |
| `-enable-memory-vms` | `true` | Memory VMS bytes |
| `-enable-memory-swap` | `true` | Memory swap bytes |
| `-enable-memory-percent` | `true` | Memory percentage |
| `-enable-heap-bytes` | `true` | Heap memory size |
| `-enable-stack-bytes` | `true` | Stack memory size |
| `-enable-start-brk` | `true` | Heap start address |

### I/O metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-io-reads-bytes` | `true` | I/O read bytes |
| `-enable-io-reads-count` | `true` | I/O read count |
| `-enable-io-writes-bytes` | `true` | I/O write bytes |
| `-enable-io-writes-count` | `true` | I/O write count |
| `-enable-blkio-ticks` | `true` | Block I/O wait ticks |

### Process state metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-fds` | `true` | Number of file descriptors |
| `-enable-threads` | `true` | Number of threads |
| `-enable-nice` | `true` | Nice value |
| `-enable-ctx-switches-voluntary` | `true` | Voluntary context switches |
| `-enable-ctx-switches-involuntary` | `true` | Involuntary context switches |

### Page fault metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-page-faults-minor` | `true` | Minor page faults |
| `-enable-page-faults-major` | `true` | Major page faults |
| `-enable-page-faults-child-minor` | `true` | Child minor page faults |
| `-enable-page-faults-child-major` | `true` | Child major page faults |

### OOM metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-oom-score` | `true` | OOM score |
| `-enable-oom-score-adj` | `true` | OOM score adjustment |
| `-enable-oom-adj` | `true` | OOM adjustment |

### Connection count metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-conn-total` | `true` | Total connections count |
| `-enable-conn-tcp` | `true` | TCP connections count |
| `-enable-conn-tcp6` | `true` | TCP6 connections count |
| `-enable-conn-udp` | `true` | UDP connections count |
| `-enable-conn-udp6` | `true` | UDP6 connections count |
| `-enable-conn-established` | `true` | ESTABLISHED connections count |
| `-enable-conn-listen` | `true` | LISTEN connections count |
| `-enable-conn-time-wait` | `true` | TIME_WAIT connections count |
| `-enable-conn-close-wait` | `true` | CLOSE_WAIT connections count |
| `-enable-conn-syn-sent` | `true` | SYN_SENT connections count |
| `-enable-conn-syn-recv` | `true` | SYN_RECV connections count |
| `-enable-conn-rx-queue` | `true` | TCP RX queue bytes |
| `-enable-conn-tx-queue` | `true` | TCP TX queue bytes |

### Network & TCP internals metrics

| Flag | Default | Description |
|------|---------|-------------|
| `-enable-net-in-octets` | `true` | Network input octets |
| `-enable-net-out-octets` | `true` | Network output octets |
| `-enable-net-delayed-acks` | `true` | Delayed ACKs |
| `-enable-net-delayed-ack-lost` | `true` | Delayed ACK lost |
| `-enable-net-tcp-delivered` | `true` | TCP delivered |
| `-enable-net-tcp-orig-data-sent` | `true` | TCP original data sent |
| `-enable-net-tcp-fast-retrans` | `true` | TCP fast retransmits |
| `-enable-net-tcp-lost-retransmit` | `true` | TCP lost retransmits |
| `-enable-net-tcp-timeouts` | `true` | TCP timeouts |
| `-enable-net-tcp-sack-recovery` | `true` | TCP SACK recovery |
| `-enable-net-tcp-ofo-queue` | `true` | TCP out-of-order queue |
| `-enable-net-tcp-rcv-coalesce` | `true` | TCP receive coalesce |
| `-enable-net-tcp-hp-hits` | `true` | TCP fast path hits |
| `-enable-net-tcp-abort-on-close` | `true` | TCP abort on close |
| `-enable-net-tcp-abort-on-data` | `true` | TCP abort on data |

---

## System-Level Metrics (PSI)

These are **always enabled** and require no flags. They report kernel
Pressure Stall Information, which quantifies time tasks were stalled waiting on
a resource.

| Metric | Description |
|--------|-------------|
| `system_pressure_cpu_some_avg10` / `avg60` / `avg300` / `total` | CPU pressure (some) |
| `system_pressure_io_some_avg10` / `avg60` / `avg300` / `total` | I/O pressure (some) |
| `system_pressure_io_full_avg10` / `avg60` / `avg300` / `total` | I/O pressure (full) |
| `system_pressure_memory_some_avg10` / `avg60` / `avg300` / `total` | Memory pressure (some) |
| `system_pressure_memory_full_avg10` / `avg60` / `avg300` / `total` | Memory pressure (full) |

> Requires **Linux kernel 4.20+**. On older kernels PSI is unavailable and these
> metrics will not be populated.

---

## Basic Authentication

Create an auth file containing a single `username:password` line:

```bash
echo "prometheus:s3cr3t" > /etc/process-exporter/auth.txt
chmod 600 /etc/process-exporter/auth.txt
```

```bash
./process_exporter_amd64 -authfile /etc/process-exporter/auth.txt
```

---

## Running as a systemd Service

Create `/etc/systemd/system/process-exporter.service`:

```ini
[Unit]
Description=Process Exporter
After=network.target

[Service]
ExecStart=/usr/local/bin/process_exporter_amd64 -port 9414
Restart=on-failure
User=root

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now process-exporter
sudo systemctl status process-exporter
```

> Note: many per-process metrics (I/O, file descriptors, OOM, connection details)
> require sufficient privileges to read `/proc/<pid>/...` for the target
> processes. Running as `root` is the simplest way to ensure full coverage.

---

## Prometheus Scrape Configuration

```yaml
scrape_configs:
  - job_name: process_exporter
    static_configs:
      - targets: ["localhost:9414"]
```

With basic auth:

```yaml
scrape_configs:
  - job_name: process_exporter
    basic_auth:
      username: prometheus
      password: s3cr3t
    static_configs:
      - targets: ["localhost:9414"]
```

---

## License

This is **proprietary freeware**. The source code is **not** provided and
remains the property of the author.

You may download, install, and run this binary free of charge for both personal
and commercial use. You may **not** reverse engineer, modify, redistribute,
resell, or sublicense it. See the [LICENSE](LICENSE) file for the full terms.

The software is provided "as is", without warranty of any kind.

## Support

This project does not provide source code or accept code contributions. For
bug reports or feature requests, please open an issue.
