# Angr Notes 

## Getting started 
Start a project:
    project = angr.Project("/path/to/bin")

Start a simulation:
    project.factory.simgr(init_state)

Exploring Simulations:
    sim.explore(find=functionOrAddr,avoid=functionOrAddr)

## What can angr do?
This is a good question. angr is quite powerful as a symbolic executing engine.
angr has the ability to symbolicly populate memory,heap,stack,files,regs and even
hook entire functions to replace them in runtime. This can even allow for certain
libraries to be read into the program. Essentialy, angr can solve to an address or state
and even explore through states acting as a state returner to preform actions on.

angr allows skilled reverse engineers to reduce their time reversing by allowing programatic
ways to interact with a program and its various procedures quickly. Though it can't solve
everything, angr allows you to make tools that can come pretty damn close.

## What can't angr do?
Understand where to look for certain solves. Avoid path explosion. Solve things that
are not feasible for symbolic execution. 

## References
### angr0-2 
Explore a binary based on states of the binary
that are passed to a function. 

### angr3 
Use claripy BitVectors to make symbolic variable and place it
in a register; then solve the binary until we find the stdoutput we are looking
for.

### angr4 
Use claripy BitVectors to make a symbolic stack after a function call.
Symbolicly reconstruct the stack with the right ebp and allocate the correct space.
Then place the symbolic variables on the stack and solve to printout

### angr5 
Assume claripy from now on. Used when the something is loaded into memory.
Construct a symbolic place in memory, and place a symbolic variable there.

### angr6
Fake Heap! Overwrite pointers and store things in a fake heap, then solve.

### angr7
Symbolic files!

## TODO
Reread angr8--in introduces avoiding a branch explosion
angr9 introduces a way to hook functions in programs and rewrite them.
 



