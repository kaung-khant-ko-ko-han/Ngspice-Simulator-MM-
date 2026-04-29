# ၂။ Circuit ဖော်ပြချက်

## ၂.၁ အထွေထွေဖွဲ့စည်းပုံနှင့် သဘောတူညီချက်များ

### ၂.၁.၁ Input ဖိုင်ဖွဲ့စည်းပုံ

Ngspice သို့ ခွဲခြမ်းစိတ်ဖြာမည့် ဆားကစ်အား element instance line များ (ဆားကစ် တိုပေါ်လော်ဂျီနှင့် element instance တန်ဖိုးများကို သတ်မှတ်ပေးသည်) နှင့် control line များ (မော်ဒယ် ပါရာမီတာများနှင့် run controls များကို သတ်မှတ်ပေးသည်) ဖြင့် ဖော်ပြသည်။ လိုင်းအားလုံးကို ngspice မှ ဖတ်ရှုရန် input file တစ်ခုတွင် စုစည်းထားသည်။ လိုင်းနှစ်ကြောင်းမှာ မရှိမဖြစ် လိုအပ်သည်-

ပထမဆုံးလိုင်းသည် ခေါင်းစဉ်ဖြစ်ရမည်၊ ၎င်းသည် ပထမနေရာတွင် အထူးအက္ခရာ လိုအပ်ခြင်းမရှိသော တစ်ခုတည်းသော မှတ်ချက်လိုင်း ဖြစ်သည်။ နောက်ဆုံးလိုင်းသည် `.end` ဖြစ်ရမည်၊ ထို့အပြင် newline delimiter တစ်ခု ထည့်ရမည်။

ကျန်လိုင်းများ၏ အစီအစဉ်မှာ အနည်းငယ်မျှသာ သတ်မှတ်ချက်ရှိသည် (သို့သော် စာကြောင်းအဆက်လိုင်းများသည် ဆက်ထားသောလိုင်း၏ နောက်တွင် ချက်ချင်းလိုက်ရမည်၊ `.subckt ... .ends`, `.if ... .endif`၊ သို့မဟုတ် `.control ... .endc` တို့သည် ၎င်းတို့၏ သီးသန့်လိုင်းများကို ဝန်းရံထားရမည်)။ လိုင်းတစ်ခု၏ ရှေ့တွင် space များပါလျှင် လျစ်လျူရှုပြီး၊ လိုင်းလွတ်များကိုလည်း လျစ်လျူရှုသည်။

အပိုင်း ၂.၁ မှ ၂.၁၂ တွင် ဖော်ပြထားသော လိုင်းများကို ပုံမှန်အားဖြင့် `.control` အပိုင်း (၁၂.၄.၃ ကိုကြည့်ပါ) ၏ အပြင်ဘက်၊ input file ၏ အဓိကနေရာတွင် အသုံးပြုသည်။ ခြွင်းချက်မှာ `.include includefile` လိုင်း (၂.၈) ဖြစ်ပြီး ၎င်းကို input file ၏ မည်သည့်နေရာတွင်မဆို ထားနိုင်သည်။ `includefile` ၏ အကြောင်းအရာများကို `.include` လိုင်း၏ နေရာတည့်တည့်တွင် အတိအကျ ထည့်သွင်းပေးမည်ဖြစ်သည်။

### ၂.၁.၂ Syntax စစ်ဆေးခြင်း

Input parser တွင် အလွန်အခြေခံကျသော syntax စစ်ဆေးခြင်းကို ထည့်သွင်းထားသည်။

#### ၂.၁.၂.၁ တရားဝင် utf-8 အက္ခရာများ

Input file ကို တရားဝင် utf-8 အက္ခရာများအတွက် စစ်ဆေးမည်။ တရားမဝင်သော အက္ခရာများ တွေ့ရှိပါက၊ input ဖတ်ခြင်းကို ရပ်တန့်မည်။

#### ၂.၁.၂.၂ လိုင်းတစ်ခုကို စတင်သော အထူးအက္ခရာများ

netlist သို့မဟုတ် `.control` လိုင်းတစ်ခုရှိ ပထမဆုံးအက္ခရာသည် `= []?() &&\\$\\$\\*!..` တို့မှ တစ်ခုခုဖြစ်ပါက ngspice က ၎င်းကို `\\*\\*` ဖြင့် အစားထိုးပြီး သတိပေးချက် ထုတ်ပေးသည်။ `set strict_errorhandling` အမိန့်ပေးချက်က ngspice အား ထွက်ခွာရန် တွန်းအားပေးလိမ့်မည်။

#### ၂.၁.၂.၃ Dot command couple ပြီးစီးမှု စစ်ဆေးခြင်း

`.control ... .endc`, `.subckt ... .ends`, `.if ... .endif` တို့အတွက် စစ်ဆေးသည်။

### ၂.၁.၃ အမည်ပေးခြင်းဆိုင်ရာ သဘောတူညီချက်အချို့

#### ၂.၁.၃.၁ လိုင်းများ

လိုင်းတစ်ခုပေါ်ရှိ fields များကို blank တစ်ခု သို့မဟုတ် တစ်ခုထက်ပိုသော blanks များ၊ ကော်မာ၊ ညီမျှခြင်းသင်္ကေတ (`=`), သို့မဟုတ် ဘယ် သို့မဟုတ် ညာ parentheses များဖြင့် ပိုင်းခြားထားပြီး၊ ပိုနေသော space များကို လျစ်လျူရှုသည်။ အောက်လိုင်းတွင် ပထမကော်လံ၌ `+` ကိုရိုက်ထည့်ခြင်းဖြင့် လိုင်းတစ်ခုကို ဆက်နိုင်သည်၊ ngspice သည် ကော်လံ ၂ မှစ၍ ဆက်လက်ဖတ်ရှုသည်။ Unix shell style တွင် နောက်ဆုံးအက္ခရာနှစ်လုံးသည် backslashes ဖြစ်နေပါကလည်း လိုင်းများကို ဆက်နိုင်သည်။ Device name field တစ်ခုသည် စာလုံး (A မှ Z) ဖြင့် စတင်ရမည်ဖြစ်ပြီး delimiter များ မပါဝင်ရပါ။

#### ၂.၁.၃.၂ နံပါတ်များ

နံပါတ် field တစ်ခုသည် integer field (12, -44)၊ floating point field (3.14159)၊ integer သို့မဟုတ် floating point နံပါတ်နောက်တွင် integer exponent (1e-14, 2.65e3)၊ သို့မဟုတ် integer သို့မဟုတ် floating point နံပါတ်နောက်တွင် အောက်ပါ scale factors များထဲမှ တစ်ခုခု ဖြစ်နိုင်သည်-

| Suffix | အမည် | Factor |
|--------|------|--------|
| T      | Tera | 10^12  |
| G      | Giga | 10^9   |
| Meg    | Mega | 10^6   |
| K      | Kilo | 10^3   |
| mil    | Mil  | 25.4 × 10^-6 |
| m      | milli| 10^-3  |
| u      | micro| 10^-6  |
| n      | nano | 10^-9  |
| p      | pico | 10^-12 |
| f      | femto| 10^-15 |
| a      | atto | 10^-18 |

#### ၂.၁.၃.၃ နံပါတ်တစ်ခု၏နောက်မှ စာလုံးများ

နံပါတ်တစ်ခု၏နောက်တွင် ချက်ချင်းလိုက်ပါလာသော scale factor မဟုတ်သည့် စာလုံးများကို လျစ်လျူရှုပြီး၊ scale factor တစ်ခု၏နောက်တွင် ချက်ချင်းလိုက်ပါလာသော စာလုံးများကိုလည်း လျစ်လျူရှုသည်။ ထို့ကြောင့် 10, 10V, 10Volts နှင့် 10Hz အားလုံးသည် တူညီသောနံပါတ်ကို ကိုယ်စားပြုပြီး၊ M, MA, MSc, နှင့် MMhos အားလုံးသည် တူညီသော scale factor ကို ကိုယ်စားပြုသည်။

သတိပြုရန်- 1000, 1000.0, 1000Hz, 1e3, 1.0e3, 1kHz, နှင့် 1k အားလုံးသည် တူညီသောနံပါတ်ကို ကိုယ်စားပြုသည်။ 'M' သို့မဟုတ် 'm' သည် 'milli' ကို ဆိုလိုသည်၊ ဆိုလိုသည်မှာ \(10^{-3}\) ဖြစ်သည်။ \(10^{6}\) အတွက် `meg` ဟူသော suffix ကို အသုံးပြုရမည်။ အကယ်၍ compatibility mode LT (၁၂.၁၄.၆) ကို သတ်မှတ်ထားပါက ngspice သည် resistance သို့မဟုတ် capacitance တန်ဖိုးများထည့်သွင်းရန်အတွက် RKM notation ကို လက်ခံလိမ့်မည်၊ ဥပမာ 2K7 သို့မဟုတ် 100R။

#### ၂.၁.၃.၄ Node အမည်များ

