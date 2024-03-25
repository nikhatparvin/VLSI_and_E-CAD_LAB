## E-CAD programs


#### 1. HDL code to realize all the logic gates

```
//Design file for all the logic gates - allgates.v

module allgates(input a,b,output yand,ynand,yor,ynor,yxor,yxnor,ynota,ynotb);

and g1(yand,a,b);
nand g2(ynand,a,b);
or g3(yor,a,b);
nor g4(ynor,a,b);
xor g5(yxor,a,b);
xnor g6(yxnor,a,b);
not g7(ynota,a);
not g8(ynotb,b);

endmodule
```

```
//Testbench file of all the logic gates - allgates_tb.v

`timescale 1ns / 1ps

module allgates_tb;

    // Inputs
    reg a, b;

    // Outputs
    wire yand, ynand, yor, ynor, yxor, yxnor, ynota, ynotb;

    // Instantiate the module under test
    allgates dut(
        .a(a),
        .b(b),
        .yand(yand),
        .ynand(ynand),
        .yor(yor),
        .ynor(ynor),
        .yxor(yxor),
        .yxnor(yxnor),
        .ynota(ynota),
        .ynotb(ynotb)
    );

     // Initial stimulus
    initial begin
        // Initialize inputs
        a = 0;
        b = 0;

        // Monitor outputs
        $monitor("Time=%0t a=%b b=%b yand=%b ynand=%b yor=%b ynor=%b yxor=%b yxnor=%b ynota=%b ynotb=%b",
                 $time, a, b, yand, ynand, yor, ynor, yxor, yxnor, ynota, ynotb);

        // Apply various input combinations
        #20 a = 0; b = 0; // Test case 1
        #20 a = 0; b = 1; // Test case 2
        #20 a = 1; b = 0; // Test case 3
        #20 a = 1; b = 1; // Test case 4

        // End simulation
        #20 $finish;
    end

endmodule
```
#### 2. Design of 2-to-4 decoder

```
//design file of 2-to-4 decoder - decoder2to4.v

module decoder2to4(input a,b,output y3,y2,y1,y0);

not g1(abar,a);
not g2(bbar,b);
and g3(y0,abar,bbar);
and g4(y1,abar,b);
and g5(y2,a,bbar);
and g6(y3,a,b);

endmodule
```
```
//Testbench file of 2-to-4 decoder - decoder2to4_tb.v

`timescale 1ns / 1ps

module decoder2to4_tb;

   
    // Inputs
    reg a, b;

    // Outputs
    wire y3, y2, y1, y0;

    // Instantiate the module under test
    decoder2to4 dut(
        .a(a),
        .b(b),
        .y3(y3),
        .y2(y2),
        .y1(y1),
        .y0(y0)
    );

  // Initial stimulus
    initial begin
        // Initialize inputs
        a = 0;
        b = 0;

        // Monitor outputs
        $monitor("Time=%0t a=%b b=%b y3=%b y2=%b y1=%b y0=%b",$time, a, b, y3, y2, y1, y0);

        // Apply various input combinations
        #20 a = 0; b = 0; // Test case 1
        #20 a = 0; b = 1; // Test case 2
        #20 a = 1; b = 0; // Test case 3
        #20 a = 1; b = 1; // Test case 4

        // End simulation
        #20 $finish;
    end

endmodule
```

#### 3. Design of 8-to-3 encoder (without and with priority)

```
//Design of 8-to-3 encoder (without priority) - encoder8to3_withoutpriority.v

module encoder8to3_withoutpriority(input [7:0]i, output [2:0]y);

reg [2:0]y;

always@(i)
case(i)

//8'bi[7]i[6]i[5]i[4]i[3]i[2]i[1]i[0] 

8'b0000_0001:y=3'b000;
8'b0000_0010:y=3'b001;
8'b0000_0100:y=3'b010;
8'b0000_1000:y=3'b011;
8'b0001_0000:y=3'b100;
8'b0010_0000:y=3'b101;
8'b0100_0000:y=3'b110;
8'b1000_0000:y=3'b111;

default : y=3'bxxx;

endcase

endmodule
```
```
//Testbench of 8-to-3 encoder (without priority)

`timescale 1ns / 1ps

module encoder8to3_withoutpriority_tb;

    // Inputs
    reg [7:0] i;

    // Outputs
    wire [2:0] y;

    // Instantiate the module under test
    encoder8to3_withoutpriority dut(.i(i),.y(y));

      // Initial stimulus
    initial begin
        // Initialize inputs
        i = 8'b0000_0000;

        // Monitor outputs
        $monitor("Time=%0t i=%b y=%b", $time, i, y);

        // Apply various input combinations
        repeat (256) begin
            #20 i = i + 1; // Increment input i
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
//Design of 8-to-3 encoder (with priority) - encoder8to3_withpriority.v

