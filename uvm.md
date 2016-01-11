# Type UVM tutorial here
https://colorlesscube.com/uvm-guide-for-beginners/
https://s3.amazonaws.com/cookbook.verification.academy/pdfs/uvm-cookbook-complete-verification-academy.pdf?AWSAccessKeyId=AKIAIEMNC32PGID7APKA&Expires=1447822485&Signature=DgkLiSG2n8824hDfLte4TlaJBfw%3D

http://www.edaplayground.com/



import uvm_pkg::*;

## Verification Components

### Modeling data item 

#### uvm_sequence_item 

- To define transfer random data with constraints, UVM class library provides **uvm_sequence_item** base class
- Inherite uvm_sequence_item class to create communication data between two or more components.

    ````Verilog
    class simple_item extends uvm_sequence_item; // extend from uvm_sequence_item base class
        rand bit   enable;
        rand int   data0 ;
        rand int   data1 ;
        constraint c_d0 { data0 < 20000; } // gives constraint
        constraint c_d1 { data1 < 30000; }
        `uvm_object_util_begin(simple_item)
            `uvm_field_int(enable, UVM_ALL_ON); // 
            `uvm_field_int(data0 , UVM_ALL_ON);
            `uvm_field_int(data1 , UVM_ALL_ON); 
        `uvm_object_util_end
    endclass
    ````
### Transaction Level Component

There are 4 transaction-level components in UMV
- Sequencer  : Stimulus generator
- Driver     : Convert transaction from sequencer to signal-level data
- Monitor    : Recognize signal-level activity and convert to transaction
- Scoreboard : Analyze transactions (such as coverage etc.)

#### Driver

- Driver convert data transaction from sequencer to DUT.
- UVM class library provides **uvm_driver** base class

    ````Verilog
    ````

#### Sequencer

- Sequencer generates stimulus and pass it to driver.

    ````Verilog
    uvm_sequencer #(simple_item, simple_rsp) sequencer; 
    // 2nd parameter (request and response) is optional parameter
    ````
