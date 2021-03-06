# SystemVerilog
## Overview
IEEE1800-2012 Standard - SystemVerilog that combined HDL(Hardware Description Language - IEEE1364-2001) and HVL(Hardware Verification Language - IEEE1364-2005). 
## Data Types
#### Integer data type
|Data type|Description|
|:-:|:-:|
|**int**|32-bit signed integer, 2-state data type|
|**logic**|unsigned 1 bit (Equivalent to *reg*), 4-state data type|
|**bit**|unsigned 1 bit, 2-state data type|
|**byte**|8-bit signed integer, 2-state data type|
|**longint**|64-bit signed integer, 2-state data type|

    - 2-state data type : each bit can have 0 or 1.
    - 4-state data type : each bit can have 0,1,x and z. 

#### Real data type
|Data type|Description|
|:-:|:-:|
|**real**|Equivalent to C *double* type(64 bit)|
|**shortreal**|Equivalent to C *float* type (32 bit)|

#### Chandle data type
- Represent storage for pointers that passed using DPI.

#### String data type
- Dynamic length string data type. Each characters of string can be represented by *byte* type. 

	```Verilog
    string str = "Hello World";
    ```
- SystemVerilog string data type have some built-in function.

	|Function|Description|
	|:-:|:-:|
	|**len()**|Returns number of characters in string|
    |**putc(int n,byte c)**|Replace n<sub>th</sub> character in string to *c*|
    |**getc(int n)**|Returns n<sub>th</sub> character in string|
    |**toupper()**|Convert characters in string to upper case|
    |**tolower()**|Convert characters in string to lower case|
    |**atoi()**|ASCII to integer|
    |**atohex()**|ASCII to hexadecimal number|
    |**atobin()**|ASCII to binary number|

#### User-defined data type
- Keyword **typedef** can make user-defined datatype same as in C/C++.

    ```Verilog
    typedef logic [7:0] vec8_t;
    ```

#### Enumeration
- Enumerated data type can be defined using **enum** keyword. Defining new data type using **typedef** can be possible.

	```Verilog
    typedef bit [1:0] { ST_IDL = 2'b00, ST_RUN = 2'b01 ST_END = 2'b11 } State;
    ```

#### Structure
- Structure is collection of data types to store more than one data types.
	
	```Verilog
	struct { logic [7:0] a; logic[31:0] b; };
	typedef struct { logic [15:0] addr; logic [31:0] data; } Packet_t;
	// keyword typedef can be used to redefine structure as a user-defined data types
	```
## Randomization
- SystemVerilog built-in randomization function that supports conditional constraint randomization
- Returns 1 if randomization is succeeded, or returns 0.
- Can give clauses using constraint block *with { }*
- Can give range and distribution coefficient using **inside** and **dist** keyword.
    
    ```Verilog
    int rvalue[2];
    logic [3:0] state;
    logic [7:0] data ;
    int noerr_flag;
    noerr_flag = randomize(rvalue[0]); // returns signed 32-bit integer
    noerr_flag = randomize(rvalue[1]) with { rvalue[1] > 0; rvalue[1] < 256; };
    // equally distributed random value between (0, 256)
    noerr_flag = randomize(state) with { state inside { [2'b00:2'b01], [2'b11] };}; 
    // set range with keyword inside - state is ranged from 2'b00 ~ 2'b01 or 2'b11
    noerr_flag = randomize(data) with { data dist { [8'd000 : 8'b063] := 3, [8'd064 : 8'd255] := 1 }; };
    // gives weight of randomization 0~63 : 3/4 and 64~255 : 1/4
    ```

## Class

- Class is type of user-defined data type that have member variables(class properties) and its own function or tasks.
- keyword **class** and **endclass** is used to define class and keyword **new** is used to create object(instance of classs) of classs

	```Verilog
	class Packet_t;
	// class properties
	logic [7:0] addr;
	logic [31:0] data;
	function new() // constructor
	    addr = 8'b0;
	    data = 32'b0;
	endfunction
	function logic [7:0] get_addr();
		return addr;
	endfunction
	function logic [31:0] get_data();
		return data;
	endfunction
	endclass
	
	Packet_t p = new; // object creation using new keyword
	```
	
#### Class constructor
- When object is created, it automatically calls new function and it usually initialize class properties.

#### This pointer
- keyword **this** denotes its own class. It used to unambiguously refer to class property.

#### Parameterized class
- Paramiterized class can be used in SystemVerilog
- Types of member variables also can be paramiterized.

    ```Verilog
    class vector_c #(int size=1);
        bit [size-1:0] data;
    endclass
    
    vector_c #(10) ubit10; // 10 bit data container
    
    class data_c #(type T = int);
        local T data;
    endclass
    
    data_c (real) myData;
    ```

