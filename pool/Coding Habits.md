# Coding Habits

Most codes in our project are encapsulated into functions. The `varargin` is used for flexibility. The following examples can help you understand the usage of our functions:

```matlab
$different methods
Cor=corVar(xs,xr)
Cor=corVar(xs,xr,'for')$for_loop
$different operations
[tau,fd]=corEsti(xs,xr)
[tau,fd]=corEsti(xs,xr,'plot','Plot 1')
[tau,fd]=corEsti(xs,xr,'save','Plot 2','Plot_2.png')
```

This helps improve the readability, maintainability and reusability of our codes, and helps with our team collaboration.

Furthermore, this project is maintained by Git.