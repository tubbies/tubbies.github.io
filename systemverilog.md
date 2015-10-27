# SystemVerilog
## Overview
IEEE1800-2012 Standard - SystemVerilog that combined HDL(Hardware Description Language - IEEE1364-2001) and HVL(Hardware Verification Language - IEEE1364-2005). 
## Data Types
### Structure
## Class
## Interface
- Interface is bundle of ports. The interface encapsulates port connectivity and functionality. To provide direction information of port can be done by keyword **modport**.

- ```Verilog
interface interface_name;
// interface items ...
endinterface
```

- In interface, it can have input and output, function and task and also can have parameter.


- For example, APB interface can be defined as follows:



- ```Verilog 
inteface apb3_bus #(AW=16, DW=32);
wire psel; 
wire penable;
wire pready;
wire pwrite;
wire [AW-1:0] paddr;
wire [DW-1:0] pwdata;
wire [DW-1:0] prdata;
endinterface
```

- Keyword **modport** specify direction within the interface. For example



- ```Verilog 
inteface apb3_bus #(AW=16, DW=32);
wire psel; 
wire penable;
wire pready;
wire pwrite;
wire [AW-1:0] paddr;
wire [DW-1:0] pwdata;
wire [DW-1:0] prdata;
modport master (input psel, input penable, input pwrite, input [AW-1:0] paddr, input [DW-1:0] pwdata, output pready, output [DW-1:0] prdata);
modport slave (output psel, output penable, output pwrite, output [AW-1:0] paddr, output [DW-1:0] pwdata, input pready, input [DW-1:0] prdata);
endinterface
```


## Package

- Another way of mechanism to share data types, functions, tasks, properties between different modules.


	
- ```Verilog
package package_name;
// data declaration
// function or task declaration
endpackage
```


- Package need to be imported within modules to use. package import can be done by keyword import. Both explicit import and wildcard import can be done.


- ```Verilog
import PackageA::*;
import PackageB::foo;
```


## DPI
#### **Overview**
- DPI (Direct Programming Interface) is foreign language(C/C++) interface for System-Verilog
Using System-Verilog DPI functions and tasks in both different languages (between C/C++ and System-Verilog) can be interfaced or shared


#### **How-to**

- In C/C++ "svdpi.h" library should be included and library path that has svdpi.h need to be included during compilation



- Also, you need to use keyword extern to declared your C/C++ function



- C/C++ file should compiled as a dynamic library. In gcc, source code need to be compiled with "-shared -fPIC" options.



- C/C++ side dynamic libraries need to be included during simulation. (In ncsim, you need to specify your dynamic library with "-sv_lib" option)



#### **Import**

- C function can be imported to System-Verilog by using keyword "import"



> import "DPI-C" [context | pure] [function | task] [return_type] \[function name] (arguments ....);


#### **Export**

- System-Verilog task/function can be exported to C code by using keyword "export"



#### **Data Types**

| System-Verilog | C/C++ |
|:-:|:-:|
|byte|char|
|int|int|
|real|double|
|chandle|void* |
|string|char* |


#### **References**

[1] http://www.asic-world.com/systemverilog/dpi.html

[2] http://testbench.in/DP_00_INDEX.html


## SVA(System Verilog Assertion)

## Randomization

## Coverage

## System Directives
- `default_nettype 
- `resetall
