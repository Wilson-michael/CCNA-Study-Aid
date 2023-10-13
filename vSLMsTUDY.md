# VLSM slash notes


## /24
| subnet mask              | 255.255.255.0                       |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.00000000 |
| Class c                  | borrowed bits 0                     |
| ip address range example | 192.168.1.0-192.168.1.255           |
| Network address          | 192.168.1.0                         |
| First useable address    | 192.168.1.1                         |
| Last useable address     | 192.168.1.254                       |
| Broadcast address        | 192.168.1.255                       |
| Total useable hosts      | 254                                 |


## /25
| subnet mask              | 255.255.255.128                     |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.10000000 |
| Class                    | c                                   |
| Borrowed bits            | 1                                   |
| ip address range example | 192.168.1.0-192.168.1.127           |
| Network address          | 192.168.1.0                         |
| First useable address    | 192.168.1.1                         |
| Last useable address     | 192.168.1.126                       |
| Broadcast address        | 192.168.1.127                       |
| Total useable hosts      | 126                                 |

## /26
| subnet mask              | 255.255.255.192                     |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.11000000 |
| Class                    | c                                   |
| Borrowed bits            | 2                                   |
| ip address range example | 192.168.1.128-192.168.1.191         |
| Network address          | 192.168.1.128                       |
| First useable address    | 192.168.1.129                       |
| Last useable address     | 192.168.1.190                       |
| Broadcast address        | 192.168.1.191                       |
| Total useable hosts      | 62                                  |

## /27
| subnet mask              | `255.255.255.224`                   |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.11100000 |
| Class                    | c                                   |
| Borrowed bits            | 3                                   |
| ip address range example | 192.168.1.192-192.168.1.223         |
| Network address          | 192.168.1.192                       |
| First useable address    | 192.168.1.193                       |
| Last useable address     | 192.168.1.222                       |
| Broadcast address        | 192.168.1.223                       |
| Total useable hosts      | 30                                  |

## /28
| subnet mask              | 255.255.255.240                     |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.11110000 |
| Class                    | c                                   |
| Borrowed bits            | 4                                   |
| ip address range example | 192.168.1.224-192.168.1.239         |
| Network address          | 192.168.1.224                       |
| First useable address    | 192.168.1.225                       |
| Last useable address     | 192.168.1.238                       |
| Broadcast address        | 192.168.1.239                       |
| Total useable hosts      | 14                                  |

## /29
| subnet mask              | 255.255.255.248                     |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.11111000 |
| Class                    | c                                   |
| Borrowed bits            | 5                                   |
| ip address range example | 192.168.1.240-192.168.1.247         |
| Network address          | 192.168.1.240                       |
| First useable address    | 192.168.1.241                       |
| Last useable address     | 192.168.1.246                       |
| Broadcast address        | 192.168.1.247                       |
| Total useable hosts      | 6                                   |

## /30
| subnet mask              | 255.255.255.252                     |
|--------------------------|-------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.11111100 |
| Class                    | c                                   |
| Borrowed bits            | 6                                   |
| ip address range example | 192.168.1.248-192.168.1.251         |
| Network address          | 192.168.1.248                       |
| First useable address    | 192.168.1.249                       |
| Last useable address     | 192.168.1.250                       |
| Broadcast address        | 192.168.1.251                       |
| Total useable hosts      | 2                                   |

## /31
| subnet mask              | 255.255.255.254                         |
|--------------------------|-----------------------------------------|
| Binary Notation          | 11111111.11111111.11111111.11111110     |
| Class                    | c                                       |
| Borrowed bits            | 7                                       |
| ip address range example | 192.168.1.252-192.168.1.254             |
| Network address          | 192.168.1.252                           |
| First useable address    | N/A                                     |
| Last useable address     | N/A                                     |
| Broadcast address        | 192.168.1.253                           |
| Total useable hosts      | 0 (Used for Point to Point Connnection) |


## Notes on subnetting 
As long as all network bits are locked, they can either be 1 or 0 to allow for multiple subnets of the same size.
For example, both 192.168.1.222 /28 and 192.168.1.237/28 are valid networks with 14 useable hosts that do not overlap, the respective binary notations being 111111111.11111111.11111111.11010000 and 111111111.11111111.11111111.11110000 and the same subnet mask of 255.255.255.240
