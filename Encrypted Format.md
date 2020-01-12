In order to encrypt and decrypt a save file, you will need to use [this NodeJS module](https://github.com/RedDuckss/partrick)

| Offset                          | Size | Description                  |
|---------------------------------|------|------------------------------|
| 0x0                             | 0x10 | Save Header. Not always used |
| 0x10 (or 0x0 if no Save Header) |      | Encrypted data               |
|                                 | 0x30 | Crypto Config                |

Different file types have different encrypted data lengths:

1. Course: 0x5BFC0
2. Course thumb: 0x1BDF0
3. Course replay: 0x67FD0
4. `later.dat`: 0xBFC0
5. `network.dat`: 0x47FC0
6. `quest.dat`: 0xBFC0
7. `save.dat`: 0xBFC0

## Save Header

| Offset | Size | Description                                                  |
|--------|------|--------------------------------------------------------------|
| 0x0    | 0x4  | Header Index                                                 |
| 0x4    | 0x2  | Save Type (1=Quest, 8=Network, 10=Later, 11=Save, 16=Course) |
| 0x6    | 0x2  | Course Flags (0=Edited, 1=New)                               |
| 0x8    | 0x4  | CRC32 over decrypted file                                    |
| 0xC    | 0x4  | Magic "SCDL" if the file is a course, empty otherwise        |

In the case of save.dat, the CRC32 in the first save header is over 0x0-0xB90F, with a second save header at 0xB910 which has a CRC32 over 0xB920-0xBFBF

## Crypto Config

Nintendo uses ENL for AES key generation. See [here](https://github.com/Kinnay/NintendoClients/wiki/ENL-Key-Generation) for more information on ENL

The AES-CMAC uses a separate key from the key used for encrypting the course data. The key is generated immediately after the key used for course data, using the same key table and state.

Size 0x30. Found at the end of an encrypted save file.

| Offset | Size    | Description                  |
|--------|---------|------------------------------|
| 0x0    | 0x4 * 4 | AES IV                       |
| 0x10   | 0x4 * 4 | Random seed                  |
| 0x20   | 0x4 * 4 | AES-CMAC over decrypted file |