module encoder8to3_withpriority(input [7:0]i, output [2:0]y);

reg [2:0]y;

always@(i)
casex(i)

//8'bi[7]i[6]i[5]i[4]i[3]i[2]i[1]i[0] 

8'b0000_0001:y=3'b000;
8'b0000_001x:y=3'b001;
8'b0000_01xx:y=3'b010;
8'b0000_1xxx:y=3'b011;
8'b0001_xxxx:y=3'b100;
8'b001x_xxxx:y=3'b101;
8'b01xx_xxxx:y=3'b110;
8'b1xxx_xxxx:y=3'b111;

default : y=3'bxxx;

endcase

endmodule
```
```
// Testbench of 8-to-3 encoder (with priority)

`timescale 1ns / 1ps

module encoder8to3_withpriority_tb;

    // Inputs
    reg [7:0] i;

    // Outputs
    wire [2:0] y;

    // Instantiate the module under test
    encoder8to3_withpriority dut(.i(i),.y(y));

       // Initial stimulus
    initial begin
        // Initialize inputs
        i = 8'b0000_0000;

        // Monitor outputs
        $monitor("Time=%0t i=%b y=%b", $time, i, y);

        // Apply various input combinations
        repeat (256) begin
            #20 i = i + 1; // Increment input i
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
#### 4. Design of 8-to-1 multiplexer and 1-to-8 demultiplexer
```
// Design file 8-to-1 multiplexer - mux8to1.v

module mux8to1(input [7:0]i, input [2:0]sel, output y);

reg y;

always@(*)
case(sel)

0: y=i[0];
1: y=i[1];
2: y=i[2];
3: y=i[3];
4: y=i[4];
5: y=i[5];
6: y=i[6];
7: y=i[7];

default: y=1'bx;

endcase

endmodule
```
```
// Testbench file 8-to-1 multiplexer  -   mux8to1_tb.v

`timescale 1ns / 1ps

module mux8to1_tb;

    // Inputs
    reg [7:0] i;
    reg [2:0] sel;

    // Output
    wire y;

    // Instantiate the module under test
    mux8to1 dut(
        .i(i),
        .sel(sel),
        .y(y)
    );

    // Initial stimulus
    initial begin
        // Initialize inputs
        i = 8'b1010_1010;
        sel = 3'b000;

        // Monitor outputs
        $monitor("Time=%0t i=%b sel=%b y=%b", $time, i, sel, y);

        // Apply various input combinations
        repeat (8) begin
            #20 sel = sel + 1; // Increment sel
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
// Design file 1-to-8 demultiplexer  -  demux1to8.v

module demux1to8(input i, input [2:0]sel, output [7:0]y);

reg [7:0]y;

always@*

case(sel)

0: y[0]=i;
1: y[1]=i;
2: y[2]=i;
3: y[3]=i;
4: y[4]=i;
5: y[5]=i;
6: y[6]=i;
7: y[7]=i;

default: y=8'bx;

endcase

endmodule
```
```
//Testbench file 1-to-8 demultiplexer - demux1to8_tb.v

`timescale 1ns / 1ps

module demux1to8_tb;

    // Inputs
    reg i;
    reg [2:0] sel;

    // Outputs
    wire [7:0] y;

    // Instantiate the module under test
    demux1to8 dut(
        .i(i),
        .sel(sel),
        .y(y)
    );

    // Initial stimulus
    initial begin
        // Initialize inputs
        i = 1'b0;
        sel = 3'b000;

        // Monitor outputs
        $monitor("Time=%0t i=%b sel=%b y=%b", $time, i, sel, y);

        // Apply various input combinations
        repeat (8) begin
            #20 sel = sel + 1; // Increment sel
			    i=~i;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
#### 5. Design of 4-bit binary to gray code converter  - binary2gray4bit.v
```
module binary2gray4bit(input [3:0]binary, output [3:0]gray);

buf (gray[3], binary[3]);
xor (gray[2], binary[3], binary[2]);
xor (gray[1], binary[2], binary[1]);
xor (gray[0], binary[1], binary[0]); 

endmodule
```
```
// Testbench file of 4-bit binary to gray code converter - binary2gray4bit_tb.v

`timescale 1ns / 1ps

