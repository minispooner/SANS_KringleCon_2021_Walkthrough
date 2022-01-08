## Summary
Go to Jacks rooftop and learn how to program FPGA (wavelengths). \
**Objective**: "Write your first FPGA program to make a doll sing. You might get some suggestions from Grody Goiterson, near Jack's elevator."
The Elf at the bottom of Jack's elevator will give you links to hints:
- https://www.youtube.com/watch?v=GFdG1PJ4QjA
- https://www.fpga4fun.com/MusicBox.html

[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Objective
Hello, students! In exercise #4, we continue our FPGA journey, documenting the creation of the sound chip for this holiday season's new Kurse 'em Out Karen doll. Our goal is to make the doll say its trademark phrase. But, as I always tell you in class, we must walk before we run.

Before the doll can say anything, we must first have it make noise. _**In this exercise you will design an FPGA module that creates a square wave tone at a variable frequency.**_

Creating a square wave output takes our clock signal (which is also a square wave) and uses a counter to divide the clock to match the desired frequency. One tricky problem that we'll encounter is that Verilog (v1364-2005) doesn't have a built-in mechanism to round real numbers to integers, so you'll need to devise a means to do that correctly if you want your module to match frequencies accurately.

Good luck and always remember:

If $rtoi(MHz * 10) - ($rtoi(freq) * 10) > 4, add 1

- Prof. Qwerty Petabyte

## Steps
After completing, turn on your audio and click on the physical device on the rooftop. Gather the item, place it into the motherboard and enter the new area on top the roof!

**Notes**\
Reverse the wave from the video\
125MHz ... 6,250 MHz /2 31:0 *10 ... (32'd625000000 / freq)*10

**Answer**
```
// Note: For this lab, we will be working with QRP Corporation's CQC-11 FPGA.
// The CQC-11 operates with a 125MHz clock.
// Your design for a tone generator must support the following 
// inputs/outputs:
// (NOTE: DO NOT CHANGE THE NAMES. OUR AUTOMATED GRADING TOOL
// REQUIRES THE USE OF THESE NAMES!)
// input clk - this will be connected to the 125MHz system clock
// input rst - this will be connected to the system board's reset bus
// input freq - a 32 bit integer indicating the required frequency
//              (0 - 9999.99Hz) formatted as follows:
//              32'hf1206 or 32'd987654 = 9876.54Hz
// output wave_out - a square wave output of the desired frequency
// you can create whatever other variables you need, but remember
// to initialize them to something!

`timescale 1ns/1ns
module tone_generator (
    input clk,
    input rst,
    input [31:0] freq,
    output wave_out
);
    // ---- DO NOT CHANGE THE CODE ABOVE THIS LINE ---- 
    // ---- IT IS NECESSARY FOR AUTOMATED ANALYSIS ----
    // TODO: Add your code below. 
    // Remove the following line and add your own implementation. 
    // Note: It's silly, but it compiles...
  real counter = (32'd625000000 / freq);
  reg sq_wave_reg;
  assign wave_out = sq_wave_reg;
  
  always @(posedge clk or posedge rst)
  begin
    if(rst==1)
      begin
        counter <= 8'h00;
        sq_wave_reg <= 1'b0;
      end
    else
      begin
        if(counter == 8'h00)
            begin
                sq_wave_reg <= ~sq_wave_reg;
                counter <= ((32'd625000000 / freq)*10) - 1;
            end
        else
            counter <= counter - 1;
      end
  end
endmodule
```
