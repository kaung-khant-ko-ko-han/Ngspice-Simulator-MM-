# ၁၀။ Digital Device Models

## ၁၀.၁ U devices (အခြေခံ digital building blocks)

PS compatibility mode ကို သတ်မှတ်ထားပါက၊ ngspice သည် digital gates, flip-flops, latches, LOGICEXP နှင့် PINDLY behavioral primitives (စာရင်းအတွက် အခန်း ၁၀.၁.၂ ကိုကြည့်ပါ)၊ နှင့် timing models များ ပါဝင်သော U* instances များဖြင့် ဖွဲ့စည်းထားသည့် `.subckt` statements များကို ထောက်ပံ့ပေးသည်။ ထုံးစံအားဖြင့် rise/fall delays များကို timing models နှင့် PINDLY statements များမှ ခန့်မှန်းတွက်ချက်သည်။ CONSTRAINT primitives နှင့် io models များကို လျစ်လျူရှုသည်။ `.subckt` အတွင်းရှိ အခြား U* instances များ (RAM, ROM, STIM နှင့် PLAs ကဲ့သို့သော) ကို ထောက်ပံ့မည်မဟုတ်ပါ၊ ထိုသို့သော `.subckt` များကို XSPICE digital primitives များအဖြစ်သို့ ပြောင်းလဲမည်မဟုတ်ပါ။

ဤ U devices များသည် 74xx သို့မဟုတ် 40xx series များကဲ့သို့သော digital circuits များကို ချက်ချင်းဖော်ပြရန် ရည်ရွယ်ခြင်းမဟုတ်ပါ။ သို့သော် ၎င်းတို့ကို ထိုကဲ့သို့သော circuits များအတွက် models များထုတ်လုပ်ရန် subcircuits များတွင် အသုံးပြုသည် (အခန်း ၁၀.၂ ကိုကြည့်ပါ)။ ၎င်းတို့၏ syntax သည် Micro-Cap နှင့် PSPICE simulators များနှင့် အတော်အသင့် တွဲဖက်အသုံးပြုနိုင်သည်။

### ၁၀.၁.၁ အထွေထွေ ဖော်မတ်

**အထွေထွေပုံစံ:**
```
U<name> <basic type> [(<parameter value>*)]
+ <digital power node> <digital ground node> <node>*
+ <timing model name> <I/O model name>
+ [MNTYMXDLY=<delay select value>]
+ [IO_LEVEL=<interface subcircuit select value>]
```
**ဥပမာ:**
```spice
U2 AND(2) $G_DPWR $G_DGND 4 5 6
+ M2 IOM2 IO_LEVEL=0 MNTYMXDLY=2
.MODEL M2 UGATE ()
.MODEL IOM2 UIO (INLD=0 OUTLD=0 DRVH=50 DRVL=50
+ ATOD1="ATOD1" DTOA1="DTOD1" ATOD2="ATOD2" DTOA2="DTOD2"
+ ATOD3="ATOD3" DTOA3="DTOD3" ATOD4="ATOD4" DTOA4="DTOD4"
+ TSWLH1=0 TSWLH2=0 TSWLH3=0 TSWLH4=0
+ TSWHL1=0 TSWHL2=0 TSWHL3=0 TSWHL4=0 DIGPOWER="DIGPOWER")
```

### ၁၀.၁.၂ Ngspice တွင် ရရှိနိုင်သော စက်ပစ္စည်းများ စာရင်း (အခြေခံ အမျိုးအစားများ)

**Standard gates:**
BUF (buffer), INV (inverter), AND, NAND, OR, NOR, XOR, NXOR (exclusive NOR), BUFA (buffer array), INVA (inverter array), ANDA, NANDA, ORA, NORA, XORA, NXORA, AO (AND-OR compound gate), OA (OR-AND compound gate), AOI (AND-NOR compound gate), OAI (OR-NAND compound gate).