Node အမည်များသည် နံပါတ်တစ်ခုဖြင့် မစတင်သည့် စိတ်ကြိုက်အက္ခရာစာကြောင်းများ ဖြစ်နိုင်သည် (အောက်ပါခြွင်းချက်များကို ကြည့်ပါ)။ Ngspice ကို batch mode (၁၂.၄.၁) တွင်အသုံးပြုပါက case insensitive ဖြစ်သည်။ အကယ်၍ interactive (၁၂.၄.၂) သို့မဟုတ် control (၁၂.၄.၃) mode တွင်သုံးပါက node အမည်များသည် သာမန်နံပါတ်များ သို့မဟုတ် စိတ်ကြိုက် အက္ခရာစာကြောင်းများ ဖြစ်နိုင်ပြီး နံပါတ်များဖြင့် မစတင်ရပါ။ XSPICE code models များကို အသုံးပြုသောအခါ (၎င်းတို့တွင် အထူးအဓိပ္ပာယ်ရှိပြီး string delimiter အဖြစ် လုပ်ဆောင်သည်) အောက်ပါအက္ခရာများ `= % ( ) , [ ] < > -` ကို node အမည်တွင် ခွင့်မပြုပါ။

#### ၂.၁.၃.၅ Ground node

Ground node ကို '0' (သုည) ဟု အမည်ပေးရမည်။ Compatibility အတွက် `gnd` ကို ground node အဖြစ် လက်ခံပြီး၊ အတွင်းပိုင်း၌ global node အဖြစ် သတ်မှတ်ကာ '0' သို့ ပြောင်းလဲမည်။ ဤသို့မဖြစ်နိုင်ပါက configuration files `spinit` သို့မဟုတ် `.spiceinit` တွင် `set no_auto_gnd` ဟု သတ်မှတ်၍ ပြောင်းလဲခြင်းကို ပိတ်ထားနိုင်သည်။ ဆားကစ်တိုင်းတွင် ground node (gnd သို့မဟုတ် 0) ရှိရမည်။ Ngspice တွင် node များကို character strings များအဖြစ် ဆက်ဆံပြီး နံပါတ်များအဖြစ် အကဲဖြတ်ခြင်း မပြုပါ၊ ထို့ကြောင့် '0' နှင့် 00 သည် ngspice တွင် သီးခြား node များဖြစ်ပြီး SPICE2 တွင်မူ မဟုတ်ပေ။

### ၂.၁.၄ Topological ကန့်သတ်ချက်များ

Ngspice သည် အောက်ပါ topological ကန့်သတ်ချက်များ ကျေနပ်ရန် လိုအပ်သည်-

- ဆားကစ်တွင် voltage sources နှင့်/သို့မဟုတ် inductors များ၏ loop တစ်ခု မပါဝင်ရ၊ current sources နှင့်/သို့မဟုတ် capacitors များ၏ cut-set တစ်ခု မပါဝင်ရ။
- ဆားကစ်ရှိ node တိုင်းသည် ground သို့ dc path တစ်ခု ရှိရမည်။
- Node တိုင်းတွင် transmission line nodes များ (အဆုံးမရှိသော transmission lines များကို ခွင့်ပြုရန်) နှင့် MOSFET substrate nodes များ (မည်သို့ပင်ဖြစ်စေ အတွင်းပိုင်းဆက်သွယ်မှု နှစ်ခုရှိသည်) မှလွဲ၍ အနည်းဆုံး ဆက်သွယ်မှု နှစ်ခု ရှိရမည်။

## ၂.၂ Dot အမိန့်ပေးချက်များ

ဤအပိုင်းသည် ngspice တွင် ရရှိနိုင်သော dot commands အားလုံးကို ၎င်းတို့၏အသေးစိတ် တင်ပြချက်များသို့ link များနှင့်အတူ အက္ခရာစဉ်အတိုင်း အကျဉ်းချုပ်ဖော်ပြထားသည်။ Control section (သို့မဟုတ် interactive) commands များကို အခန်း ၁၃.၅ တွင် စာရင်းပြုလုပ်၍ ရှင်းပြထားသည်။

- `.AC` - AC simulation တစ်ခုစတင်ရန် (၁၁.၃.၁)
- `.CONTROL` - `.control` အပိုင်းတစ်ခု စတင်ရန် (၁၂.၄.၃)
- `.CSPARAM` - Control section တစ်ခုတွင် ရရှိနိုင်သော parameter(s) သတ်မှတ်ရန် (၂.၁၃)
- `.DC` - DC simulation တစ်ခုစတင်ရန် (၁၁.၃.၂)
- `.DISTO` - Distortion analysis simulation တစ်ခုစတင်ရန် (၁၁.၃.၃)
- `.ELSE` - Netlist ထဲတွင် conditional branching (၂.၁၅)
- `.ELSEIF` - Netlist ထဲတွင် conditional branching (၂.၁၅)
- `.END` - Netlist ၏အဆုံး (၂.၄.၂)
- `.ENDC` - `.control` အပိုင်း၏အဆုံး (၁၂.၄.၃)
- `.ENDIF` - Netlist ထဲတွင် conditional branching (၂.၁၅)
- `.ENDS` - Subcircuit definition ၏အဆုံး (၂.၆.၂)
- `.FOUR` - Transient simulation output ၏ Fourier analysis (၁၁.၆.၄)
- `.FUNC` - Function တစ်ခု သတ်မှတ်ရန် (၂.၁၂)
- `.GLOBAL` - Global nodes များ သတ်မှတ်ရန် (၂.၇)
- `.IC` - Initial conditions သတ်မှတ်ရန် (၁၁.၂.၂)
- `.IF` - Netlist ထဲတွင် conditional branching (၂.၁၅)
- `.INCLUDE` - Netlist ၏တစ်စိတ်တစ်ပိုင်းကို ထည့်သွင်းရန် (၂.၈)
- `.INCPSLT` - Netlist ၏တစ်စိတ်တစ်ပိုင်းကို compatibility mode 'pslt' ဖြင့် ထည့်သွင်းရန် (၂.၉၊ ၁၂.၁၄.၄.၂)
- `.LIB` - Library တစ်ခု ထည့်သွင်းရန် (၂.၁၀)
- `.MEAS` - Simulation အတွင်း တိုင်းတာမှုများ (၁၁.၄)
- `.MODEL` - Device model parameters များစာရင်း (၂.၅)
- `.NODESET` - Initial conditions သတ်မှတ်ရန် (၁၁.၂.၁)
- `.NOISE` - Noise simulation တစ်ခုစတင်ရန် (၁၁.၃.၄)
- `.OP` - Operating point simulation တစ်ခုစတင်ရန် (၁၁.၃.၅)
- `.OPTIONS` - Simulator options များသတ်မှတ်ရန် (၁၁.၁)
- `.PARAM` - Parameter(s) သတ်မှတ်ရန် (၂.၁၁)
- `.PLOT` - Batch simulation အတွင်း printer plot (၁၁.၆.၃)
- `.PRINT` - Batch simulation အတွင်း tabular listing (၁၁.၆.၂)
- `.PROBE` - Device currents, voltages and differential voltages များကို သိမ်းဆည်းရန် (၁၁.၆.၅)
- `.PSS` - Periodic steady state analysis တစ်ခုစတင်ရန် (၁၁.၃.၁၂)
- `.PZ` - Pole-zero analysis simulation တစ်ခုစတင်ရန် (၁၁.၃.၆)
- `.SAVE` - သိမ်းဆည်းမည့် simulation result vectors များကို အမည်ပေးရန် (၁၁.၆.၁)
- `.SENS` - Sensitivity analysis တစ်ခုစတင်ရန် (၁၁.၃.၇)
- `.SP` - S parameter analysis (၁၁.၃.၈)
- `.SUBCKT` - Subcircuit definitions များ၏အစ (၂.၆)
- `.TEMP` - Circuit temperature သတ်မှတ်ရန် (၂.၁၄)
- `.TF` - Transfer function analysis တစ်ခုစတင်ရန် (၁၁.၃.၉)
- `.TITLE` - Netlist ၏ခေါင်းစဉ် (၂.၄.၁)
- `.TRAN` - Transient simulation တစ်ခုစတင်ရန် (၁၁.၃.၁၀)
- `.WIDTH` - Printer plot ၏ width (၁၁.၆.၇)

## ၂.၃ Circuit element များ (device instances)

ဆားကစ်အတွင်းရှိ element တိုင်းသည် instance line တစ်ခုဖြင့် သတ်မှတ်ထားသော device instance တစ်ခုဖြစ်သည်၊ ၎င်းတွင်-

- element instance အမည်၊
- element ချိတ်ဆက်ထားသော circuit nodes များ၊
- element ၏ လျှပ်စစ်ဝိသေသလက္ခဏာများကို ဆုံးဖြတ်ပေးသော parameters များ၏တန်ဖိုး တို့ပါဝင်သည်။

