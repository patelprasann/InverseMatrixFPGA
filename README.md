# InverseMatrixFPGA
The Following code in Made in VLSI such that it can invert a matrix of size 5*5 on a FPGA 

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 	Prasann Patel
// Project Work: 5*5 Matrix Inversion on FP 
// SUBJECT: LINEAR ALGEIRA
// Create Date:    17:04:38 10/05/2016 
// Design Name: 
// Module Name:    LA5 
// Project Name: 
// Target Devices: 
// Tool versions: 
// Description: 
//
// Dependencies: 
//
// Revision: 
// Revision 0.01 - File Created
// Additional Comments: 
//inputData11,inputData12,inputData13,inputData14,inputData15,inputData21,inputData22,inputData23,inputData24,inputData25,inputData31,inputData32,inputData33,inputData34,inputData35,inputData41,inputData42,inputData43,inputData44,inputData45,inputData51,inputData52,inputData53,inputData54,inputData55
//////////////////////////////////////////////////////////////////////////////////

//main module

module LA5(clk, reset, data_out, address, identity11,identity12,identity13,identity14,identity15,identity21,identity22,identity23,identity24,identity25,identity31,identity32,identity33,identity34,identity35,identity41,identity42,identity43,identity44,identity45,identity51,identity52,identity53,identity54,identity55,identity11d,identity12d,identity13d,identity14d,identity15d,identity21d,identity22d,identity23d,identity24d,identity25d,identity31d,identity32d,identity33d,identity34d,identity35d,identity41d,identity42d,identity43d,identity44d,identity45d,identity51d,identity52d,identity53d,identity54d,identity55d);

output [31:0] identity11d,identity12d,identity13d,identity14d,identity15d;			//32Bits
output [31:0] identity21d,identity22d,identity23d,identity24d,identity25d;
output [31:0] identity31d,identity32d,identity33d,identity34d,identity35d;
output [31:0] identity41d,identity42d,identity43d,identity44d,identity45d;
output [31:0] identity51d,identity52d,identity53d,identity54d,identity55d;

output reg [31:0] identity11,identity12,identity13,identity14,identity15;			//Register Wires of 32Bits
output reg [31:0] identity21,identity22,identity23,identity24,identity25;
output reg [31:0] identity31,identity32,identity33,identity34,identity35;
output reg [31:0] identity41,identity42,identity43,identity44,identity45;
output reg [31:0] identity51,identity52,identity53,identity54,identity55;

reg [31:0] matrix[0:25];
reg [31:0] inverse[0:24];

output [31:0] data_out;

input reset;
input [4:0] address;

reg [31:0] data_in11,data_in12,data_in13,data_in14,data_in15;
reg [31:0] data_in21,data_in22,data_in23,data_in24,data_in25;
reg [31:0] data_in31,data_in32,data_in33,data_in34,data_in35;
reg [31:0] data_in41,data_in42,data_in43,data_in44,data_in45;
reg [31:0] data_in51,data_in52,data_in53,data_in54,data_in55;
input clk;


//Initializing the Identity Matrix

reg [31:0] identity11t = 32'd1;
reg [31:0] identity12t = 32'd0;
reg [31:0] identity13t = 32'd0;
reg [31:0] identity14t = 32'd0;
reg [31:0] identity15t = 32'd0;

reg [31:0] identity21t = 32'd0;
reg [31:0] identity22t = 32'd1;
reg [31:0] identity23t = 32'd0;
reg [31:0] identity24t = 32'd0;
reg [31:0] identity25t = 32'd0;

reg [31:0] identity31t = 32'd0;
reg [31:0] identity32t = 32'd0;
reg [31:0] identity33t = 32'd1;
reg [31:0] identity34t = 32'd0;
reg [31:0] identity35t = 32'd0;

reg [31:0] identity41t = 32'd0;
reg [31:0] identity42t = 32'd0;
reg [31:0] identity43t = 32'd0;
reg [31:0] identity44t = 32'd1;
reg [31:0] identity45t = 32'd0;

reg [31:0] identity51t = 32'd0;
reg [31:0] identity52t = 32'd0;
reg [31:0] identity53t = 32'd0;
reg [31:0] identity54t = 32'd0;
reg [31:0] identity55t = 32'd1;

reg [31:0] answer_temp11,answer_temp12,answer_temp13,answer_temp14,answer_temp15,
		answer_temp21,answer_temp22,answer_temp23,answer_temp24,answer_temp25,
			answer_temp31,answer_temp32,answer_temp33,answer_temp34,answer_temp35,
				answer_temp41,answer_temp42,answer_temp43,answer_temp44,answer_temp45,
					answer_temp51,answer_temp52,answer_temp53t,answer_temp54t,answer_temp55t;

