# transcode-sros


## Description:
Transcode a SROS config file collected by admin display-config to a flat config file format.

This is a fork and update of the orginal work from https://github.com/door7302/transcode-sros so all praise is owed to @door7302, I've simply made the minimal required changes to get it up and working again.


## Limitations:
 * The script interpets 4 continuous spaces characters as entering a new config sub-hierarchy/sub-stanza, which means you must not use 4 continuous space characters anywhere in your actual config, e.g. a description.


## Usage:
`$ ./transcode-sros.sh <your-config-file>`
or
`$ python3 transcode-sros.sh <your-config-file> > flat-config.txt`


## Example:
```bash
$ cat example.cfg 
Generated FRI NOV 25 08:15:08 2016 UTC

exit all
configure
--------------------------------------------------
echo "System Configuration"
--------------------------------------------------
    system
        name "BOB"
        location "Bayonne Hardoy"
        chassis-mode d
        config-backup 6
        dns
        exit
        load-balancing
            l4-load-balancing
            lsr-load-balancing lbl-ip
        exit
        rollback
            rollback-location "cf3:/rollback"
            local-max-checkpoints 20
        exit
        snmp
            packet-size 9216
        exit
        time
            ntp
                server x.x.x.x prefer
                no shutdown
            exit
            sntp
                shutdown
            exit
            dst-zone CEST
                start last sunday march 02:00
                end last sunday october 03:00
            exit
            zone CET
        exit
        thresholds
            rmon
            exit
        exit
    exit
    
$ ./trancode-sros.py example.cfg
/configure system name "BOB"
/configure system location "Bayonne Hardoy"
/configure system chassis-mode d
/configure system config-backup 6
/configure system dns
/configure system load-balancing l4-load-balancing
/configure system load-balancing lsr-load-balancing lbl-ip
/configure system rollback rollback-location "cf3:/rollback"
/configure system rollback local-max-checkpoints 20
/configure system snmp packet-size 9216
/configure system time ntp server x.x.x.x prefer
/configure system time ntp no shutdown
/configure system time sntp shutdown
/configure system time dst-zone CEST start last sunday march 02:00
/configure system time dst-zone CEST end last sunday october 03:00
/configure system time zone CET
/configure system thresholds rmon
```