module binary2gray4bit_tb;

    
    // Inputs
    reg [3:0] binary;

    // Outputs
    wire [3:0] gray;

    // Instantiate the module under test
    binary2gray4bit dut(
        .binary(binary),
        .gray(gray)
    );

    // Initial stimulus
    initial begin
        // Initialize inputs
        binary = 4'b0000;

        // Monitor outputs
        $monitor("Time=%0t binary=%b gray=%b", $time, binary, gray);

        // Apply various input combinations
        repeat (16) begin
            #20 binary = binary + 1; // Increment binary
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
#### 6. Design of 4-bit comparator - comparator4bit.v
```
module comparator4bit(input [3:0] a,b, output aeqb,altb,agtb);

assign aeqb = a==b;
assign altb = a<b;
assign agtb = a>b;

endmodule
```
```
// Testbench of 4-bit comparator - comparator4bit_tb.v

`timescale 1ns / 1ps

module comparator4bit_tb;

    // Inputs
    reg [3:0] a, b;

    // Outputs
    wire aeqb, altb, agtb;

    // Instantiate the module under test
    comparator4bit dut(
        .a(a),
        .b(b),
        .aeqb(aeqb),
        .altb(altb),
        .agtb(agtb)
    );

    // Initial stimulus
    initial begin
        // Monitor outputs
        $monitor("Time=%0t a=%b b=%b aeqb=%b altb=%b agtb=%b", $time, a, b, aeqb, altb, agtb);

        // Apply various input combinations
        repeat (16) begin
            #20 a = $random;
            #20 b = $random;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
####  7. Design of Full adder using 3 modeling styles
```
// 7.1 Gatelevel modeling fa_gatelevel.v

	module fa_gatelevel(input a,b,cin, output sum,co);
	
	xor g1(sum,a,b,cin);
	and g2(c1,a,b);
	and g3(c2,a,cin);
	and g4(c3,b,cin);
	or g5(co,c1,c2,c3);	
	
	endmodule
	```
```
// 7.1 Testbench of Gatelevel modeling fa_gatelevel_tb.v

`timescale 1ns / 1ps

module fa_gatelevel_tb;

    // Inputs
    reg a, b, cin;

    // Outputs
    wire sum, co;
	
	integer i;

    // Instantiate the module under test
    fa_gatelevel dut(
        .a(a),
        .b(b),
        .cin(cin),
        .sum(sum),
        .co(co)
    );

    // Initial stimulus
    initial begin
        // Monitor outputs
        $monitor("Time=%0t a=%b b=%b cin=%b sum=%b co=%b", $time, a, b, cin, sum, co);

        // Apply various input combinations
        for(i=0;i<8; i=i+1) begin
            {a,b,cin}=i;
			#10;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
// 7.2 Dataflow modeling  - fa_dataflow.v

	module fa_dataflow(input a,b,cin, output sum,co);
	
	
	assign sum=a^b^cin;
	assign co=(a&b)|(a&cin)|(b&cin);
	
	endmodule
```
```	
// 7.2 Testbench of Dataflow modeling fa_dataflow_tb.v

`timescale 1ns / 1ps

module fa_dataflow_tb;

    // Inputs
    reg a, b, cin;

    // Outputs
    wire sum, co;
	
	integer i;

    // Instantiate the module under test
    fa_dataflow dut(
        .a(a),
        .b(b),
        .cin(cin),
        .sum(sum),
        .co(co)
    );

    // Initial stimulus
    initial begin
        // Monitor outputs
        $monitor("Time=%0t a=%b b=%b cin=%b sum=%b co=%b", $time, a, b, cin, sum, co);

        // Apply various input combinations
        for(i=0;i<8; i=i+1) begin
            {a,b,cin}=i;
			#10;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```	
// 7.3 Behavioural modeling  - fa_behavioural.v

	module fa_behavioural(input a,b,cin, output sum,co);
	
	reg sum,co;
	
	always@*
	case({a,b,cin})
	0: begin sum=0; co=0; end
	1,2,4: begin sum=1; co=0; end
	5,6: begin sum=0; co=1; end
	7: begin sum=1; co=1; end
	
	endcase
	
	endmodule
```
```
// 7.3 Testbench of Behavioural modeling - fa_behavioural_tb.v