**Tristate gates:**
BUF3, INV3, AND3, NAND3, OR3, NOR3, XOR3, NXOR3, BUF3A (buffer array), INV3A, AND3A, NAND3A, OR3A, NOR3A, XORA3, NXOR3A.

**Flip-flops and latches:**
DFF (D-type flip-flop, positive-edge triggered), JKFF (J-K flip-flop, negative-edge triggered), DLTCH (D-type latch), SRFF (S-R flip-flop).

**Delay lines:**
DLYLINE (Delay line).

**Behavioral primitives:**
LOGICEXP (Combinational logic expressions), PINDLY (Output buffers and tristate buffers with estimated delays).

### ၁၀.၁.၃ URC transmission line နှင့် U devices တို့၏ ကွာခြားချက်

ပထမဆုံးအနေဖြင့် ngspice တွင် reference designator U ကို မတူညီသော devices နှစ်ခုဖြစ်သည့် Uniformly distributed RC line နှင့် digital basic types or primitives များ (U devices) အတွက် အသုံးပြုရာတွင် အမည်ကွဲလွဲမှု ရှိနိုင်သည်။

- **U-devices:** compatibility mode flag (12.14.1) ကို PS ဟု သတ်မှတ်ရန် လိုအပ်သည်။ ထို့အပြင် U devices များကို subcircuit တစ်ခုအတွင်း၌သာ အသိအမှတ်ပြုသည်။ နောက်ဆုံးတွင် basic type (U instance line ၏ ဒုတိယ token) သည် အထက်ဖော်ပြပါဇယားမှ basic types များနှင့် ကိုက်ညီရမည်။
- **URC:** အခြားကိစ္စရပ်များတွင် ngspice က URC (uniformly distributed) transmission line ဟု ယူဆမည်။

## ၁၀.၂ Standard digital device များအတွက် ပံ့ပိုးမှု

Digital primitives (U devices) များသည် ngspice တွင်အသုံးပြုသော digital ICs များအတွက် models များ၏ အခြေခံ building blocks များဖြစ်သည်။ And Gate တစ်ခုအတွက် ရိုးရှင်းသော subcircuit model ၏ ဥပမာကို အောက်တွင် ဖော်ပြထားသည်-

**ဥပမာ: 74LV08A Quad 2-Input And Gate**
```spice
* ------------------------- 74LV08A ------
* Quad 2-Input And Gate
* TI PDF File bss 2/21/03
.SUBCKT 74LV08A 1A 1B 1Y
+ optional: DPWR_3V=$G_DPWR_3V DGND_3V=$G_DGND_3V
+ params: MNTYMXDLY=0 IO_LEVEL=0
U1 and(2) DPWR_3V DGND_3V 1A 1B 1Y
+ DLY_LV08 IO_LV-A MNTYMXDLY={MNTYMXDLY} IO_LEVEL={IO_LEVEL}
.model DLY_LV08 ugate (tplhTY=7.5ns tplhMX=12.3ns tphlTY=7.5ns tphlMX=12.3ns)
.ENDS 74LV08A
```

ngspice/examples/digital/digital_devices ရှိ circuit example ex4.cir သည် stimulus file ex4.stim နှင့်အတူ 74xx series ICs များဖြင့် အပြည့်အဝ digital, event based full-adder simulation ကို တင်ဆက်ထားသည်။ ngspice ၏ အတွင်းပိုင်း plotting capability ကို အသုံးပြုထားသည်။ ex5.cir သည် stimulus file ex5.stim ဖြင့် D-Type Flip-Flop တစ်ခု၏ ပြောင်းလဲခြင်းကို သရုပ်ပြသည်။ ex283.cir သည် stimulus နှင့် GTKWave ကိုအသုံးပြု၍ plotting ပြုလုပ်ထားသော 74283 4-bit full adder တစ်ခုဖြစ်သည်။ ထို့အပြင် LOGICEXP နှင့် PINDLY တို့ကို သရုပ်ပြသော ဥပမာအသစ်များစွာလည်း ရှိသည်။

