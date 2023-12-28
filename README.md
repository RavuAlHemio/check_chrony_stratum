# check_chrony_stratum and check_chrony_peer

`check_chrony_stratum` queries a Chrony server using its management protocol for its current stratum.

`check_chrony_peer` queries a Chrony server using its management protocol for the status of its peers.

## check_chrony_stratum Usage

```
./check_chrony_stratum -H <hostname> [-p <port>] [-W <warn>] [-C <crit>]
```

The server is chosen using the `-H <hostname>` option, which may be a hostname or an IP address.

`-p <port>` sets the port that should be contacted. Note that this is not the regular NTP port but the port for `chronyd`'s management interface, also used by `chronyc`. By default, the port is 323.

`-W <warn>` and `-C <crit>` set the thresholds for reporting a too-high stratum. If the reported stratum is greater than or equal to the given threshold, the script reports the respective (warning or critical) state. Both options are optional; if none is given, the script always reports an OK state. If the criteria for both warning and critical states are met, critical takes precedence; if both are to be used, it makes sense for the critical threshold to be greater than the warning threshold.

## check_chrony_peer Usage

```
./check_chrony_peer -H <hostname> [-p <port>] [thresholds]
```

The server is chosen using the `-H <hostname>` option, which may be a hostname or an IP address.

`-p <port>` sets the port that should be contacted. Note that this is not the regular NTP port but the port for `chronyd`'s management interface, also used by `chronyc`. By default, the port is 323.

The following thresholds can be specified:

| measurement | crit short | crit long | warn short | warn long | default | description |
| ----------- | ---------- | --------- | ---------- | --------- | ------- | ----------- |
| offset | `-c` | `--ocrit` | `-w` | `--owarn` | w=0:60 c=0:120 | the absolute time offset between Chrony and its chosen peer (in seconds) |
| jitter | `-k` | `--jcrit` | `-j` | `--jwarn` | none | the standard deviation of network packet transmission durations between Chrony and the chosen peer |
| stratum | `-C` | `--scrit` | `-W` | `--swarn` | none | the stratum of the chosen peer |
| truechimers | `-n` | `--tcrit` | `-m` | `--twarn` | none | the number of choosable peers |

If a threshold is not specified, the default threshold for that measurement is taken. If there is no default threshold for a measurement, that measurement is ignored when determining the overall status.

## Exit code

Exit codes match those expected of Nagios/Icinga check plugins:

* 0 = OK
* 1 = warning (`check_chrony_stratum`: reported stratum >= stratum warning threshold)
* 2 = critical (`check_chrony_stratum`: reported stratum >= stratum critical threshold)
* 3 = unknown (an error occurred while querying)

For `check_chrony_peer`, full thresholds (minimum and maximum) are specified; the given status is reported if the value lies outside of the boundaries given by each threshold value pair.