`timescale 1ns / 1ps

module fa_behavioural_tb;

    // Inputs
    reg a, b, cin;

    // Outputs
    wire sum, co;
	
	integer i;

    // Instantiate the module under test
    fa_behavioural dut(
        .a(a),
        .b(b),
        .cin(cin),
        .sum(sum),
        .co(co)
    );

    // Initial stimulus
    initial begin
        // Monitor outputs
        $monitor("Time=%0t a=%b b=%b cin=%b sum=%b co=%b", $time, a, b, cin, sum, co);

        // Apply various input combinations
        for(i=0;i<8; i=i+1) begin
            {a,b,cin}=i;
			#10;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
#### 8. Design of flip flops: SR, D, JK, T
```
// 8.1 SR Flipflop - srff.v

module srff(input clk,rst,s,r, output reg q,qbar);

always@(posedge clk)
begin
if(rst)
begin
q<=0;
qbar<=1;
end
else
case({s,r})
2'b00: begin q<=q; qbar<=qbar; end
2'b01: begin q<=0; qbar<=1; end
2'b10: begin q<=1; qbar<=0; end
2'b11: begin q<=1'bx; qbar<=1'bx; end
endcase

end

endmodule
```
```
// Testbench of 8.1 SR Flipflop - srff_tb.v 

`timescale 1ns / 1ps

module srff_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst, s, r;

    // Outputs
    wire q, qbar;

    // Instantiate the module under test
    srff dut(
        .clk(clk),
        .rst(rst),
        .s(s),
        .r(r),
        .q(q),
        .qbar(qbar)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
		clk = 0;
        rst = 1;
        s = 0;
        r = 0;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b s=%b r=%b q=%b qbar=%b", $time, clk, rst, s, r, q, qbar);

        // Apply reset
        #20 rst = 0;

        // Apply various input values
        for(i=0;i<4;i=i+1) begin
           {s,r}=i;
		   #20;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
// 8.2 D Flipflop - dff.v

module dff(input clk,rst,d,output reg q,qbar);

always@(posedge clk)
begin
if(rst)
begin
q<=0;
qbar<=1;
end
else
case(d)
0: begin q<=0; qbar<=1; end
1: begin q<=1; qbar<=0; end
endcase

end

endmodule
```
```
// Testbench of 8.2 D Flipflop - dff_tb.v

`timescale 1ns / 1ps

