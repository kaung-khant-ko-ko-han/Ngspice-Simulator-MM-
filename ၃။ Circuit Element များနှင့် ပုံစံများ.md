# ၃။ Circuit Element များနှင့် ပုံစံများ

"ဤအပိုင်းတွင်ဖော်ပြထားသော '< >' အတွင်းရှိဒေတာfields များမှာ စိတ်ကြိုက်ရွေးချယ်နိုင်သည်။ ဖော်ပြထားသော ပုဒ်ဖြတ်ပုဒ်ရပ်များ (parentheses၊ equal signs စသည်ဖြင့်) အားလုံးသည် စိတ်ကြိုက်ဖြစ်သော်လည်း delimiter တစ်ခုခုရှိကြောင်း ဖော်ပြသည်။ ထို့အပြင် အနာဂတ်အကောင်အထည်ဖော်မှုများတွင် ဖော်ပြထားသည့်အတိုင်း ပုဒ်ဖြတ်ပုဒ်ရပ်များ လိုအပ်နိုင်သည်။ ဤနေရာတွင်ဖော်ပြထားသော ပုဒ်ဖြတ်ပုဒ်ရပ်များနှင့်အညီ တသမတ်တည်းပုံစံကျသော ရေးသားမှုသည် ထည့်သွင်းမှုကို နားလည်ရန်ပိုမိုလွယ်ကူစေသည်။ Branch voltages နှင့် currents များနှင့်ပတ်သက်၍ ngspice သည် ဆက်စပ်ရည်ညွှန်းချက်သဘောတူညီချက် (current သည် voltage drop ၏ direction အတိုင်း စီးဆင်းသည်) ကို တစ်သမတ်တည်း အသုံးပြုသည်။"

## ၃.၁ Netlist၊ device instance၊ model နှင့် model parameter များအကြောင်း

Ngspice သို့ ထည့်သွင်းမှုသည် netlist တစ်ခုဖြစ်ပြီး၊ ၎င်းသည် circuit elements အားလုံး၊ ၎င်းတို့၏ အပြန်အလှန်ချိတ်ဆက်မှုများနှင့် model parameters များကို စာရင်းပြုလုပ်ထားသည်။

ရိုးရှင်းသော bipolar amplifier တစ်ခု၏ Netlist ဥပမာ-
```spice
bipolar amplifier 
R3 vcc intc 10k 
R1 vcc intb 68k 
R2 intb 0 10k 
Cout out intc 10u 
Cin intb in 10u 
RLoad out 0 100k 
Q1 intc intb 0 BC546B 

VCC vcc 0 5 
Vin in 0 dc 0 ac 1 sin(0 1m 500) 

.model BC546B npn (IS=7.59E-15 VAF=73.4 BF=480 IKF=0.0962 
+ NE=1.2665 ISE=3.278E-15 IKR=0.03 ISC=2.00E-13 NC=1.2 NR=1 
+ BR=5 RC=0.25 CJC=6.33E-12 FC=0.5 MJC=0.33 VJC=0.65 
+ CJE=1.25E-11 MJE=0.55 VJE=0.65 TF=4.26E-10 ITF=0.6 VTF=3 
+ XTF=20 RB=100 IRB=0.0001 RBM=10 RE=0.5 TR=1.50E-07) 
.end
```
ပထမလိုင်း (အမြဲတမ်းခေါင်းစဉ်တစ်ခုသာ) ပြီးနောက်၊ netlist စတင်သည်။ ဤနေရာတွင် လိုင်းတစ်ခုချင်းစီသည် device instance တစ်ခုဖြစ်သည် (အစက် '.' ဖြင့်စတင်သော လိုင်းများမှလွဲ၍)။ ကျွန်ုပ်တို့တွင် တစ်ကြောင်းတည်းသာပါဝင်သော ရိုးရှင်းသည့် circuit elements များရှိသည်၊ ဥပမာ R3 ကဲ့သို့သော resistors များ။ ၎င်း၏ အရိုးရှင်းဆုံး အကောင်အထည်ဖော်မှုတွင်၊ resistor model သည် resistance value (Cout ကဲ့သို့ capacitors များအတွက်လည်း အလားတူ) မှလွဲ၍ မည်သည့် model parameters မျှ မလိုအပ်ပါ။ R3 vcc intc 10k ကဲ့သို့သော Netlist လိုင်းများကို instance lines ဟုခေါ်သည်၊ အကြောင်းမှာ လိုင်းတစ်ခုချင်းစီသည် ngspice simulator အတွင်းသို့ hard-coded လုပ်ထားသော (ဤနေရာတွင်- resistor) generic model တစ်ခု၏ instance တစ်ခုကို ကိုယ်စားပြုသောကြောင့်ဖြစ်သည်။ R3 သည် device name ကိုဖော်ပြသည်။ ၎င်း၏ ပထမဆုံးအက္ခရာ R သည် resistor ကိုဖော်ပြသည်။ နောက် tokens နှစ်ခုဖြစ်သော vcc intc သည် resistor ၏ node နှစ်ခုဖြစ်ပြီး၊ 10k သည် resistance value ဖြစ်သည်။ မတူညီသော devices များပေါ်တွင် တူညီသော node names များသည် ထို node များကြား ချိတ်ဆက်မှုကို ဖော်ပြသည်။

ပိုမိုရှုပ်ထွေးသော device ကို Q1 intc intb 0 BC546B instance ဖြင့် ဖော်ပြသည်။ Q သည် bipolar transistor ကိုဖော်ပြပြီး၊ intc intb 0 သည် collector၊ base၊ နှင့် emitter ဟူသော node သုံးခုဖြစ်သည်။ BC546B သည် model parameter set တစ်ခု၏ အမည်ဖြစ်ပြီး၊ တကယ့် transistor တစ်ခုကို အစွဲပြု၍ အမည်ပေးထားကာ (အကောင်အထည်ဖော်ထားသော bipolar transistor model နှင့်အတူ) ၎င်း၏လျှပ်စစ်အပြုအမူကို ဖော်ပြသည်။ ဆက်စပ် model parameters များကို .model BC546B npn (IS=7.59E-15 ...) လိုင်းတွင် ပေးထားသည်။ ၎င်းသည် instance line မဟုတ်ပါ၊ အကြောင်းမှာ အစက်ဖြင့်စတင်သောကြောင့်ဖြစ်သည်။ ၎င်းတွင် device ထုတ်လုပ်သူမှ သို့မဟုတ် လျှပ်စစ်အပြုအမူနှင့် data sheet မှ ထုတ်ယူထားသူများ (ဥပမာ သူ သို့မဟုတ် သူမ၏ web pages များတွင်တွေ့ရှိနိုင်သည်) မှ ပေးသော model parameters များပါဝင်သည်။ BC546B သည် model parameter set ၏အမည်ဖြစ်ပြီး ၎င်းကို device instance နှင့် ဆက်စပ်ပေးသည်။ npn သည် device အမျိုးအစားဖြစ်သည်။ Parameters များ (name=value) ကို brackets များဖြင့် ပေးထားသည်။

Q1... instance သည် model parameters များလိုအပ်သည်။ လျင်မြန်သော စမ်းသပ်မှုအတွက် device maker ၏ model parameters များမပါဘဲ လုပ်ဆောင်နိုင်သည်။

ရိုးရှင်းသော bipolar transistor instance နှင့် model parameter set-
```spice
Q1 intc intb 0 defaultmod
.model defaultmod npn
```
အထက်တွင်ဖော်ပြထားသည့်အတိုင်း bipolar transistor instance ကို ထည့်သွင်းပါက၊ သင်သည် ngspice မှ ပေးသော default model parameter set ကို အသုံးပြုသည်။ defaultmod သည် စိတ်ကြိုက်အမည်တစ်ခုဖြစ်သည်။ ဤလုပ်ထုံးလုပ်နည်းသည် စီးပွားဖြစ် device တစ်ခုခုနှင့် မတူသော generic bipolar transistor တစ်ခုကို model လုပ်သည်။ Default parameter values များကို shownod Q1 command ဖြင့် အကဲဖြတ်နိုင်သည်။

နောက်ဆက်တွဲအခန်းများ 3.3 မှ 12 တွင် devices၊ instances နှင့် models များအကြောင်း ပိုမိုသိရှိနိုင်မည်။

## ၃.၂ အထွေထွေ ရွေးချယ်မှုများ

### ၃.၂.၁ မြှောက်ကိန်း m ဖြင့် စက်ပစ္စည်းများကို ပြိုင်ဆက်ခြင်း

