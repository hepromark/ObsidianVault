**Static vs. Dynamic Libraries**
Static:
Essentially is 'more code' in your software / binary.
- Always laoded
- No dependency at runtime
- Fast startup
- Safe from library version conflicts
- Library code is tied to your code -> new library updates means a new entire code build

Dynamic libraries:
Code that is stored and versioned separately from your own code.
- A shared library that needs to be linked at runtime
- Multiple programs use same instance of library
- Libraries can get updated and still be used by the program

### Keywords

##### `volatile`
- Tells compiler not to optimise accesses to this variable (ex: don't cache it, always take from main memory)
- When interfacing w/ hardware that changes value itself
- When another thread running that also uses variable
- When there's a signal handler that might change value of variable

```c
void SendCommand(MyHardwareGadget* gadget, int command, int data)
{
  // Without volatile, isBusy might just read isBusy once and never then go into
  // infinite loop (because no other visible code writes to it)
  while (gadget->isBusy)
  {
    // do nothing here.
  }
  
  // set data first - but compiller might do the write first
  gadget->data    = data;
  
  // Write after
  gadget->command = command;
}
```

##### `static`
1. Declared Static variable inside a function keeps its value between function calls
	- Not too good programming practice tho

2. Static global variables / functions is only 'seen' inside the files its declared
	- Good access control feature for encapsulation