ngspice မှ လက်ရှိထောက်ပံ့ထားသော 74xx devices များအတွက် ထိုကဲ့သို့သော models များ အစုအဝေးကို Micro-Cap library မှ ဆင်းသက်လာပြီး ngspice models မှ 74xx-models.7z အဖြစ် ရရှိနိုင်သည်။

## ၁၀.၃ Hardware Description Language ဖြင့် သတ်မှတ်ထားသော Digital device များ

Ngspice သည် Verilog (VHDL မရသေး) ကဲ့သို့သော Hardware Description Language ဖြင့် ဖော်ပြချက်မှ digital device တစ်ခုကို ပြုလုပ်နိုင်သည်။ ၎င်းကို HDL code ကို သီးခြား process တစ်ခုတွင် run ခြင်း၊ သို့မဟုတ် partial netlist အဖြစ်သို့ တိုက်ရိုက် compile လုပ်ခြင်း အပါအဝင် နည်းလမ်းများစွာဖြင့် လုပ်ဆောင်နိုင်သည်။ လက်ရှိ Ngspice source code နှင့် binary packages များတွင် ပိုမိုတိုက်ရိုက်ကျသော နည်းလမ်းနှစ်ခုအတွက် ထောက်ပံ့မှုလည်း ရှိသည်- HDL files များကို Verilator သို့မဟုတ် Icarus Verilog ဖြင့် compile လုပ်၍ output file ကို d_cosim XSPICE code model (8.4.25) ၏ instance တစ်ခုထဲသို့ load လုပ်နိုင်သည်။ (သတိပေးချက်- circuit တစ်ခုတွင် d_cosim instances များစွာ အသုံးပြုခြင်းသည် ဂရုတစိုက် အစီအစဉ်ချမှတ်ရန် လိုအပ်သည်။)

ထိုကဲ့သို့သော digital device တစ်ခုကို အသုံးပြုရန် အဆင့်များမှာ- device behaviour ကို သတ်မှတ်သည့် top-level module ပါသော HDL code ကိုရေးခြင်း၊ HDL file ကို d_cosim မှလက်ခံနိုင်သော ပုံစံသို့ compile လုပ်ခြင်း၊ နှင့် netlist တွင် device အသစ်ကို circuit တစ်ခုအတွင်းထည့်သွင်းရန် device နှင့် model လိုင်းများ ပါဝင်သည်။

### ၁၀.၃.၁ Verilator, Verilog နှင့် code model d_cosim ကို အသုံးပြုခြင်း

Verilator ကိုအသုံးပြုပါက version 4.210 သို့မဟုတ် နောက်ပိုင်းလိုအပ်သည်။ Compile လုပ်သည့်အဆင့်မှာ ရိုးရှင်းသည်မဟုတ်ဘဲ၊ Verilator ၏ compiler မှ ဖန်တီးသော C++ software သို့ "glue code" များ ထည့်သွင်းရမည်ဖြစ်ပြီး ၎င်းကို d_cosim instance တစ်ခုသို့ ချိတ်ဆက်နိုင်စေရန်ဖြစ်သည်။ ဤအဆင့်ကို လွယ်ကူစေရန် script တစ်ခုကို ထောက်ပံ့ထားသည်-
```
ngspice vlnggen source.v
```
Ngspice ကို vlnggen script ကို run ရန် အသုံးပြုပြီး Verilog source file `source.v` ကို input အဖြစ် ပေးပို့သည်။ ဤ script သည် Verilator မှ output ထွက်လာသော C++ code ကို ခွဲခြမ်းစိတ်ဖြာပြီး top-level module ၏ ports များကို ဖော်ပြရန် နောက်ထပ် code များ ဖန်တီးပေးသည်။ ထို့နောက် ထုတ်လုပ်ထားသော code အားလုံးကို compile လုပ်သည်။ Output အနေဖြင့် netlist တွင် အသုံးပြုနိုင်သော shared library/DLL `source.so`/`source.DLL` ကို ရရှိမည်ဖြစ်သည်။