တူညီသောအမျိုးအစား စက်ပစ္စည်းများစွာကို parallel ဆက်ရန် လိုအပ်သောအခါ၊ ဇယား ၃.၁ တွင်ဖော်ပြထားသော စက်ပစ္စည်းများအတွက် ရရှိနိုင်သော 'm' (parallel multiplier) instance parameter ကို အသုံးပြုပါ။ ၎င်းသည် element ၏ matrix stamp ၏ value ကို m ၏ value ဖြင့် မြှောက်ပေးသည်။ အောက်ပါ netlist သည် parallel multiplier ကို မှန်ကန်စွာ အသုံးပြုပုံကို ပြသသည်-

Multiple device ဥပမာ-
```spice
d1 2 0 mydiode m=10
d01 1 0 mydiode
d02 1 0 mydiode
d03 1 0 mydiode
d04 1 0 mydiode
d05 1 0 mydiode
d06 1 0 mydiode
d07 1 0 mydiode
d08 1 0 mydiode
d09 1 0 mydiode
d10 1 0 mydiode
```
node 2 နှင့် 0 ကြားချိတ်ဆက်ထားသော d1 instance သည် node 1 နှင့် 0 ကြားချိတ်ဆက်ထားသော parallel devices ၁၀ ခု d01-d10 နှင့် ညီမျှသည်။

အောက်ပါစက်ပစ္စည်းများသည် multiplier m ကို ထောက်ပံ့သည်-

| ပထမစာလုံး | Element ဖော်ပြချက် |
|-----------|-------------------|
| C | Capacitor |
| D | Diode |
| F | Current-controlled current source (CCCS) |
| G | Voltage-controlled current source (VCCS) |
| I | Current source |
| J | Junction field effect transistor (JFET) |
| L | Inductor |
| M | Metal oxide field effect transistor (MOSFET) |
| Q | Bipolar junction transistor (BJT) |
| R | Resistor |
| X | Subcircuit (အသေးစိတ်အတွက် အောက်တွင်ကြည့်ပါ) |
| Z | Metal semiconductor field effect transistor (MESFET) |

X line (ဥပမာ x1 a b sub1 m=5) တွင် token m=value (ပြထားသည့်အတိုင်း) သို့မဟုတ် m=expression ပါဝင်သောအခါ၊ subcircuit invocation ကို အထူးနည်းလမ်းဖြင့် ပြုလုပ်သည်။ အကယ်၍ subcircuit sub1 ၏ instance line တစ်ခုတွင် ဇယား ၃.၁ တွင်ပြထားသော elements များတစ်ခုခုပါဝင်ပါက၊ ထို elements များကို နောက်ထပ် parameter m (ဤဥပမာတွင် value 5) ဖြင့် instantiate လုပ်သည်။ အကယ်၍ ထို element တွင် m multiplier parameter ရှိပြီးသားဖြစ်ပါက၊ element m ကို X line မှရသော m နှင့် မြှောက်သည်။ ၎င်းသည် recursive ဖြစ်သည်၊ ဆိုလိုသည်မှာ subcircuit တစ်ခုတွင် အခြား subcircuit တစ်ခု (nested X line) ပါဝင်ပါက၊ နောက် m parameter ကို ယခင် m နှင့် မြှောက်မည်၊ စသည်ဖြင့်။

ဥပမာ ၁-
```spice
.param madd = 6
X1 a b sub1 m=5
.subckt sub1 a1 b1
Cs1 a1 b1 C=5p m='madd-2'
.ends
```
ဥပမာ ၁ တွင်၊ node a နှင့် b အကြား capacitance သည် C = 5pF*(madd-2)*5 = 100pF ဖြစ်လိမ့်မည်။

ဥပမာ ၂-
```spice
.param madd = 4
X1 a b sub1 m=3
.subckt sub1 a1 b1
X2 a1 b1 sub2 m='madd-2'
.ends
.subckt sub2 a2 b2
Cs2 a2 b2 3p m=2
.ends
```
ဥပမာ ၂ တွင်၊ node a နှင့် b အကြား capacitance သည် C = 3pF*2*(madd-2)*3 = 36pF ဖြစ်လိမ့်မည်။

MOS transistors ကဲ့သို့သော တကယ့်စက်ပစ္စည်းများအတွက် ဂျီဩမေတြီဆိုင်ရာ ဂုဏ်သတ္တိများကို မှန်ကန်စွာဖော်ပြရန် m ကို အသုံးပြုခြင်းသည် ပျက်ကွက်နိုင်သည်။ M1 d g s nmos W=0.3u L=0.18u m=20 သည် M1 d g s nmos W=6u L=0.18u နှင့် တူညီမည်မဟုတ်ပါ၊ အကြောင်းမှာ ယခင် device သည် small width (or edge) effects များဒဏ်ခံရနိုင်ပြီး၊ နောက် device သည် ရိုးရှင်းစွာ wide transistor တစ်ခုဖြစ်သောကြောင့်ဖြစ်သည်။

### ၃.၂.၂ Instance နှင့် model parameter များ

အောက်ပါ ရိုးရှင်းသော device ဥပမာတွင် လိုင်းနှစ်ကြောင်းပါဝင်သည်- Lload ... ဖြင့်စတင်သော instance line တွင် device ကို သတ်မှတ်သည်- ပထမစာလုံးသည် device အမျိုးအစားကို ဆုံးဖြတ်ပေးသည် (ဤဥပမာတွင် inductor)။ Device name ၏နောက်တွင် node နှစ်ခုဖြစ်သော 1 နှင့် 2၊ ထို့နောက် inductance value 1u ကိုသတ်မှတ်သည်။ Model name ind1 သည် သက်ဆိုင်ရာ model line သို့ ချိတ်ဆက်မှုတစ်ခုဖြစ်သည်။ နောက်ဆုံးအနေဖြင့် instance line ပေါ်တွင် parameter တစ်ခုရှိပြီး ၎င်း၏ value dtemp=5 နှင့်အတူ။ Instance line တစ်ခုပေါ်ရှိ Parameters များကို instance parameters ဟုခေါ်သည်။

Model line သည် .model token ဖြင့်စတင်ပြီး၊ model name၊ model type နှင့် အနည်းဆုံး model parameter တစ်ခုဖြစ်သော tc1=0.001 တို့ပါဝင်သည်။ parameter ၁၀၀ ကျော်ပါသော ရှုပ်ထွေးသည့် models များရှိသည်။
```spice
Lload 1 2 1u ind1 dtemp=5
.MODEL ind1 L tc1=0.001
```
Instance parameters များကို အောက်ပါ device descriptions တစ်ခုချင်းစီတွင် ဖော်ပြထားသည်။ တစ်ခါတစ်ရံ Model parameters များကို BSIM transistor models ကဲ့သို့သော ရှုပ်ထွေးသော models များအတွက် အောက်တွင်လည်း ပေးထားသည်၊ ၎င်းတို့ကို model makers ၏ documentation တွင် ရရှိနိုင်သည်။ Instance parameters များကို .model line တွင်လည်း ထားရှိနိုင်သည်။ ထို့ကြောင့် ၎င်းတို့ကို ထို model ကိုရည်ညွှန်းသော device instance တစ်ခုချင်းစီမှ အသိအမှတ်ပြုသည်။ ၎င်းတို့၏ values များကို device တစ်ခု၏ specific instance အတွက် instance line ပေါ်တွင် ထပ်မံထည့်သွင်းခြင်းဖြင့် override လုပ်နိုင်သည်။

### ၃.၂.၃ Model binning

Binning သည် MOSFET ကဲ့သို့သော geometry dependent models များအတွက် range partitioning ကဲ့သို့သော အရာဖြစ်သည်။ ရည်ရွယ်ချက်မှာ model built-in geometry formulas ထက် ပိုမိုတိကျမှုဖြင့် ပိုမိုကျယ်ပြန့်သော geometry ranges (Width and Length) များကို cover လုပ်ရန်ဖြစ်သည်။ နောက်ထပ် model parameters LMIN, LMAX, WMIN and WMAX တို့ဖြင့် ဖော်ပြထားသော size range တစ်ခုချင်းစီတွင် ၎င်း၏ကိုယ်ပိုင် model parameter set ရှိသည်။ ဤ model cards များကို 'nch.1' ကဲ့သို့ number extension ဖြင့် သတ်မှတ်သည်။ ngspice တွင် တောင်းဆိုထားသော W နှင့် L ဖြင့် မှန်ကန်သော model card ကို ရွေးချယ်ရန် algorithm တစ်ခုရှိသည်။

၎င်းကို BSIM3 (7.6.3.3) နှင့် BSIM4 (7.6.3.4) models များအတွက် အကောင်အထည်ဖော်ထားသည်။

