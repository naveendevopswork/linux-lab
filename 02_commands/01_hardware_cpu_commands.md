# Linux Command Reference – Hardware and CPU

Read-only inspection commands for RHEL/CentOS-style systems. Some commands may require packages such as `sysstat` or `dmidecode`.

## CPU overview
```bash
lscpu
```
Displays architecture, logical CPU count, sockets, cores per socket, threads per core, NUMA topology and CPU model details.

## CPU cache
```bash
lscpu | grep -i cache
```
## Processor details from procfs
```bash
cat /proc/cpuinfo
grep -E 'processor|physical id|core id|cpu cores|siblings|model name|cache size' /proc/cpuinfo
```
Fields and their interpretation vary by architecture and virtualized environment.

## Processing units available to the current process
```bash
nproc
```
This can be lower than the count shown by `lscpu` if CPU affinity or resource restrictions apply.

## Online CPUs
```bash
cat /sys/devices/system/cpu/online
```

## Virtualization detection
```bash
systemd-detect-virt
lscpu | grep -i hypervisor
```
`none` means no supported environment was detected; it is not conclusive proof of bare metal. `Virtualization: VT-x` indicates a CPU capability, not proof that the current OS is a VM.

## CPU utilization
```bash
top
top -b -n 1 | head -20
mpstat -P ALL 1
```
`mpstat` requires the `sysstat` package. Do not install packages on production servers without authorization.

## DMI / hardware inventory
```bash
sudo dmidecode -t processor
sudo dmidecode -t system
sudo dmidecode -t bios
```
These may require root and may show virtualized or incomplete information inside a VM.

## Quick check block
```bash
echo "===== OS ====="
cat /etc/os-release

echo "===== CPU SUMMARY ====="
lscpu

echo "===== CPU TOPOLOGY ====="
lscpu -e=CPU,SOCKET,CORE,NODE,ONLINE

echo "===== CPU CACHE ====="
lscpu -C

echo "===== ONLINE CPUS ====="
cat /sys/devices/system/cpu/online

echo "===== PROCESSING UNITS ====="
nproc

echo "===== VIRTUALIZATION ====="
systemd-detect-virt
```
- These are inspection commands; they do not change CPU configuration.
