## PROJECT0

### 2.1 Find the Faulting Instruction

1. What virtual address did the program try to access from userspace that caused it to crash?

   > Page fault at 0xc0000008: rights violation error reading page in user context.     

2. What is the virtual address of the instruction that resulted in the crash?

   > do-nothing: dying due to interrupt 0x0e (#PF Page-Fault Exception).

3. To investigate, disassemble the do-nothing binary using objdump (you used this tool in Homework 0). What is the name of the function the program was in when it crashed? Copy the disassembled code for that function onto Gradescope, and identify the instruction at which the program crashed.

   > 8048757: 8b 44 24 24           mov    0x24(%esp),%eaxs

4. Find the C code for the function you identified above (hint: it was executed in userspace, so it’s either in do-nothing.c or one of the files in proj0/src/lib or proj0/src/lib/user), and copy it onto Gradescope. For each instruction in the disassembled function in #3, explain in a few words why it’s necessary and/or what it’s trying to do. Hint: see 80x86 Calling Convention.

5. Why did the instruction you identified in #3 try to access memory at the virtual address you identified in #1? Don’t explain this in terms of the values of registers; we’re looking for a higher level explanation.

6. Step into the process_execute function. What is the name and address of the thread running this function? What other threads are present in Pintos at this time? Copy their struct threads. (Hint: for the last part dumplist &all_list thread allelem may be useful.)

   > Thread <main>     process_execute (file_name=file_name@entry=0xc0007d50 "do-nothing") at ../../userprog/process.c:32

7. What is the backtrace for the current thread? Copy the backtrace from GDB as your answer and also copy down the line of C code corresponding to each function call.

   ![image-20200731161343832](/Users/huangweijun/Library/Application Support/typora-user-images/image-20200731161343832.png)

8. Set a breakpoint at start_process and continue to that point. What is the name and address of the thread running this function? What other threads are present in Pintos at this time? Copy their struct threads.![image-20200731162721737](/Users/huangweijun/Library/Application Support/typora-user-images/image-20200731162721737.png)

9. Where is the thread running start_process created? Copy down this line of code.

10. Step through the start_process() function until you have stepped over the call to load(). Note that load() sets the eip and esp fields in the if_ structure. Print out the value of the if_ structure, displaying the values in hex (hint: print/x if_).

    ![image-20200801122251933](/Users/huangweijun/Library/Application Support/typora-user-images/image-20200801122251933.png)

11. Once you’ve executed iret, type info registers to print out the contents of registers. Include the output of this command on Gradescope. How do these values compare to those when you printed out if_?

    ![image-20200801194729952](/Users/huangweijun/Library/Application Support/typora-user-images/image-20200801194729952.png)

12. Notice that if you try to get your current location with backtrace you’ll only get a hex address. This is because because pintos-gdb ./kernel.o only loads in the symbols from the kernel. Now that we are in userspace, we have to load in the symbols from the Pintos executable we are running, namely do-nothing. To do this, use loadusersymbols tests/userprog/do-nothing. Now, using backtrace, you’ll see that you’re currently in the _start function. Using the disassemble and stepi commands, step through userspace instruction by instruction until the page fault occurs. At this point, the processor has immediately entered kernel mode to handle the page fault, so backtrace will show the current stack in kernel mode, not the user stack at the time of the page fault. However, you can use btpagefault to find the user stack at the time of the page fault. Copy down the output of btpagefault.