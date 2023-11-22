# Math Notes For Development

### Need a fast calculation of FASC-N BDC (Binary Coded Decimal)
### With the final value being an xor checksum
### Note each FASC-N number is 5 bits 
#### original at gtri-lead/bae-lib:client-api/src/main/java/util/bcd
#### original at gtri-lead/bae-lib:client-api/src/main/java/org/gtri/gfipm/bae/v2_0/FASCNSubjectIdentifier.java
```
AGENCY CODE 4 Identifies the government agency
issuing the credential
SYSTEM CODE 4
Identifies the system the card is
enrolled in and is unique for each
site
CREDENTIAL
NUMBER 6
Encoded by the issuing agency. For
a given system no duplicate
numbers are active
CS 1
CREDENTIAL SERIES
(SERIES CODE)
Field is available to reflect major
system changes
ICI 1
INDIVIDUAL CREDENTIAL ISSUE
(CREDENTIAL CODE)
Recommend coding as a “1” always
PI 10
PERSON IDENTIFIER
Numeric Code used by the identity
source to uniquely identify the token
carrier. (e.g. DoD EDI PN ID, TWIC
credential number, NASA UUPIC)
OC 1
ORGANIZATIONAL CATEGORY
1 - Federal Government Agency
2 - State Government Agency
3 - Commercial Enterprise
4 - Foreign Government
OI 4
ORGANIZATIONAL IDENTIFIER
OC=1 – NIST SP800-87 Agency
Code
OC=2 – State Code
OC=3 – Company Code
OC=4 – Numeric Country Code
POA 1
PERSON/ORGANIZATION
ASSOCIATION CATEGORY
1 – Employee
2 – Civil
3 – Executive Staff
4 – Uniformed Service
5 – Contractor
6 – Organizational Affiliate
7 – Organizational Beneficiary
SS 1
Start Sentinel. Leading character
which is read first when card is
swiped
FS 1 Field Separator
ES 1 End Sentinel

-------------
```
(Fourth column is parity)
| BCD | Parity | Value |
| --- | --- | --- |
| 0 0 0 0 | 1 | 0 |
| 1 0 0 0 | 0 | 1 |
| 0 1 0 0 | 0 | 2 |
| 1 1 0 0 | 1 | 3 |
| 0 0 1 0 | 0 | 4 |
| 1 0 1 0 | 1 | 5 |
| 0 1 1 0 | 1 | 6 |
| 1 1 1 0 | 0 | 7 |
| 0 0 0 1 | 0 | 8 |
| 1 0 0 1 | 1 | 9 |
| 1 1 0 1 | 0 | Start Sentinel (SS) |
| 1 0 1 1 | 0 | Field Separator (FS) |
| 1 1 1 1 | 1 | End Sentinel (ES) |
| ? ? ? ? | ? | Calculated Longitudinal Redundancy Character (LRC) | 

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
# Assumes average over a infinite number of samples b/c count does not have an upper limit
really_big_float next = 0
really_big_float current = 0
int count = 0

function average(number) {
  count = count + 1
  next = current + (number - current)/count
  current = next
}
```

