# Math Notes For Development

### Need a fast calculation of FASC-N BDC (Binary Coded Decimal)
### With the final value being an xor checksum
### Note each FASC-N number is 5 bits 
#### original at gtri-lead/bae-lib:client-api/src/main/java/util/bcd
#### original at gtri-lead/bae-lib:client-api/src/main/java/org/gtri/gfipm/bae/v2_0/FASCNSubjectIdentifier.java
```
This is normalllyy fead in as 200 bits - figure out a fast way to process it

AGENCY CODE 4 Identifies the government agency issuing the credential
???

SYSTEM CODE 4 Identifies the system the card is enrolled in and is unique for each site

CREDENTIAL NUMBER 6
Encoded by the issuing agency. For a given system no duplicate numbers are active

CS 1
CREDENTIAL SERIES
(SERIES CODE)
Field is available to reflect major system changes

ICI 1
INDIVIDUAL CREDENTIAL ISSUE
(CREDENTIAL CODE)
Recommend coding as a “1” always

PI 10 - Could mean different things to different agencies (The length is 9 for some agencies)
PERSON IDENTIFIER
Numeric Code used by the identity source to uniquely identify the token carrier. 
(e.g. DoD EDI PN ID, TWIC credential number, NASA UUPIC)

OC 1
ORGANIZATIONAL CATEGORY
OC=1 – NIST SP800-87 Agency Code
OC=2 – State Code
OC=3 – Company Code
OC=4 – Numeric Country Code

OI 4
1200 AGRICULTURE, Department of
1300 COMMERCE, Department of
9700 DEFENSE, Department of (except military departments)
5700 AIR FORCE, Department of the (Headquarters, USAF)
2100 ARMY, Department of the (except Corps of Engineers Civil Program Financing)
1700 NAVY, Department of the
9100 EDUCATION, Department of
8900 ENERGY, Department of
7500 HEALTH AND HUMAN SERVICES, Department of
7000 HOMELAND SECURITY, Department of
8600 HOUSING AND URBAN DEVELOPMENT, Department of
1400 INTERIOR, Department of the
1500 JUSTICE, Department of
1600 LABOR, Department of
1900 STATE, Department of
6900 TRANSPORTATION, Department of
2000 TREASURY, Department of the
3600 VETERANS AFFAIRS, Department of
6800 ENVIRONMENTAL PROTECTION AGENCY
4700 GENERAL SERVICES ADMINISTRATION
8000 NATIONAL AERONAUTICS AND SPACE ADMINISTRATION
8800 NATIONAL ARCHIVES AND RECORDS ADMINISTRATION
4900 NATIONAL SCIENCE FOUNDATION
2400 OFFICE OF PERSONNEL MANAGEMENT
2800 SOCIAL SECURITY ADMINISTRATION

POA 1
PERSON/ORGANIZATION ASSOCIATION CATEGORY
1 – Employee
2 – Civil
3 – Executive Staff
4 – Uniformed Service
5 – Contractor
6 – Organizational Affiliate
7 – Organizational Beneficiary

SS 1 Start Sentinel. Leading character which is read first when card is swiped
FS 1 Field Separator
ES 1 End Sentinel

-------------
```
| FASC-N Math |
| --- |
| BCD |
| Odd parity |
| Least significant bit first |
| Parity bit is calculated by ensuring that there are always an odd number of bits |
| Each LRC bit is to ensure the parity of each column |
| The parity of the resulting LRC row is calculated as normal |
| ~ complement (invert all bits) |
| ^ xor |
| & and |
| \| or |
| >> Right shift |
| << Left shift |

| Non BCD Parity |
| --- |
| value = x |
| value ^= value >> 8 | 
| value ^= value >> 4 |
| value ^= value >> 2 |
| value ^= value >> 1 |
| parity = value ^ 1 |

| Convert BCD as String to Base 10 |
| --- |
| Integrer.parseInt("1001",2) |

Which basically means an even number of high bits = 1, and an odd number = 0
One way or another its 200 bits either bcd -> bits or from a certificate
(Fourth column is parity)
| BCD | Parity | Value | Non BCD |
| --- | --- | --- | --- | 
| 0 0 0 0 | 1 | 0 | 0 |
| 1 0 0 0 | 0 | 1 | 8 |
| 0 1 0 0 | 0 | 2 | 4 |
| 1 1 0 0 | 1 | 3 | 12 |
| 0 0 1 0 | 0 | 4 | 2 |
| 1 0 1 0 | 1 | 5 | 10 |
| 0 1 1 0 | 1 | 6 | 6 |
| 1 1 1 0 | 0 | 7 | 14 |
| 0 0 0 1 | 0 | 8 | 1 |
| 1 0 0 1 | 1 | 9 | 9 |
| 1 1 0 1 | 0 | Start Sentinel (SS) | 13 |
| 1 0 1 1 | 0 | Field Separator (FS) | 11 |
| 1 1 1 1 | 1 | End Sentinel (ES) | 15 |
| ? ? ? ? | ? | Calculated Longitudinal Redundancy Character (LRC) | ? | 

| Code | Length |
| --- | --- |
| SS | 1 |
| AGENCY CODE | 4 |
| FS | 1 |
| SYSTEM CODE | 4 |
| FS | 1 |
| CREDENTIAL NUMBER | 6 |
| FS | 1 |
| CS | 1 |
| FS | 1 |
| ICI | 1 |
| FS | 1 |
| PI | 10 |
| OC | 1 |
| OI | 4 |
| POA | 1 |
| ES | 1 |
| LRC | 1 |


### Fast algorithm for the following running average

```
# Assumes average over a infinite number of samples unless max_count is set
really_big_float next = 0
really_big_float current = 0
int count = 0
int max_count = 0

function average(number) {
  count = count + 1
  if (max_count != 0) && (count > max_count) { count = max_count }
  next = current + (number - current)/count
  current = next
}

# Could just pass a struct rather than global variables
```

