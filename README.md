# ldb
Requires Java 8 to run.

# Motivation
This project was created for intellectual purposes as a way to explore code deobfuscation techniques on Lua script. This Lua obfuscator has been used by cybercriminals as a way of protecting malicious exploits they sell for Robolox or bots for World of Warcraft. This project may be of interest to game security developers to prevent cheating in their games by discovering how hackers are breaking their code.

Obfuscated scripts are extremely complicated to tackle, especially in an automated matter. The process is essentially.

- Lex and parse input Lua file to generate parse tree
- Convert parse tree into abstract syntax treee
- Optimize the abstract syntax tree
- Rename variables, populate symbol table.
- Detect and rename variables and function names into names such as ```get_float64```, etc
- Detect VM handlers
- Find all encryption information in Lua VM
- Load and decrypt bytecode using above information.
- Decrypt and populate chunks by symbolically executing Lua code (constants, instructions, prototypes).
- Process VM handler redirection and decryption (bytecode redirection).
- Remove antidecompiler tricks from optimized bytecode
- Remove junk code and further optimize bytecode using various techniques and algorithms.
- Generate luac file

ldb provides a Lua deobfuscation engine which can be ported to other Lua obfuscators trivially. This source code can be used, with minor modifications, for a vast array of things related to Lua. I emplore the community to reuse my code and engine to create other Lua deobfuscators, as long as they state they are using my code and provide a link back to this project.

# Simple usage
Generate a .luac file from an obfuscated VM script


```java -jar ldb.jar -i obfuscated.lua -o obfuscated.luac```

You may now use a Lua decompiler such as unluac to decompile the code back into Lua. I recommend unluac for decompilation and luadec for generating disassemblies with pseudocode.

# Advanced Use: Inspecting VM

If we are curious about manually inspecting the VM ourselves, we can use ldb to generate a pseudocode source listing of the Luraph VM either in psuedocode or with its graphical display

To save the VM to a file

```java -jar LuraphDeobfuscator -i file.lua -s > file.vm.lua```

To copy the VM to your clipboard (CTRL+C/CTRL+P)

```java -jar LuraphDeobfuscator -i file.lua -c```

And when we look at a screenshot we can now easily read the VM code. This process is used internally by ldb in order to fully deobfuscate Luraph code.

We can see it even assigns names to functions and variables. Pretty neat!

![Optimized VM](https://i.imgur.com/LuingqO.png)

# Advanced Use: Viewing Abstract Syntax Trees

We can also view the optimized VM graphically:

```java -jar LuraphDeobfuscator -i file.lua -s```

The following window will be displayed:

![Luraph AST](https://i.imgur.com/Hv3rTQ7.png)

# Advanced Use: Dumping Bytecode

It is normally better to use luadec to generate a disassembly of bytecode, but for whatever reason if that doesn't happen or you are curious about exploring ldb's features, you can also use it to dump optimized bytecode.

```java -jar ldb -i file.lua -b > file.bytecode.txt```

```lua
DUMPING CHUNK
#UPVALUES = 0.0
#PARAMS = 0.0
#MAXSTACKSIZE = 9.0
#CONSTANTS = 7
#INSTRUCTIONS = 24
#PROTOTYPES = 0

Constant 0 = 0.0
Constant 1 = name: 
Constant 2 = 10.0
Constant 3 = 1.0
Constant 4 = ,
Constant 5 = print
Constant 6 = 

[0] LOADK 0.0 0.0
[1] LOADK 1.0 1.0
[2] LOADK 2.0 0.0
[3] LOADK 3.0 2.0
[4] LOADK 4.0 3.0
[5] FORPREP 2.0 1.0
[6] ADD 0.0 0.0 5.0
[7] FORLOOP 2.0 -2.0
[8] LOADK 2.0 0.0
[9] LOADK 3.0 2.0
[10] LOADK 4.0 3.0
[11] FORPREP 2.0 4.0
[12] MOVE 6.0 1.0
[13] MOVE 7.0 5.0
[14] LOADK 8.0 4.0
[15] CONCAT 1.0 6.0 8.0
[16] FORLOOP 2.0 -5.0
[17] GETGLOBAL 2.0 5.0
[18] MOVE 3.0 0.0
[19] CALL 2.0 2.0 1.0
[20] GETGLOBAL 2.0 5.0
[21] MOVE 3.0 1.0
[22] CALL 2.0 2.0 1.0
[23] RETURN 0.0 1.0


END CHUNK
```