### ၃.၂.၄ Initial condition များ

အချို့သောစက်ပစ္စည်းများအတွက် initial conditions ပုံစံနှစ်မျိုး သတ်မှတ်နိုင်သည်။ ပထမပုံစံသည် stable state တစ်ခုထက်ပိုပါဝင်သော circuits များအတွက် dc convergence ကို ပိုမိုကောင်းမွန်စေရန် ထည့်သွင်းထားသည်။ အကယ်၍ device တစ်ခုကို OFF ဟုသတ်မှတ်ပါက၊ ထို device အတွက် terminal voltages များ သုညဟုသတ်မှတ်ကာ dc operating point ကို ဆုံးဖြတ်သည်။ Convergence ရရှိပြီးနောက်၊ terminal voltages များအတွက် တိကျသောတန်ဖိုးကို ရယူရန် ပရိုဂရမ်က ဆက်လက် iterate လုပ်သည်။ အကယ်၍ circuit တစ်ခုတွင် dc stable state တစ်ခုထက်ပိုပါက၊ OFF option ကို အသုံးပြု၍ solution ကို အလိုရှိသော state တစ်ခုနှင့် ကိုက်ညီစေရန် တွန်းအားပေးနိုင်သည်။ အကယ်၍ device တစ်ခုသည် အမှန်တကယ် conducting ဖြစ်နေချိန်တွင် OFF ဟုသတ်မှတ်ပါက၊ ပရိုဂရမ်သည် မှန်ကန်သော solution ကို ဆက်လက်ရရှိသည် (solutions များ converge ဖြစ်သည်ဟုယူဆရင်) သို့သော် ပရိုဂရမ်သည် သီးခြား solutions နှစ်ခုသို့ independently converge ဖြစ်ရမည်ဖြစ်သောကြောင့် ပိုမိုများပြားသော iterations များလိုအပ်သည်။

.NODESET control line (Chapt. 11.2.1 ကိုကြည့်ပါ) သည် OFF option နှင့် ဆင်တူသော ရည်ရွယ်ချက်ကို ဆောင်ရွက်သည်။ .NODESET option သည် အသုံးချရန် ပိုမိုလွယ်ကူပြီး convergence ကိုကူညီရန် ပိုမိုနှစ်သက်သော နည်းလမ်းဖြစ်သည်။ ဒုတိယပုံစံ initial conditions များကို transient analysis နှင့် အသုံးပြုရန် သတ်မှတ်သည်။ ၎င်းတို့သည် အထက်ဖော်ပြပါ convergence aids များနှင့် ဆန့်ကျင်ဘက်ဖြစ်သော 'true initial conditions' များဖြစ်သည်။ အသေးစိတ်ရှင်းပြချက်အတွက် .IC control line (Chapt. 11.2.2) နှင့် .TRAN control line (Chapt. 11.3.10) တို့၏ဖော်ပြချက်ကို ကြည့်ပါ။

## ၃.၃ Elementary Devices (အခြေခံ စက်ပစ္စည်းများ)

### ၃.၃.၁ Resistor များ

**ယေဘူယျပုံစံ-**
```
RXXXXXXX n+ n- <resistance|r>=value <ac=val> <m=val>
+ <scale=val> <temp=val> <dtemp=val> <tcl=val> <tc2=val>
+ <noisy=0|1>
```
**ဥပမာများ-**
```spice
R1 1 2 100
RC1 12 17 1K
R2 5 7 1K ac=2K
RL 1 4 2K m=2
```
Ngspice တွင် resistors များအတွက် အတော်အတန် ရှုပ်ထွေးသော model ရှိသည်။ ၎င်းသည် discrete နှင့် semiconductor resistors နှစ်မျိုးလုံးကို simulate လုပ်နိုင်သည်။ Ngspice တွင် semiconductor resistors ဆိုသည်မှာ- ဂျီဩမေတြီဆိုင်ရာ parameters များဖြင့် ဖော်ပြထားသော resistors များဖြစ်သည်။ ထို့ကြောင့် semiconductor effects များ၏အသေးစိတ် modeling ကို မမျှော်လင့်ပါနှင့်။

n+ နှင့် n- သည် element node နှစ်ခုဖြစ်ပြီး၊ value သည် resistance (ohms ဖြင့်) ဖြစ်ကာ positive သို့မဟုတ် negative ဖြစ်နိုင်သော်လည်း သုညမဖြစ်ရပါ။ အကယ်၍ value resistance 0 ကိုပေးပါက၊ ၎င်းကို 1e-12 သို့ အလိုအလျောက်သတ်မှတ်မည်။

တန်ဖိုးသေးငယ်သော resistors များကို simulating လုပ်ခြင်း- အကယ်၍ သင်သည် အလွန်သေးငယ်သော resistors (0.001 Ohm သို့မဟုတ် ထို့ထက်နည်း) ကို simulate လုပ်ရန်လိုအပ်ပါက၊ CCVS (transresistance) ကို အသုံးပြုသင့်သည်။ ၎င်းသည် ထိရောက်မှု နည်းသော်လည်း အလုံးစုံ numerical accuracy ကို ပိုမိုကောင်းမွန်စေသည်။ small resistance ကို large conductance တစ်ခုအဖြစ် ယူဆပါ။

Ngspice သည် resistor instance တစ်ခုအား AC analysis အတွက် မတူညီသော value တစ်ခု သတ်မှတ်နိုင်ပြီး ac keyword ကိုအသုံးပြု၍ သတ်မှတ်သည်။ ဤ value သည် အထက်တွင်ဖော်ပြထားသည့်အတိုင်း သုညမဖြစ်ရပါ။ AC resistance ကို AC analysis တွင်သာ (Pole-Zero နှင့် Noise မဟုတ်) အသုံးပြုသည်။ အကယ်၍ သင် ac parameter ကို မသတ်မှတ်ပါက၊ ၎င်းကို value အတိုင်း default လုပ်သည်။

Ngspice သည် nominal resistance ကို အောက်ပါအတိုင်း တွက်ချက်သည်-
\[
R_{nom} = \frac{\text{VALUE scale}}{m}
\]
\[
R_{acnom} = \frac{\text{ac scale}}{m}
\]

resistor တစ်ခု၏ temperature dependence ကို simulate လုပ်လိုပါက၊ .model line ကိုသုံး၍ သို့မဟုတ် instance parameters များအဖြစ်၊ အောက်ပါဥပမာများတွင်ကဲ့သို့ ၎င်း၏ temperature coefficients များကို သတ်မှတ်ရန် လိုအပ်သည်-

**ဥပမာများ-**
```spice
RE1 1 2 800 newres dtemp=5
.MODEL newres R tc1=0.001

RE2 a b 1.4k tc1=2m tc2=1.4u

RE3 n1 n2 1Meg tce=700m
```
temperature coefficients tc1 နှင့် tc2 တို့သည် resistance ၏ quadratic temperature dependence (equation 1.6 ကိုကြည့်ပါ) ကိုဖော်ပြသည်။ instance line (R... line) တွင်ပေးပါက ၎င်းတို့၏ values များသည် .model line (3.3.3) မှ tc1 နှင့် tc2 ကို override လုပ်လိမ့်မည်။ Ngspice တွင် model သို့မဟုတ် instance line တွင် ပေးထားသော tce ဖြင့် parameterized လုပ်ထားသည့် နောက်ထပ် temperature model equation 3.2 ရှိသည်။ parameters အားလုံး (quadratic and exponential) ပေးထားပါက exponential temperature model ကို ရွေးချယ်သည်။

\[
R(T) = R(T_0)\left[1.01^{TCE\cdot (T - T_0)}\right]
\]

ဤနေရာတွင် T သည် circuit temperature၊ \(T_0\) သည် nominal temperature၊ နှင့် TCE သည် exponential temperature coefficients ဖြစ်သည်။

resistance သည် temperature နှင့် မပြောင်းလဲသော်လည်း Instance temperature သည် အသုံးဝင်သည်၊ အကြောင်းမှာ resistor တစ်ခုမှထုတ်လွှတ်သော thermal noise သည် ၎င်း၏ absolute temperature ပေါ်တွင်မူတည်သောကြောင့်ဖြစ်သည်။ Ngspice ရှိ resistors များသည် thermal နှင့် flicker ဟူ၍ မတူညီသော noise နှစ်မျိုး ထုတ်လုပ်သည်။ Thermal noise ကို resistor တွင် အမြဲထုတ်လုပ်နေသော်လည်း၊ flicker noise source တစ်ခုထည့်ရန် flicker noise parameters များကို သတ်မှတ်ထားသော .model card တစ်ခု ထည့်ရမည်။ noisy (or noise) keyword ကို အသုံးပြု၍ ၎င်းကို သုညသတ်မှတ်ခြင်းဖြင့် မည်သည့် noise အမျိုးအစားကိုမျှ မထုတ်လုပ်သော resistors များကို simulate လုပ်နိုင်သည်၊ အောက်ပါဥပမာတွင်ကဲ့သို့-

