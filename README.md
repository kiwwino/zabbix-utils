# zabbix-utils

## zabbix_jsonify
Never write your own script again just to format data, if you wanna use new source for zabbix low level discovery. With zabbix_jsonify you can turn nearly every standard output to zabbix-compliant json. Simply by piping your command to zabbix_jsonify, you have your LLD ready and you can save a lot of time without debugging "Value should be a JSON object" error messages.
### Usage
```
usage: zabbix_jsonify [-h] [--delimiter DELIMITER] keyname [keyname ...]
Converts lines from standard input to JSON for Zabbix discovery.

positional arguments:
  keyname               The {#KEYNAME} that will be returned.

optional arguments:
  -h, --help            show this help message and exit
  --delimiter DELIMITER
                        Delimiter of columns. Default is space
```

### Example
```
root@localhost:~# cat /proc/mounts  | zabbix_jsonify FSTYPE FSNAME
{"data":
[
    {
        "{#FSTYPE}": "sysfs", 
        "{#FSNAME}": "/sys"
    }, 
...
   {
        "{#FSTYPE}": "tmpfs", 
        "{#FSNAME}": "/run/user/10067"
    }
]
}
```

```
root@localhost:~# cat /etc/zabbix/zabbix_agentd.d/userparameter_varnish.conf
UserParameter=varnish.backend.discovery,echo "backend.list" | sudo varnishadm |  awk 'NR>2 {print substr($$1,6)}' | zabbix_jsonify VARNISHBACKEND
```
