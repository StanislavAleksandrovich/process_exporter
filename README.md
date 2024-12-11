```
Monitor linux processes
./process_exporter -authfile auth.txt -cmdname cmdname.txt -enable-cpu-percent=true -enable-memory-rss=false

Monitor all processes if cmdname.txt doesn't exist or is empty
Monitor specific processes by listing them in cmdname.txt
Use the -cmdname flag to specify a different file name

Help menu
  -authfile string
    	File containing the username:password for basic auth (default "auth.txt")
  -cmdname string
    	File containing the process names to monitor (optional) (default "cmdname.txt")
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
  -enable-nice
    	Enable nice value metric (default true)
  -enable-threads
    	Enable number of threads metric (default true)
  -port int
    	Port to run the server on (default 9414)


auth.txt format
username:password

```