**ဥပမာ-**
```spice
Rmd 134 57 1.5k noisy=0
```
အကယ်၍ သင်သည် temperature effects သို့မဟုတ် noise equations များကို စိတ်ဝင်စားပါက၊ semiconductor resistors ဆိုင်ရာ နောက်အပိုင်းကို ဖတ်ပါ။

### ၃.၃.၂ Semiconductor Resistor များ

**ယေဘူယျပုံစံ-**
```
RXXXXXXX n+ n- <value> <mname> <l=length> <w=width>
+ <temp=val> <dtemp=val> <m=val> <ac=val> <scale=val>
+ <noisy = 0|1>
```
**ဥပမာများ-**
```spice
RLOAD 2 10 10K
RMOD 3 7 RMODEL L=10u W=1u
```
၎င်းသည် ယခင် (၃.၃.၁) တွင်တင်ပြခဲ့သော resistor ၏ ပိုမိုယေဘူယျကျသည့်ပုံစံဖြစ်ပြီး temperature effects များကို modeling လုပ်ခြင်းနှင့် strictly geometric information နှင့် process ၏ specifications များမှ actual resistance value ကို တွက်ချက်ခြင်းတို့ကို ခွင့်ပြုသည်။ value ကို သတ်မှတ်ပါက၊ ၎င်းသည် geometric information ကို override လုပ်ပြီး resistance ကို သတ်မှတ်သည်။ mname ကို သတ်မှတ်ပါက၊ resistance ကို model mname ရှိ process information နှင့် ပေးထားသော length နှင့် width တို့မှ တွက်ချက်နိုင်သည်။ value ကို မသတ်မှတ်ပါက၊ mname နှင့် length ကို သတ်မှတ်ရမည်။ width ကို မသတ်မှတ်ပါက၊ model တွင်ပေးထားသော default width မှ ယူသည်။

(စိတ်ကြိုက်) temp value သည် ဤ device လည်ပတ်မည့် temperature ဖြစ်ပြီး၊ .option control line ပေါ်ရှိ temperature specification နှင့် dtemp တွင်သတ်မှတ်ထားသော value ကို override လုပ်သည်။

### ၃.၃.၃ Semiconductor Resistor Model (R)

Resistor model တွင် geometric information မှ resistance ကို တွက်ချက်ရန်နှင့် temperature အတွက် ပြင်ဆင်ရန် ခွင့်ပြုသော process-related device data များ ပါဝင်သည်။ ရရှိနိုင်သော parameters များမှာ အောက်ပါအတိုင်းဖြစ်သည်-

| အမည် | Parameter | ယူနစ် | Default | ဥပမာ |
|------|-----------|--------|---------|------|
| TC1 | first order temperature coeff. | Ω/°C | 0.0 | - |
| TC2 | second order temperature coeff. | Ω/°C² | 0.0 | - |
| RSH | sheet resistance | Ω/□ | - | 50 |
| DEFW | default width | m | 1e-6 | 2e-6 |
| NARROW | narrowing due to side etching | m | 0.0 | 1e-7 |
| SHORT | shortening due to side etching | m | 0.0 | 1e-7 |
| TNOM | parameter measurement temperature | °C | 27 | 50 |
| KF | flicker noise coefficient | - | 0.0 | 1e-25 |
| AF | flicker noise exponent | - | 1.0 | 1.0 |
| WF | flicker noise width exponent | - | 1.0 | - |
| LF | flicker noise length exponent | - | 1.0 | - |
| EF | flicker noise frequency exponent | - | 1.0 | - |
| R (RES) | default value if element value not given | Ω | - | 1000 |

Sheet resistance ကို narrowing parameter နှင့် resistor device မှ l နှင့် w တို့နှင့်အတူ အသုံးပြု၍ nominal resistance ကို အောက်ပါဖော်မြူလာဖြင့် ဆုံးဖြတ်သည်-

\[
R_{nom} = \text{rsh}\frac{l - \text{SHORT}}{w - \text{NARROW}}
\]

DEFW ကို device အတွက် w တစ်ခု မသတ်မှတ်ပါက default value တစ်ခု ပေးရန် အသုံးပြုသည်။ rsh သို့မဟုတ် l ကို မသတ်မှတ်ပါက၊ standard default resistance value 1 mOhm ကို အသုံးပြုသည်။ TNOM ကို ဤ model ၏ parameters များကို မတူညီသော temperature တွင် တိုင်းတာထားသည့် .options control line ပေါ်တွင် ပေးထားသော circuit-wide value ကို override လုပ်ရန် အသုံးပြုသည်။ Nominal resistance ကို တွက်ချက်ပြီးနောက်၊ ၎င်းကို temperature အတွက် အောက်ပါဖော်မြူလာဖြင့် ချိန်ညှိသည်-

\[
R(T) = R(\text{TNOM})\left(1 + TC_1(T - \text{TNOM}) + TC_2(T - \text{TNOM})^2\right)
\]

\(R(\text{TNOM}) = R_{nom}|R_{acnom}\) ။ အထက်ဖော်မြူလာတွင် \(T'\) သည် instance temperature ကိုကိုယ်စားပြုပြီး၊ ၎င်းကို temp keyword သုံး၍ အတိအလင်းသတ်မှတ်နိုင်သည် သို့မဟုတ် circuit temperature နှင့် dtemp (ရှိပါက) ကိုအသုံးပြု၍ တွက်ချက်နိုင်သည်။ temp နှင့် dtemp နှစ်ခုလုံးကို သတ်မှတ်ပါက၊ နောက်တစ်ခုကို လျစ်လျူရှုသည်။ Ngspice သည် SPICE ၏ resistors noise model ကို ပိုမိုကောင်းမွန်စေပြီး၊ ၎င်းတွင် flicker noise (\(1/f\)) နှင့် noiseless resistors များကို simulate လုပ်ရန် noisy (or noise) keyword တို့ကို ထည့်သွင်းထားသည်။ Resistors များတွင် thermal noise ကို အောက်ပါ equation အတိုင်း model လုပ်သည်-

\[
\bar{i}_R^2 = \frac{4kT}{R}\Delta f
\]

၎င်းတွင် \(k\) သည် Boltzmann's constant၊ နှင့် \(T'\) သည် instance temperature ဖြစ်သည်။

Flicker noise model မှာ-
\[
i_{Rfn}^2 = \frac{\text{KFI}_R^{\text{AF}}}{W^{\text{WF}}L^{\text{LF}}f^{\text{EF}}}\Delta f
\]

အောက်တွင် conductors များအတွက် sheet resistances (in \(\Omega/\square\)) စာရင်းအသေးကို ပြထားသည်။ ဤဇယားသည် 0.5 - 1 um range ရှိ MOS processes များအတွက် ပုံမှန်တန်ဖိုးများကို ကိုယ်စားပြုသည်။ ဇယားကို N. Weste, K. Eshraghian - Principles of CMOS VLSI Design 2nd Edition, Addison Wesley မှ ယူထားသည်။

| Material | Min. | Typ. | Max. |
|----------|------|------|------|
| Inter-metal (metal1 - metal2) | 0.005 | 0.007 | 0.1 |
| Top-metal (metal3) | 0.003 | 0.004 | 0.05 |
| Polysilicon (poly) | 15 | 20 | 30 |
| Silicide | 2 | 3 | 6 |
| Diffusion (n+, p+) | 10 | 25 | 100 |
| Silicided diffusion | 2 | 4 | 10 |
| n-well | 1000 | 2000 | 5000 |

### ၃.၃.၄ Expression များပေါ်မူတည်သော Resistor များ (behavioral resistor)

