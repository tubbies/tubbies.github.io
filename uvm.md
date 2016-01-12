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
    class simple_drv extends uvm_driver #(simple_item);
        simple_item s_item;
        virtual dut_if vif;
        
        function new(string name="simple driver");
            $super.new(name,parent);
        endfunction : new // constructor
        
        function void build_phase(uvm_phase phase);
            super.build_phase(phase);
        endfunction : build_phase
        
        task run_phase(uvm_phase phase);
            forever begin
                seq_item_port.get_next_item(s_item);
                // describe driving to vif;
                seq_item_port.item_done();
            end
        endtask : run_phase
    endclass
    ````

#### Sequencer

- Sequencer generates stimulus and pass it to driver.

    ````Verilog
    uvm_sequencer #(simple_item, simple_rsp) sequencer; 
    // 2nd parameter (request and response) is optional parameter
    ````
