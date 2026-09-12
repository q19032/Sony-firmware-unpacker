# Sony-firmware-unpacker
Unlock Sony camera firmware package，The process of discovering Sony firmware unpacking
（CXD90057+CXD90058）Use FF03 to read memory，0xFFFFB800 0xF340F000 8B 89 4F 14 F8 B5 32 23 F8 5B 27 31 97 B1 4E 0C CD E2 52 AF D3 0D D0 4C C3 24 0A C2 35 2C E6 D5
(CXD90045) Use FF03 to read memory，FFFF5000 0xF340F000 C3FB23712B9E979B8B74DA4DC6B5945C845BD6171CEA7C8EDD2F40D7936CA671
Memory is in /dev/mem
Calculation formula: Key = SHA256(A's 32 bytes + B's 16 bytes + 32 bytes of fixed constant)
08a73c: adrp x0, #0x1a0000
08a740: add x0, x0, #0x270 → 0x1a0270 (address of salt 8b894f14)
08a744: ldp x2, x3, [x0] load salt [0:16]
08a748: ldp x0, x1, [x0, #0x10] load salt [16:32]
08a74c: stp x2, x3, [x29, #0xa0] store salt to stack (input buffer [48:64])
08a750: stp x0, x1, [x29, #0xb0] store salt to stack (input buffer [64:80])
08a754: mov x2, x21
08a758: mov x0, x20
08a75c: mov x1, #0x50 ★ 0x50 = 80 bytes (= 32 + 16 + 32)
08a760: mov w20, #0
08a764: bl #0x87bd0 call SHA256 derivation
