**Malware**: software that is doing something malevolent for the computer that is residing in.
- Viruses: snippet of code that when inserted in programs do something bad
- Worms: programs that self propagate
- Trojan horses: programs that hide themselves as genuine programs while they are malevolent 
Better antivirus is making programs vulnerability free.
## Malicious Code Lifecycle
Reproduce-> Infect-> Stay Hidden-> Run Payload
Typically you want to remain inactive for a period of time so when you became active you have infected a lot of machines. 
## Infection Techniques
- Boot viruses 
- File infectors
You can infect the padding bytes between the functions code, the binary remains the same and you added code. 
Boot viruses infect the code that boot up the machines, so if you have administrative rights when you read these code you can modify the code and infect the boot session(done as before we didn't check the boot sector as we thought it was secure).
## Macro Viruses
There is some files that can embed programs. Some office documents can fit Turing complete programs. Used for Melissa virus. This happens in the hands of non technical operator. They are difficult to remove as you would have to remove all the macros in the system. Can be resolved asking Office to not run macros/ask before running a macro.
## Worms
A worm that run infinite loop will be detected fast as it leaves a great trace. 
The modern worms are Mass Scanners, seek out other targets with a network scanner. So you either get a list of IP, generate a random IP list or start from the first IP possible and go on. 
Worms where not used in great quantity after 2001 as they would break internet as a consequence of the attack, so they weren't profitable.
Also there where a shift in objectives: go from disturbance to money directly(direct(abuse of credit card,...) or indirect(sell data)). 
Malware become also a SaaS way of monetization.
Easy to spot also because copying myself means that there is same code in both infected machines(this needed polymorphism to change the overwhelming majority of bytes in the malware code, can only do run time analysis signature based analysis are made useless. Using metamorphism means that we write multiple version of the code so they look different but have the same semantic(do the same))
## Bots
Nowadays the DDoS are done through bots and with the victim/other infrastructure.
## Defending against Malware
You can patch the vulnerabilities. Recognize the signature of the viruses
Antiviruses tries to do pattern matching in the signatures of messages, programs, files, etcs... Done like that as it is cheap. Can also do signature based detection. Heuristics or detect behavoral detection through running the code(run them in a full architectural copy)
## Virus stealth techniques
A virus can either run at the start of the program or after a period of time. The run at the start is easier to detect by an antivirus. 
You can overwrite import table addresses and to do so you should also return the correct behaviour to the user to not rise doubt. Or you  can overwrite function calls instructions. These methods are not easy to detect.
### Polymorphism
Based on the same decryption routine so it is easier to detect
### Metamorphism
You can change the semantic of the code but not the functioning.


## Malware general stealth techniques
- Dormant period
- Event-triggered payload
- Anti-virtualization techinques 
- Encryption/Packing
- Rootkit techniques
### Anti virtualization techniques
You can check the time and see if the execution is slow I may be in a VM. Also check for debugger or if I can open a VM(if not I am in one)
A malware can check for register keys, some instructions can tell you if you are in  a virtual environment. The hardest part to detect for a malware is an emulator.
One of the principal difference is the execution time. 
The processes can check if they are debugging themselves.
So the idea is that a malware wants to know if it is in a virtual environment before executing the malevolous payload.
## Malware Analysis vs Stealth Techniques
Static analysis pros:
- Can see  all the code without executing it
Static analysis cons:
- If the payload is encrypted/packed you cannot see the real instruction of the malicious part of the code
- No visibility on the runtime of the code
Dynamic analysis pros:
- Bypasses obfuscation
- At some point the code of the malicious part would be seen as clear text
Dynamics analysis cons:
- You may not execute all the malicious code
- You may be detected
## Rootkits
When a malware is in a system wants to remain for a long period of time. Usually they want to be root to disguise themselves as code/hide in kernel. 
You can have user land utilities to attack and use to hide. 
In the kernel is different as you are at the core of the OS, but writing rootkits for kernel space is harder as you have to write also in machine language/assembly instead of only C code, need to take care of different modules, need to hide your processes so need to change the processes organization and also the file system module as you need to hide yourself.
You can recognize rootkits through Post mortem on different system, cross layer examination, intuition,....