element instance အမည်၏ ပထမဆုံးစာလုံးသည် element အမျိုးအစားကို သတ်မှတ်ပေးသည်။ Ngspice element အမျိုးအစားများအတွက် format ကို အောက်ပါလက်စွဲအခန်းများတွင် ဖော်ပြထားသည်၊ ဥပမာ `BZZZZZ`။ `XXXXXXXX`, `YYYYYYYY`, နှင့် `ZZZZZZZ` တို့သည် စိတ်ကြိုက် alphanumeric strings များကို ကိုယ်စားပြုသည်။

ဥပမာ၊ resistor instance name တစ်ခုသည် R စာလုံးဖြင့်စပြီး နောက်တွင် စာလုံးတစ်လုံး သို့မဟုတ် တစ်လုံးထက်ပို၍ ပါဝင်နိုင်သည်။ ထို့ကြောင့် `R`, `R1`, `RSE`, `ROUT` နှင့် `R3AC2ZY` တို့သည် တရားဝင် resistor အမည်များဖြစ်သည်။ စက်ပစ္စည်းအမျိုးအစားတစ်ခုစီ၏ အသေးစိတ်ကို နောက်ဆက်တွဲအပိုင်း ၃ တွင် ဖော်ပြထားသည်။ အောက်ပါဇယားတွင် ngspice တွင်ရရှိနိုင်သော element အမျိုးအစားများကို ၎င်းတို့၏ ပထမစာလုံးအလိုက် စီထားသည်-

| ပထမစာလုံး | Element ဖော်ပြချက်        | Links                    |
|-----------|---------------------------|--------------------------|
| A         | XSPICE code model         | analog (၈.၂)၊ digital (၈.၄)၊ mixed signal (၈.၃) |
| B         | Behavioral (arbitrary) source | ၅.၁                    |
| C         | Capacitor                 | ၃.၃.၆                    |
| D         | Diode                     | ၇                        |
| E         | Voltage-controlled voltage source (VCVS) | linear (၄.၂.၂)၊ non-linear (၅.၂) |
| F         | Current-controlled current source (CCCS) | linear (၄.၂.၃)          |
| G         | Voltage-controlled current source (VCCS) | linear (၄.၂.၁)၊ non-linear (၅.၃) |
| H         | Current-controlled voltage source (CCVS) | linear (၄.၂.၄)          |
| I         | Current source            | ၄.၁                      |
| J         | Junction field effect transistor (JFET) | ၇.၄               |
| K         | Coupled (Mutual) Inductors | ၃.၃.၁၂                   |
| L         | Inductor                  | ၃.၃.၁၀                   |
| M         | Metal oxide field effect transistor (MOSFET) | ၇.၆ BSIM3 (၇.၆.၃.၃) BSIM4 (၇.၆.၃.၄) |
| N         | Verilog-A Compact Device Models | ၉                        |
| O         | Lossy transmission line   | ၆.၂                      |
| P         | Coupled multiconductor line (CPL) | ၆.၄.၂                |
| Q         | Bipolar junction transistor (BJT) | ၇.၃                      |
| R         | Resistor                  | ၃.၃.၁                    |
| S         | Switch (voltage-controlled) | ၃.၃.၁၅                 |
| T         | Lossless transmission line | ၆.၁                      |
| U         | Uniformly distributed RC line | ၆.၃                    |
| U*        | Basic digital building blocks using XSPICE | ၁၀       |
| V         | Voltage source            | ၄.၁                      |
| W         | Switch (current-controlled) | ၃.၃.၁၅                  |
| X         | Subcircuit                | ၂.၆.၃                    |
| Y         | Single lossy transmission line (TXL) | ၆.၄.၁              |
| Z         | Metal semiconductor field effect transistor (MESFET) | ၇.၅ |

## ၂.၄ အခြေခံ စာကြောင်းများ

### ၂.၄.၁ .TITLE လိုင်း

**ဥပမာများ:**
```
POWER AMPLIFIER CIRCUIT
* additional lines following
*...
Test of CAM cell
* additional lines following
*...
```
ခေါင်းစဉ်လိုင်းသည် input file တွင် ပထမဆုံးဖြစ်ရမည်။ ၎င်း၏အကြောင်းအရာများကို output ၏ section တစ်ခုချင်းစီအတွက် ခေါင်းစည်းအဖြစ် အတိအကျ print ထုတ်သည်။

အခြားနည်းလမ်းအနေဖြင့်၊ သင့် input deck တွင် `.TITLE <any title>` လိုင်းကို မည်သည့်နေရာ၌မဆို ထားရှိနိုင်သည်။ `.TITLE` statement ၏နောက်မှ ဤလိုင်း၏အကြောင်းအရာများဖြင့် သင့် input deck ၏ ပထမလိုင်းကို override လုပ်မည်။

**`.TITLE` လိုင်း ဥပမာ:**
```
* additional lines following
*...
.TITLE Test of CAM cell
* additional lines following
*...
```
အတွင်းပိုင်းတွင် အောက်ပါအတိုင်း အစားထိုးမည်-
```
Test of CAM cell
* additional lines following
*...
*TITLE Test of CAM cell
* additional lines following
*...
```

### ၂.၄.၂ .END လိုင်း

**ဥပမာ:**
```
.end
```
`.end` လိုင်းသည် input file တွင် အမြဲတမ်း နောက်ဆုံးဖြစ်ရမည်။ အစက် (period) သည် အမည်၏ အဓိကအစိတ်အပိုင်းဖြစ်ကြောင်း သတိပြုပါ။

### ၂.၄.၃ မှတ်ချက်များ

**ယေဘူယျပုံစံ:**
```
* <any comment>
```
**ဥပမာများ:**
```
* RF=1K Gain should be 100
* Check open-loop gain and phase margin
```
ပထမကော်လံရှိ asterisk သည် ဤလိုင်းသည် မှတ်ချက်လိုင်းဖြစ်ကြောင်း ညွှန်ပြသည်။ မှတ်ချက်လိုင်းများကို circuit description ၏ မည်သည့်နေရာတွင်မဆို ထားရှိနိုင်သည်။

### ၂.၄.၄ စာကြောင်းအဆုံး မှတ်ချက်များ

**ယေဘူယျပုံစံ:**
```
<any command> $ <any comment>
<any command> ; <any comment>
<any command> // <any comment>
```
**ဥပမာများ:**
```
RF2=1K $ Gain should be 100
C1=10p ; Check open-loop gain and phase margin
.param n1=1 //new value
```
Ngspice သည် `$ ` (dollar plus space) သို့မဟုတ် `//` အက္ခရာနှစ်လုံးဖြင့် စတင်သော မှတ်ချက်များကို ထောက်ပံ့သည်။ ဖတ်ရှုရလွယ်ကူစေရန် မှတ်ချက်အက္ခရာတစ်ခုချင်းစီ၏ ရှေ့တွင် space တစ်ခုထားသင့်သည်။ Ngspice သည် တစ်ခုတည်းသော `$` အက္ခရာကိုလည်း လက်ခံသည်။

ကျေးဇူးပြု၍ သတိပြုပါ- PSPICE compatibility mode (၁၂.၁၄.၅) ကိုရွေးချယ်ထားပါက `$` အက္ခရာသည် တရားဝင် end-of-line comment delimiter မဟုတ်ပါ။ ထိုအခါ `$` သည် သာမန်အက္ခရာ ဖြစ်လာသည်။

### ၂.၄.၅ စာကြောင်းအဆက် လိုင်းများ

**ယေဘူယျပုံစံ:**
```
<any command>
+ <continuation of any command> ; some comment
+ <further continuation of any command>
```
Input လိုင်းများ အလွန်ရှည်လျားပါက (ဥပမာ ပိုမိုဖတ်ရှုရလွယ်ကူစေရန်) ၎င်းတို့ကို လိုင်းနှစ်ကြောင်း သို့မဟုတ် ထို့ထက်ပို၍ ခွဲနိုင်သည်။ အတွင်းပိုင်းတွင် ၎င်းတို့ကို တစ်ကြောင်းတည်းအဖြစ် ပေါင်းစည်းမည်။ နောက်ဆက်တွဲလိုင်းတစ်ခုစီသည် `+` အက္ခရာနှင့် နောက်ထပ် space တစ်ခုတို့ဖြင့် စတင်သည်။ နောက်ဆက်တွဲလိုင်းများသည် တစ်ခုနှင့်တစ်ခု ချက်ချင်းဆက်နေရမည်။ စာကြောင်းအဆုံးမှတ်ချက်များကို လျစ်လျူရှုမည်။ Unix shells များတွင်သုံးသကဲ့သို့ backslash နှစ်ခုဖြင့် လိုင်းကိုအဆုံးသတ်ခြင်းဖြင့်လည်း လိုင်းများကို ဆက်နိုင်သည်။ အောက်ပါလိုင်းများသည် continuation lines များအသုံးပြုခွင့်မပြုပါ- `.title`, `.lib`, နှင့် `.include`။

