## IPv6 Study Guide, part 1

# Overview
    
This study guide will cover IPV6 for the CCNA exam.

# Hexadecimal
IPV6 uses Hexadecimal for it's addressing scheme. It is denoted by using 0x, rather than 0b for binary or 0d for decimal. Below is a table comparing how values are noted in each of these systems.

## Hexadecimal Table
| Decimal | Binary | Hexadecimal |
|---------|--------|-------------|
| 0       | 0000   | 0           |
| 1       | 0001   | 1           |
| 2       | 0010   | 2           |
| 3       | 0011   | 3           |
| 4       | 0100   | 4           |
| 5       | 0101   | 5           |
| 6       | 0110   | 6           |
| 7       | 0111   | 7           |
| 8       | 1000   | 8           |
| 9       | 1001   | 9           |
| 10      | 1010   | a           |
| 11      | 1011   | b           |
| 12      | 1100   | c           |
| 13      | 1101   | d           |
| 14      | 1110   | e           |
| 15      | 1111   | f           |

# Why do we need IPv6
Simply put, there are no longer enough IPv4 addresses available for us. When IPv4 was originally implemented, the idea that 4 billion plus addresses wouldn't be able to cover all of the needs of the internet was inconcievable.  IPv4 addresses are assigned by IANA, which distributes IPv4 address space to various Regional Internet Registries (RIRs), which assign them to companies that need them. The north american RIR, ARIN, exhausted its address pool in September of 2015, and in August of 2020, the latin american RIR, LACNIC, assigned its final IPv4 address. However, as the size of the internet has grown, different measures such as VLSM, private IPv4 address spaces and NAT have been implemented to conserve the available IPv4 addresses. These are just short term solutions, and a more permanent option had to be designed. Enter IPv6. By utilizing 32 hexadecimal characters consisting of 0-9 and A-F separated into 8 groups of 4 separated by colons to create a total of 2^128 (around 340 undeciillion) addresses. Below is an example of an IPv6 address written in binary, decimal and hexadecimal.
    0b:001000000000000100001101101110000101100100010111111010101011110101100101011000100010111111010101100100100101101010110011011101
0d:32.1.13.184.89.23.234.189.101.98.23.234.210.45.89.189
0x:2001:0DB8:5917:EABD:6562:17EA:C92D:59BD
As you can see, using hexadecimal is a much more efficient method of writing out an address than either binary or decimal. This doesn't mean it's necessarily an easy address to read, however. Below are some methods to simplify them.
First off, leading 0s can be removed. For example, 2001:0DB8:000A:001B:20A1:0020:0080:34BD becomes 2001:DB8:A:1B:20A1:20:80:34DB, which is an easier address to read and comprehend.
Another method is replacing consecutive quartets of all 0s with a double colon (::). This can only be done with one set of consecutive 0s. 2001:0DB8:0000:0000:0000:0000:0080:34BD becomes 2001:DB8::80:34BD. However, 2001:0000:0000:0000:20A1:0000:0080:34BD cannot be 2001::20A1::34BD, because we cannot determine where in the address each quartet is; it could be 2001:0000:0000:0000:20A1:0000:0080:34BD or 2001:0000:0000:20A1:0000:0000:0080:34BD, which are two completely different addresses. The correct way to shorten the address would be 2001::20A1:0:0:34BD
    
# Finding the IPv6 prefix

IPv6 prefixes denote the global unicast address for a given set of IPv6 addresses.
Typically, an enterprise that requests IPv6 addresses from their ISP will recive a /48 block.
IPv6 subnets typically use a /64 prefix length.
This means that an enterprise will usually have 16 bits available to make subnets.
Here is an example:
2001:0DB8:8B00:0001:0000:0000:0000:0001/64
2001:0DB8:8B00: is the global routing prefix of the address assigned by the ISP
0001: is the subnet identifier portion of the address, used by the enterprise to make various subnets.
These two combine to create the network portion of the address.
The last portion of the address, 0000:0000:0000:0001 is the 64-interface indentifier, the host portion of the address.
To find the IPv6 prefix of a /64 network, simply replace the second half of the address with all 0s, e.g 2001:0DB8:8B00:0001:0000:0000:0000:0001/64 is  2001:DB8:8B00:1::/64. If the prefix length is a multiple of 4, it is still simple to find the prefix. 
For example, if you have a network address 300D:00F2:   0B34:2100:0000:0000:1200:0001/56, simply add the value of bits in each portion of the address together until you come to the correct number: 300D =16, 00F2 = 32, 0B34 = 48, 2 = 52, and 1 = 56, meaning that the prefix is 300D:F2:B34:2100::/56. Note that even though the 0s in 2100 are a part of the host portion of the network, they cannot be removed, as that would give a network address of 300D:F2:B34:21::/56, which, after replacing the leading 0s is 300D:00F2:0B34:0021::/56, a completely different address. 
If the prefix length is not a multiple of 4, there are some added steps. 
For example, the address 2001:0DB8:8B00:0001:FB89:017B:0020:0011/93
Begin as you would with a prefix length divisible by 4, in this case going to 017, which is 92 bits. Then, convert B to 0b, which is 1011 (11 in 0d). We need the first bit, so we convert the remaining bits to 0, then convert it it back to 0x, making it 0x8. This makes the newtork prefix of this address 2001:DB8:8B00:1:FB89:178. 
    