**ယေဘူယျပုံစံ-**
```
RXXXXXXX n+ n- R = 'expression' <tc1=value> <tc2=value> <noisy=0>
RXXXXXXX n+ n- 'expression' <tc1=value> <tc2=value> <noisy=O>
```
**ဥပမာများ-**
```spice
R1 rr 0 r = 'V(rr) < {Vt} ? {R0} : {2*R0}' tc1=2e-03 tc2=3.3e-06
R2 r2 rr r = {5k + 50*TEMPER}
.param rpl = 20
R3 no1 no2 r = '5k * rpl' noisy=1
```
Expression သည် equation တစ်ခု သို့မဟုတ် node voltages သို့မဟုတ် branch currents (i(vm) ပုံစံဖြင့်) များပါဝင်သော expression တစ်ခုဖြစ်နိုင်ပြီး B source အတွက်ပေးထားသော Chapt. 5.1 တွင်ဖော်ပြထားသည့် အခြား terms များလည်းပါဝင်နိုင်သည်။ ၎င်းတွင် parameters (2.11.1) နှင့် special variables time, temper, and hertz (5.1.2) တို့ ပါဝင်နိုင်သည်။ နမူနာဖိုင်ကို အောက်တွင်ဖော်ပြထားသည်။ resistor (11.3.4) တွင် small signal noise ကို resistance၊ temperature နှင့် tc1, tc2 တို့ပေါ်မူတည်၍ white noise အဖြစ် အကဲဖြတ်နိုင်သည်။ noise calculation ကို enable လုပ်ရန်၊ instance line တွင် noisy=1 flag ကိုထည့်ပါ။ default အနေဖြင့် behavioral resistor သည် noiseless ဖြစ်သည်။

Non-linear resistor အတွက် နမူနာ input file-
```spice
Non-linear resistor
.param R0 = 1k Vi=1 Vt=0.5
* resistor depending on control voltage V(rr)
R1 rr 0 r = 'V(rr) < {Vt} ? {R0} : {2*R0}'
* control voltage
V1 rr 0 PWL(0 0 100u {Vi})
.control
unset askquit
tran 100n 100u uic
plot i(V1)
.endc
.end
```

### ၃.၃.၅ Nonlinear r2_cmc သို့မဟုတ် r3_cmc ပုံစံများပါသော Resistor

CMC ၏ resistor subcommittee မှတီထွင်ထားသော 2-terminal resistor models များကို OpenVAF-compiled Verilog-A models (အသေးစိတ်အတွက် chapter 9.2 ကိုကြည့်ပါ) ကို load လုပ်ခြင်းဖြင့် OSDI interface မှတစ်ဆင့် ရရှိနိုင်သည်။ ရည်မှန်းချက်မှာ standard parameter names များနှင့် standard၊ numerically well behaved nonlinearity model ပါသော standard 2-terminal resistor model တစ်ခုရှိရန်ဖြစ်သည်။

### ၃.၃.၆ Capacitor များ

**ယေဘူယျပုံစံ-**
```
CXXXXXXX n+ n- <value> <mname> <m=val> <scale=val> <temp=val>
+ <dtemp=val> <tc1=val> <tc2=val> <ic=init_condition>
```
**ဥပမာများ-**
```spice
CBYP 13 0 1UF
COSC 17 23 10U IC=3V
```
Ngspice သည် capacitors များအတွက် အသေးစိတ် model တစ်ခုကို ထောက်ပံ့ပေးသည်။ Netlist ရှိ capacitors များကို ၎င်းတို့၏ capacitance သို့မဟုတ် geometric and physical characteristics များဖြင့် သတ်မှတ်နိုင်သည်။ မူလ SPICE3 'convention' ကိုလိုက်၍၊ geometric or physical characteristics များဖြင့် သတ်မှတ်ထားသော capacitors များကို 'semiconductor capacitors' ဟုခေါ်ပြီး နောက်အပိုင်းတွင် ဖော်ပြထားသည်။

ဤပထမပုံစံတွင် n+ နှင့် n- သည် positive နှင့် negative element nodes များဖြစ်ပြီး၊ value သည် capacitance in Farads ဖြစ်သည်။

Capacitance ကို အထက်ပါဥပမာများကဲ့သို့ instance line တွင် သို့မဟုတ် အောက်ပါဥပမာတွင်ကဲ့သို့ .model line တွင် သတ်မှတ်နိုင်သည်-
```spice
C1 15 5 cstd
C2 2 7 cstd
.model cstd C cap=3n
```
Capacitor နှစ်ခုလုံးသည် capacitance 3nF ရှိသည်။

capacitor တစ်ခု၏ temperature dependence ကို simulate လုပ်လိုပါက၊ .model line ကိုအသုံးပြု၍ ၎င်း၏ temperature coefficients များကို သတ်မှတ်ရန် လိုအပ်သည်၊ အောက်ပါဥပမာတွင်ကဲ့သို့-
```spice
CEB 1 2 1u cap1 dtemp=5
.MODEL cap1 C tc1=0.001
```
(စိတ်ကြိုက်) initial condition သည် capacitor voltage (in Volts) ၏ initial (time zero) value ဖြစ်သည်။ သတိပြုရန်- initial conditions (ရှိပါက) များသည် .tran control line ပေါ်တွင် uic option ကို သတ်မှတ်မှသာ သက်ရောက်သည်။

Ngspice သည် nominal capacitance ကို အောက်ပါအတိုင်း တွက်ချက်သည်-
\[
C_{nom} = \text{value} \cdot \text{scale} \cdot m
\]

temperature coefficients tc1 နှင့် tc2 တို့သည် capacitance ၏ quadratic temperature dependence (equation13.14 ကိုကြည့်ပါ) ကိုဖော်ပြသည်။ instance line (C... line) တွင်ပေးပါက ၎င်းတို့၏ values များသည် .model line (3.3.8) မှ tc1 နှင့် tc2 ကို override လုပ်လိမ့်မည်။

### ၃.၃.၇ Semiconductor Capacitor များ

**ယေဘူယျပုံစံ-**
```
CXXXXXX n+ n- <value> <mname> <l=length> <w=width> <m=val>
+ <scale=val> <temp=val> <dtemp=val> <ic=init_condition>
```
**ဥပမာများ-**
```spice
CLOAD 2 10 10P
CMD 3 7 CMDEL L=10u W=1u
```
၎င်းသည် section (3.3.6) တွင်တင်ပြခဲ့သော Capacitor ၏ ပိုမိုယေဘူယျကျသည့်ပုံစံဖြစ်ပြီး၊ strictly geometric information နှင့် process ၏ specifications များမှ actual capacitance value ကို တွက်ချက်ခွင့်ပြုသည်။ value ကို သတ်မှတ်ပါက၊ ၎င်းသည် capacitance ကိုသတ်မှတ်ပြီး process နှင့် geometrical information နှစ်ခုလုံးကို စွန့်ပစ်သည်။ value ကို မသတ်မှတ်ပါက၊ capacitance ကို model mname တွင်ပါဝင်သော information နှင့် ပေးထားသော length နှင့် width (l, w keywords အသီးသီး) တို့မှ တွက်ချက်သည်။

geometric dimensions မပါဘဲ mname ကိုသာ သတ်မှတ်နိုင်ပြီး .model line (3.3.6) တွင် capacitance ကိုသတ်မှတ်နိုင်သည်။

### ၃.၃.၈ Semiconductor Capacitor Model (C)

Capacitor model တွင် strictly geometric information မှ capacitance ကို တွက်ချက်ရန် အသုံးပြုနိုင်သော process information များပါဝင်သည်။

| အမည် | Parameter | ယူနစ် | Default | ဥပမာ |
|------|-----------|--------|---------|------|
| CAP | model capacitance | F | 0.0 | 1e-6 |
| CJ | junction bottom capacitance | F/m² | - | 5e-5 |
| CJSW | junction sidewall capacitance | F/m | - | 2e-11 |
| DEFW | default device width | m | 1e-6 | 2e-6 |
| DEFL | default device length | m | 0.0 | 1e-6 |
| NARROW | narrowing due to side etching | m | 0.0 | 1e-7 |
| SHORT | shortening due to side etching | m | 0.0 | 1e-7 |
| TC1 | first order temperature coeff. | F/°C | 0.0 | 0.001 |
| TC2 | second order temperature coeff. | F/°C² | 0.0 | 0.0001 |
| TNOM | parameter measurement temperature | °C | 27 | 50 |
| DI | relative dielectric constant | F/m | - | 1 |
| THICK | insulator thickness | m | 0.0 | 1e-9 |

Capacitor ၏ capacitance ကို အောက်ပါအတိုင်း တွက်ချက်သည်-

instance line တွင် value ကိုသတ်မှတ်ပါက
\[
C_{nom} = \text{value} \cdot \text{scale} \cdot m
\]

model capacitance ကိုသတ်မှတ်ပါက
\[
C_{nom} = \text{CAP} \cdot \text{scale} \cdot m
\]

value နှင့် CAP နှစ်ခုလုံးကို မသတ်မှတ်ပါက၊ geometrical and physical parameters များကို ထည့်သွင်းစဉ်းစားသည်-
\[
C_0 = \text{CJ}(l - \text{SHORT})(w - \text{NARROW}) + 2\text{CJSW}(l - \text{SHORT} + w - \text{NARROW})
\]