## ၂.၅ .MODEL စက်ပစ္စည်း ပုံစံများ

**ယေဘူယျပုံစံ:**
```
.model mname type(pname1 = pval1 pname2 = pval2 ... )
```
**ဥပမာ:**
```
.model MOD1 npn (bf=50 is=1e-13 vbf=50)
```
အများဆုံးသော ရိုးရှင်းသည့် circuit elements များသည် ပုံမှန်အားဖြင့် parameter တန်ဖိုး အနည်းငယ်သာ လိုအပ်သည်။ သို့သော် ngspice တွင်ပါဝင်သော အချို့စက်ပစ္စည်းများ (အထူးသဖြင့် semiconductor devices) သည် parameter တန်ဖိုး အများအပြား လိုအပ်သည်။ ဆားကစ်တစ်ခုရှိ စက်ပစ္စည်းများစွာကို တူညီသော device model parameters အစုဖြင့် သတ်မှတ်လေ့ရှိသည်။ ဤအကြောင်းများကြောင့်၊ device model parameters အစုကို သီးခြား `.model` လိုင်းတစ်ခုပေါ်တွင် သတ်မှတ်ပြီး ထူးခြားသော model name တစ်ခု သတ်မှတ်ပေးသည်။ ထို့နောက် ngspice ရှိ device element lines များက model name ကို ရည်ညွှန်းသည်။

ဤကဲ့သို့ ပိုမိုရှုပ်ထွေးသော device အမျိုးအစားများအတွက်၊ device element line တစ်ခုချင်းစီတွင် device name၊ device ချိတ်ဆက်ထားသော nodes များ၊ နှင့် device model name တို့ပါဝင်သည်။ ထို့အပြင် အချို့စက်ပစ္စည်းများအတွက် အခြားသော ရွေးချယ်နိုင်သည့် parameters များကိုလည်း သတ်မှတ်နိုင်သည်- geometric factors နှင့် initial condition (အသေးစိတ်အတွက် Transistors (၇.၃ မှ ၇.၆) နှင့် Diodes (၇) ဆိုင်ရာ နောက်ဆက်တွဲအပိုင်းများကို ကြည့်ပါ)။ အထက်ပါ `mname` သည် model name ဖြစ်ပြီး၊ `type` သည် အောက်ပါအမျိုးအစား ၁၅ မျိုးထဲမှ တစ်ခုဖြစ်သည်-

| Model Type | အမျိုးအစား                     |
|------------|-------------------------------|
| R          | Semiconductor resistor model  |
| C          | Semiconductor capacitor model |
| L          | Inductor model                |
| SW         | Voltage controlled switch     |
| CSW        | Current controlled switch     |
| URC        | Uniform distributed RC model  |
| LTRA       | Lossy transmission line model |
| D          | Diode model                   |
| NPN        | NPN BJT model                 |
| PNP        | PNP BJT model                 |
| NJF        | N-channel JFET model          |
| PJF        | P-channel JFET model          |
| NMOS       | N-channel MOSFET model        |
| PMOS       | P-channel MOSFET model        |
| NMF        | N-channel MESFET model        |
| PMF        | P-channel MESFET model        |
| VDMOS      | Power MOS model               |

Parameter တန်ဖိုးများကို parameter name နောက်တွင် equal sign နှင့် parameter value ကိုထည့်ခြင်းဖြင့် သတ်မှတ်သည်။ တန်ဖိုးမသတ်မှတ်ထားသော Model parameters များကို model type တစ်ခုချင်းစီအတွက် အောက်တွင်ဖော်ပြထားသော default values များ သတ်မှတ်ပေးသည်။ Models များကို device တစ်ခုချင်းစီဆိုင်ရာ section တွင် device element lines များ၏ဖော်ပြချက်နှင့်အတူ စာရင်းပြုလုပ်ထားသည်။ Model parameters နှင့် ၎င်းတို့၏ default values များကို Chapt. 27 တွင် ဖော်ပြထားသည်။

## ၂.၆ .SUBCKT Subcircuits

Ngspice element များပါဝင်သော subcircuits များကို device models များကဲ့သို့ပင် သတ်မှတ်၍ အသုံးပြုနိုင်သည်။ Subcircuits များသည် ngspice က hierarchical modeling အကောင်အထည်ဖော်သည့်နည်းလမ်းဖြစ်ပြီး ထပ်တလဲလဲပါဝင်သော sections များပါသည့် ဆားကစ်များကို ပိုမိုလွယ်ကူစွာ ကိုယ်စားပြုနိုင်စေသည်။ SPICE deck တစ်ခုကို parse လုပ်စဉ်တွင်၊ subcircuit instance တစ်ခုချင်းစီကို text expansion သုံး၍ ၎င်း၏ definition ဖြင့် အစားထိုးပြီး input processing ပြီးနောက် hierarchy မရှိတော့ပါ။

Subcircuit ကို `.subckt` နှင့် `.ends` cards (သို့မဟုတ် `substart` နှင့် `subend` options (၁၃.၇ ကိုကြည့်ပါ) များဖြင့် သတ်မှတ်ထားသော keywords များ) တို့ဖြင့် ပိုင်းခြားထားသော element cards အုပ်စုဖြင့် input deck တွင် သတ်မှတ်သည်၊ ထို့နောက် ပရိုဂရမ်က subcircuit ကို ရည်ညွှန်းသည့်နေရာတိုင်းတွင် သတ်မှတ်ထားသော element အုပ်စုကို အလိုအလျောက် ထည့်သွင်းပေးသည်။ ပိုမိုကြီးမားသော ဆားကစ်အတွင်းရှိ subcircuits များ၏ instances များကို 'X' စာလုံးဖြင့် စတင်သည့် instance card တစ်ခုအသုံးပြုခြင်းဖြင့် သတ်မှတ်သည်။ ဤ cards သုံးခုလုံး၏ ပြီးပြည့်စုံသော ဥပမာမှာ-

**ဥပမာ:**
```
* The following is the instance card:
* xdiv1 10 7 0 vdivide
*
* The following are the subcircuit definition cards:
* .subckt vdivide 1 2 3
r1 1 2 10K
r2 2 3 5K
.ends
```
အထက်ပါအရာက ports '1', '2' နှင့် '3' ပါသော subcircuit တစ်ခုကို သတ်မှတ်သည်-

Resistor 'R1' သည် port '1' မှ port '2' သို့ ချိတ်ဆက်ပြီး တန်ဖိုး \(10\mathrm{kOhms}\) ရှိသည်။
Resistor 'R2' သည် port '2' မှ port '3' သို့ ချိတ်ဆက်ပြီး တန်ဖိုး \(5\mathrm{kOhms}\) ရှိသည်။

Instance card ကို ngspice deck တွင်ထားရှိသောအခါ၊ subcircuit port '1' သည် circuit node '10' နှင့် ညီမျှစေရန်၊ port '2' သည် node '7' နှင့် ညီမျှစေရန်နှင့် port '3' သည် node '0' နှင့် ညီမျှစေရန် ဆောင်ရွက်မည်။

Subcircuits များ၏ အရွယ်အစား သို့မဟုတ် ရှုပ်ထွေးမှုအပေါ် ကန့်သတ်ချက်မရှိပါ၊ subcircuits များတွင် အခြား subcircuits များ ပါဝင်နိုင်သည်။ Subcircuit အသုံးပြုမှု ဥပမာကို Chapt. 17.6 တွင် ဖော်ပြထားသည်။

### ၂.၆.၁ .SUBCKT လိုင်း

**ယေဘူယျပုံစံ:**
```
.SUBCKT subnam N1 <N2 N3 ...>
```
**ဥပမာ:**
```
.SUBCKT OPAMP 1 2 3 4
```
Circuit definition တစ်ခုကို `.SUBCKT` လိုင်းဖြင့် စတင်သည်။ `subnam` သည် subcircuit အမည်ဖြစ်ပြီး၊ `N1, N2, ...` တို့သည် external nodes များဖြစ်ကာ၊ ၎င်းတို့သည် သုည မဖြစ်ရပါ။ `.SUBCKT` လိုင်း၏ နောက်တွင် ချက်ချင်းလိုက်ပါလာသော element lines အုပ်စုသည် subcircuit ကို သတ်မှတ်သည်။ subcircuit definition တစ်ခုရှိ နောက်ဆုံးလိုင်းသည် `.ENDS` လိုင်း (အောက်တွင်ကြည့်ပါ) ဖြစ်သည်။ Control lines များသည် subcircuit definition တစ်ခုအတွင်း မပေါ်နိုင်ပါ၊ သို့သော် subcircuit definitions များတွင် အခြား subcircuit definitions များ၊ device models များ၊ နှင့် subcircuit calls များ (အောက်တွင်ကြည့်ပါ) အပါအဝင် အခြားအရာများ ပါဝင်နိုင်သည်။ subcircuit definition ၏ တစ်စိတ်တစ်ပိုင်းအဖြစ် ပါဝင်သော မည်သည့် device models သို့မဟုတ် subcircuit definitions မဆို strictly local ဖြစ်သည် (ဆိုလိုသည်မှာ ထို models နှင့် definitions များကို subcircuit definition အပြင်ဘက်တွင် မသိနိုင်ပါ)။ ထို့အပြင် 0 (ground) မှလွဲ၍ `.SUBCKT` လိုင်းပေါ်တွင် မပါဝင်သော မည်သည့် element nodes မဆို strictly local ဖြစ်သည်။ 0 (ground) သည် အမြဲတမ်း global ဖြစ်သည်။ အကယ်၍ parameters များ သုံးပါက `.SUBCKT` လိုင်းကို တိုးချဲ့မည် (၂.၁၁.၃ ကိုကြည့်ပါ)။

