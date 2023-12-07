# EDR Bypass
Windows has two different privilege levels to protect the OS from for example crashes caused by installed application. All applications installed on the os runs in the user mode. The kernel and the device drivers run in the kernel mode. Apps in the user mode cant access or manipulate memory in the kernel mode. AV/EDR can only monitor application behavior in the user mode. 

EDR use userland hooking to monitor application behavior. They inject a custom dll file into every new process. These loaded dll files monitor the process for specific windows api calls. 

If a program loads a function from kernel32.dll a copy of kernel32.dll is placed into the memory. The EDR manipulate the in memory copy of this file and add their own code into specific functions. When that function gets called by the malicious program the EDR code is executed first. For example, some vendors may place a JMP instruction at the beginning of the code to change the execution flow to redirect to the EDR inspection module and it will evaluate if the program exhibits any suspicious behavior. 

To unhook the API we need to know how it looked like before it got modified by the EDR. Usually you can check the first few bytes of the function before it gets loaded into the memory. If we replace the bytes that were replaced by the EDR to the original then the EDR should become blind and no longer able to monitor that api call. 

Another method is to use direct syscalls to bypass userland hooking. The goal is that we wont be loading any function from ntdll.dll at runtime but call them directly with the corresponding assembler code. 

To do this you need to know the exact functions needed for your script and extract the corresponding assembler code via disassembling. It takes a lot of time and effort and if theres a new windows version you have to keep changing it. 
