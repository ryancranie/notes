# OSI Model
## 7 Layer Architecture
The OSI Model follows a layered architecture
7. **Application Layer**:
6. **Presentation Layer**:
5. **Session Layer**:
4. **Transport Layer**:
3. **Network Layer**:
2. **Data Link Layer**:
1. **Physical Layer**:
| Layer | Name         | Description |
| ----- | ------------ | ----------- |
| 7     | Application  |             |
| 6     | Presentation |             |
| 5     | Session      |             |
| 4     | Transport    |             |
| 3     | Network      |             |
| 2     | Data Link    |             |
| 1     | Physical     |             |
## The OSI Model & TCP/IP Model
[TCP/IP Model](TCP/IP%20Model.md)

script fr
okay so this function is great, replaces exactly where I want it to. which is awesome.

to remind you this is what note_data.csv consists of
| note name | alias | alias ignore | references |

so it can find an alias in the note, and try reference it, which is great, but the link is wrong

for example:

"TCP-IP Model" has an alias of "TCP/IP Model" in my csv file.

The program finds it, but references it as if the alias is the note.

so TCP/IP Model becomes

[TCP/IP Model](TCP/IP%20Model.md)

WHEN it should be

[TCP/IP Model](TCP-IP%20Model.md)

it should be - not / since its an alias

I guess before replacing anything, once a reference is confirmed to be replaced, ensure the link must be the note name value on that same csv row.

so it should return

[TCP/IP Model](TCP-IP%20Model.md)



## Links
### Policies
.
### Notes
.
### Revision History

001: XXXX-XX-XX - INITIALIZE

---
<font size=3>[NETWORK SECURITY CONTENTS](-%20Network%20Security%20Contents.md)
[README](README.md)
[LICENSE](LICENSE)<font>