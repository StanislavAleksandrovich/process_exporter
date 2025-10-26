```
Monitor linux processes
./process_exporter -authfile auth.txt -cmdname cmdname.txt -enable-cpu-percent=true -enable-memory-rss=false

Monitor all processes if cmdname.txt doesn't exist or is empty
Monitor specific processes by listing them in cmdname.txt
Use the -cmdname flag to specify a different file name

get process name
cat /proc/PID/stat | awk '{print $2}' | sed 's/^(//;s/)$//'

cmdname.txt
processname1
processname2
processname3
.
.
.
processnameN

auth.txt format
username:password


Help menu
  -authfile string
    	File containing the username:password for basic auth (default "auth.txt")
  -cmdname string
    	File containing the process names to monitor (optional) (default "cmdname.txt")
  -enable-blkio-ticks
    	Enable block I/O wait ticks metric (default true)
  -enable-conn-close-wait
    	Enable CLOSE_WAIT connections count metric (default true)
  -enable-conn-established
    	Enable ESTABLISHED connections count metric (default true)
  -enable-conn-listen
    	Enable LISTEN connections count metric (default true)
  -enable-conn-rx-queue
    	Enable TCP RX queue bytes metric (default true)
  -enable-conn-syn-recv
    	Enable SYN_RECV connections count metric (default true)
  -enable-conn-syn-sent
    	Enable SYN_SENT connections count metric (default true)
  -enable-conn-tcp
    	Enable TCP connections count metric (default true)
  -enable-conn-tcp6
    	Enable TCP6 connections count metric (default true)
  -enable-conn-time-wait
    	Enable TIME_WAIT connections count metric (default true)
  -enable-conn-total
    	Enable total connections count metric (default true)
  -enable-conn-tx-queue
    	Enable TCP TX queue bytes metric (default true)
  -enable-conn-udp
    	Enable UDP connections count metric (default true)
  -enable-conn-udp6
    	Enable UDP6 connections count metric (default true)
  -enable-cpu-percent
    	Enable CPU percentage metric (default true)
  -enable-cpu-system
    	Enable CPU system time metric (default true)
  -enable-cpu-user
    	Enable CPU user time metric (default true)
  -enable-ctx-switches-involuntary
    	Enable involuntary context switches metric (default true)
  -enable-ctx-switches-voluntary
    	Enable voluntary context switches metric (default true)
  -enable-fds
    	Enable number of file descriptors metric (default true)
  -enable-heap-bytes
    	Enable heap memory size metric (default true)
  -enable-io-reads-bytes
    	Enable IO read bytes metric (default true)
  -enable-io-reads-count
    	Enable IO read count metric (default true)
  -enable-io-writes-bytes
    	Enable IO write bytes metric (default true)
  -enable-io-writes-count
    	Enable IO write count metric (default true)
  -enable-memory-percent
    	Enable memory percentage metric (default true)
  -enable-memory-rss
    	Enable memory RSS bytes metric (default true)
  -enable-memory-swap
    	Enable memory swap bytes metric (default true)
  -enable-memory-vms
    	Enable memory VMS bytes metric (default true)
  -enable-net-delayed-ack-lost
    	Enable delayed ACK lost metric (default true)
  -enable-net-delayed-acks
    	Enable delayed ACKs metric (default true)
  -enable-net-in-octets
    	Enable network input octets metric (default true)
  -enable-net-out-octets
    	Enable network output octets metric (default true)
  -enable-net-tcp-abort-on-close
    	Enable TCP abort on close metric (default true)
  -enable-net-tcp-abort-on-data
    	Enable TCP abort on data metric (default true)
  -enable-net-tcp-delivered
    	Enable TCP delivered metric (default true)
  -enable-net-tcp-fast-retrans
    	Enable TCP fast retransmits metric (default true)
  -enable-net-tcp-hp-hits
    	Enable TCP fast path hits metric (default true)
  -enable-net-tcp-lost-retransmit
    	Enable TCP lost retransmits metric (default true)
  -enable-net-tcp-ofo-queue
    	Enable TCP out-of-order queue metric (default true)
  -enable-net-tcp-orig-data-sent
    	Enable TCP original data sent metric (default true)
  -enable-net-tcp-rcv-coalesce
    	Enable TCP receive coalesce metric (default true)
  -enable-net-tcp-sack-recovery
    	Enable TCP SACK recovery metric (default true)
  -enable-net-tcp-timeouts
    	Enable TCP timeouts metric (default true)
  -enable-nice
    	Enable nice value metric (default true)
  -enable-oom-adj
    	Enable OOM adjustment metric (default true)
  -enable-oom-score
    	Enable OOM score metric (default true)
  -enable-oom-score-adj
    	Enable OOM score adjustment metric (default true)
  -enable-page-faults-child-major
    	Enable child major page faults metric (default true)
  -enable-page-faults-child-minor
    	Enable child minor page faults metric (default true)
  -enable-page-faults-major
    	Enable major page faults metric (default true)
  -enable-page-faults-minor
    	Enable minor page faults metric (default true)
  -enable-stack-bytes
    	Enable stack memory size metric (default true)
  -enable-start-brk
    	Enable heap start address metric (default true)
  -enable-threads
    	Enable number of threads metric (default true)
  -port int
    	Port to run the server on (default 9414)

System-Level Metrics (always enabled, no flags):
  Pressure Stall Information (PSI) Metrics:
    - system_pressure_cpu_some_avg10/avg60/avg300/total 
    - system_pressure_io_some_avg10/avg60/avg300/total 
    - system_pressure_io_full_avg10/avg60/avg300/total 
    - system_pressure_memory_some_avg10/avg60/avg300/total 
    - system_pressure_memory_full_avg10/avg60/avg300/total 
  PSI metrics requires Linux kernel 4.20+

```
