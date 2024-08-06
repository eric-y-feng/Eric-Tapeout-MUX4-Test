<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->
This project is a Wokwi implementation of a basic 4-input Multiplexer (MUX4).

![Screenshot 2024-08-02 024817](https://github.com/user-attachments/assets/fd687b6b-547d-4460-ba87-8791ec2b6780)

The design is based off of the above gate-level circuit diagram, which was taken from [this Verilog tutorial](https://drive.google.com/file/d/1u0lsldNjEUQ04maekM16Yn_WOX5NVKvn/view). The corresponding Wokwi project can be found [here](https://wokwi.com/projects/405140925994796033). 

## How it works

The MUX4 as shown in the example diagram uses two selection lines, 'sel[0]' and 'sel[1]', to choose one of four inputs ('a', 'b', 'c', 'd') and output it to 'out'. The original circuit diagram suggests the following mapping:

| sel[1]  | sel[0] |  Output|
| ------------- | ------------- | ------------- | 
| 0  | 0  | 'c'  |
| 0  | 1  | 'a'  |
| 1  | 0  | 'd'  |
| 1  | 1  | 'b'  |

The Wokwi project is a gate-level implementation of this design, which can be compiled to a bitstream and downloaded to a TinyTapeout FPGA demoboard. It uses six input switches, with the following mappings: 

| Switch  | Input Mapping |
| ------------- | ------------- |
| 1  | sel[1]  |
| 2  | sel[0]  |
| 3  | 'a'  |
| 4  | 'c'  |
| 5  | 'b'  |
| 6  | 'd'  |

Given a set of inputs determined by the switch configurations, the FPGA board will display either '1' or '0' on its seven segment display, based on the corresponding output.

## How to test
Instructions partially based off of [this TinyTapeout setup guide](https://docs.google.com/document/d/1Wb9GcpYAzLTsxxpKRAacE_PDGy1jexddqpRyOgfhkH0/edit#heading=h.9c9dnt6q3ojq).

1. Click on the "Actions" tab at the top of the page.
2. From the left sidebar, click "fpga".
3. Click on the latest commit, to the right of the green checkmark.
4. Scroll down to "Artifacts", and download the .zip file titled "fpga_bitstream".
5. Unzip the .zip file.
6. Connect the TinyTapeout FPGA demoboard to your computer via USB.
7. Navigate to [this website](https://devanlai.github.io/webdfu/dfu-util/). On the demoboard, hold the red "BOOT" button. While holding, press "RESET" once, and then let go of "BOOT". a flashing purple light should appear on the board.
8. Click "Connect" on the website and select the TinyTapeout demoboard. When prompted, select the "ICE40 bistream" option and then confirm by selecting "Select Interface".
9. Under "Firmware Download", click the "Choose File" select the tt_fpga.bin file, which can be found within the "build" folder of the unzipped bitstream file. Then, click ‘Download’. Wait for the progress bar to complete.

Testing:
- Switches 1 and 2 on the board map to sel[1] and sel[0], respectively. Switches 3, 4, 5, and 6 on the board map to inputs 'a', 'c', 'b', and 'd', respectively.
- The seven-segment display should initialize to "0", as none of the switches are on.
- Turn on switch 4. This correlates to the selection "sel[1] = 0, sel[0] = 0", which outputs 'c'. The display should change to "1".
- Turn off switch 4, which should change the display to "0". Then, turn on switches 2 and 3. This correlates to the selection "sel[1] = 0, sel[0] = 1", which outputs 'a'. The display should change to "1".
- Turn off switch 3, which should change the display to "0". Then, turn on switches 1 and 5. This correlates to the selection "sel[1] = 1, sel[0] = 1", which outputs 'b'. The display should change to "1".
- Turn off switches 2 and 5, which should change the display to "0". Then, turn on switch 6. This correlates to the selection "sel[1] = 1, sel[0] = 0", which outputs 'd'. The display should change to "1".

## External hardware

- TinyTapeout FPGA demoboard
- USB-A to USB-C cable

## Todo/Current issues

- The current switch-input mappings map switches 3, 4, 5, and 6 to 'a', 'c', 'b', and 'd' respectively. I am working on changing the design to be more intuitive, such that switches 3, 4, 5, and 6, map to 'a', 'b', 'c', and 'd', respectively.
- Upon downloading the tt_fpga.bin file to the TinyTapeout demoboard, the following error message is shown:
  ![image](https://github.com/user-attachments/assets/167eac7d-5835-446b-80ad-8cb9dbf6cd20)
- The TinyTapeout board will not update until I manually press "RESET" upon downloading the tt_fpga.bin file. Additionally, after unplugging/plugging back in the board, I have to re-download the bitstream file onto the board before it will work again.