CJ ကို .model line တွင် အတိအလင်းပေးနိုင်သည် သို့မဟုတ် physical parameters များဖြင့် တွက်ချက်နိုင်သည်။ CJ ကို မပေးသောအခါ၊ အောက်ပါအတိုင်း တွက်ချက်သည်-

THICK သည် သုညမဟုတ်ပါက:
\[
\text{CJ} = \frac{\text{DI}\epsilon_0}{\text{THICK}} \quad \text{if DI is specified,}
\]
\[
\text{CJ} = \frac{\epsilon_{SiO_2}}{\text{THICK}} \quad \text{otherwise.}
\]

relative dielectric constant ကိုမသတ်မှတ်ပါက SiO2 အတွက်ကို အသုံးပြုသည်။ constants များ၏တန်ဖိုးများမှာ \(\epsilon_0 = 8.854214871e-12\frac{F}{m}\) နှင့် \(\epsilon_{SiO_2} = 3.4531479969e-11\frac{F}{m}\) ဖြစ်သည်။ Nominal capacitance ကို ထို့နောက် အောက်ပါအတိုင်း တွက်ချက်သည်-
\[
C_{nom} = C_0 \cdot \text{scale} \cdot m
\]

Nominal capacitance ကို တွက်ချက်ပြီးနောက်၊ ၎င်းကို temperature အတွက် အောက်ပါဖော်မြူလာဖြင့် ချိန်ညှိသည်-
\[
C(T) = C(\text{TNOM})\left(1 + TC_1(T - \text{TNOM}) + TC_2(T - \text{TNOM})^2\right)
\]
ဤနေရာတွင် \(C(\text{TNOM}) = C_{nom}\) ဖြစ်သည်။

အထက်ဖော်မြူလာတွင် \(T^{*}\) သည် instance temperature ကိုကိုယ်စားပြုပြီး၊ ၎င်းကို temp keyword သုံး၍ အတိအလင်းသတ်မှတ်နိုင်သည် သို့မဟုတ် circuit temperature နှင့် dtemp (ရှိပါက) ကိုအသုံးပြု၍ တွက်ချက်နိုင်သည်။

### ၃.၃.၉ Expression များပေါ်မူတည်သော Capacitor များ (behavioral capacitor)

Behavioral capacitors အတွက် ခွင့်ပြုထားသောပုံစံနှစ်မျိုးရှိသည်-
1. Capacitance ဖော်မြူလာသုံးထားသော expression \(\mathbf{C} =\) 'expression'
2. Charge ဖော်မြူလာသုံးထားသော expression \(\mathbf{Q} =\) 'expression'

**ယေဘူယျပုံစံ-**
```
CXXXXXXX n+ n- C = 'expression' <tcl=value> <tc2=value>
CXXXXXXX n+ n- 'expression' <tcl=value> <tc2=value>

CXXXXXXX n+ n- Q = 'expression' <tcl=value> <tc2=value>
```
**ဥပမာများ-**
```spice
C1 cc 0 c = 'V(cc) < {Vt} ? {C1} : {Ch} ' tc1=-1e-03 tc2=1.3e-05
C1 a b q = '1u*(4*atan(V(a,b)/4)*2+V(a,b))/3'
```
Expression သည် equation တစ်ခု သို့မဟုတ် node voltages သို့မဟုတ် branch currents (i(vm) ပုံစံဖြင့်) များပါဝင်သော expression တစ်ခုဖြစ်နိုင်ပြီး B source အတွက်ပေးထားသော Chapt. 5.1 တွင်ဖော်ပြထားသည့် အခြား terms များလည်းပါဝင်နိုင်သည်။ ၎င်းတွင် parameters (2.11.1) နှင့် special variables time, temper, and hertz (5.1.2) တို့ ပါဝင်နိုင်သည်။

နမူနာ input file-
```spice
Behavioral Capacitor
.param Cl=5n Ch=1n Vt=1m Il=100n
.ic v(cc) = 0 v(cc2) = 0
* capacitor depending on control voltage V(cc)
C1 cc 0 c = 'V(cc) < {Vt} ? {Cl} : {Ch}'
I1 0 1 {Il}
Exxx n1-copy n2 n2 cc2 1
Cxxx n1-copy n2 1
Bxxx cc2 n2 I = 'V(cc2) < {Vt} ? {Cl} : {Ch} '
* i(Exxx)
I2 n2 22 {Il}
vn2 n2 0 DC
* measure charge by integrating current
aint1 %id(1 cc) 2 time_count
aint2 %id(22 cc2) 3 time_count
.model time_count int(in_offset=0.0 gain=1.0
+ out_lower_limit=-1e12 out_upper_limit=1e12
+ limit_range=1e-9 out_ic=0.0)
.control
unset askquit
tran 100n 100u
plot v(2)
plot v(cc) v(cc2)
.endc
.end
```

### ၃.၃.၁၀ Inductor များ

**ယေဘူယျပုံစံ-**
```
LYYYYYY n+ n- <value> <mname> <nt=val> <m=val>
+ <scale=val> <temp=val> <dtemp=val> <tcl=val>
+ <tc2=val> <ic=init_condition>
```
**ဥပမာများ-**
```spice
LLINK 42 69 1UH
LSHUNT 23 51 10U IC=15.7MA
```
Ngspice အတွင်းသို့ အကောင်အထည်ဖော်ထားသော inductor device သည် မူရင်းထက် များစွာသော မြှင့်တင်ချက်များရှိသည်။ n+ နှင့် n- သည် positive နှင့် negative element nodes များအသီးသီးဖြစ်သည်။ value သည် inductance in Henry ဖြစ်သည်။ initial condition (L မှတစ်ဆင့် current) သည် .tran line ပေါ်တွင် uic parameter ကိုသတ်မှတ်မှသာ အကျိုးသက်ရောက်သည်။ Inductance ကို အထက်ပါဥပမာများကဲ့သို့ instance line တွင် သို့မဟုတ် အောက်ပါဥပမာတွင်ကဲ့သို့ .model line တွင် သတ်မှတ်နိုင်သည်-
```spice
L1 15 5 indmod1
L2 27 indmod1
.model indmod1 L ind=3n
```
Inductor နှစ်ခုလုံးသည် inductance 3nH ရှိသည်။

nt ကို .model line နှင့်တွဲဖက်၍ inductor ၏ number of turns ကိုသတ်မှတ်ရန် အသုံးပြုသည်။ inductor တစ်ခု၏ temperature dependence ကို simulate လုပ်လိုပါက၊ .model line ကိုအသုံးပြု၍ ၎င်း၏ temperature coefficients များကို သတ်မှတ်ရန် လိုအပ်သည်၊ အောက်ပါဥပမာတွင်ကဲ့သို့-
```spice
Lload 1 2 1u ind1 dtemp=5
.MODEL ind1 L tcl=0.001
```
(စိတ်ကြိုက်) initial condition သည် n+ မှ inductor မှတစ်ဆင့် n- သို့စီးဆင်းသော inductor current (in Amps) ၏ initial (time zero) value ဖြစ်သည်။ သတိပြုရန်- initial conditions (ရှိပါက) များသည် .TRAN analysis line ပေါ်တွင် UIC option ကို သတ်မှတ်မှသာ သက်ရောက်သည်။

Ngspice သည် nominal inductance ကို အောက်ပါအတိုင်း တွက်ချက်သည်-
\[
L_{nom} = \frac{\text{value scale}}{m}
\]

### ၃.၃.၁၁ Inductor ပုံစံ

Inductor model တွင် solenoids နှင့် toroids ကဲ့သို့သော၊ လေ သို့မဟုတ် constant magnetic permeability ရှိသောအခြားပစ္စည်းများတွင် အလုပ်လုပ်သော အချို့သော common topologies များ၏ inductance ကို တွက်ချက်ရန် အသုံးပြုနိုင်သော physical and geometrical information များပါဝင်သည်။

| အမည် | Parameter | ယူနစ် | Default | ဥပမာ |
|------|-----------|--------|---------|------|
| IND | model inductance | H | 0.0 | 1e-3 |
| CSECT | cross section | m² | 0.0 | 1e-6 |
| DIA | coil diameter | m | 0.0 | 1e-3 |
| LENGTH | length | m | 0.0 | 1e-2 |
| TC1 | first order temperature coeff. | H/°C | 0.0 | 0.001 |
| TC2 | second order temperature coeff. | H/°C² | 0.0 | 0.0001 |
| TNOM | parameter measurement temperature | °C | 27 | 50 |
| NT | number of turns | - | 0.0 | 10 |
| MU | relative magnetic permeability | - | 1.0 | - |