reg [31:0] x;
reg [31:0] y;

matrix_ROM set_value (
  .clka(clk), // input clka
  .addra(address), // input [4 : 0] addra
  .douta(data_out) // output [31 : 0] douta
);


always@(posedge clk)
begin

if(reset==1'b0)
begin
	matrix[address] <= data_out;
end

else
begin

						//assigning values

data_in11 = matrix[1];
data_in12 = matrix[2];
data_in13 = matrix[3];
data_in14 = matrix[4];
data_in15 = matrix[5];

data_in21 = matrix[6];
data_in22 = matrix[7];
data_in23 = matrix[8];
data_in24 = matrix[9];
data_in25 = matrix[10];

data_in31 = matrix[11];
data_in32= matrix[12];
data_in33= matrix[13];
data_in34 = matrix[14];
data_in35 = matrix[15];

data_in41 = matrix[16];
data_in42 =  matrix[17];
data_in43 = matrix[18];
data_in44 = matrix[19];
data_in45 = matrix[20];

data_in51 = matrix[21];
data_in52 = matrix[22];
data_in53 = matrix[23];
data_in54 = matrix[24];
data_in55 = matrix[25];



answer_temp11 = (data_in11 > 0 ? data_in11 : (~data_in11+32'd1));
answer_temp12 = (data_in12 > 0 ? data_in12 : (~data_in12+32'd1));
answer_temp13 = (data_in13 > 0 ? data_in13 : (~data_in13+32'd1));
answer_temp14 = (data_in14 > 0 ? data_in14 : (~data_in14+32'd1));
answer_temp15 = (data_in15 > 0 ? data_in15 : (~data_in15+32'd1));

answer_temp21 = (data_in21 > 0 ? data_in21 : (~data_in21 + 32'd1));
answer_temp22 = (data_in22 > 0 ? data_in22 : (~data_in22 + 32'd1));
answer_temp23 = (data_in23 > 0 ? data_in23 : (~data_in23 + 32'd1));
answer_temp24 = (data_in24 > 0 ? data_in24 : (~data_in24 + 32'd1));
answer_temp25 = (data_in25 > 0 ? data_in25 : (~data_in25 + 32'd1));

answer_temp31 = (data_in31 > 0 ? data_in31 : (~data_in31 + 32'd1));
answer_temp32 = (data_in32 > 0 ? data_in32 : (~data_in32 + 32'd1));
answer_temp33 = (data_in33 > 0 ? data_in33 : (~data_in33 + 32'd1));
answer_temp34 = (data_in34 > 0 ? data_in34 : (~data_in34 + 32'd1));
answer_temp35 = (data_in35 > 0 ? data_in35 : (~data_in35 + 32'd1));

answer_temp41 = (data_in41 > 0 ? data_in41 : (~data_in41+32'd1));
answer_temp42 = (data_in42 > 0 ? data_in42 : (~data_in42+32'd1));
answer_temp43 = (data_in43 > 0 ? data_in43 : (~data_in43+32'd1));
answer_temp44 = (data_in44 > 0 ? data_in44 : (~data_in44+32'd1));
answer_temp45 = (data_in45 > 0 ? data_in45 : (~data_in45+32'd1));

answer_temp51 = (data_in51 > 0 ? data_in51 : (~data_in51+32'd1));
answer_temp52 = (data_in52 > 0 ? data_in52 : (~data_in52+32'd1));
answer_temp53t = (data_in53 > 0 ? data_in53 : (~data_in53+32'd1));
answer_temp54t = (data_in54 > 0 ? data_in54 : (~data_in54+32'd1));
answer_temp55t = (data_in55 > 0 ? data_in55 : (~data_in55+32'd1));



x = answer_temp11;
y = answer_temp21;

answer_temp21 = x*answer_temp21 - y*answer_temp11;
answer_temp22 = x*answer_temp22 - y*answer_temp12;
answer_temp23 = x*answer_temp23 - y*answer_temp13;
answer_temp24 = x*answer_temp24 - y*answer_temp14;
answer_temp25 = x*answer_temp25 - y*answer_temp15;
identity21t = x*identity21t - y*identity11t;
identity22t = x*identity22t - y*identity12t;
identity23t = x*identity23t - y*identity13t;
identity24t = x*identity24t - y*identity14t;
identity25t = x*identity25t - y*identity15t;

x = answer_temp11;
y = answer_temp31;

answer_temp31 = x*answer_temp31 - y*answer_temp11;
answer_temp32 = x*answer_temp32 - y*answer_temp12;
answer_temp33 = x*answer_temp33 - y*answer_temp13;
answer_temp34 = x*answer_temp34 - y*answer_temp14;
answer_temp35 = x*answer_temp35 - y*answer_temp15;
identity31t = x*identity31t - y*identity11t;
identity32t = x*identity32t - y*identity12t;
identity33t = x*identity33t - y*identity13t;
identity34t = x*identity34t - y*identity14t;
identity35t = x*identity35t - y*identity15t;

x = answer_temp11;
y = answer_temp41;

answer_temp41 = x*answer_temp41 - y*answer_temp11;
answer_temp42 = x*answer_temp42 - y*answer_temp12;
answer_temp43 = x*answer_temp43 - y*answer_temp13;
answer_temp44 = x*answer_temp44 - y*answer_temp14;
answer_temp45 = x*answer_temp45 - y*answer_temp15;
identity41t = x*identity41t - y*identity11t;
identity42t = x*identity42t - y*identity12t;
identity43t = x*identity43t - y*identity13t;
identity44t = x*identity44t - y*identity14t;
identity45t = x*identity45t - y*identity15t;

x = answer_temp11;
y = answer_temp51;

answer_temp51 = x*answer_temp51 - y*answer_temp11;
answer_temp52 = x*answer_temp52 - y*answer_temp12;
answer_temp53t = x*answer_temp53t - y*answer_temp13;
answer_temp54t = x*answer_temp54t - y*answer_temp14;
answer_temp55t = x*answer_temp55t - y*answer_temp15;
identity51t = x*identity51t - y*identity11t;
identity52t = x*identity52t - y*identity12t;
identity53t = x*identity53t - y*identity13t;
identity54t = x*identity54t - y*identity14t;
identity55t = x*identity55t - y*identity15t;


/*         */


x = answer_temp22;
y = answer_temp12;

answer_temp11 = x*answer_temp11 - y*answer_temp21;
answer_temp12 = x*answer_temp12 - y*answer_temp22;
answer_temp13 = x*answer_temp13 - y*answer_temp23;
answer_temp14 = x*answer_temp14 - y*answer_temp24;
answer_temp15 = x*answer_temp15 - y*answer_temp25;
identity11t = x*identity11t - y*identity21t;
identity12t = x*identity12t - y*identity22t;
identity13t = x*identity13t - y*identity23t;
identity14t = x*identity14t - y*identity24t;
identity15t = x*identity15t - y*identity25t;

x = answer_temp22;
y = answer_temp32;

answer_temp31 = x*answer_temp31 - y*answer_temp21;
answer_temp32 = x*answer_temp32 - y*answer_temp22;
answer_temp33 = x*answer_temp33 - y*answer_temp23;
answer_temp34 = x*answer_temp34 - y*answer_temp24;
answer_temp35 = x*answer_temp35 - y*answer_temp25;
identity31t = x*identity31t - y*identity21t;
identity32t = x*identity32t - y*identity22t;
identity33t = x*identity33t - y*identity23t;
identity34t = x*identity34t - y*identity24t;
identity35t = x*identity35t - y*identity25t;

x = answer_temp22;
y = answer_temp42;

answer_temp41 = x*answer_temp41 - y*answer_temp21;
answer_temp42 = x*answer_temp42 - y*answer_temp22;
answer_temp43 = x*answer_temp43 - y*answer_temp23;
answer_temp44 = x*answer_temp44 - y*answer_temp24;
answer_temp45 = x*answer_temp45 - y*answer_temp25;
identity41t = x*identity41t - y*identity21t;
identity42t = x*identity42t - y*identity22t;
identity43t = x*identity43t - y*identity23t;
identity44t = x*identity44t - y*identity24t;
identity45t = x*identity45t - y*identity25t;

x = answer_temp22;
y = answer_temp52;

answer_temp51 = x*answer_temp51 - y*answer_temp21;
answer_temp52 = x*answer_temp52 - y*answer_temp22;
answer_temp53t = x*answer_temp53t - y*answer_temp23;
answer_temp54t = x*answer_temp54t - y*answer_temp24;
answer_temp55t = x*answer_temp55t - y*answer_temp25;
identity51t = x*identity51t - y*identity21t;
identity52t = x*identity52t - y*identity22t;
identity53t = x*identity53t - y*identity23t;
identity54t = x*identity54t - y*identity24t;
identity55t = x*identity55t - y*identity25t;


/*         */


x = answer_temp33;
y = answer_temp13;

answer_temp11 = x*answer_temp11 - y*answer_temp31;
answer_temp12 = x*answer_temp12 - y*answer_temp32;
answer_temp13 = x*answer_temp13 - y*answer_temp33;
answer_temp14 = x*answer_temp14 - y*answer_temp34;
answer_temp15 = x*answer_temp15 - y*answer_temp35;
identity11t = x*identity11t - y*identity31t;
identity12t = x*identity12t - y*identity32t;
identity13t = x*identity13t - y*identity33t;
identity14t = x*identity14t - y*identity34t;
identity15t = x*identity15t - y*identity35t;

x = answer_temp33;
y = answer_temp23;

answer_temp21 = x*answer_temp21 - y*answer_temp31;
answer_temp22 = x*answer_temp22 - y*answer_temp32;
answer_temp23 = x*answer_temp23 - y*answer_temp33;
answer_temp24 = x*answer_temp24 - y*answer_temp34;
answer_temp25 = x*answer_temp25 - y*answer_temp35;
identity21t = x*identity21t - y*identity31t;
identity22t = x*identity22t - y*identity32t;
identity23t = x*identity23t - y*identity33t;
identity24t = x*identity24t - y*identity34t;
identity25t = x*identity25t - y*identity35t;

x = answer_temp33;
y = answer_temp43;

answer_temp41 = x*answer_temp41 - y*answer_temp31;
answer_temp42 = x*answer_temp42 - y*answer_temp32;
answer_temp43 = x*answer_temp43 - y*answer_temp33;
answer_temp44 = x*answer_temp44 - y*answer_temp34;
answer_temp45 = x*answer_temp45 - y*answer_temp35;
identity41t = x*identity41t - y*identity31t;
identity42t = x*identity42t - y*identity32t;
identity43t = x*identity43t - y*identity33t;
identity44t = x*identity44t - y*identity34t;
identity45t = x*identity45t - y*identity35t;

x = answer_temp33;
y = answer_temp53t;

answer_temp51 = x*answer_temp51 - y*answer_temp31;
answer_temp52 = x*answer_temp52 - y*answer_temp32;
answer_temp53t = x*answer_temp53t - y*answer_temp33;
answer_temp54t = x*answer_temp54t - y*answer_temp34;
answer_temp55t = x*answer_temp55t - y*answer_temp35;
identity51t = x*identity51t - y*identity31t;
identity52t = x*identity52t - y*identity32t;
identity53t = x*identity53t - y*identity33t;
identity54t = x*identity54t - y*identity34t;
identity55t = x*identity55t - y*identity35t;


/*         */


x = answer_temp44;
y = answer_temp14;

answer_temp11 = x*answer_temp11 - y*answer_temp41;
answer_temp12 = x*answer_temp12 - y*answer_temp42;
answer_temp13 = x*answer_temp13 - y*answer_temp43;
answer_temp14 = x*answer_temp14 - y*answer_temp44;
answer_temp15 = x*answer_temp15 - y*answer_temp45;
identity11t = x*identity11t - y*identity41t;
identity12t = x*identity12t - y*identity42t;
identity13t = x*identity13t - y*identity43t;
identity14t = x*identity14t - y*identity44t;
identity15t = x*identity15t - y*identity45t;

x = answer_temp44;
y = answer_temp24;

answer_temp21 = x*answer_temp21 - y*answer_temp41;
answer_temp22 = x*answer_temp22 - y*answer_temp42;
answer_temp23 = x*answer_temp23 - y*answer_temp43;
answer_temp24 = x*answer_temp24 - y*answer_temp44;
answer_temp25 = x*answer_temp25 - y*answer_temp45;
identity21t = x*identity21t - y*identity41t;
identity22t = x*identity22t - y*identity42t;
identity23t = x*identity23t - y*identity43t;
identity24t = x*identity24t - y*identity44t;
identity25t = x*identity25t - y*identity45t;

x = answer_temp44;
y = answer_temp34;

answer_temp31 = x*answer_temp31 - y*answer_temp41;
answer_temp32 = x*answer_temp32 - y*answer_temp42;
answer_temp33 = x*answer_temp33 - y*answer_temp43;
answer_temp34 = x*answer_temp34 - y*answer_temp44;
answer_temp35 = x*answer_temp35 - y*answer_temp45;
identity31t = x*identity31t - y*identity41t;
identity32t = x*identity32t - y*identity42t;
identity33t = x*identity33t - y*identity43t;
identity34t = x*identity34t - y*identity44t;
identity35t = x*identity35t - y*identity45t;

x = answer_temp44;
y = answer_temp54t;

answer_temp51 = x*answer_temp51 - y*answer_temp41;
answer_temp52 = x*answer_temp52 - y*answer_temp42;
answer_temp53t = x*answer_temp53t - y*answer_temp43;
answer_temp54t = x*answer_temp54t - y*answer_temp44;
answer_temp55t = x*answer_temp55t - y*answer_temp45;
identity51t = x*identity51t - y*identity41t;
identity52t = x*identity52t - y*identity42t;
identity53t = x*identity53t - y*identity43t;
identity54t = x*identity54t - y*identity44t;
identity55t = x*identity55t - y*identity45t;


/*         */


x = answer_temp55t;
y = answer_temp15;

answer_temp11 = x*answer_temp11 - y*answer_temp51;
answer_temp12 = x*answer_temp12 - y*answer_temp52;
answer_temp13 = x*answer_temp13 - y*answer_temp53t;
answer_temp14 = x*answer_temp14 - y*answer_temp54t;
answer_temp15 = x*answer_temp15 - y*answer_temp55t;
identity11t = x*identity11t - y*identity51t;
identity12t = x*identity12t - y*identity52t;
identity13t = x*identity13t - y*identity53t;
identity14t = x*identity14t - y*identity54t;
identity15t = x*identity15t - y*identity55t;

x = answer_temp55t;
y = answer_temp25;

answer_temp21 = x*answer_temp21 - y*answer_temp51;
answer_temp22 = x*answer_temp22 - y*answer_temp52;
answer_temp23 = x*answer_temp23 - y*answer_temp53t;
answer_temp24 = x*answer_temp24 - y*answer_temp54t;
answer_temp25 = x*answer_temp25 - y*answer_temp55t;
identity21t = x*identity21t - y*identity51t;
identity22t = x*identity22t - y*identity52t;
identity23t = x*identity23t - y*identity53t;
identity24t = x*identity24t - y*identity54t;
identity25t = x*identity25t - y*identity55t;

x = answer_temp55t;
y = answer_temp35;

answer_temp31 = x*answer_temp31 - y*answer_temp51;
answer_temp32 = x*answer_temp32 - y*answer_temp52;
answer_temp33 = x*answer_temp33 - y*answer_temp53t;
answer_temp34 = x*answer_temp34 - y*answer_temp54t;
answer_temp35 = x*answer_temp35 - y*answer_temp55t;
identity31t = x*identity31t - y*identity51t;
identity32t = x*identity32t - y*identity52t;
identity33t = x*identity33t - y*identity53t;
identity34t = x*identity34t - y*identity54t;
identity35t = x*identity35t - y*identity55t;

x = answer_temp55t;
y = answer_temp45;

answer_temp41 = x*answer_temp41 - y*answer_temp51;
answer_temp42 = x*answer_temp42 - y*answer_temp52;
answer_temp43 = x*answer_temp43 - y*answer_temp53t;
answer_temp44 = x*answer_temp44 - y*answer_temp54t;
answer_temp45 = x*answer_temp45 - y*answer_temp55t;
identity41t = x*identity41t - y*identity51t;
identity42t = x*identity42t - y*identity52t;
identity43t = x*identity43t - y*identity53t;
identity44t = x*identity44t - y*identity54t;
identity45t = x*identity45t - y*identity55t;



/*         */
/*
if(answer_temp11>0)
begin
	identity11t = identity11t/answer_temp11;
	identity12t = identity12t/answer_temp11;
	identity13t = identity13t/answer_temp11;
	identity14t = identity14t/answer_temp11;
	identity15t = identity15t/answer_temp11;
end

if(answer_temp22>0)
begin
	identity21t = identity11t/answer_temp22;
	identity22t = identity22t/answer_temp22;
	identity23t = identity23t/answer_temp22;
	identity24t = identity24t/answer_temp22;
	identity25t = identity25t/answer_temp22;
end

if(answer_temp33>0)
begin
	identity31t = identity31t/answer_temp33;
	identity32t = identity32t/answer_temp33;
	identity33t = identity33t/answer_temp33;
	identity34t = identity34t/answer_temp33;
	identity35t = identity35t/answer_temp33;
end

if(answer_temp44>0)
begin
	identity41t = identity41t/answer_temp44;
	identity42t = identity42t/answer_temp44;
	identity43t = identity43t/answer_temp44;
	identity44t = identity44t/answer_temp44;
	identity45t = identity45t/answer_temp44;
end

if(answer_temp55t>0)
begin
	identity51t = identity51t/answer_temp55t;
	identity52t = identity52t/answer_temp55t;
	identity53t = identity53t/answer_temp55t;
	identity54t = identity54t/answer_temp55t;
	identity55t = identity55t/answer_temp55t;
end*/
	



/*				*/
if(answer_temp11==32'd0 || answer_temp22 == 32'd0 || answer_temp33 == 32'd0 || answer_temp44==32'd0 || answer_temp55t==32'd0)
begin
	identity11=32'dx;
	identity22=32'dx;
	identity33=32'dx;
	identity44=32'dx;
	identity55=32'dx;
end

else
begin
	identity11 = identity11t;
	identity12 = identity12t;
	identity13 = identity13t;
	identity14 = identity14t;
	identity15 = identity15t;

	identity21 = identity21t;
	identity22 = identity22t;
	identity23 = identity23t;
	identity24 = identity24t;
	identity25 = identity25t;

	identity31 = identity31t;
	identity32 = identity32t;
	identity33 = identity33t;
	identity34 = identity34t;
	identity35 = identity35t;

	identity41 = identity41t;
	identity42 = identity42t;
	identity43 = identity43t;
	identity44 = identity44t;
	identity45 = identity45t;

	identity51 = identity51t;
	identity52 = identity52t;
	identity53 = identity53t;
	identity54 = identity54t;
	identity55 = identity55t;
end


inverse[0] = identity11;
inverse[1] = identity12;
inverse[2] = identity13;
inverse[3] = identity14;
inverse[4] = identity15;

inverse[5] = identity21;
inverse[6] = identity22;
inverse[7] = identity23;
inverse[8] = identity24;
inverse[9] = identity25;

inverse[10] = identity31;
inverse[11] = identity32;
inverse[12] = identity33;
inverse[13] = identity34;
inverse[14] = identity35;

inverse[15] = identity41;
inverse[16] = identity42;
inverse[17] = identity43;
inverse[18] = identity44;
inverse[19] = identity45;

inverse[20] = identity51;
inverse[21] = identity52;
inverse[22] = identity53;
inverse[23] = identity54;
inverse[24] = identity55;

end

end



wire [31:0] identity11dt,identity12dt,identity13dt,identity14dt,identity15dt;
wire [31:0] identity21dt,identity22dt,identity23dt,identity24dt,identity25dt;
wire [31:0] identity31dt,identity32dt,identity33dt,identity34dt,identity35dt;
wire [31:0] identity41dt,identity42dt,identity43dt,identity44dt,identity45dt;
wire [31:0] identity51dt,identity52dt,identity53dt,identity54dt,identity55dt;
/*
wire  ridentity11dt,ridentity12dt,ridentity13dt,ridentity14dt,ridentity15dt;
wire  ridentity21dt,ridentity22dt,ridentity23dt,ridentity24dt,ridentity25dt;
wire  ridentity31dt,ridentity32dt,ridentity33dt,ridentity34dt,ridentity35dt;
wire  ridentity41dt,ridentity42dt,ridentity43dt,ridentity44dt,ridentity45dt;
wire  ridentity51dt,ridentity52dt,ridentity53dt,ridentity54dt,ridentity55dt;

wire [31:0] fidentity11dt,fidentity12dt,fidentity13dt,fidentity14dt,fidentity15dt;
wire [31:0] fidentity21dt,fidentity22dt,fidentity23dt,fidentity24dt,fidentity25dt;
wire [31:0] fidentity31dt,fidentity32dt,fidentity33dt,fidentity34dt,fidentity35dt;
wire [31:0] fidentity41dt,fidentity42dt,fidentity43dt,fidentity44dt,fidentity45dt;
wire [31:0] fidentity51dt,fidentity52dt,fidentity53dt,fidentity54dt,fidentity55dt;*/

assign identity11d = identity11;
assign identity12d = identity12;
assign identity13d = identity13;
assign identity14d = identity14;
assign identity15d = identity15;

assign identity21d = identity21;
assign identity22d = identity22;
assign identity23d = identity23;
assign identity24d = identity24;
assign identity25d = identity25;

assign identity31d = identity31;
assign identity32d = identity32;
assign identity33d = identity33;
assign identity34d = identity34;
assign identity35d = identity35;

assign identity41d = identity41;
assign identity42d = identity42;
assign identity43d = identity43;
assign identity44d = identity44;
assign identity45d = identity45;

assign identity51d = identity51;
assign identity52d = identity52;
assign identity53d = identity53;
assign identity54d = identity54;
assign identity55d = identity55;

wire [31:0] a,b,c,d,e;

assign a = answer_temp11;
assign b = answer_temp22;
assign c = answer_temp33;
assign d = answer_temp44;
assign e = answer_temp55t;


assign identity11d = identity11dt;
assign identity12d = identity12dt;
assign identity13d = identity13dt;
assign identity14d = identity14dt;
assign identity15d = identity15dt;

assign identity21d = identity21dt;
assign identity22d = identity22dt;
assign identity23d = identity23dt;
assign identity24d = identity24dt;
assign identity25d = identity25dt;

assign identity31d = identity31dt;
assign identity32d = identity32dt;
assign identity33d = identity33dt;
assign identity34d = identity34dt;
assign identity35d = identity35dt;

assign identity41d = identity41dt;
assign identity42d = identity42dt;
assign identity43d = identity43dt;
assign identity44d = identity44dt;
assign identity45d = identity45dt;

assign identity51d = identity51dt;
assign identity52d = identity52dt;
assign identity53d = identity53dt;
assign identity54d = identity54dt;
assign identity55d = identity55dt;



endmodule


/*
div identity11div 
(
	.clk(clk), // input clk
	.rfd(ridentity11dt), // output rfd
	.dividend(identity11), // input [31 : 0] dividend
	.divisor(a), // input [31 : 0] divisor
	.quotient(identity11dt), // output [31 : 0] quotient
	.fractional(fidentity11dt) // output [31 : 0] fractional
);

div identity12div 
(
	.clk(clk), // input clk
	.rfd(ridentity12dt), // output rfd
	.dividend(identity12), // input [31 : 0] dividend
	.divisor(a), // input [31 : 0] divisor
	.quotient(identity12dt), // output [31 : 0] quotient
	.fractional(fidentity12dt) // output [31 : 0] fractional
);

div identity13div 
(
	.clk(clk), // input clk
	.rfd(ridentity13dt), // output rfd
	.dividend(identity13), // input [31 : 0] dividend
	.divisor(a), // input [31 : 0] divisor
	.quotient(identity13dt), // output [31 : 0] quotient
	.fractional(fidentity13dt) // output [31 : 0] fractional
);

div identity14div 
(
	.clk(clk), // input clk
	.rfd(ridentity14dt), // output rfd
	.dividend(identity14), // input [31 : 0] dividend
	.divisor(a), // input [31 : 0] divisor
	.quotient(identity14dt), // output [31 : 0] quotient
	.fractional(fidentity14dt) // output [31 : 0] fractional
);

div identity15div 
(
	.clk(clk), // input clk
	.rfd(ridentity15dt), // output rfd
	.dividend(identity15), // input [31 : 0] dividend
	.divisor(a), // input [31 : 0] divisor
	.quotient(identity15dt), // output [31 : 0] quotient
	.fractional(fidentity15dt) // output [31 : 0] fractional
);


div identity21div 
(
	.clk(clk), // input clk
	.rfd(ridentity21dt), // output rfd
	.dividend(identity21), // input [31 : 0] dividend
	.divisor(b), // input [31 : 0] divisor
	.quotient(identity21dt), // output [31 : 0] quotient
	.fractional(fidentity21dt) // output [31 : 0] fractional
);

div identity22div 
(
	.clk(clk), // input clk
	.rfd(ridentity22dt), // output rfd
	.dividend(identity22), // input [31 : 0] dividend
	.divisor(b), // input [31 : 0] divisor
	.quotient(identity22dt), // output [31 : 0] quotient
	.fractional(fidentity22dt) // output [31 : 0] fractional
);

div identity23div 
(
	.clk(clk), // input clk
	.rfd(ridentity23dt), // output rfd
	.dividend(identity23), // input [31 : 0] dividend
	.divisor(b), // input [31 : 0] divisor
	.quotient(identity23dt), // output [31 : 0] quotient
	.fractional(fidentity23dt) // output [31 : 0] fractional
);

div identity24div 
(
	.clk(clk), // input clk
	.rfd(ridentity24dt), // output rfd
	.dividend(identity24), // input [31 : 0] dividend
	.divisor(b), // input [31 : 0] divisor
	.quotient(identity24dt), // output [31 : 0] quotient
	.fractional(fidentity24dt) // output [31 : 0] fractional
);

div identity25div 
(
	.clk(clk), // input clk
	.rfd(ridentity25dt), // output rfd
	.dividend(identity25), // input [31 : 0] dividend
	.divisor(b), // input [31 : 0] divisor
	.quotient(identity25dt), // output [31 : 0] quotient
	.fractional(fidentity25dt) // output [31 : 0] fractional
);


div identity31div 
(
	.clk(clk), // input clk
	.rfd(ridentity31dt), // output rfd
	.dividend(identity31), // input [31 : 0] dividend
	.divisor(c), // input [31 : 0] divisor
	.quotient(identity31dt), // output [31 : 0] quotient
	.fractional(fidentity31dt) // output [31 : 0] fractional
);

div identity32div 
(
	.clk(clk), // input clk
	.rfd(ridentity32dt), // output rfd
	.dividend(identity32), // input [31 : 0] dividend
	.divisor(c), // input [31 : 0] divisor
	.quotient(identity32dt), // output [31 : 0] quotient
	.fractional(fidentity32dt) // output [31 : 0] fractional
);

div identity33div 
(
	.clk(clk), // input clk
	.rfd(ridentity33dt), // output rfd
	.dividend(identity33), // input [31 : 0] dividend
	.divisor(c), // input [31 : 0] divisor
	.quotient(identity33dt), // output [31 : 0] quotient
	.fractional(fidentity33dt) // output [31 : 0] fractional
);

div identity34div 
(
	.clk(clk), // input clk
	.rfd(ridentity34dt), // output rfd
	.dividend(identity34), // input [31 : 0] dividend
	.divisor(c), // input [31 : 0] divisor
	.quotient(identity34dt), // output [31 : 0] quotient
	.fractional(fidentity34dt) // output [31 : 0] fractional
);

div identity35div 
(
	.clk(clk), // input clk
	.rfd(ridentity35dt), // output rfd
	.dividend(identity35), // input [31 : 0] dividend
	.divisor(c), // input [31 : 0] divisor
	.quotient(identity35dt), // output [31 : 0] quotient
	.fractional(fidentity35dt) // output [31 : 0] fractional
);


div identity41div 
(
	.clk(clk), // input clk
	.rfd(ridentity41dt), // output rfd
	.dividend(identity41), // input [31 : 0] dividend
	.divisor(d), // input [31 : 0] divisor
	.quotient(identity41dt), // output [31 : 0] quotient
	.fractional(fidentity41dt) // output [31 : 0] fractional
);

div identity42div 
(
	.clk(clk), // input clk
	.rfd(ridentity42dt), // output rfd
	.dividend(identity42), // input [31 : 0] dividend
	.divisor(d), // input [31 : 0] divisor
	.quotient(identity42dt), // output [31 : 0] quotient
	.fractional(fidentity42dt) // output [31 : 0] fractional
);

div identity43div 
(
	.clk(clk), // input clk
	.rfd(ridentity43dt), // output rfd
	.dividend(identity43), // input [31 : 0] dividend
	.divisor(d), // input [31 : 0] divisor
	.quotient(identity43dt), // output [31 : 0] quotient
	.fractional(fidentity43dt) // output [31 : 0] fractional
);

div identity44div 
(
	.clk(clk), // input clk
	.rfd(ridentity44dt), // output rfd
	.dividend(identity44), // input [31 : 0] dividend
	.divisor(d), // input [31 : 0] divisor
	.quotient(identity44dt), // output [31 : 0] quotient
	.fractional(fidentity44dt) // output [31 : 0] fractional
);

div identity45div 
(
	.clk(clk), // input clk
	.rfd(ridentity45dt), // output rfd
	.dividend(identity45), // input [31 : 0] dividend
	.divisor(d), // input [31 : 0] divisor
	.quotient(identity45dt), // output [31 : 0] quotient
	.fractional(fidentity45dt) // output [31 : 0] fractional
);


div identity51div 
(
	.clk(clk), // input clk
	.rfd(ridentity51dt), // output rfd
	.dividend(identity51), // input [31 : 0] dividend
	.divisor(e), // input [31 : 0] divisor
	.quotient(identity51dt), // output [31 : 0] quotient
	.fractional(fidentity51dt) // output [31 : 0] fractional
);

div identity52div 
(
	.clk(clk), // input clk
	.rfd(ridentity52dt), // output rfd
	.dividend(identity52), // input [31 : 0] dividend
	.divisor(e), // input [31 : 0] divisor
	.quotient(identity52dt), // output [31 : 0] quotient
	.fractional(fidentity52dt) // output [31 : 0] fractional
);

div identity53div 
(
	.clk(clk), // input clk
	.rfd(ridentity53dt), // output rfd
	.dividend(identity53), // input [31 : 0] dividend
	.divisor(e), // input [31 : 0] divisor
	.quotient(identity53dt), // output [31 : 0] quotient
	.fractional(fidentity53dt) // output [31 : 0] fractional
);

div identity54div 
(
	.clk(clk), // input clk
	.rfd(ridentity54dt), // output rfd
	.dividend(identity54), // input [31 : 0] dividend
	.divisor(e), // input [31 : 0] divisor
	.quotient(identity54dt), // output [31 : 0] quotient
	.fractional(fidentity54dt) // output [31 : 0] fractional
);

div identity55div 
(
	.clk(clk), // input clk
	.rfd(ridentity55dt), // output rfd
	.dividend(identity55), // input [31 : 0] dividend
	.divisor(e), // input [31 : 0] divisor
	.quotient(identity55dt), // output [31 : 0] quotient
	.fractional(fidentity55dt) // output [31 : 0] fractional
);*/

