# SVF fusemap

Unfortunately, the VHDL project that reproduces the behaviour of the original 
gate-array is not publicly available. Only the resulting SVF file is given.

As a reference, the quartus file is also provided.

## Programming the ATF1508

### Hardware setup

I use an FT323H usb dongle and a JTAG link to perform the programming.

For easier handling, there is a project to hold the FT232H module, the gate-array replacement. And it has
a witness LED that blinks when the programming is succesful, driven by the E signal (1MHz clock generated
by the gate array).

See https://github.com/sporniket/thomson-mo5--gate-array-development-board

## Using openOCD 

```sh
openocd -f /usr/share/openocd/scripts/interface/ftdi/um232h.cfg \
  -c "adapter speed 400" \
  -c "transport select jtag" \
  -c "jtag newtap ATF1508AS tap -irlen 3 -expected-id 0x0150803f" \
  -c init \
  -c "svf gate-array--motorola.svf quiet progress" \
  -c "sleep 200" \
  -c shutdown
```

