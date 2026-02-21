section 1: 
Grabbed table from the readme, filled out with 

| PC | Opcode | src1_addr | src1_out   | src2_addr | src_out    |dst_addr | dst_data   |
|---:|-------:|----------:|-----------:|----------:|-----------:|--------:|-----------:|
| 0  | 0x23   | 0         | 0x00000000 | 2         | 0x00000000| 0        | 0x00000000 |
| 4  | 0x23   | 0         | 0x00000000 | 2         | 0x00000000| 0        | 0x00000000 |
| 8  | 0x00   | 2         | 0x00000000 | 2         | 0x00000000| 0        | 0xXXXXXXXX |
| 12 | 0x2B   | 0         | 0x00000000 | 3         | 0x00000000| 2        | 0x00000056 |
| 16 | 0x00   | 3         | 0x00000000 | 2         | 0x00000056| 2        | 0x00000056 |
| 20 | 0x08   | 3         | 0x00000000 | 5         | 0x00000000| 3        | 0x00000000 |
| 24 | 0x00   | 5         | 0x00000000 | 3         | 0x00000000| 3        | 0x00000084 |
| 28 | 0x00   | 6         | 0x00000000 | 2         | 0x00000056| 4        | 0xFFFFFFAA |
| 32 | 0x00   | 6         | 0x00000000 | 2         | 0x00000056| 5        | 0x0000000C |
| 36 | 0x00   | 5         | 0x0000000C | 4         | 0xFFFFFFAA| 6        | 0x00000000 |
| 40 | 0x04   | 5         | 0x0000000C | 0         | 0x00000000| 7        | 0x00000056 |
| 44 | 0x23   | 0         | 0x00000000 | 8         | 0x00000000| 8        | 0xFFFFFFA9 |
| 48 | 0x00   | 0         | 0x00000000 | 0         | 0x00000000| 6        | 0x00000000 |
| 52 | 0x00   | 0         | 0x00000000 | 0         | 0x00000000| 31       | 0x0000000C |
| 56 | 0x00   | 0         | 0x00000000 | 0         | 0x00000000| 8        | 0x00000000 |

section 2: 
Since the example above is pipelined, of course itll be different from the original table, but pipelining does bring the risk of hazards since its overlapping instructions. A lot of the times, hazards happen when an instruction is dependent on a value to completed from a previous instruction but is still taking a while to compute fully, for example. A good hazard is lw v0 31(zer0), followed by an add function , add v1 v0 v0, this is a read after write hazard since add is reading from the v0 register before lw has fully written/finished the pebious instruction. Another one or $a3 $a2 $v0, this is dependent on the previous instruction and $a2 $a1 $v1, these regisyers wouldn't hold the most up to date values which would mess up the computations/instructions that were supposed to properly happen. These are just a few of the tradeoffs of hazards from pipelining. 

Section 3:
An r type instruction between instructions 1 and 2 can lead to the prevention of a stall since we wouldnt be using v0, letting the first instruction have time to complete fully while still using the time to pipeline another instruction, so a good r type would have to be like an addi t1, $zero, 0. This is because its an r type instruction, doesnt touch any major registers, but does create a cycle to help prevent a stall, since step 2 needs to use v0 and we'd need instruction 1 to be completely done to yield the correct output we want. 