### ၂.၆.၂ .ENDS လိုင်း

**ယေဘူယျပုံစံ:**
```
.ENDS <SUBNAM>
```
**ဥပမာ:**
```
.ENDS OPAMP
```
`.ENDS` လိုင်းသည် မည်သည့် subcircuit definition အတွက်မဆို နောက်ဆုံးလိုင်းဖြစ်ရမည်။ Subcircuit အမည်ကို ထည့်သွင်းပါက၊ မည်သည့် subcircuit definition ကို အဆုံးသတ်ကြောင်း ဖော်ပြသည်၊ ချန်လှပ်ပါက သတ်မှတ်ဆဲ subcircuits အားလုံးကို အဆုံးသတ်သည်။ Nested subcircuit definitions များပြုလုပ်နေချိန်တွင်သာ အမည် လိုအပ်သည်။

### ၂.၆.၃ Subcircuit ခေါ်ယူမှုများ

**ယေဘူယျပုံစံ:**
```
XXXXXXX N1 <N2 N3 ...> SUBNAM
```
**ဥပမာ:**
```
X1 2 4 17 3 1 MULTI
```
Ngspice တွင် Subcircuits များကို X စာလုံးဖြင့်စတင်သော pseudo-elements များကို သတ်မှတ်၍ subcircuit ကိုချဲ့ထွင်ရာတွင်အသုံးပြုမည့် circuit nodes များဖြင့် နောက်တွင် အသုံးပြုသည်။ အကယ်၍ parameters များသုံးပါက subcircuit call ကို ပြုပြင်မွမ်းမံမည် (၂.၁၁.၃ ကိုကြည့်ပါ)။

## ၂.၇ .GLOBAL

**ယေဘူယျပုံစံ:**
```
.GLOBAL nodename1 [ nodename2 ... ]
```
**ဥပမာ:**
```
.GLOBAL gnd vcc
```
`.GLOBAL` statement တွင် သတ်မှတ်ထားသော Nodes များသည် မည်သည့် circuit hierarchy မှ သီးခြားလွတ်ကင်းစွာ circuit နှင့် subcircuit blocks အားလုံးအတွက် ရရှိနိုင်သည်။ Circuit ကို parse လုပ်ပြီးနောက်၊ ဤ node များကို top level မှ ဝင်ရောက်နိုင်သည်။

## ၂.၈ .INCLUDE

**ယေဘူယျပုံစံ:**
```
.INCLUDE filename
```
**ဥပမာ:**
```
.INCLUDE /users/spice/common/bsim3-param.mod
```
Circuit descriptions များ၏ အစိတ်အပိုင်းများကို input files များစွာတွင် မကြာခဏ ပြန်လည်အသုံးပြုလေ့ရှိသည်၊ အထူးသဖြင့် common models နှင့် subcircuits များဖြစ်သည်။ မည်သည့် ngspice input file တွင်မဆို၊ `.INCLUDE` လိုင်းကို အသုံးပြု၍ အခြားဖိုင်တစ်ခုကို မူရင်းဖိုင်ရှိ `.INCLUDE` လိုင်း၏နေရာတွင် ထိုဒုတိယဖိုင် ပေါ်လာသကဲ့သို့ ကူးယူနိုင်သည်။

filename သည် relative path ဖြစ်ပြီး ဖိုင်ကိုရှာမတွေ့ပါက၊ variable `sourcepath` (၁၃.၇) မှပေးသော တည်နေရာများတွင် ရှာဖွေသည်။ ngspice က လက်ခံသော file name တွင် local operating system မှ ချမှတ်သော ကန့်သတ်ချက်များထက် ပိုမိုသော ကန့်သတ်ချက်မရှိပါ။

## ၂.၉ .INCPSLT

**ယေဘူယျပုံစံ:**
```
.INCPSLT filename
```
**ဥပမာ:**
```
.INCPSLT /users/spice/models/OPA1641.lib
```
Netlist တစ်ခု၏အစိတ်အပိုင်းကို include လုပ်ခြင်း၏ အထူးပုံစံတစ်မျိုး- Main netlist တွင် မတူညီသော compatibility mode ရှိနေသော်လည်း include လုပ်ထားသောအပိုင်းကို ၎င်း၏ compatibility mode ကို 'pslt' အဖြစ် သတ်မှတ်ထားသကဲ့သို့ ဆက်ဆံသည်။ အခန်း ၁၂.၁၄.၄.၂ ကိုလည်းကြည့်ပါ။

filename သည် relative path ဖြစ်ပြီး ဖိုင်ကိုရှာမတွေ့ပါက၊ variable `sourcepath` (၁၃.၇) မှပေးသော တည်နေရာများတွင် ရှာဖွေသည်။ ngspice က လက်ခံသော file name တွင် local operating system မှ ချမှတ်သော ကန့်သတ်ချက်များထက် ပိုမိုသော ကန့်သတ်ချက်မရှိပါ။

## ၂.၁၀ .LIB

**ယေဘူယျပုံစံ:**
```
.LIB filename libname
```
**ဥပမာ:**
```
.LIB /users/spice/common/mosfets.lib mos1
```
`.LIB` statement သည် input file ထဲသို့ library descriptions များကို include လုပ်ခွင့်ပြုသည်။ `*.lib` file အတွင်းတွင် library `libname` ကို ရွေးချယ်မည်။ `*.lib` file အတွင်းရှိ library တစ်ခုချင်းစီ၏ statements များကို `.LIB libname <...>. ENDL` statements များဖြင့် ဝန်းရံထားသည်။ `.include` ကဲ့သို့ပင် ဖိုင်ကို ရှာဖွေသည်။

spinit (၁၂.၅) သို့မဟုတ် .spiceinit (၁၂.၆) တွင် `set ngbehavior=ps` (၁၃.၇) ဖြင့် compatibility mode (၁၂.၁၄) ကို 'ps' ဟု သတ်မှတ်ပါက၊ ရိုးရှင်းသော syntax `.LIB filename` ကို ရရှိနိုင်သည်- သတိပေးချက်တစ်ခု ထုတ်ပေးပြီး filename ကို Chapt. 2.8 တွင်ဖော်ပြထားသည့်အတိုင်း ရိုးရိုး include လုပ်သည်။

## ၂.၁၁ .PARAM Parametric netlists

Ngspice သည် netlists များတွင် parametric attributes များကို သတ်မှတ်ခွင့်ပြုသည်။ ၎င်းသည် circuit description language သို့ arithmetic functionality ကို ပေါင်းထည့်ပေးသော ngspice front-end ၏ မြှင့်တင်ချက်တစ်ခုဖြစ်သည်။

### ၂.၁၁.၁ .param လိုင်း

**ယေဘူယျပုံစံ:**
```
.param <ident> = <expr> <ident> = <expr> ...
```
**ဥပမာများ:**
```
.param pippo=5
.param po=6 pp=7.8 pap={AGAUSS(pippo, 1, 1.67)}
.param pippp={pippo + pp}
.param p={pp}
.param pop='pp+p'
```
ဤလိုင်းသည် identifiers များသို့ ဂဏန်းတန်ဖိုးများ သတ်မှတ်ပေးသည်။ space ခြားသုံး၍ တစ်ကြောင်းတည်းတွင် assignment တစ်ခုထက်ပို၍ ပြုလုပ်နိုင်သည်။ Parameter identifier names များသည် alphabetic character ဖြင့် စတင်ရမည်။ အခြားအက္ခရာများသည် alphabetic, number, သို့မဟုတ် ! # $ % [ ] _ အထူးအက္ခရာများဖြစ်ရမည်။ variables `time`, `temper`, နှင့် `hertz` (၅.၁.၁ ကိုကြည့်ပါ) တို့သည် တရားဝင် identifier names များမဟုတ်ပါ။ အမည်ပေးခြင်းဆိုင်ရာ အခြားကန့်သတ်ချက်များလည်း သက်ရောက်သည်၊ ၂.၁၁.၆ ကိုကြည့်ပါ။