module dff_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst, d;

    // Outputs
    wire q, qbar;

    // Instantiate the module under test
    dff dut(
        .clk(clk),
        .rst(rst),
        .d(d),
        .q(q),
        .qbar(qbar)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
		clk = 0;
        rst = 1;
        d = 0;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b d=%b q=%b qbar=%b", $time, clk, rst, d, q, qbar);

        // Apply reset
        #20 rst = 0;

        // Apply various input values
       for(i=0;i<20; i=i+1) begin
            #20 d = ~d;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
// 8.3 JK Flipflop

module jkff(input clk,rst,j,k, output reg q,qbar);

always@(posedge clk)
begin
if(rst)
begin
q<=0;
qbar<=1;
end
else
case({j,k})
2'b00: begin q<=q; qbar<=qbar; end
2'b01: begin q<=0; qbar<=1; end
2'b10: begin q<=1; qbar<=0; end
2'b11: begin q<=~q; qbar<=~qbar; end
endcase

end

endmodule
```
```
// Testbench of 8.3 JK Flipflop - jkff_tb.v

`timescale 1ns / 1ps

module jkff_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst, j, k;

    // Outputs
    wire q, qbar;

    // Instantiate the module under test
    jkff dut(
        .clk(clk),
        .rst(rst),
        .j(j),
        .k(k),
        .q(q),
        .qbar(qbar)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
		clk = 0;
        rst = 1;
        j = 0;
        k = 0;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b j=%b k=%b q=%b qbar=%b", $time, clk, rst, j, k, q, qbar);

        // Apply reset
        #20 rst = 0;

        // Apply various inputs
        for(i=0;i<4;i=i+1) begin
           {j,k}=i;
		   #20;
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
// 8.4 T Flipflop

module tff(input clk,rst,t,output reg q,qbar);

always@(posedge clk)
begin
if(rst)
begin
q<=0;
qbar<=1;
end
else
case(t)
0: begin q<=0; qbar<=1; end
1: begin q<=~q; qbar<=~qbar; end
endcase

end

endmodule
```
```
// Testbench of 8.4 T Flipflop tff_tb.v

`timescale 1ns / 1ps

module tff_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst, t;

    // Outputs
    wire q, qbar;

    // Instantiate the module under test
    tff dut(
        .clk(clk),
        .rst(rst),
        .t(t),
        .q(q),
        .qbar(qbar)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
		clk = 0;
        rst = 1;
        t = 0;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b t=%b q=%b qbar=%b", $time, clk, rst, t, q, qbar);

        // Apply reset
        #20 rst = 0;

        // Apply toggling 't' input
        repeat (10) begin
            #20 t = ~t; // Toggle 't' input
        end

        // End simulation
        #20 $finish;
    end

endmodule
```
```
//9. Design of 4-bit binary, BCD counters (synchronous/asynchronous reset) or any sequence counter

// 9.1  Design of 4-bit binary counter (synchronous reset) - binary4bitcounter_sr.v

module binary4bitcounter_sr(input clk,rst,output reg [3:0]count);

always@(posedge clk)

if(rst)
count<=4'b0000;
else
count<=count+1'b1;

endmodule
```
```
// Testbench of 4-bit binary counter (synchronous reset) - binary4bitcounter_sr_tb.v

`timescale 1ns / 1ps

module binary4bitcounter_sr_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst;

    // Outputs
    reg [3:0] count;

    // Instantiate the module under test
    binary4bitcounter_sr dut(
        .clk(clk),
        .rst(rst),
        .count(count)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
		clk = 0;
        rst = 1;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b count=%b", $time, clk, rst, count);

        // Apply reset
        #20 rst = 0;

        // End simulation
        #200 $finish;
    end

endmodule
```
```
// 9.2  Design of 4-bit binary counter (asynchronous reset) - binary4bitcounter_asr.v

module binary4bitcounter_asr(input clk,rst,output reg [3:0]count);

always@(posedge clk,posedge rst)

if(rst)
count<=4'b0000;
else
count<=count+1'b1;

endmodule
```
```
// 9.2 Testbench of 4-bit binary counter (asynchronous reset) - binary4bitcounter_asr_tb.v

`timescale 1ns / 1ps

module binary4bitcounter_asr_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst;

    // Outputs
    reg [3:0] count;

    // Instantiate the module under test
    binary4bitcounter_asr dut(
        .clk(clk),
        .rst(rst),
        .count(count)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
       clk = 0;
        rst = 1;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b count=%b", $time, clk, rst, count);

        // Apply reset
        #20 rst = 0;
      
      #50 rst = 1;
      
      #40 rst=0;

        // End simulation
        #200 $finish;
    end
  
endmodule
```
```
// 9.3  Design of 4-bit BCD counter (synchronous reset) bcd4bitcounter_sr.v

module bcd4bitcounter_sr(input clk,rst,output reg [3:0]count);

always@(posedge clk)

begin

if(rst)
count<=4'b0000;
else
begin
if(count<4'b1001)
count<=count+1'b1;
else
count<=count;
end

end
endmodule
```
```
// Testbench of 4-bit BCD counter (synchronous reset)  bcd4bitcounter_tb.v

`timescale 1ns / 1ps

module bcd4bitcounter_sr_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst;

    // Outputs
    reg [3:0] count;

    // Instantiate the module under test
    bcd4bitcounter_sr dut(
        .clk(clk),
        .rst(rst),
        .count(count)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
        rst = 1;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b count=%b", $time, clk, rst, count);

        // Apply reset
        #20 rst = 0;

        // End simulation
        #200 $finish;
    end

endmodule
```
```
// 9.4  Design of 4-bit BCD counter (asynchronous reset) -  bcd4bitcounter_asr.v

module bcd4bitcounter_asr(input clk,rst,output reg [3:0]count);

always@(posedge clk,posedge rst)

begin

if(rst)
count<=4'b0000;
else
begin
if(count<4'b1001)
count<=count+1'b1;
else
count<=count;
end

end
endmodule
```
```
// 9.4 Testbench of 4-bit BCD counter (asynchronous reset) -  bcd4bitcounter_asr_tb.v

`timescale 1ns / 1ps

module bcd4bitcounter_asr_tb;

    // Define constants
    parameter PERIOD = 10;

    // Inputs
    reg clk, rst;

    // Outputs
    reg [3:0] count;

    // Instantiate the module under test
    bcd4bitcounter_asr dut(
        .clk(clk),
        .rst(rst),
        .count(count)
    );

    // Clock generation
    always #((PERIOD)/2) clk = ~clk; // Toggle clk every half period

    // Initial stimulus
    initial begin
        // Initialize inputs
       clk = 0;
        rst = 1;

        // Monitor outputs
        $monitor("Time=%0t clk=%b rst=%b count=%b", $time, clk, rst, count);

        // Apply reset
        #20 rst = 0;
      
      #50 rst = 1;
      
      #40 rst=0;

        // End simulation
        #200 $finish;
    end
  
endmodule
```