#### Inheritance
- Keyword **extends** can extends its class to subclasses

    ```Verilog
    class Packet_p; // super class
        int data0;
        function print_st();
            $display("Data0 : %d\n",data0);
        endfunction
    endclass
    
    class Packet_c extends Packet_p; // sub class
        int data1;
        function print_st();
            $display("Data0 : %d\n",data0);
            $display("Data1 : %d\n",data1);
        endfunction
    endclass
    ```
- Keyword **super** is used to access parent's class member variables or member functions.

#### Encaptulation
- By default, class member variables are visible outside of class. (public)
- To hide member variables, it need to be declared with keyword **local**.
    
    ```Verilog
    class Packet;
        local int x;
    endclass
    ```
#### Virtual Classes

- Virtual Method : Provides prototypes for the method for its descendant class. Thus, virtual method should be overriden in child class.

- Abstract Class : Set a abstract class by using keyword **pure virtual** to make a prototypes of class. Abstract class cannot be instantiated.

    ```Verilog
    class class_p;
        pure virtual function integer my_method(bit [31:0] data); 
        // no implementation (only prototypes)
        endfunction
    endclass
    
    class class_c1;
        virtual function integer my_method(bit [31:0] data);
            $display("data : %d",data);
            return 0;
        endfunction
    endclass
    
    class class_c2;
        virtual function integer my_method(bit [31:0] data);
            return data;
        endfunction
    endclass
    
    ```
    
#### Polymorphism 


## Interface
- Interface is bundle of ports. The interface encapsulates port connectivity and functionality. To provide direction information of port can be done by keyword **modport**.  
	```Verilog
	interface interface_name;
	// interface items ...
	endinterface
	```

- In interface, it can have input and output, function and task and also can have parameter.


- For example, APB interface can be defined as follows:
	```Verilog
	inteface apb3_bus #(AW=16, DW=32) (input clk);
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
	```Verilog
	inteface apb3_bus #(AW=16, DW=32) (input clk);
	wire psel; 
	wire penable;
	wire pready;
	wire pwrite;
	wire [AW-1:0] paddr;
	wire [DW-1:0] pwdata;
	wire [DW-1:0] prdata;
	modport master (input psel, input penable, input pwrite, input [AW-1:0] paddr, input [DW-1:0] pwdata, output pready, output [DW-1:0] prdata);
	modport slave (output psel, output penable, output pwrite, output [AW-1:0] paddr, output [DW-1:0] pwdata, input pready, 	input [DW-1:0] prdata);
	endinterface
	```
	
## Clocking Block

- Gives time-skew description to testbench. (input and output skew) 
- When input signal skew is specified, input signal will be sampled *before* clock edge.
- When output signal skew is speficied, output signal will be driven *after clock edge.

    ```Verilog
    clocking myClk @(posedge clk);
        input #1 sig1;   // input driven before #1 of posedge
        output #1 sig2;  // output driven after #1 of posedge
    endcloking
    ```

## Package

- Another way of mechanism to share data types, functions, tasks, properties between different modules.	
	```Verilog
	package package_name;
	// data declaration
	// function or task declaration
	endpackage
	```


- Package need to be imported within modules to use. package import can be done by keyword import. Both explicit import and wildcard import can be done.
	```Verilog
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

## Functional Coverage (Covergroup)

	```Verilog
	covergroup cg; /* Insert covergroup content here*/ endgroup
	cg cg_inst = new; // covergroup instantiation
	````

## SVA(System Verilog Assertion)

Assertion is used to validate functionality (property or interface protocol). There are two kinds of assertions - Immediate assertion and Concurrent assertion. 

- Immediate assertion : Called as "simple assertion" and pass / fail of assertion take place immediately. 

	```verilog
	assert ( A == B) else $faltal("A and B is not same); 
	// assertion fails when A and B is not same and 
	```
	
- Concurrent assertion : Pass / fail of assertion take place over time. Unlike immediate assertion, evaluation of assertion take place when clock tick. 
	
	```verilog
	assert @(posedge clk) req |-> #1 ack; 
	// ack is high 1-clk after req is high
	```
	
Behaviour of assertion failure can be specified with clauses **else**. There are 4 types of severity system tasks that can be uses in else clauses. 

	- $fatal   : displays run-time fatal messages and stop the simulation
	- $error   : displays run-time error messages and stop the simulation
	- $warning : displays run-time warning messages
	- $info    : indicates assertion failure
	
#### Sequence



#### Property


## Coverage

## System Directives
- `default_nettype 
- `resetall

## References

[1] http://www.asic-world.com/systemverilog/dpi.html

[2] http://testbench.in/DP_00_INDEX.html


