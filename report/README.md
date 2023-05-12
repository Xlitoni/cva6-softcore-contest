# Problèmes rencontrés avec l'installation de l'environnement

## ETAPE make cva6_fpga

### makefile 

dans le makefile, changer 
``` digilentinc.com:zybo-z7-20:part0:1.0 ```  
par 
``` digilentinc.com:zybo-z7-20:part0:1.1 ```

### cv6-softcore-contest/fpga/scripts/run_cva6_fpga.tcl

ligne 46 à 51 changer tous les ```read_ip``` en ```import_ip```

ligne 79 il faut rajouter les commandes suivantes 

```
synth_ip [get_files /home/risc/Desktop/cva6-softcore-contest/fpga/cva6_fpga.srcs/sources_1/ip/xlnx_blk_mem_gen/xlnx_blk_mem_gen.xci]
synth_ip [get_files /home/risc/Desktop/cva6-softcore-contest/fpga/xilinx/xlnx_clk_gen/ip/xlnx_clk_gen.xci]
synth_ip [get_files /home/risc/Desktop/cva6-softcore-contest/fpga/cva6_fpga.srcs/sources_1/ip/xlnx_axi_dwidth_converter_dm_master/xlnx_axi_dwidth_converter_dm_master.xci]
synth_ip [get_files /home/risc/Desktop/cva6-softcore-contest/fpga/cva6_fpga.srcs/sources_1/ip/xlnx_axi_dwidth_converter_dm_slave/xlnx_axi_dwidth_converter_dm_slave.xci]
```

Juste avant ce bloc de code

```
# synth_design -verilog_define PS7_DDR=$::env(PS7_DDR) -verilog_define BRAM=$::env(BRAM) -rtl -name rtl_1
 if { $::env(PS7_DDR) == 1 } {
   synth_design -verilog_define PS7_DDR=PS7_DDR -rtl -name rtl_1
} elseif {$::env(BRAM) == 1} {
   synth_design -verilog_define BRAM=BRAM -rtl -name rtl_1
} else {
   puts "None of the values is matching"
}
```


##  ETAPE make program_cva6_fpga

### Problème : make program_cva6_fpga : no targets available

il faut installer les drivers cable manuellement :

```sudo /media/risc/home/rsx05/tools/Xilinx/Vivado/2022.1/data/xicom/cable_drivers/lin64/install_script/install_drivers/install_drivers```

## ETAPE HYPERTERMINAL

```sudo picocom -b 115200 /dev/ttyUSB0```

## ETAPE BUILD DOCKER

rajouter dans le dockerfile les variables env suivantes (propre à la salle réseau peut-être)
```
ENV http_proxy="http://proxy.isae.fr:3128/"
ENV https_proxy="http://proxy.isae.fr:3128/"
```

Lorsqu'on le run

```docker run -ti --privileged -v /dev/bus/usb/001/***:/dev/bus/usb/001/*** -v `realpath workspace`:/workdir zephyr-build:v1```

remplacer les etoiles par le bon (voir dans /dev/bus/usb/001/)


