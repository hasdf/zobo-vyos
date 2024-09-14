# ZoBo - Zone Based Firewall Bootstrapper for VyOS

ZoBo helps you bootstrap your VyOS Zone-Based Firewall through an easy config file to get you up and running asap.

## Running

### Requirements
Didn't get it to work an Mac OS with M2. Had to use windows... 

- You need to have the [Dotnet Core SDK 3.0](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-3.0.103-windows-x64-installer) installed!

- Edit zones.yaml example file

### From Source


```sh
git clone https://github.com/hasdf/zobo-vyos
cd zobo-vyos
dotnet restore
dotnet run
```


## Example

### zones.yaml
```yaml
zones:
  - wan
  - local
  - lan
  - mgmt

definitions:
  wan:
    interface: ["eth0"]
    description: "WAN Network"
    allow_ping_to: "local"
    allow_traffic_to:
      local:
        ports: ["22"]
  local:
    description: "Local Zone"
    is_local_zone: true
    allow_traffic_to: "*"
  lan:
    description: "LAN Network"
    interface: ["eth1"]
    allow_traffic_to:
      local:
        # Whitelist DNS
        ports: ["53/tcp_udp"]
      wan:
  mgmt:
    description: "Management Network"
    interface: ["eth1.1"]
    allow_ping_to: "*"
    allow_traffic_to:
      # Allow SSH to any zone
      "*":
        ports: ["22"]
      wan:
```

