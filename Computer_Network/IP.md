# IP

## 1. IPv4

- 32 Bits/4 Bytes, 8 Bits/1 Bytes per Group, 4 Groups: `XXX.XXX.XXX.XXX`
- Each Group's Range: [0, 255]

### 1.1. Reserved

|    Addr Pattern    |            Range            |      Scope      |              Description               |
| :----------------: | :-------------------------: | :-------------: | :------------------------------------: |
|     0.0.0.0/8      |    0.0.0.0-0.255.255.255    |    Software     |            Current Network             |
|     10.0.0.0/8     |   10.0.0.0-10.255.255.255   | Private Network | Local Communication in Private Network |
|   100.64.0.0/10    | 100.64.0.0-100.127.255.255  | Private Network |      Service Communication in LSN      |
|    127.0.0.0/8     |  127.0.0.0-127.255.255.255  |      Host       |      Loopback Addr of Local Host       |
|   169.254.0.0/16   | 169.254.0.0-169.254.255.255 |     Subnet      |               Local Link               |
|   172.16.0.0/12    |  172.16.0.0-172.31.255.255  | Private Netword | Local Communication in Private Network |
|    192.0.2.0/24    |    192.0.2.0-192.0.2.255    |    TEST-NET     |                  Demo                  |
|   192.88.99.0/24   |  192.88.99.0-192.88.99.255  |    Internet     |            IPv4/IPv6 Relay             |
|   192.168.0.0/16   | 192.168.0.0-192.168.255.255 | Private Network | Local Communication in Private Network |
|   198.18.0.0/15    |  198.18.0.0-198.19.255.255  | Private Network |         Inter-Subnet Benchmark         |
|    224.0.0.0/4     |  224.0.0.0-239.255.255.255  |    Internet     |              IP Multicast              |
|    240.0.0.0/4     |  240.0.0.0-255.255.255.254  |    Internet     |              Future Usage              |
| 255.255.255.255/32 |       255.255.255.255       |     Subnet      |     Limited Broadcast Destination      |

## 2. IPv6

- 128 Bits/16 Bytes, 16 Bits/2 Bytes per Group, 8 Groups: `XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX`
    - `::` stands for `0(:0)*`
- `::ffff:XXX.XXX.XXX.XXX`: IPv4 Mapping Addr

### 2.1. Reserved

|  Addr Pattern   |                       Range                       |      Scope      |         Description         |
| :-------------: | :-----------------------------------------------: | :-------------: | :-------------------------: |
|     ::/128      |                        ::                         |    Software     |         Unspecified         |
|     ::1/128     |                        ::1                        |      Host       | Loopback Addr of Local Host |
|  ::ffff:0:0/96  |            ::ffff:0:0-::ffff:ffff:ffff            |    Software     |         Mapped IPv4         |
| ::ffff:0:0:0/96 |          ::ffff:0:0:0-::ffff:0:ffff:ffff          |    Software     |       Translated IPv4       |
|  64:ff9b::/96   |           64:ff9b::-64:ff9b::ffff:ffff            |    Internet     |    IPv4/IPv6 Translation    |
|    100::/64     |          100::-100::ffff:ffff:ffff:ffff           |    Internet     |       Discard Routing       |
|    2001::/32    |    2001::-2001::ffff:ffff:ffff:ffff:ffff:ffff     |    Internet     |      Teredo Tunneling       |
|  2001:20::/28   |  2001:20::-2001:2f:ffff:ffff:ffff:ffff:ffff:ffff  |    Software     |           ORCHID            |
|  2001:db8::/32  | 2001:db8::-2001:db8:ffff:ffff:ffff:ffff:ffff:ffff |    TEST-NET     |            Demo             |
|    fc00::/7     |  fc00::-fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff   | Private Network |      Unique Local Addr      |
|    fe80::/10    |  fe80::-febf:ffff:ffff:ffff:ffff:ffff:ffff:ffff   |     Subnet      |         Local Link          |
|     ff00:/8     |  ff00::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff   |    Internet     |       Multicast Addr        |
