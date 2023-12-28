# check_chrony_stratum

Queries a Chrony server using its management protocol for its current stratum.

## Usage

```
./check_chrony_stratum -H <hostname> [-p <port>] [-W <warn>] [-C <crit>]
```

The server is chosen using the `-H <hostname>` option, which may be a hostname or an IP address.

`-p <port>` sets the port that should be contacted. Note that this is not the regular NTP port but the port for `chronyd`'s management interface, also used by `chronyc`. By default, the port is 323.

`-W <warn>` and `-C <crit>` set the thresholds for reporting a too-high stratum. If the reported stratum is greater than or equal to the given threshold, the script reports the respective (warning or critical) state. Both options are optional; if none is given, the script always reports an OK state. If the criteria for both warning and critical states are met, critical takes precedence; if both are to be used, it makes sense for the critical threshold to be greater than the warning threshold.

## Exit code

Exit codes match those expected of Nagios/Icinga check plugins:

* 0 = OK
* 1 = warning (reported stratum >= stratum warning threshold)
* 2 = critical (reported stratum >= stratum critical threshold)
* 3 = unknown (an error occurred while querying)
