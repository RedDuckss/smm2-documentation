# Course replay.dat structure
## Still largely unknown

| Offset | Size    | Description                               |
|--------|---------|-------------------------------------------|
| 0x0    | 0x4     | CRC over rest of data                     |
| 0x4    | 0x1     | Unknown                                   |
| 0x5    | 0x3     | Padding?                                  |
| 0x8    | 0x4     | Magic "SPRH"                              |
| 0xC    | 0x67FC4 | Unknown. Button inputs? Player positions? |