Inductor ၏ inductance ကို အောက်ပါအတိုင်း တွက်ချက်သည်-

instance line တွင် value ကိုသတ်မှတ်ပါက
\[
L_{nom} = \frac{\text{value scale}}{m}
\]

model inductance ကိုသတ်မှတ်ပါက
\[
L_{nom} = \frac{\text{IND scale}}{m}
\]

value နှင့် IND နှစ်ခုလုံးကို မသတ်မှတ်ပါက၊ geometrical and physical parameters များကို ထည့်သွင်းစဉ်းစားသည်။ အောက်ပါဖော်မြူလာများတွင် NT သည် instance နှင့် model parameter နှစ်ခုလုံးကို ရည်ညွှန်းသည် (instance parameter က model parameter ကို override လုပ်သည်)-

LENGTH သည် သုညမဟုတ်ပါက:
\[
\left\{
\begin{array}{ll}
L_{nom} = \frac{\text{MU}\mu_0\text{NT}^2\pi\text{DIA}^2}{4\text{LENGTH}} & \text{if DIA is specified,}\\
L_{nom} = \frac{\text{MU}\mu_0\text{NT}^2\text{CSEKT}}{\text{LENGTH}} & \text{otherwise.}
\end{array}
\right.
\]
\(\mu_0 = 1.25663706143592\frac{\mu H}{m}\) ဖြင့်။ DIA သည် CSECT ထက် ဦးစားပေးခံရသည်။ \(kl\) သည် Lundin (D. W. Knight, pp. 35-36 ကိုကြည့်ပါ) အရ geometry correction factor ဖြစ်ပြီး၊ inductor length နှင့် diameter တူညီသော order of magnitude ရှိသည့်အခါ အရေးကြီးသည်။ Nominal inductance ကို တွက်ချက်ပြီးနောက်၊ ၎င်းကို temperature အတွက် အောက်ပါဖော်မြူလာဖြင့် ချိန်ညှိသည်-
\[
L(T) = L(\text{TNOM})\left(1 + TC_1(T - \text{TNOM}) + TC_2(T - \text{TNOM})^2\right)
\]
ဤနေရာတွင် \(L(\text{TNOM}) = L_{nom}\) ဖြစ်သည်။ အထက်ဖော်မြူလာတွင် 'T' သည် instance temperature ကိုကိုယ်စားပြုပြီး၊ ၎င်းကို temp keyword သုံး၍ အတိအလင်းသတ်မှတ်နိုင်သည် သို့မဟုတ် circuit temperature နှင့် dtemp (ရှိပါက) ကိုအသုံးပြု၍ တွက်ချက်နိုင်သည်။

### ၃.၃.၁၂ Coupled (Mutual) Inductor များ

**ယေဘူယျပုံစံ-**
```
XXXXXXXX LYYYYYY LZZZZZZZ value
```
**ဥပမာများ-**
```spice
K43 LAA LBB 0.999
KXFRMR L1 L2 0.87
```
LYYYYYY နှင့် LZZZZZZZ တို့သည် coupled inductors နှစ်ခု၏အမည်များဖြစ်ပြီး၊ value သည် coefficient of coupling, K ဖြစ်ကာ၊ ၎င်းသည် 0 ထက်ကြီးပြီး 1 နှင့်ညီ သို့မဟုတ် ထက်နည်းရမည်။ coupled inductors များကိုဆွဲရန် 'dot' convention ကိုအသုံးပြု၍ inductor တစ်ခုချင်းစီ၏ ပထမ node တွင် 'dot' တစ်ခုထားပါ။ inductor နှစ်ခုထက်ပို၍ အပြန်အလှန်သက်ရောက်နေပါက၊ pairwise coupling ကို ထောက်ပံ့သည်။

inductor နှစ်ခုထက်ပိုသော Pairwise coupling-
```spice
L1 1 0 10u
L2 2 0 11u
L3 3 0 10u

K12 L1 L2 0.99
K23 L2 L3 0.99
K13 L1 L3 0.98
```
inductor နှစ်ခုထက်ပို၍ အပြန်အလှန်သက်ရောက်မှုအတွက် coupled လုပ်သောအခါ၊ coupling constants များ၏ ပေါင်းစပ်မှုအချို့သည် magnetic fields များက energy conservation ကိုချိုးဖောက်သောကြောင့် ရုပ်ပိုင်းဆိုင်ရာအရ မဖြစ်နိုင်ပါ။ ngspice သည် coupling matrix ကို ထိုသို့သော conditions များအတွက် စစ်ဆေးပြီး သတိပေးချက် ထုတ်ပြန်သည်။

K statement တစ်ခုတည်းတွင် inductor နှစ်ခုထက်ပို၍ coupling လုပ်ခြင်းကိုလည်း ထောက်ပံ့သည်။ Coupling constants အားလုံးသည် ထိုအခါ တူညီသည်။

statement တစ်ခုတည်းတွင် inductor နှစ်ခုထက်ပိုသော Coupling-
```spice
L1 1 0 10u
L2 2 0 11u
L3 3 0 10u

K123 L1 L2 L3 0.97
```

### ၃.၃.၁၃ Expression များပေါ်မူတည်သော Inductor များ (behavioral inductor)

**ယေဘူယျပုံစံ-**
```
LXXXXXX n+ n- L = 'expression' <tc1=value> <tc2=value>
LXXXXXX n+ n- 'expression' <tc1=value> <tc2=value>
```
**ဥပမာများ-**
```spice
L1 L2 LLL L = 'i(Vm) < {It} ? {L1} : {Lh}' tc1=-4e-03 tc2=6e-05
```
Expression သည် equation တစ်ခု သို့မဟုတ် node voltages သို့မဟုတ် branch currents (i(vm) ပုံစံဖြင့်) များပါဝင်သော expression တစ်ခုဖြစ်နိုင်ပြီး B source အတွက်ပေးထားသော Chapt. 5.1 တွင်ဖော်ပြထားသည့် အခြား terms များလည်းပါဝင်နိုင်သည်။ ၎င်းတွင် parameters (2.11.1) နှင့် special variables time, temper, and hertz (5.1.2) တို့ ပါဝင်နိုင်သည်။

နမူနာ input file-
```spice
Variable inductor
.param L1=0.5m Lh=5m It=50u Vi=2m
.ic v(int21) = 0

* variable inductor depending on control current i(Vm)
L1 l2 lll L = 'i(Vm) < {It} ? {Ll} : {Lh}'
* measure current through inductor
vm lll 0 dc 0
* voltage on inductor
V1 l2 0 {Vi}

* fixed inductor
L3 33 331 {Ll}
* measure current through inductor
vm33 331 0 dc 0
* voltage on inductor
V3 33 0 {Vi}

* non linear inductor (discrete setup)
F21 int21 0 B21 -1
L21 int21 0 1
B21 n1 n2 V = ' (i(Vm21) < {It} ? {Ll} : {Lh})' * v(int21)
* measure current through inductor
vm21 n2 0 dc 0
V21 n1 0 {Vi}

.control
unset askquit
tran 1u 100u uic
plot i(Vm) i(vm33)
plot i(vm21) i(vm33)
plot i(vm)- i(vm21)
.endc
.end
```

### ၃.၃.၁၄ Initial condition ပါသော Capacitor သို့မဟုတ် Inductor

Simulator သည် capacitor နှင့် inductor models များပေါ်တွင် voltage and current initial conditions များ သတ်မှတ်ခြင်းကို ထောက်ပံ့သည်။ ဤ models များသည် SPICE3 ဖြင့်ပေးသော standard models များမဟုတ်ဘဲ၊ realistic initial conditions လိုအပ်သည့်အခါ SPICE models များနေရာတွင် အစားထိုးနိုင်သော code models များဖြစ်သည်။ အသေးစိတ်အတွက် Chapter 8 ကို ကျေးဇူးပြု၍ ကိုးကားပါ။ ဤ models များကိုအသုံးပြုထားသော XSPICE deck နမူနာကို အောက်တွင်ပြထားသည်-
```spice

* This circuit contains a capacitor and an inductor with
* initial conditions on them. Each of the components
* has a parallel resistor so that an exponential decay
* of the initial condition occurs with a time constant of 1 second.

a1 1 0 cap
.model cap capacitive (c=1000uf ic=1)
r1 1 0 1k

a2 2 0 ind
.model ind inductor (l=1H ic=1)
r2 2 0 1.0

.control
tran 0.01 3
plot v(1) v(2)
.endc
.end
```

### ၃.၃.၁၅ Switch များ

Switch အမျိုးအစားနှစ်မျိုး ရရှိနိုင်သည်- voltage controlled switch (အမျိုးအစား SXXXXXX၊ model SW) နှင့် current controlled switch (အမျိုးအစား WXXXXXXX၊ model CSW)။ Switching hysteresis ကို သတ်မှတ်နိုင်သည့်အပြင် on- နှင့် off- resistances များ (\(0< R< \infty\)) ကိုလည်း သတ်မှတ်နိုင်သည်။

**ယေဘူယျပုံစံ-**
```
SXXXXXXX N+ N- NC+ NC- MODEL <ON><OFF>
WYYYYYYY N+ N- VNAM MODEL <ON><OFF>
```
**ဥပမာများ-**
```spice
s1 1 2 3 4 switch1 ON
s2 5 6 3 0 sm2 off
Switch1 1 2 10 0 smodel1
w1 1 2 vclock switchmod1
W2 3 0 vramp sm1 ON
wreset 5 6 vclck lossyswitch OFF
```
Node 1 နှင့် 2 သည် switch terminals များချိတ်ဆက်သည့် node များဖြစ်သည်။ Model name သည် မဖြစ်မနေ လိုအပ်ပြီး initial conditions များမှာ စိတ်ကြိုက်ဖြစ်သည်။ Voltage controlled switch အတွက်၊ node 3 နှင့် 4 သည် positive နှင့် negative controlling nodes များအသီးသီးဖြစ်သည်။ Current controlled switch အတွက်၊ controlling current သည် သတ်မှတ်ထားသော voltage source မှတစ်ဆင့် စီးဆင်းသော current ဖြစ်သည်။ positive controlling current flow ၏ direction သည် positive node မှ source မှတစ်ဆင့် negative node သို့ဖြစ်သည်။

Instance parameters ON သို့မဟုတ် OFF သည် controlling voltage (current) သည် hysteresis loop ၏ range အတွင်း (forward နှင့် backward voltage or current ramp အတွင်း မတူညီသော outputs) တွင် စတင်သည့်အခါ လိုအပ်သည်။ ထိုအခါ ON သို့မဟုတ် OFF သည် switch ၏ initial state ကို ဆုံးဖြတ်ပေးသည်။

### ၃.၃.၁၆ Switch Model (SW/CSW)

Switch model သည် ngspice တွင် almost ideal switch တစ်ခုကို ဖော်ပြခွင့်ပြုသည်။ Switch သည် resistance သည် 0 မှ infinity သို့ ချက်ခြင်းမပြောင်းလဲနိုင်ဘဲ၊ အမြဲတမ်း finite positive value ရှိရမည်ဖြစ်သောကြောင့် အတော်အသင့် ideal မဖြစ်ပါ။ on and off resistances များကို သင့်လျော်စွာ ရွေးချယ်ခြင်းဖြင့်၊ ၎င်းတို့သည် အခြား circuit elements များနှင့် နှိုင်းယှဉ်ပါက effectively zero and infinity ဖြစ်နိုင်သည်။ ရရှိနိုင်သော parameters များကို အောက်တွင်ပြထားသည်။

| အမည် | Parameter | ယူနစ် | Default | Switch model |
|------|-----------|--------|---------|--------------|
| VT | threshold voltage | V | 0.0 | SW |
| IT | threshold current | A | 0.0 | CSW |
| VH | hysteresis voltage | V | 0.0 | SW |
| IH | hysteresis current | A | 0.0 | CSW |
| RON | on resistance | Ω | 1.0 | SW, CSW |
| ROFF | off resistance | Ω | 1.0e+12 (*) | SW, CSW |

(*) သို့မဟုတ် \(1/GMIN\)၊ အကယ်၍ သင် \(GMIN\) ကို အခြားတန်ဖိုးတစ်ခုသို့ သတ်မှတ်ထားပါက၊ .OPTIONS control line (၁၁.၁.၂) တွင် \(GMIN\) ၏ဖော်ပြချက်ကိုကြည့်ပါ၊ ၎င်း၏ default value သည် 1.0e+12 ohms ၏ off-resistance ကိုဖြစ်ပေါ်စေသည်။

switch ကဲ့သို့ highly nonlinear ဖြစ်သော ideal element တစ်ခုကို အသုံးပြုခြင်းသည် circuit node voltages များတွင် large discontinuities များဖြစ်ပေါ်စေနိုင်သည်။ switch changing state နှင့်ဆက်စပ်သော rapid change သည် numerical round-off or tolerance problems များကိုဖြစ်စေပြီး erroneous results သို့မဟုတ် time step difficulties များဖြစ်ပေါ်နိုင်သည်။ switch အသုံးပြုသူသည် အောက်ပါအဆင့်များကိုလုပ်ဆောင်ခြင်းဖြင့် အခြေအနေကိုတိုးတက်စေနိုင်သည်-

ပထမ၊ ideal switch impedance ကို အခြား circuit elements များနှင့်စပ်လျဉ်း၍ negligible ဖြစ်ရုံမျှသာ high သို့မဟုတ် low ထားရန် ပညာရှိသည်။ ကိစ္စအားလုံးတွင် 'ideal' နှင့်နီးသော switch impedances များကိုအသုံးပြုခြင်းသည် အထက်တွင်ဖော်ပြခဲ့သော discontinuities များ၏ပြဿနာကို ပိုမိုဆိုးရွားစေသည်။ သေချာသည်မှာ၊ MOSFETS ကဲ့သို့သော real devices များကို modeling လုပ်သောအခါ၊ on resistance ကို modeling လုပ်နေသော device ၏ size ပေါ်မူတည်၍ realistic level တစ်ခုသို့ ချိန်ညှိသင့်သည်။

switches များတွင် ON to OFF resistance (ROFF/RON > 1e+12) ၏ ကျယ်ပြန့်သော range ကိုအသုံးပြုရမည်ဆိုပါက၊ .OPTIONS control line ကိုအသုံးပြု၍ TRTOL ကို default value 7.0 ထက်နည်းအောင် သတ်မှတ်ခြင်းဖြင့် transient analysis အတွင်း ခွင့်ပြုထားသော errors များအပေါ် tolerance ကို လျှော့ချသင့်သည်။

switches များကို capacitors များပတ်လည်တွင် ထားရှိသောအခါ၊ option CHGTOL ကိုလည်း လျှော့ချသင့်သည်။ ဤ options နှစ်ခုအတွက် အကြံပြုထားသောတန်ဖိုးများမှာ 1.0 နှင့် 1e-16 အသီးသီးဖြစ်သည်။ ဤပြောင်းလဲမှုများသည် circuit အတွင်း rapid change ကြောင့် အမှားများမဖြစ်စေရန် switch points များပတ်လည်တွင် ngspice ကို ပိုမိုဂရုစိုက်ရန် အသိပေးသည်။

နမူနာ input file-
```spice
Switch test
.tran 2us 5ms
*switch control voltage
v1 1 0 DC 0.0 PWL(0 0 2e-3 2 4e-3 0)
*switch control voltage starting inside hysteresis window
*please note influence of instance parameters ON, OFF
v2 2 0 DC 0.0 PWL(0 0.9 2e-3 2 4e-3 0.4)
*switch control current
i3 3 0 DC 0.0 PWL(0 0 2e-3 2m 4e-3 0) ; - - - - - - - - - - switch control current
*load voltage
v4 4 0 DC 2.0
*input load for current source i3
r3 3 33 10k
vm3 33 0 dc 0 ; - - - - - - - - - - - measure the current
*ouput load resistors
r10 4 10 10k
r20 4 20 10k
r30 4 30 10k
r40 4 40 10k
*
s1 10 0 1 0 switch1 OFF
s2 20 0 2 0 switch1 OFF
s3 30 0 2 0 switch1 ON
.model switch1 sw vt=1 vh=0.2 ron=1 roff=10k
*
w1 40 0 vm3 wswitch1 off
.model wswitch1 csw it=1m ih=0.2m ron=1 roff=10k
*
.control
run
plot v(1) v(10)
plot v(10) vs v(1) ; - - - - - - - - - - - - get hysteresis loop
plot v(2) v(20) ; - - - - - - - - - - - - different initial values
plot v(20) vs v(2) ; - - - - - - - - - - - - get hysteresis loop
plot v(2) v(30) ; - - - - - - - - - - - - different initial values
plot v(30) vs v(2) ; - - - - - - - - - - - - get hysteresis loop
plot v(40) vs vm3#branch ; - - - - - - - - - - - - current controlled switch hysteresis
.endc
.end
```
