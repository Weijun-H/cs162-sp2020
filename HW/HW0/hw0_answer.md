## GDB

1. Run GDB on the map executable.

   ```bash
   gdb map
   ```

2. Set a breakpoint at the beginning of the program’s execution.

   ```shell
   break 1
   ```

3. Run the program until the breakpoint. 

   ```bash
   run
   ```

4. Where does argv point to?

   ```bash
   argv=0x7fffffffe448
   ```

5. What’s at the address of argv?

   ```bash
   p argv[0] //0x7fffffffe69a
   ```

6. Step until you reach the first call to recur.

   ```basic
   step 
   ```

7. What is the memory address of the recur function? 

   ```bash
   info address recur // 0x55555555480b.
   ```

8. Step into the first call to recur.

   ```bash
   continue
   ```

9. Step until you reach the if statement.

   ```bash
   next
   ```

10. Switch into assembly view.

    ```bash
    layout asm
    ```

11. Step over instructions until you reach the callq instruction.

12. What values are in all the registers? m. Step into the callq instruction.

    ```bash
    info registers
    ```

13. Switch back to C code mode.

    ```bash
    tui disable
    ```

14. Now print out the current call stack. (Hint: what does the backtrace command do?)

    ```bash
    backtrace
    ```

15. Now set a breakpoint on the recur function which is only triggered when the argument is 0. 

    ```bash
    condition 1 i = 0
    ```

16. Continue until the breakpoint is hit.

17. Print the call stack now.

18. Now go up the call stack until you reach main. What was argc? 

    ```bash
    argc = 1
    ```

19. Now step until the return statement in recur.

20. Switch back into the assembly view.

21. Which instructions correspond to the return 0 in C?

22. Now switch back to the source layout. 

23. Finish the remaining 3 function calls.

24. Run the program to completion.

25. Quit GDB.