Subcircuits အတွင်းရှိ `.param` လိုင်းများကို အခြားလိုင်းများကဲ့သို့ပင် call တစ်ခုချင်းစီအတွက် ကူးယူသည်။ Assignment အားလုံးကို ချဲ့ထွင်ထားသော circuit တစ်လျှောက် အစဉ်လိုက် execute လုပ်သည်။ ၎င်း၏ ပထမဆုံးအသုံးမပြုမီ၊ parameter name တစ်ခုကို value တစ်ခု သတ်မှတ်ပေးထားရမည်။ parameter တစ်ခုကို သတ်မှတ်သည့် Expression များကို braces `{p + p2}` အတွင်း သို့မဟုတ် အခြားနည်းအနေဖြင့် single quotes `'AGAUSS(pippo, 1, 1.67)'` အတွင်း ထည့်သွင်းသင့်သည်။ Assignment တစ်ခုသည် self-referential မဖြစ်နိုင်ပါ၊ `.param pip = 'pip+3'` ကဲ့သို့သော အရာသည် အလုပ်မလုပ်ပါ။

လက်ရှိ ngspice version သည် expression များတွင် quotes သို့မဟုတ် braces များ အမြဲမလိုအပ်ပါ၊ အထူးသဖြင့် spaces များကို နည်းနည်းသာသုံးသောအခါ။ သို့သော် အောက်ပါဥပမာများက သရုပ်ပြသကဲ့သို့ ထည့်သွင်းရန် အကြံပြုပါသည်။

Parameters များသည် string values များလည်း ဖြစ်နိုင်သော်လည်း ထောက်ပံ့မှုမှာ ကန့်သတ်ထားသည်။ String-valued parameters များကို `.param` ဖြင့် သတ်မှတ်နိုင်ပြီး numeric parameters များကဲ့သို့ပင် အသုံးပြုနိုင်သည်။ String values များအတွက် တစ်ခုတည်းသော operation မှာ concatenation ဖြစ်ပြီး ၎င်းကို top-level `.param` assignments များတွင်သာ ပြုလုပ်နိုင်သည်။

### ၂.၁၁.၂ Circuit element များတွင် Brace expression များ

**ယေဘူယျပုံစံ:**
```
{<expr>}
```
**ဥပမာများ:**
ဤအရာများကို `.model` လိုင်းများနှင့် device lines များတွင် ခွင့်ပြုသည်။ SPICE number တစ်ခုသည် optional scaling suffix ပါသော floating point number တစ်ခုဖြစ်ပြီး၊ numeric tokens များနှင့် ချက်ချင်းကပ်လျက် ရှိရမည် (Chapt. 2.11.5 ကိုကြည့်ပါ)။ Brace expressions ( `{..}` ) များကို node names သို့မဟုတ် names ၏အစိတ်အပိုင်းများကို parameterize လုပ်ရန် မသုံးနိုင်ပါ။ `<expr>` အတွင်းသုံးထားသော identifiers အားလုံးသည် line ကို အကဲဖြတ်သည့်အချိန်တွင် သိထားသော value များ ရှိရမည်၊ မဟုတ်ပါက error အလံပြမည်။

### ၂.၁၁.၃ Subcircuit parameter များ

**ယေဘူယျပုံစံ:**
```
.subckt <identn> node node ... <ident>=<value> <ident>=<value> ...
```
**ဥပမာ:**
```
.subckt myfilter in out rval = 100k cval = 100nF
```
`<identn>` သည် user မှပေးသော subcircuit ၏အမည်ဖြစ်သည်။ `node` သည် external nodes များထဲမှ တစ်ခုအတွက် integer number သို့မဟုတ် identifier ဖြစ်သည်။ ပထမ `<ident>=<value>` သည် line ၏ optional section တစ်ခုကို စတင်သည်။ `<ident>` တစ်ခုချင်းစီသည် formal parameter တစ်ခုဖြစ်ပြီး၊ `<value>` တစ်ခုချင်းစီသည် SPICE number သို့မဟုတ် brace expression တစ်ခုဖြစ်သည်။ `.subckt ... .ends` context အတွင်းတွင်၊ formal parameter တစ်ခုချင်းစီကို `.param` control line ပေါ်တွင် သတ်မှတ်ထားသော identifier ကဲ့သို့ပင် အသုံးပြုနိုင်သည်။ `<value>` အပိုင်းများသည် parameter များ၏ default values များဖြစ်သည်။

Subcircuit call (invocation) တစ်ခု၏ syntax မှာ-

**ယေဘူယျပုံစံ:**
```
X<name> node node ... <identn> <ident>=<value> <ident>=<value> ...
```
**ဥပမာ:**
```
X1 input output myfilter rval = 1k
```
ဤနေရာတွင် `<name>` သည် subcircuit ၏ ထို instance အား ပေးထားသော symbolic name ဖြစ်ပြီး၊ `<identn>` သည် ကြိုတင်သတ်မှတ်ထားသော subcircuit တစ်ခု၏အမည်ဖြစ်သည်။ `node node ...` သည် subcircuit ချိတ်ဆက်မည့် actual nodes များ၏စာရင်းဖြစ်သည်။ `<value>` သည် SPICE number သို့မဟုတ် brace expression `{ <expr> }` ဖြစ်သည်။

Parameters ပါသော Subcircuit ဥပမာ-
```
* Param- example
.param amplitude = 1V
*
.subckt myfilter in out rval = 100k cval = 100nF
Ra in p1 {2*rval}
Rb p1 out {2*rval}
C1 p1 0 {2*cval}
Ca in p2 {cval}
Cb p2 out {cval}
R1 p2 0 {rval}
.ends myfilter
*
X1 input output myfilter rval = 1k cval = 1n
V1 input 0 AC {amplitude}
.end
```

### ၂.၁၁.၄ Symbol scope

Subcircuit နှင့် model names အားလုံးကို global ဟုသတ်မှတ်ပြီး unique ဖြစ်ရမည်။ မည်သည့် `.subckt ... .ends` section ၏အပြင်ဘက်တွင် သတ်မှတ်ထားသော `.param` symbols များသည် global ဖြစ်သည်။ ထို section အတွင်းတွင်၊ သက်ဆိုင်ရာ params: symbols များနှင့် `.param` assignments များအားလုံးကို local ဟုသတ်မှတ်သည်- ၎င်းတို့သည် `.ends` line ကိုမတွေ့မချင်း တူညီသော global နာမည်များကို mask လုပ်သည်။ `.subckt` တစ်ခုအတွင်းတွင် global number တစ်ခုသို့ reassign မလုပ်နိုင်ပါ၊ အစား local copy တစ်ခုကို ဖန်တီးသည်။ Scope nesting သည် level 10 အထိ အလုပ်လုပ်သည်။

### ၂.၁၁.၅ Expression များ၏ Syntax

`<expr>` (optional parts within `[ ]`)

Expression တစ်ခုသည် အောက်ပါတို့မှ တစ်ခုဖြစ်နိုင်သည်-
- `<atom>` (atom သည် spice number သို့မဟုတ် identifier ဖြစ်သည်)
- `<unary-operator> <atom>`
- `<function-name> (<expr> [, <expr> ...] )`
- `<atom> <binary-operator> <expr>`
- `( <expr> )`

မျှော်လင့်ထားသည့်အတိုင်း atoms, built-in function calls နှင့် parentheses အတွင်းရှိအရာများကို အခြား operator များထက် ဦးစားပေး၍ အကဲဖြတ်သည်။ Operators များကို C language နှင့်နီးစပ်သော precedence list အတိုင်း အကဲဖြတ်သည်။ Precedence တူညီသော binary ops များအတွက်၊ evaluation သည် ဘယ်မှညာသို့ သွားသည်။ Functions များသည် real values များပေါ်တွင်သာ လုပ်ဆောင်သည်-

| Operator       | Alias      | Precedence | Description               |
|----------------|------------|------------|---------------------------|
| -              |            | 1          | unary -                   |
| !              |            | 1          | unary not                 |
| **, ^          |            | 2          | power, like pwr           |
| *              |            | 3          | multiply                  |
| /              |            | 3          | divide                    |
| %              |            | 3          | modulo                    |
| \              |            | 3          | integer divide            |
| +              |            | 4          | add                       |
| -              |            | 4          | subtract                  |
| ==             |            | 5          | equality                  |
| !=, <>         |            | 5          | non-equal                 |
| <=             |            | 5          | less or equal             |
| >=             |            | 5          | greater or equal          |
| <              |            | 5          | less than                 |
| >              |            | 5          | greater than              |
| &&             |            | 6          | boolean and               |
| \|\|           |            | 7          | boolean or                |
| a?x:y          |            | 8          | ternary operator          |

Power functions `**` သို့မဟုတ် `^` တို့၏ evaluation သည် ရွေးချယ်ထားသော compatibility mode (၁၂.၁၄.၁) ပေါ်တွင်မူတည်သည်။

သုညကို boolean False ကိုကိုယ်စားပြုရန် အသုံးပြုပြီး၊ အခြားမည်သည့်နံပါတ်မဆို boolean True ကို ကိုယ်စားပြုသည်။ Logical operators များ၏ရလဒ်သည် 1 သို့မဟုတ် 0 ဖြစ်သည်။