### Output
```bash
set firewall zone 'wan' default-action 'drop'
set firewall zone 'wan' description 'WAN Network'
set firewall zone 'wan' interface 'eth0'
set firewall zone 'local' default-action 'drop'
set firewall zone 'local' description 'Local Zone'
set firewall zone 'local' local-zone
set firewall zone 'lan' default-action 'drop'
set firewall zone 'lan' description 'LAN Network'
set firewall zone 'lan' interface 'eth1'
set firewall zone 'mgmt' default-action 'drop'
set firewall zone 'mgmt' description 'Management Network'
set firewall zone 'mgmt' interface 'eth1.1'
set firewall ipv4 name 'wan-local' default-action drop
set firewall ipv4 name 'wan-local' enable-default-log
set firewall ipv4 name 'wan-local' rule 10 action accept
set firewall ipv4 name 'wan-local' rule 10 state established enable
set firewall ipv4 name 'wan-local' rule 10 state related enable
set firewall ipv4 name 'wan-local' rule 10 description 'Allow established connections'
set firewall ipv4 name 'wan-local' rule 15 action accept
set firewall ipv4 name 'wan-local' rule 15 protocol icmp
set firewall ipv4 name 'wan-local' rule 15 description 'Allow pings'
set firewall ipv4 name 'wan-local' rule 50 action accept
set firewall ipv4 name 'wan-local' rule 50 protocol tcp
set firewall ipv4 name 'wan-local' rule 50 destination port 22
set firewall zone local from wan firewall name wan-local
set firewall ipv4 name 'wan-lan' default-action drop
set firewall ipv4 name 'wan-lan' enable-default-log
set firewall ipv4 name 'wan-lan' rule 10 action accept
set firewall ipv4 name 'wan-lan' rule 10 state established enable
set firewall ipv4 name 'wan-lan' rule 10 state related enable
set firewall ipv4 name 'wan-lan' rule 10 description 'Allow established connections'
set firewall zone lan from wan firewall name wan-lan
set firewall ipv4 name 'wan-mgmt' default-action drop
set firewall ipv4 name 'wan-mgmt' enable-default-log
set firewall ipv4 name 'wan-mgmt' rule 10 action accept
set firewall ipv4 name 'wan-mgmt' rule 10 state established enable
set firewall ipv4 name 'wan-mgmt' rule 10 state related enable
set firewall ipv4 name 'wan-mgmt' rule 10 description 'Allow established connections'
set firewall zone mgmt from wan firewall name wan-mgmt
set firewall ipv4 name 'local-wan' default-action accept
set firewall ipv4 name 'local-wan' enable-default-log
set firewall ipv4 name 'local-wan' rule 10 action accept
set firewall ipv4 name 'local-wan' rule 10 state established enable
set firewall ipv4 name 'local-wan' rule 10 state related enable
set firewall ipv4 name 'local-wan' rule 10 description 'Allow established connections'
set firewall zone wan from local firewall name local-wan
set firewall ipv4 name 'local-lan' default-action accept
set firewall ipv4 name 'local-lan' enable-default-log
set firewall ipv4 name 'local-lan' rule 10 action accept
set firewall ipv4 name 'local-lan' rule 10 state established enable
set firewall ipv4 name 'local-lan' rule 10 state related enable
set firewall ipv4 name 'local-lan' rule 10 description 'Allow established connections'
set firewall zone lan from local firewall name local-lan
set firewall ipv4 name 'local-mgmt' default-action accept
set firewall ipv4 name 'local-mgmt' enable-default-log
set firewall ipv4 name 'local-mgmt' rule 10 action accept
set firewall ipv4 name 'local-mgmt' rule 10 state established enable
set firewall ipv4 name 'local-mgmt' rule 10 state related enable
set firewall ipv4 name 'local-mgmt' rule 10 description 'Allow established connections'
set firewall zone mgmt from local firewall name local-mgmt
set firewall ipv4 name 'lan-wan' default-action accept
set firewall ipv4 name 'lan-wan' enable-default-log
set firewall ipv4 name 'lan-wan' rule 10 action accept
set firewall ipv4 name 'lan-wan' rule 10 state established enable
set firewall ipv4 name 'lan-wan' rule 10 state related enable
set firewall ipv4 name 'lan-wan' rule 10 description 'Allow established connections'
set firewall zone wan from lan firewall name lan-wan
set firewall ipv4 name 'lan-local' default-action drop
set firewall ipv4 name 'lan-local' enable-default-log
set firewall ipv4 name 'lan-local' rule 10 action accept
set firewall ipv4 name 'lan-local' rule 10 state established enable
set firewall ipv4 name 'lan-local' rule 10 state related enable
set firewall ipv4 name 'lan-local' rule 10 description 'Allow established connections'
set firewall ipv4 name 'lan-local' rule 50 action accept
set firewall ipv4 name 'lan-local' rule 50 protocol tcp_udp
set firewall ipv4 name 'lan-local' rule 50 destination port 53
set firewall zone local from lan firewall name lan-local
set firewall ipv4 name 'lan-mgmt' default-action drop
set firewall ipv4 name 'lan-mgmt' enable-default-log
set firewall ipv4 name 'lan-mgmt' rule 10 action accept
set firewall ipv4 name 'lan-mgmt' rule 10 state established enable
set firewall ipv4 name 'lan-mgmt' rule 10 state related enable
set firewall ipv4 name 'lan-mgmt' rule 10 description 'Allow established connections'
set firewall zone mgmt from lan firewall name lan-mgmt
set firewall ipv4 name 'mgmt-wan' default-action accept
set firewall ipv4 name 'mgmt-wan' enable-default-log
set firewall ipv4 name 'mgmt-wan' rule 10 action accept
set firewall ipv4 name 'mgmt-wan' rule 10 state established enable
set firewall ipv4 name 'mgmt-wan' rule 10 state related enable
set firewall ipv4 name 'mgmt-wan' rule 10 description 'Allow established connections'
set firewall ipv4 name 'mgmt-wan' rule 15 action accept
set firewall ipv4 name 'mgmt-wan' rule 15 protocol icmp
set firewall ipv4 name 'mgmt-wan' rule 15 description 'Allow pings'
set firewall ipv4 name 'mgmt-wan' rule 50 action accept
set firewall ipv4 name 'mgmt-wan' rule 50 protocol tcp
set firewall ipv4 name 'mgmt-wan' rule 50 destination port 22
set firewall zone wan from mgmt firewall name mgmt-wan
set firewall ipv4 name 'mgmt-local' default-action drop
set firewall ipv4 name 'mgmt-local' enable-default-log
set firewall ipv4 name 'mgmt-local' rule 10 action accept
set firewall ipv4 name 'mgmt-local' rule 10 state established enable
set firewall ipv4 name 'mgmt-local' rule 10 state related enable
set firewall ipv4 name 'mgmt-local' rule 10 description 'Allow established connections'
set firewall ipv4 name 'mgmt-local' rule 15 action accept
set firewall ipv4 name 'mgmt-local' rule 15 protocol icmp
set firewall ipv4 name 'mgmt-local' rule 15 description 'Allow pings'
set firewall ipv4 name 'mgmt-local' rule 50 action accept
set firewall ipv4 name 'mgmt-local' rule 50 protocol tcp
set firewall ipv4 name 'mgmt-local' rule 50 destination port 22
set firewall zone local from mgmt firewall name mgmt-local
set firewall ipv4 name 'mgmt-lan' default-action drop
set firewall ipv4 name 'mgmt-lan' enable-default-log
set firewall ipv4 name 'mgmt-lan' rule 10 action accept
set firewall ipv4 name 'mgmt-lan' rule 10 state established enable
set firewall ipv4 name 'mgmt-lan' rule 10 state related enable
set firewall ipv4 name 'mgmt-lan' rule 10 description 'Allow established connections'
set firewall ipv4 name 'mgmt-lan' rule 15 action accept
set firewall ipv4 name 'mgmt-lan' rule 15 protocol icmp
set firewall ipv4 name 'mgmt-lan' rule 15 description 'Allow pings'
set firewall ipv4 name 'mgmt-lan' rule 50 action accept
set firewall ipv4 name 'mgmt-lan' rule 50 protocol tcp
set firewall ipv4 name 'mgmt-lan' rule 50 destination port 22
set firewall zone lan from mgmt firewall name mgmt-lan
```

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/Ebrithil95/zobo/tags).

## License

This project is licensed under the AGPLv3 License - see the [LICENSE](LICENSE) file for details.
