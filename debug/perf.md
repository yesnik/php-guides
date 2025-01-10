# Perf

Installation:

```
apt install linux-tools-generic
```

Usage:

```
perf stat \
    -e cycles \
    -e instructions \
    -e cache-references \
    -e cache-misses \
    -e cpu_core/L1-dcache-loads/ \
    -e cpu_core/L1-dcache-load-misses/ \
    -e cpu_core/LLC-loads/ \
    -e cpu_core/LLC-load-misses/ \
    -e cpu_core/dTLB-load-misses/ \
    php script.php
```
