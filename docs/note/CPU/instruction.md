# LUI
|imm|rd|opcode|
|---|---|---|
|imm[31:12]|rd|0110111|

=== "操作"
    将 20 位立即数的值左移 12 位（低 12 位补零），成为一个 32 位数，将其写回 rd 
=== "作用"
    主要是为了在寄存器中存入比较大的立即数，比如，要想在寄存器 x1 中存入一个数，可以使用 addi 指令实现（addi x1, x0, 100），
    但这个数的范围有限 （-2048～2047），因为 addi 指令的立即数部分只有 12 位，能表示最大的无符号数位 0xfff（十进制 4095），对应的有符号数的范围则是 -2048～2047。当立即数超出这个范围，则需要用 LUI 指令来实现。
!!!example
    LUI x1, 0xffff 二进制指令 00001111111111111111000010110111
    x1 = 0xffff000
# AUIPC
|imm|rd|opcode|
|---|---|---|
|imm[31:12]|rd|0010111|

=== "操作"
    将 20 位立即数的值左移 12 位（低 12 位补零）成为一个 32 位数，再加上该指令的 pc 值，再将结果写入 rd
=== "作用"
    在处理全局地址时非常有用，因为它可以将一个相对较大的立即数与当前 pc 的上位部分相加，从而实现较大范围的跳转和地址计算。
!!!example
    auipc x2, 0xfff 二进制指令 00000000111111111111000100010111
    x2 = pc + 0xfff000 = 0xfff004 (当前 pc 为 4)
# JAL
|imm|rd|opcode|
|---|---|---|
|imm[20|10:1|11|19:12]|rd|1101111|

=== "操作"
    PC + 4 的结果送 rd 但不送入 PC，然后计算下条指令地址。转移地址采用相对寻址，基准地址为当前指令地址（即 PC），偏移量为立即数 imm20 经符拓展后的值的 2 倍。
!!!note
    在实际的汇编程序编写中，跳转的目标往往使用汇编程序中的 label，汇编器会自动根据 label 所在的地址计算出相对的偏移量赋予指令编码。
!!!example
    jal x3, label1 二进制指令：000000001000000000 

    