**နမူနာ netlist:**
```spice
ahdldevice [ inputs ... ] [ outputs ... ] dmod
.model dmod d_cosim simulation="some/path/source"
```

### ၁၀.၃.၂ Icarus Verilog နှင့် code model d_cosim ကို အသုံးပြုခြင်း

Icarus Verilog သည် Verilator ထက် full SystemVerilog specification ၏ ပိုမိုကြီးမားသော subset ကို ကိုင်တွယ်နိုင်ပြီး အဓိကအားဖြင့် interpreted code အဖြစ် compile လုပ်သည်။ Ngspice တွင် interpreter (VVP) ကို d_cosim code model ၏ instance တစ်ခုအတွင်း run ရန် components များပါဝင်သည်။ ဤ feature ကိုအသုံးပြုရန် Verilog code ကို Icarus Verilog အတွက် ထုံးစံအတိုင်း compile လုပ်သည်-
```
iverilog -o adc adc.v
```
Output file သည် Unix-like systems များတွင် တိုက်ရိုက် execute လုပ်နိုင်သော သို့မဟုတ် Windows တွင် VVP.EXE မှ run နိုင်သော ပုံစံဖြစ်သည်။ ၎င်းကို Ngspice ဖြင့် co-simulation တွင် အောက်ပါ netlist လိုင်းများဖြင့် ထည့်သွင်းနိုင်သည်-
```spice
aivldevice [ inputs ... ] [ outputs ... ][ inouts ... ] dmod
.model dmod d_cosim simulation="ivlng" sim_args=["adc"]
```

### ၁၀.၃.၃ Independent processes (ဥပမာ C coded), pipes နှင့် code model d_process ကို အသုံးပြုခြင်း

C-coded executables များဖြင့် ပြုလုပ်ထားသော သီးခြားလုပ်ငန်းစဉ်များ (processes) ကို d_process code model ကို အသုံးပြု၍ ngspice အတွင်းသို့ ပေါင်းစည်းနိုင်သည်။ C-coded executables နှင့် ngspice netlists များဖြင့် ဤ interface ကိုအသုံးပြုရန် template ကို examples/xspice/d_process တွင် ရရှိနိုင်သည်။ README သည် လုပ်ဆောင်ချက်ကို အသေးစိတ်ဖော်ပြပေးမည်။ Uros Platise မှ Isotel တွင် ပေးထားသော motor control ကဲ့သို့ ရှုပ်ထွေးသော ဥပမာတစ်ခုရှိသည်။ သူ၏ d_process code model ကို MS Windows တွင်လည်း ဝန်ဆောင်မှုပေးနိုင်ရန် မြှင့်တင်ထားပြီး ngspice version 42 မှစ၍ ပါဝင်သည်။

### ၁၀.၃.၄ Yosys ကို အသုံးပြု၍ digital Verilog ကို အခြေခံ code model cells များဆီသို့ မြေပုံဆွဲခြင်း

HDL code ကို mixed-signal simulation အတွက် ngspice netlists ထဲသို့ သယ်ဆောင်ရန် အခြားနည်းလမ်းမှာ Yosys ကို အသုံးပြု၍ HDL ကို compile လုပ်ပြီး အခြေခံ XSPICE elements (BUF, NOT, NAND, NOR, DLATCH, DFF) များကို အသုံးပြုထားသော ngspice subcircuit definition တစ်ခုသို့ ထုတ်လုပ်ထားသော synthesizable cells များကို တိုက်ရိုက်မြေပုံဆွဲခြင်းဖြစ်သည်။ Uros Platise မှ Isotel တွင် demonstrator တစ်ခုကို တီထွင်ခဲ့သည် (description and code ကိုကြည့်ပါ)။