Built-in functions များမှာ-
| Built-in function                | Notes                                                              |
|----------------------------------|--------------------------------------------------------------------|
| sqrt(x)                          | y = sqrt(x)                                                        |
| sin(x), cos(x), tan(x)           |                                                                    |
| sinh(x), cosh(x), tanh(x)        |                                                                    |
| asin(x), acos(x), atan(x)        |                                                                    |
| asinh(x), acosh(x), atanh(x)     |                                                                    |
| arctan(x)                        | atan(x), compatibility အတွက် သိမ်းထားသည်                          |
| exp(x)                           |                                                                    |
| ln(x), log(x)                    |                                                                    |
| abs(x)                           |                                                                    |
| nint(x)                          | အနီးဆုံး integer၊ half integers towards even                        |
| int(x)                           | 0 ဘက်သို့ rounded လုပ်ထားသော အနီးဆုံး integer                    |
| floor(x)                         | -∞ ဘက်သို့ rounded လုပ်ထားသော အနီးဆုံး integer                    |
| ceil(x)                          | +∞ ဘက်သို့ rounded လုပ်ထားသော အနီးဆုံး integer                    |
| pow(x,y)                         | x raised to the power of y (C runtime library မှ pow)              |
| pwr(x,y)                         | pow(fabs(x), y)                                                    |
| min(x,y)                         |                                                                    |
| max(x,y)                         |                                                                    |
| sgn(x)                           | x > 0 အတွက် 1.0၊ x == 0 အတွက် 0.0၊ x < 0 အတွက် -1.0                 |
| ternary_fcn(x,y,z)               | x ? y : z                                                          |
| gauss(nom, rvar, sigma)          | nominal value + Gaussian distribution (mean 0, std dev rvar relative to nom), sigma ဖြင့်စား |
| agauss(nom, avar, sigma)         | nominal value + Gaussian distribution (mean 0, std dev avar absolute), sigma ဖြင့်စား |
| unif(nom, rvar)                  | nominal value + relative variation uniformly distributed between +/-rvar |
| aunif(nom, avar)                 | nominal value + absolute variation uniformly distributed between +/-avar |
| limit(nom, avar)                 | random number in [-1, 1[ ပေါ်မူတည်၍ nominal value +/-avar         |

Scaling suffixes များ (မည်သည့် decorative alphanumeric string မဆို နောက်မှလိုက်နိုင်သည်):
| suffix | value  |
|--------|--------|
| g      | 1e9    |
| meg    | 1e6    |
| k      | 1e3    |
| m      | 1e-3   |
| u      | 1e-6   |
| n      | 1e-9   |
| p      | 1e-12  |
| f      | 1e-15  |

မှတ်ချက်- expression syntax တွင် တမင်သက်သက် redundancy များရှိသည်၊ ဥပမာ \(x^{\wedge}y\) \(x^{**}y\) နှင့် \(\mathrm{pwr}(x,y)\) တို့အားလုံးသည် ရလဒ်တူညီလုနီးပါးရှိသည်။

### ၂.၁၁.၆ Reserved word များ

အထက်ပါ function names များနှင့် verbose operators (not, and, or, div, mod) များအပြင် အခြားသော word များကို reserved လုပ်ထားပြီး parameter names များအဖြစ် မသုံးနိုင်ပါ- `or`, `defined`, `sqr`, `sqrt`, `sin`, `cos`, `exp`, `ln`, `log`, `log10`, `arctan`, `abs`, `pwr`, `time`, `temper`, `hertz`။

### ၂.၁၁.၇ Ngspice expression parser သုံးခုအကြောင်း သတိပေးချက်

Historical parameter notation ဖြစ်သော `&` ကို line ၏ပထမအက္ခရာအဖြစ် `.param.` နှင့်ညီမျှသုံးခြင်းမှာ deprecated ဖြစ်ပြီး လာမည့် release တွင် ဖယ်ရှားမည်။

Ngspice တွင် ၎င်း၏ multiple numerical expression features များကြောင့် confusion ဖြစ်နိုင်သည်။ `.param` lines နှင့် brace expressions (2.11.1 နှင့် 2.11.2 ကိုကြည့်ပါ) များကို front-end တွင် အကဲဖြတ်သည်၊ ဆိုလိုသည်မှာ subcircuit expansion ပြီးချိန်တွင် ဖြစ်သည်။ (နည်းပညာအရ၊ actual parameters များကို မှန်ကန်စွာ အစားထိုးနိုင်ရန်အတွက် X lines များကို expanded circuit တွင် comments များအဖြစ် ထိန်းသိမ်းထားသည်။) ထို့ကြောင့် netlist expansion ပြီးနောက်နှင့် internal data setup မတိုင်မီ circuit အတွင်းရှိ number attributes အားလုံးကို known constants များအဖြစ် သိရှိသည်။

သို့သော်၊ Spice တွင် ဤအချက်တွင်မဟုတ်ဘဲ circuit analysis အတွင်း၌သာ အကဲဖြတ်သည့် arithmetic expressions များကို လက်ခံသော circuit elements များရှိသည်။ ၎င်းတို့မှာ arbitrary current and voltage sources (B-sources, 5) အပြင် E- နှင့် G- sources နှင့် R- , L- , or C- devices များဖြစ်သည်။ Syntactic difference မှာ 'compile-time' expressions များသည် braces အတွင်း၌ရှိပြီး၊ 'run-time' expressions များတွင် braces မရှိပါ။ ပိုမိုရှုပ်ထွေးစေရန်၊ back-end ngspice scripting language သည် ၎င်း၏ကိုယ်ပိုင် scalar or vector data sets (13.2) များပေါ်တွင်သာ လုပ်ဆောင်သော arithmetic/logic expressions များကို လက်ခံသည်။ ကျေးဇူးပြု၍ Chapt. 2.16 ကိုကြည့်ပါ။

အထက်ဖော်ပြပါ contexts သုံးခုအတွက် တူညီသော expression syntax, operator and function set, and precedence rules ရှိစေရန်မှာ နှစ်လိုဖွယ်ဖြစ်သည်။ လက်ရှိ Numparam implementation တွင် ထိုရည်မှန်းချက် မအောင်မြင်သေးပါ။

## ၂.၁၂ .FUNC

ဤ keyword သည် function တစ်ခုကို သတ်မှတ်သည်။ Expression ၏ syntax သည် `.param` (၂.၁၁.၅) အတိုင်းဖြစ်သည်။

**ယေဘူယျပုံစံ:**
```
.func <ident> { <expr> }
.func <ident> = { <expr> }
```
**ဥပမာများ:**
```
.func icos(x) {cos(x) - 1}
.func f(x,y) {x*y}
.func foo(a,b) = {a + b}
```
`.func` သည် replacement operation တစ်ခုကို အစပြုလိမ့်မည်။ Input files များကိုဖတ်ပြီးနောက် parameters များမအကဲဖြတ်မီ၊ `icos(x)` function ၏ occurrences အားလုံးကို `cos(x)-1` ဖြင့် အစားထိုးမည်။ `f(x,y)` ၏ occurrences အားလုံးကို `x*y` ဖြင့် အစားထိုးမည်။ Function statements များကို t.b.d. အနက်အထိ nest လုပ်နိုင်သည်။

## ၂.၁၃ .CSPARAM

Plot (၁၃.၃) `const` ရှိ parameter တစ်ခုမှ constant vector (၁၃.၈.၂ ကိုကြည့်ပါ) တစ်ခုကို ဖန်တီးပါ။

**ယေဘူယျပုံစံ:**
```
.csparam <ident> = <expr>
```
**ဥပမာများ:**
```
.param pippo=5
.param pp=6
.csparam pippp={pippo + pp}
.param p = {pp}
.csparam pap='pp+p'
```
ပြထားသော ဥပမာတွင်၊ vectors `pippp` နှင့် `pap` တို့သည် plot `const` တွင် ရှိပြီးသား constants များထဲသို့ ထည့်သွင်းပြီး၊ length တစ် နှင့် real values များ ရှိသည်။ ဤ vectors များကို circuit parsing အတွင်း ထုတ်လုပ်သောကြောင့် နောက်ပိုင်းတွင် မပြောင်းလဲနိုင်ပါ (သာမန် parameters များကဲ့သို့ပင်)။ ၎င်းတို့ကို ngspice scripts နှင့် `.control` sections (Chapt. 13 ကိုကြည့်ပါ) များတွင် သုံးနိုင်သည်။

`.csparam` အသုံးပြုမှုသည် စမ်းသပ်ဆဲဖြစ်ပြီး စမ်းသပ်ရမည်။

## ၂.၁၄ .TEMP

Circuit temperature ကို degrees Celsius ဖြင့် သတ်မှတ်သည်။

**ယေဘူယျပုံစံ:**
```
.temp value
```
**ဥပမာ:**
```
.temp 27
```
ဤ card သည် `.option` line (၁၁.၁.၁) တွင် ပေးထားသော circuit temperature ကို override လုပ်သည်။

## ၂.၁၅ .IF Condition-Controlled Netlist

ရိုးရှင်းသော `.IF - .ELSE(IF)` block သည် netlist ကို condition-ဖြင့် ထိန်းချုပ်ခွင့်ပြုသည်။ `boolean expression` သည် parameters များကို အကဲဖြတ်ပြီး boolean 1 သို့မဟုတ် 0 ကို ပြန်ပေးသော Chapt. 2.11.5 အရ မည်သည့် expression မဆိုဖြစ်သည်။ `.if ... .endif` statements များကြားရှိ netlist block သည် logic condition အရ ရွေးချယ်ထားသော device instances သို့မဟုတ် `.model` cards များ ပါဝင်နိုင်သည်။

**ယေဘူယျပုံစံ:**
```
.if(boolean expression)
.elseif(boolean expression)
.else
.endif
```

**ဥပမာ ၁ (IF-ELSE block အတွင်းရှိ device instance):**
```
.param ok=0 ok2=1
v1 1 0 1
R1 1 0 2
.if (ok && ok2)
R11 1 0 2
.else
R11 1 0 0.5   <--- selected
.endif
```

**ဥပမာ ၂ (IF-ELSE block အတွင်းရှိ .model):**
```
.param m0=0 m1=1
M1 1 2 3 4 N1 W=1 L=0.5
.if(m0==1)
.model N1 NMOS level=49 Version=3.1
.elseif(m1==1)
.model N1 NMOS level=49 Version=3.2.4   <--- selected
.else
.model N1 NMOS level=49 Version=3.3.0
.endif
```

`.IF - .ELSE(IF) - .ENDIF` blocks များကို nesting လုပ်နိုင်သည်။ Block တစ်ခုလျှင် `.elseif` အများအပြား (သို့သော် `.else` တစ်ခုတည်းသာ) ခွင့်ပြုသည်။ သို့သော် အချို့ကန့်သတ်ချက်များ သက်ရောက်ပြီး၊ အောက်ပါ netlist components များကို `.IF- .ENDIF` block အတွင်းတွင် မထောက်ပံ့ပါ- `.SUBCKT`, `.INC`, `.LIB`, နှင့် `.PARAM`။

## ၂.၁၆ Parameter များ၊ function များ၊ expression များနှင့် command script များ

Ngspice တွင် functional dependencies များကို ဖော်ပြရန် နည်းလမ်းများစွာရှိသည်။ အမှန်တကယ်တွင် simulation ၏မတိုင်မီ၊ အတွင်းနှင့် အပြီးတွင် active ဖြစ်သော သီးခြား function parser သုံးခုရှိသည်။ ထို့ကြောင့် ၎င်းတို့၏ interdependence အကြောင်း စကားအနည်းငယ် ပြောသင့်သည်။

### ၂.၁၆.၁ Parameter များ

Parameters (Chapt. 2.11.1) နှင့် `.param` statement အတွင်း သို့မဟုတ် `.func` statement (Chapt. 2.12) ဖြင့် သတ်မှတ်ထားသော functions များကို simulation မစတင်မီ၊ ဆိုလိုသည်မှာ input နှင့် circuit setup လုပ်နေစဉ်အတွင်း အကဲဖြတ်သည်။ ထို့ကြောင့် ဤ statements များတွင် ၎င်းသည် ရိုးရှင်းစွာ မရရှိနိုင်သေးသောကြောင့် simulation output (voltage or current vectors) များ မပါဝင်နိုင်ပါ။ Syntax ကို Chapt. 2.11.5 တွင် ဖော်ပြထားသည်။ Circuit setup အတွင်း functions အားလုံးကို အကဲဖြတ်ပြီး၊ parameters အားလုံးကို ၎င်းတို့၏ ရလဒ် numerical values များဖြင့် အစားထိုးသည်။ ထို့ကြောင့် မည်သည့် parameter ကိုမဆို ပြောင်းလဲရန် နောက်ပိုင်းအဆင့် (simulation အတွင်း သို့မဟုတ် အပြီး) မှ feedback ရယူရန် မဖြစ်နိုင်ပါ။

### ၂.၁၆.၂ Nonlinear source များ

Simulation အတွင်း၊ B source (Chapt. 5) နှင့် ၎င်းတို့နှင့် ဆက်စပ်သော E နှင့် G sources များ၊ အပြင် အချို့ devices (R, C, L) များတွင် expressions များ ပါဝင်နိုင်သည်။ ဤ expressions များတွင် အထက်မှ parameters များ (ngspice စတင်ချိန်တွင် ချက်ခြင်းအကဲဖြတ်သည်)၊ numerical data၊ predefined functions များအပြင် simulation မှရလာသော node voltages နှင့် branch currents များလည်း ပါဝင်နိုင်သည်။ Source or device values များကို simulation အတွင်း စဉ်ဆက်မပြတ် update လုပ်သည်။ ထို့ကြောင့် sources များသည် non-linear behavior ကို သတ်မှတ်ရန် အစွမ်းထက်သော ကိရိယာများဖြစ်ပြီး၊ သင်ကိုယ်တိုင် 'devices' အသစ်များကိုပင် ဖန်တီးနိုင်သည်။ ကံမကောင်းစွာဖြင့် expression syntax (Chapt. 5.1 ကိုကြည့်ပါ) နှင့် predefined functions များသည် 2.11.1 တွင်ဖော်ပြထားသော parameters များအတွက် syntax နှင့် ကွဲလွဲနိုင်သည်။

### ၂.၁၆.၃ Control command များ၊ Command script များ

Commands များကို Chapt. 13.5 တွင် အသေးစိတ်ဖော်ပြထားသည့်အတိုင်း interactive သုံးနိုင်သလို၊ `.control ... .endc` lines များဖြင့် ဝန်းရံထားသော command script တစ်ခုအနေဖြင့်လည်း သုံးနိုင်သည်။ Scripts များတွင် expressions (Chapt. 13.2 ကိုကြည့်ပါ) ပါဝင်နိုင်သည်။ Expressions များသည် simulation output vectors (node voltages, branch currents) များအပြင် predefined or user defined vectors and variables များပေါ်တွင် လုပ်ဆောင်နိုင်ပြီး၊ simulation ပြီးနောက်မှ invoke လုပ်သည်။ `.param` statement ဖြင့် သတ်မှတ်ထားသော 2.11.1 မှ Parameters များကို ဤ expressions များတွင် ခွင့်မပြုပါ။ သို့သော် ထိုသို့သော parameters များကို `.csparam` (၂.၁၃) ဖြင့် သတ်မှတ်နိုင်သည်။ တဖန် expression syntax (Chapt. 13.2 ကိုကြည့်ပါ) သည် 2.11.1 နှင့် 5.1 တွင်ဖော်ပြထားသော parameters သို့မဟုတ် B sources များအတွက် syntax နှင့် ကွဲလွဲလိမ့်မည်။

အကယ်၍ သင်၏ control script အတွင်းတွင် 2.11.1 မှ parameters များကို အသုံးပြုလိုပါက၊ `.csparam` (၂.၁၃) ကိုသုံးနိုင်သည် သို့မဟုတ် parameter ကို ၎င်း၏ value အဖြစ်ရှိသော voltage source တစ်ခုသတ်မှတ်ကာ၊ ထို့နောက် ၎င်းကို (ဥပမာ transient simulation တစ်ခုပြီးနောက်) တည်ငြိမ်သော output (parameter) ဖြင့် vector တစ်ခုအဖြစ် ရရှိနိုင်သည်။ ဤနေရာမှ parameters (၂.၁၆.၁) သို့ feedback ပြန်လုပ်ရန် ဘယ်သောအခါမျှ မဖြစ်နိုင်ပါ။ ထို့အပြင် သင်သည် ယခင် simulation ၏ non-linear sources များကို ဝင်ရောက်မရယူနိုင်ပါ။

သို့သော်၊ သင်သည် သင်၏ control script အတွင်း ပထမဆုံး simulation တစ်ခုကို စတင်နိုင်ပြီး၊ expression များအသုံးပြု၍ ၎င်း၏ output ကို အကဲဖြတ်ကာ၊ `alter` နှင့် `altermod` statements (၁၃.၅.၃ ကိုကြည့်ပါ) ကိုသုံး၍ element သို့မဟုတ် model parameters အချို့ကို ပြောင်းလဲပြီး၊ ထို့နောက် simulation အသစ်တစ်ခုကို အလိုအလျောက် စတင်နိုင်သည်။

Expressions နှင့် scripting သည် ngspice အတွင်းရှိ အစွမ်းထက်သော ကိရိယာများဖြစ်ပြီး၊ ဤ features များကို ဖော်ပြရန် Chapt. 17 တွင် ပေးထားသော ဥပမာများကို ကျွန်ုပ်တို့ စဉ်ဆက်မပြတ် မြှင့်တင်သွားမည်။
