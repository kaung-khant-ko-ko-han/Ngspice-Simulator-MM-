# ၁၁။ ခွဲခြမ်းစိတ်ဖြာမှုများနှင့် Output ထိန်းချုပ်ခြင်း (batch mode)

ဤအခန်းတွင်ဖော်ပြထားသော command line များကို circuit description file အတွင်းရှိ ခွဲခြမ်းစိတ်ဖြာမှုများနှင့် output များကို သတ်မှတ်ရန် အသုံးပြုသည်။ ၎င်းတို့သည် '.' (dot commands) ဖြင့် စတင်သည်။ Dot commands များအသုံးပြု၍ input file တွင် ခွဲခြမ်းစိတ်ဖြာမှုများနှင့် plot များ (သို့မဟုတ် table များ) ကို သတ်မှတ်ခြင်းကို batch runs များတွင် အသုံးပြုသည်။

## ၁၁.၁ Simulator Variables (.options)

Ngspice တွင်ရရှိနိုင်သော simulation များ၏ parameters အမျိုးမျိုးကို accuracy၊ speed သို့မဟုတ် default values များကို ထိန်းချုပ်ရန် ပြောင်းလဲနိုင်သည်။ ဤ parameters များကို `option` command (Chapt. 13.5.55 တွင် ဖော်ပြထားသည်) သို့မဟုတ် `.options` line မှတစ်ဆင့် ပြောင်းလဲနိုင်သည်-

**ယေဘူယျပုံစံ:**
```
.options opt1 opt2 ... (or opt=optval ...)
```
**ဥပမာ:**
```
.options reltol=.005 trtol=8
```
`.options` line သည် သတ်မှတ်ထားသော simulation ရည်ရွယ်ချက်များအတွက် program control နှင့် user options များကို ပြန်လည်သတ်မှတ်ရန် အသုံးပြုသူအား ခွင့်ပြုသည်။ `option` command (13.5.55 ကိုကြည့်ပါ) မှတစ်ဆင့် ngspice သို့ သတ်မှတ်ထားသော Options များကိုလည်း `.options` line တွင် သတ်မှတ်ထားသကဲ့သို့ လက်ဆင့်ကမ်းသည်။ အောက်ပါ options များ၏ မည်သည့်ပေါင်းစပ်မှုမဆို မည်သည့်အစဉ်အတိုင်းဖြစ်စေ ထည့်သွင်းနိုင်သည်။

### ၁၁.၁.၁ General Options

- **SPARSE** သည် Sparse 1.3 matrix solver ကိုရွေးချယ်ပြီး option မပေးသောအခါ standard လည်းဖြစ်သည်။ behavioural device models များကို simulation ပြုလုပ်ရန်ပိုမိုနှစ်သက်သည်။ ဤ option သည် noise (11.3.4) သို့မဟုတ် CIDER (26) simulation နှင့်အတူ လိုအပ်သည်။
- **KLU** သည် KLU matrix solver ကိုရွေးချယ်ပြီး MOS devices များပါဝင်သော (ကြီးမားသော) circuit များကို simulation ပြုလုပ်သည့်အခါ ပိုမိုနှစ်သက်သည် (ပိုမိုမြန်ဆန်သော simulation ရရှိသည်)။ Small signal noise (11.3.4) သို့မဟုတ် CIDER (26) simulations များကို (မရသေး) မထောက်ပံ့ပါ။
- **ACCT** သည် accounting နှင့် run time statistics များကို print ထုတ်ရန် ဖြစ်ပေါ်စေသည်။
- **NOACCT** သည် statistics များ print မထုတ်ရန်၊ Initial Transient Solution ကို print မထုတ်ရန်။
- **NOINIT** သည် Initial Transient Solution ကိုသာ print ထုတ်ခြင်းကို နှိမ်နင်းသည်၊ ACCT နှင့် ပေါင်းစပ်နိုင်သည်။
- **LIST** သည် input data ၏ summary listing ကို print ထုတ်ရန် ဖြစ်ပေါ်စေသည်။
- **NOMOD** သည် model parameters များ၏ printout ကို နှိမ်နင်းသည်။
- **NOPAGE** သည် page ejects များကို နှိမ်နင်းသည်။
- **NODE** သည် node table ၏ printing ကို ဖြစ်ပေါ်စေသည်။
- **NOREFVALUE** သည် ngspice ကို configure option `--enable-ndev` ဖြင့် compile လုပ်ထားသည့်အခါ reference values များ print ထုတ်ခြင်းကို နှိမ်နင်းသည်။
- **OPTS** သည် option values များကို print ထုတ်ရန် ဖြစ်ပေါ်စေသည်။
- **SEED=val|random** သည် random number generator ၏ seed value ကို သတ်မှတ်သည်။ val သည် 0 ထက်ကြီးသော မည်သည့် integer မဆိုဖြစ်နိုင်သည်။ အခြားနည်းလမ်းအနေဖြင့် random သည် seed value ကို လက်ရှိ Unix epoch time (1.1.1970 မှစ၍ leap seconds မပါ စက္ကန့်အရေအတွက်) သို့ သတ်မှတ်မည်။
- **SEEDINFO** သည် seed value ကို integer အသစ်တစ်ခုသို့ သတ်မှတ်သောအခါ ၎င်းကို print ထုတ်မည်။
- **TEMP=x** သည် circuit ၏ operating temperature ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 27°C (300K) ဖြစ်သည်။ TEMP ကို temperature dependent instance တစ်ခုခုပေါ်တွင် temperature specification ဖြင့် device အလိုက် override လုပ်နိုင်သည်။ .TEMP card (2.14) ဖြင့်လည်း ယေဘူယျအားဖြင့် override လုပ်နိုင်သည်။
- **TNOM=x** သည် device parameters များကို တိုင်းတာသည့် nominal temperature ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 27°C (300 deg K) ဖြစ်သည်။ TNOM ကို temperature dependent device model တစ်ခုခုပေါ်တွင် specification ဖြင့် override လုပ်နိုင်သည်။
- **WARN=1|0** သည် SOA (Safe Operating Area) voltage warning messages များကို enable သို့မဟုတ် turn off လုပ်သည် (default: 0)။
- **MAXWARNS=x** သည် model တစ်ခုလျှင် SOA warning messages အရေအတွက် အများဆုံးကို သတ်မှတ်သည် (default: 5)။
- **SAVECURRENTS** သည် အောက်ပါ devices များ၏ terminals အားလုံးမှတစ်ဆင့် currents များကို save လုပ်သည်- M, J, Q, D, R, C, L, B, F, G, W, S, I (2.3 ကိုကြည့်ပါ)။ သေးငယ်သော circuits များအတွက်သာ အကြံပြုသည်၊ အခြားသို့မဟုတ်ပါက memory requirements များ ပေါက်ကွဲပြီး simulation speed ထိခိုက်သည်။ အသေးစိတ်အတွက် 11.7 ကိုကြည့်ပါ။ ဤ option သည် op, dc, and tran simulation အတွက်သာ ရရှိနိုင်ပြီး ac အတွက်မရပါ။ Transient simulation အတွင်း return ပြန်လာသော value သည် time step တစ်ခုနောက်ကျနိုင်သည်။ M devices များအတွက် MOS level 1 ကို အပြည့်အဝထောက်ပံ့သော်လည်း၊ အခြား MOS devices များအတွက် node အားလုံးကို report မပြုပါ။

### ၁၁.၁.၂ OP and DC Solution Options

အောက်ပါ options များသည် DC and OP (operating point) ခွဲခြမ်းစိတ်ဖြာမှုများနှင့် algorithms များနှင့် သက်ဆိုင်သော properties များကို ထိန်းချုပ်သည်။ Transient analysis (11.1.4) သည် OP ကိုအခြေခံသောကြောင့် options အများအပြားသည် transient simulation ကိုလည်း သက်ရောက်သည်။ AC analysis (11.1.3) ကို stable operating point တစ်ခုတွေ့ရှိမှသာ လုပ်ဆောင်နိုင်သည်။

- **ABSTOL=x** သည် program ၏ absolute current error tolerance ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 1 pA ဖြစ်သည်။
- **GMIN=x** သည် program မှခွင့်ပြုသော minimum conductance ဖြစ်သည့် GMIN ၏ value ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 1.0e-12 ဖြစ်သည်။
- **GMINSTEPS=x** [*] သည် ကြိုးစားမည့် Gmin steps အရေအတွက်ကို သတ်မှတ်သည်။ value ကို သုညဟုသတ်မှတ်ပါက standard gmin stepping algorithm ကို ကျော်သွားသည်။ standard behavior မှာ source stepping algorithm သို့မသွားမီ gmin stepping ကို စမ်းသပ်သည်။
- **ITL1=x** သည် dc iteration limit ကို ပြန်လည်သတ်မှတ်သည်။ default မှာ 100 ဖြစ်သည်။
- **ITL2=x** သည် dc transfer curve iteration limit ကို ပြန်လည်သတ်မှတ်သည်။ default မှာ 50 ဖြစ်သည်။
- **KEEPOPINFO** သည် AC, Distortion, or Pole-Zero analysis တစ်ခုခုကို run သောအခါ operating point information ကို ဆက်ထိန်းထားသည်။ circuit သည် ကြီးမားပြီး (redundant) .OP analysis ကို မလုပ်ဆောင်လိုပါက ၎င်းသည် အထူးအသုံးဝင်သည်။
- **NOOPITER** သည် ပထမ iteration ကိုကျော်ပြီး gmin stepping သို့ တိုက်ရိုက်သွားသည်။
- **PIVREL=x** သည် largest column entry နှင့် acceptable pivot value အကြား relative ratio ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 1.0e-3 ဖြစ်သည်။
- **PIVTOL=x** သည် matrix entry တစ်ခုကို pivot အဖြစ် လက်ခံရန် absolute minimum value ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 1.0e-13 ဖြစ်သည်။
- **RELTOL=x** သည် program ၏ relative error tolerance ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 0.001 (0.1%) ဖြစ်သည်။
- **RSHUNT=x** သည် analog node တစ်ခုချင်းစီမှ ground သို့ resistor တစ်ခုစီကို ထည့်သွင်းသည်။ resistor ၏ value သည် circuit operations များကို မနှောင့်ယှက်နိုင်လောက်အောင် မြင့်သင့်သည်။ XSPICE option ကို enabled လုပ်ထားရမည် (28.1.8 ကိုကြည့်ပါ)။
- **VNTOL=x** သည် program ၏ absolute voltage error tolerance ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 1 µV ဖြစ်သည်။

#### ၁၁.၁.၂.၁ Matrix Conditioning info

SPICE-based simulators များတွင် specific circuit topologies များဖြင့် specific problems များ ပေါ်ပေါက်သည်။ ပြဿနာတစ်ခုမှာ node တစ်ခုခုတွင် ground သို့ DC path မရှိခြင်းဖြစ်သည်။ XSPICE ဖြင့် configure လုပ်ခြင်းသည် ဤပြဿနာကို ဖယ်ရှားရန် `rshunt` option ကို မိတ်ဆက်သည်။ တစ်နည်းအားဖြင့် `rshunt = 1.0e12` ထည့်ပါ။ မသင့်လျော်သော cases များတွင် value ကို 10GΩ သို့မဟုတ် 1GΩ အထိ လျှော့ချနိုင်သည်။

အခြား matrix conditioning problem တစ်ခုမှာ inductor တစ်ခုကို voltage source တစ်ခုနှင့် parallel ထားလျှင် ဖြစ်သည်။ AC simulation သည် OP analysis ဖြင့် ရှေ့သွားသောကြောင့် ကျရှုံးမည်။ circuit သည် linear ဖြစ်လျှင် NOOPAC (11.1.3) option က အကူအညီဖြစ်မည်။ သို့သော် circuit သည် non-linear ဖြစ်လျှင် OP analysis သည် essential ဖြစ်သည်။ ထိုကိစ္စတွင် inductor နှင့် series အနည်းငယ် resistor (ဥပမာ 0.1mΩ) ထည့်ခြင်းသည် convergence ရရှိရန် အကူအညီဖြစ်မည်။

- `.option rseries = 1.0e-4` သည် circuit ရှိ inductor တစ်ခုချင်းစီသို့ series resistor တစ်ခုထည့်သည်။ behavioral inductors (3.3.13 ကိုကြည့်ပါ) ကိုအသုံးပြုသည့်အခါ ရလဒ်သည် ခန့်မှန်းမရနိုင်သောကြောင့် သတိထားပါ။
- `.option cshunt = 1.3e-13` သည် circuit ရှိ voltage node တစ်ခုချင်းစီမှ ground သို့ capacitor တစ်ခုထည့်သည်။

### ၁၁.၁.၃ AC Solution Options

- **NOOPAC** သည် AC analysis မတိုင်မီ operating point (OP) analysis ကို မလုပ်ဆောင်ပါ။ ဤ option သည် circuit သည် linear (R, L, C devices, independent V, I sources နှင့် linear dependent E, G, H, F sources များသာပါဝင်သည်၊ poly statement, non-behavioral မပါ) ဖြစ်ရန် လိုအပ်သည်။ Non-linear device တစ်ခုတွေ့ရှိပါက OP analysis ကို အလိုအလျောက် လုပ်ဆောင်သည်။ L devices များအတွက် series resistance မရှိသော nested LC circuits များတွင် စိတ်ဝင်စားဖွယ်ဖြစ်သည်။ OP analysis အတွင်း ill-formed matrix တစ်ခု တွေ့ကြုံရနိုင်ပြီး simulator ကို error message ဖြင့် abort ဖြစ်စေသည်။ အလွန်ကြီးမားသော linear arrays များ (node 10000 နှင့်အထက်) ရှိပါက simulation speedup 10 ဆအထိ ရရှိနိုင်သည်။

### ၁၁.၁.၄ Transient Analysis Options

- **AUTOSTOP** သည် dot command `.meas` ဖြင့် သတ်မှတ်ထားသော functions (11.4) အားလုံးကို အောင်မြင်စွာ တွက်ချက်ပြီးနောက် transient analysis ကို ရပ်တန့်သည်။ Autostop သည် control mode တွင်အသုံးပြုသော `meas` (13.5.50) command ဖြင့် မရရှိနိုင်ပါ။
- **CHGTOL=x** သည် program ၏ charge tolerance ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 1.0e-14 ဖြစ်သည်။
- **CONVSTEP=x** သည် code models များသို့ သက်ရောက်သော relative step limit ဖြစ်သည်။
- **CONVABSSTEP=x** သည် code models များသို့ သက်ရောက်သော absolute step limit ဖြစ်သည်။
- **INTERP** သည် TSTEP grid (11.3.10) ပေါ်တွင် fixed time steps များအဖြစ် output data ကို interpolate လုပ်သည်။ ယခင် နှင့် နောက် time values များကြား linear interpolation ကို အသုံးပြုသည်။ Simulation ကိုယ်တိုင်က ဤ option ကြောင့် လွှမ်းမိုးမှုမရှိပါ။
- **ITL3=x** သည် lower transient analysis iteration limit ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 4 ဖြစ်သည်။ (မှတ်ချက်- Spice3 တွင် အကောင်အထည်မဖော်ပါ)။
- **ITL4=x** သည် transient analysis time-point iteration limit ကို ပြန်လည်သတ်မှတ်သည်။ default မှာ 10 ဖြစ်သည်။
- **ITL5=x** သည် transient analysis total iteration limit ကို ပြန်လည်သတ်မှတ်သည်။ default မှာ 5000 ဖြစ်သည်။
- ဤစစ်ဆေးမှုကိုချန်လှပ်ရန် ITL5=0 သတ်မှတ်ပါ။ (မှတ်ချက်- Spice3 တွင် အကောင်အထည်မဖော်ပါ)။
- **ITL6=x** [*] သည် SRCSTEPS အတွက် အဓိပ္ပါယ်တူစကားလုံးဖြစ်သည်။
- **MAXEVTITER=x** သည် analysis point တစ်ခုလျှင် event iterations အများဆုံးအရေအတွက်ကို သတ်မှတ်သည်။
- **MAXOPALTER=x** သည် hybrid circuit တစ်ခုကိုဖြေရှင်းရန် simulator က အသုံးပြုမည့် analog/event alternations အများဆုံးအရေအတွက်ကို သတ်မှတ်သည်။
- **MAXORD=x** [*] သည် SPICE မှအသုံးပြုသော numerical integration method အတွက် maximum order ကို သတ်မှတ်သည်။ Gear method အတွက် ဖြစ်နိုင်သောတန်ဖိုးများမှာ 2 (default) မှ 6 အထိဖြစ်သည်။ trapezoidal method နှင့် value 1 ကိုအသုံးပြုပါက backward Euler integration ကို သတ်မှတ်သည်။
- **METHOD=name** သည် SPICE မှအသုံးပြုသော numerical integration method ကို သတ်မှတ်သည်။ ဖြစ်နိုင်သောအမည်များမှာ 'Gear' သို့မဟုတ် 'trapezoidal' (သို့မဟုတ် 'trap') ဖြစ်သည်။ default မှာ trapezoidal ဖြစ်သည်။
- **NOOPALTER=TRUE|FALSE** သည် false ဟုသတ်မှတ်ပါက initial DC operating analysis အတွင်း XSPICE models များသို့ analog နှင့် event calls များကြား alternations များကို enable လုပ်သည်။
- **RAMPTIME=x** သည် source stepping အတွင်း independent supplies များ၏ ပြောင်းလဲမှုနှုန်းကို သတ်မှတ်သည်။ initial conditions သတ်မှတ်ထားသော code model inductors နှင့် capacitors များကိုလည်း သက်ရောက်သည်။
- **SRCSTEPS=x** [*] သည် non-zero value ဖြစ်ပါက SPICE သည် DC operating point ကိုရှာဖွေရန် source-stepping method ကို အသုံးပြုစေသည်။ value သည် steps အရေအတွက်ကို သတ်မှတ်သည်။
- **TRTOL=x** သည် transient error tolerance ကို ပြန်လည်သတ်မှတ်သည်။ default value မှာ 7 ဖြစ်သည်။ XSPICE ကို configure လုပ်ထားပြီး 'A' devices များပါဝင်ပါက value ကို ပိုမိုတိကျမှုအတွက် internally 1 ဟုသတ်မှတ်သည်။ ၎င်းသည် transient analysis ကို factor of two နှေးစေသည်။
- **XMU=x** သည် trapezoidal integration အတွက် damping factor ကို သတ်မှတ်သည်။ default value မှာ XMU=0.5 ဖြစ်သည်။

### ၁၁.၁.၅ ELEMENT Specific options

- **diode_cj0=x** သည် .model statement တွင် မည်သည့် capacitance မျှသတ်မှတ်မထားပါက optional diode junction capacitance ကို ထည့်ပေးသည်။
- **diode_rser=x** သည် .model statement တွင် မည်သည့် series resistance မျှသတ်မှတ်မထားပါက optional diode series resistance ကို ထည့်ပေးသည်။
- **BADMOS3** သည် 'kappa' discontinuity ပါသော MOS3 model ၏ ဗားရှင်းအဟောင်းကို အသုံးပြုသည်။
- **DEFAD=x**, **DEFAS=x**, **DEFL=x**, **DEFW=x** တို့သည် MOSFET parameters များ၏ default values များကို ပြန်လည်သတ်မှတ်သည်။
- **SCALE=x** သည် unit meters ရှိသော geometric element parameters များအတွက် element scaling factor ကို သတ်မှတ်သည်။

### ၁၁.၁.၆ Transmission Lines Specific Options

- **TRYTOCOMPACT** သည် LTRA model (6.2.1 ကိုကြည့်ပါ) နှင့်သာ သက်ဆိုင်သည်။ သတ်မှတ်ပါက simulator သည် LTRA transmission line ၏ အတိတ် input voltages နှင့် currents များ၏ history ကို condense လုပ်ရန် ကြိုးစားသည်။

### ၁၁.၁.၇ option နှင့် .options commands များ၏ အရင်အဆင့်

Ngspice တွင် simulator variables များကို နည်းလမ်းမျိုးစုံဖြင့် သတ်မှတ်နိုင်သည်။ `option` command (13.5.55) ဖြင့် spinit သို့မဟုတ် .spiceinit ဖိုင်များတွင် သတ်မှတ်ပါက default values များကို အစားထိုးမည်။ input file ရှိ `.options` line ဖြင့် သတ်မှတ်ပါက default နှင့် init file data များကို အစားထိုးမည်။ `.control ... .endc` section အတွင်းတွင် options များသတ်မှတ်ပါက ယခင်ပေးထားသမျှ simulator variables များကို ထပ်မံအစားထိုးမည်။

## ၁၁.၂ Initial Conditions

### ၁၁.၂.၁ .NODESET- Initial Node Voltage Guesses သတ်မှတ်ခြင်း

**ယေဘူယျပုံစံ:**
```
.nodeset v(nodnum)=val v(nodnum)=val ...
.nodeset all=val
```
**ဥပမာ:**
```
.nodeset v(12)=4.5 v(4)=2.23
.nodeset all=1.5
```
`.nodeset` line သည် program က DC သို့မဟုတ် initial transient solution ကိုရှာဖွေရာတွင် သတ်မှတ်ထားသော nodes များကို ပေးထားသော voltages များဖြင့် ထိန်းထားကာ ပဏာမ pass တစ်ခုပြုလုပ်ခြင်းဖြင့် အကူအညီပေးသည်။ ထို့နောက် ကန့်သတ်ချက်များကို ဖြေလွှတ်ပြီး iteration သည် true solution သို့ ဆက်လက်လုပ်ဆောင်သည်။ `.nodeset` line သည် bistable or astable circuits များတွင် convergence အတွက် လိုအပ်နိုင်သည်။ `.nodeset all=val` သည် ground node မှလွဲ၍ စတင်သည့် node voltages အားလုံးကို တူညီသော value သို့ သတ်မှတ်သည်။ ယေဘူယျအားဖြင့် `.nodeset` line မလိုအပ်ပါ။

### ၁၁.၂.၂ .IC- Initial Conditions သတ်မှတ်ခြင်း

**ယေဘူယျပုံစံ:**
```
.ic v(nodnum)=val v(nodnum)=val ...
```
**ဥပမာ:**
```
.ic v(11)=5 v(4)=-5 v(2)=2.2
```
`.ic` line သည် transient initial conditions များသတ်မှတ်ရန် ဖြစ်သည်။ ၎င်းတွင် `.tran` control line ပေါ်တွင် `uic` parameter ကို သတ်မှတ်သည်/မသတ်မှတ်သည်ပေါ်မူတည်၍ အနက်ပြန်ဆိုချက် နှစ်မျိုးရှိသည်။

1. `.tran` line ပေါ်တွင် `uic` parameter ကို သတ်မှတ်ထားသည့်အခါ- `.ic` control line တွင်သတ်မှတ်ထားသော node voltages များကို capacitor, diode, BJT, JFET, နှင့် MOSFET initial conditions များကို တွက်ချက်ရန် အသုံးပြုသည်။ device line တစ်ခုချင်းစီပေါ်တွင် `ic=...` parameter ကို သတ်မှတ်ခြင်းနှင့် ညီမျှသော်လည်း ပိုမိုအဆင်ပြေသည်။ device initial conditions များတွက်ချက်ရန် အသုံးပြုမည်ဆိုပါက `.ic` control line တွင် dc source voltages အားလုံးကို သတ်မှတ်ရန် ဂရုစိုက်သင့်သည်။

2. `.tran` control line ပေါ်တွင် `uic` parameter ကို မသတ်မှတ်ထားသည့်အခါ- transient analysis မတိုင်မီ DC bias (initial transient) solution ကို တွက်ချက်သည်။ ဤကိစ္စတွင် `.ic` control lines ပေါ်တွင် သတ်မှတ်ထားသော node voltages များကို bias solution အတွင်း အလိုရှိသော initial values များအဖြစ် forced လုပ်သည်။ Transient analysis အတွင်း ဤ node voltages များအပေါ် ကန့်သတ်ချက်ကို ဖယ်ရှားသည်။ ၎င်းသည် Ngspice က consistent DC solution တစ်ခုတွက်ချက်ရန် ခွင့်ပြုသောကြောင့် ပိုမိုနှစ်သက်သော နည်းလမ်းဖြစ်သည်။

`.ic` format ဖြင့် node voltages များကို `.include` ဖြင့် ပြန်လည်ထည့်သွင်းနိုင်ရန် `wrnodev` command 13.5.108 က သိမ်းဆည်းပေးသည်။

## ၁၁.၃ Analyses

### ၁၁.၃.၁ .AC- Small-Signal AC Analysis

**ယေဘူယျပုံစံ:**
```
.ac dec nd fstart fstop
.ac oct no fstart fstop
.ac lin np fstart fstop
```
**ဥပမာ:**
```
.ac dec 10 1 10K
.ac lin 100 1 100HZ
```
`.ac` line သည် သတ်မှတ်ထားသော frequency range အတွင်း circuit ၏ AC analysis ကိုလုပ်ဆောင်သည်။ ဤ analysis သည် meaningful ဖြစ်ရန် အနည်းဆုံး independent source တစ်ခုကို ac value ဖြင့် သတ်မှတ်ရမည်။ non-linear devices အားလုံးကို ၎င်းတို့၏ actual DC operating point ပတ်လည်တွင် linearize လုပ်သည်။ L and C devices များသည် actual frequency step ပေါ်မူတည်၍ imaginary value များရရှိသည်။ Output vector တစ်ခုချင်းစီကို AC value ဖြင့်ပေးထားသော input voltage (current) နှင့် ဆက်စပ်၍ တွက်ချက်သည်။ node voltages (နှင့် branch currents) များသည် complex vectors များဖြစ်သည်။

Linear circuit များအတွက် `noopac` option (11.1.3) ကို သုံးနိုင်သည်။ `@m1[cgs]` or `@r1[i]` ကဲ့သို့သော output parameters များကို AC simulation အတွင်း မထောက်ပံ့ပါ။

### ၁၁.၃.၂ .DC- DC Transfer Function

**ယေဘူယျပုံစံ:**
```
.dc srcnam vstart vstop vincr [src2 start2 stop2 incr2]
```
**ဥပမာ:**
```
.dc VIN 0.25 5.0 0.25
.dc TEMP -15 75 5
```
`.dc` line သည် dc transfer curve source နှင့် sweep limits (capacitors open, inductors shorted) ကို သတ်မှတ်သည်။ srcnam သည် independent voltage or current source၊ resistor သို့မဟုတ် circuit temperature ၏ အမည်ဖြစ်သည်။ ဒုတိယ source (src2) ကို စိတ်ကြိုက်သတ်မှတ်နိုင်သည်။

### ၁၁.၃.၃ .DISTO- Distortion Analysis

**ယေဘူယျပုံစံ:**
```
.disto dec nd fstart fstop <f2overf1>
.disto oct no fstart fstop <f2overf1>
.disto lin np fstart fstop <f2overf1>
```
`.disto` line သည် circuit ၏ small-signal distortion analysis ကိုလုပ်ဆောင်သည်။ Multi-dimensional Volterra series analysis ကို operating point ရှိ nonlinearities များကိုကိုယ်စားပြုရန် multi-dimensional Taylor series ကိုအသုံးပြုသည်။ `f2overf1` ကိုမသတ်မှတ်ပါက harmonic analysis (single input frequency F1) ကိုလုပ်ဆောင်သည်။ သတ်မှတ်ပါက spectral analysis (inputs at two frequencies F1 and F2) ကိုလုပ်ဆောင်သည်။

ngspice nonlinear device models များ၏ subset တစ်ခုကသာ distortion analysis ကို ထောက်ပံ့သည်- Diodes (DIO), BJT, JFET (level 1), MOSFETs (levels 1, 2, 3, 9, and BSIM1), MESFET (level 1)။

### ၁၁.၃.၄ .NOISE- Noise Analysis

**ယေဘူယျပုံစံ:**
```
.noise v(output <,ref>) src ( dec | lin | oct ) pts fstart fstop + <pts_per_summary>
```
**ဥပမာ:**
```
.noise v(5) VIN dec 10 1kHz 100MEG
```
`.noise` line သည် circuit ၏ noise analysis ကိုလုပ်ဆောင်သည်။ output noise နှင့် equivalent input noise ကိုတွက်ချက်သည်။ plots နှစ်ခုထုတ်လုပ်သည်- Noise Spectral Density (noise1) နှင့် Total Integrated Noise (noise2)။ `sqrnoise` variable ကိုသုံး၍ units များကို ပြောင်းနိုင်သည်။ KLU matrix solver (11.1.1) သည် noise simulation နှင့် သဟဇာတမဖြစ်ပါ။

### ၁၁.၃.၅ .OP- Operating Point Analysis

**ယေဘူယျပုံစံ:** `.op`

inductors shorted, capacitors opened ဖြင့် circuit ၏ DC operating point ကိုတွက်ချက်သည်။ DC solution ရှာရန်ခက်ခဲပါက ngspice သည် convergence aids များကို အစဉ်လိုက်အသုံးပြုသည်- gmin stepping, source stepping, transient operating point (optional)။ `optran` command ဖြင့် transient op calculation ကို configure လုပ်နိုင်သည်။

### ၁၁.၃.၆ .PZ- Pole-Zero Analysis

**ယေဘူယျပုံစံ:**
```
.pz node1 node2 node3 node4 cur pol
.pz node1 node2 node3 node4 vol zer
.pz node1 node2 node3 node4 cur pz
```
`cur` သည် (output voltage)/(input current)၊ `vol` သည် (output voltage)/(input voltage) transfer function။ `pol` (pole only), `zer` (zero only), `pz` (both)။

### ၁၁.၃.၇ .SENS- DC or Small-Signal AC Sensitivity Analysis

**ယေဘူယျပုံစံ:**
```
.SENS OUTVAR [< filter ...>] [DC]
.SENS OUTVAR [< filter ...>] AC DEC ND FSTART FSTOP
```
output variable ၏ sensitivity ကို device and model parameters အားလုံးသို့ တွက်ချက်သည်။ filter strings များဖြင့် parameter ရွေးချယ်မှုကို စစ်ထုတ်နိုင်သည်။

### ၁၁.၃.၈ .SP S-Parameter Analysis

**ယေဘူယျပုံစံ:** `.sp dec nd fstart fstop <donoise>` (စသည်)
AC နှင့်ဆင်တူသည်။ RF ports (4.1.11) လိုအပ်သည်။ S, Y, Z matrices များကိုထုတ်ပေးသည်။ donoise=1 ဖြင့် noise correlation matrix, noise figure etc. တွက်သည်။

### ၁၁.၃.၉ .TF- Transfer Function Analysis

**ယေဘူယျပုံစံ:** `.tf outvar insrc`
dc small-signal analysis အတွက် transfer function (output/input), input resistance, output resistance တို့ကို တွက်ချက်သည်။

### ၁၁.၃.၁၀ .TRAN- Transient Analysis

**ယေဘူယျပုံစံ:**
```
.tran tstep tstop <tstart <tmax>> <uic>
```
`tstep` သည် printing/plotting increment၊ `tstop` သည် final time၊ `tstart` သည် start time (default 0)၊ `tmax` သည် maximum stepsize။ `uic` သည် use initial conditions (quiescent operating point ကို skip လုပ်သည်)။

### ၁၁.၃.၁၁ Transient noise analysis (ကြိမ်နှုန်းနည်းတွင်)

Transient noise simulation ကို independent voltage source vsrc တွင် `TRNOISE` option (4.1.7) ဖြင့် .tran နှင့်အတူ အသုံးပြုသည်။ White noise (Box-Muller transform), 1/f noise (Kasdin algorithm), Random Telegraph Signal (RTS) noise တို့ကို ထောက်ပံ့သည်။ Seed value ကို `setseed nn` ဖြင့်သတ်မှတ်နိုင်သည်။ `notrnoise` variable ဖြင့် noise sources အားလုံးကိုပိတ်နိုင်သည်။

### ၁၁.၃.၁၂ .PSS- Periodic Steady State Analysis

စမ်းသပ်ဆဲ code၊ အများပြည်သူသို့ မထုတ်ပြန်သေးပါ။

**ယေဘူယျပုံစံ:** `.pss gfreq tstab oscnob psspoints harms sciter steadycoeff <uic>`
oscillators များအတွက် fundamental frequency နှင့် amplitude များကို ခန့်မှန်းရန် time domain shooting method ကိုအသုံးပြုသည်။

## ၁၁.၄ AC, DC နှင့် Transient Analysis ပြီးနောက် တိုင်းတာမှုများ

### ၁၁.၄.၁ .meas(ure)

`.meas` သို့မဟုတ် `.measure` statement (နှင့် ၎င်းနှင့်ညီမျှသော `meas` command, Chapt. 13.5.50) ကို tran, ac, or dc simulation တစ်ခု၏ output data ကို ခွဲခြမ်းစိတ်ဖြာရန် အသုံးပြုသည်။

### ၁၁.၄.၂ batch နှင့် interactive mode

Batch mode (-b option) တွင် rawfile (-r rawfile) နှင့်အတူ `.meas` analysis ကို မသုံးနိုင်ပါ။ Interactive mode သို့မဟုတ် `.control ... .endc` ဖြင့် quasi-batch mode ကိုသုံးပါ။

### ၁၁.၄.၃ အထွေထွေ မှတ်ချက်များ

Measure type {DC|AC|TRAN|SP} သည် အကဲဖြတ်မည့် data ပေါ်မူတည်သည်။ SP type ကို `spec` or `fft` commands များမှ spectrum အတွက် `meas` command တွင်သာ ရရှိနိုင်သည်။

### ၁၁.၄.၄ မှ ၁၁.၄.၁၂

**.measure command အမျိုးမျိုး၏ ပုံစံများ:**
- **Trig Targ:** အမှတ်နှစ်ခုကြား ခြားနားချက်ကို တိုင်းတာသည်။
- **Find ... When:** signal တစ်ခုက တန်ဖိုးတစ်ခုသို့ရောက်သည့်အခါ အခြား signal ၏တန်ဖိုးကို ရှာဖွေသည်။
- **AVG|MIN|MAX|PP|RMS|MIN_AT|MAX_AT:** သတ်မှတ်ထားသော ကြားကာလအတွင်း vector တစ်ခု၏ statistical values များကို တွက်ချက်သည်။
- **Integ:** သတ်မှတ်ထားသော ကြားကာလအတွင်း vector ၏ ဧရိယာကို တွက်ချက်သည်။
- **param:** expression တစ်ခုကို အကဲဖြတ်သည် (`.param` variables များသုံးနိုင်သည်)။
- **par('expression'):** algebraic expression ကို B source syntax ဖြင့် အသုံးပြုသည်။
- **Deriv:** သတ်မှတ်ထားသော အမှတ်တွင် vector တစ်ခု၏ derivative ကိုတွက်ချက်သည်။

## ၁၁.၅ Safe Operating Area (SOA) သတိပေးစာများ

`.option warn=1` ကို သတ်မှတ်ခြင်းဖြင့် SOA check ကို enable လုပ်သည်။ Resistors, Capacitors, Diodes, BJTs, MOSFETs, VDMOS များအတွက် voltage, current, power, temperature limits (model parameters) များကို ကျော်လွန်ပါက analysis အတွင်း (.op, .dc, .tran) သတိပေးချက်များ ထုတ်ပေးသည်။ `maxwarns` ဖြင့် သတိပေးချက်အရေအတွက်ကို ကန့်သတ်နိုင်သည်။

## ၁၁.၆ Batch Output

batch mode (12.4.1) တွင်သာ အောက်ပါ commands များ အလုပ်လုပ်သည်- `.print`, `.plot`, `.four`။ `.save` နှင့် `.probe` သည် modes အားလုံးတွင် အလုပ်လုပ်သည်။

### ၁၁.၆.၁ .SAVE- Name vector(s) to be saved in raw file

raw file တွင် သိမ်းဆည်းမည့် vectors များကို သတ်မှတ်သည်။ `all` keyword သည် default set အပြင် နောက်ထပ် vectors များကို သိမ်းဆည်းသည်။

### ၁၁.၆.၂ .PRINT Lines

analysis type အတွက် tabular listing ၏ contents ကို သတ်မှတ်သည်။ output variables များကို V(N1<,N2>), I(VXXXXXXX) စသည်ဖြင့် သတ်မှတ်သည်။ `par('expression')` option ကို သုံးနိုင်သည်။

### ၁၁.၆.၃ .PLOT Lines

printer plot output တစ်ခု ဖန်တီးသည်။ `.plot` line သည် plot တစ်ခု၏ contents ကို သတ်မှတ်သည်။ variable တစ်ခုထက်ပိုပါက ပထမ variable ကို print လည်းထုတ်ပြီး plotted လုပ်သည်။

### ၁၁.၆.၄ .FOUR- Fourier Analysis of Transient Analysis Output

**ယေဘူယျပုံစံ:** `.four freq ov1 <ov2 ov3 ...>`
transient analysis ၏ တစ်စိတ်တစ်ပိုင်းအဖြစ် Fourier analysis ကို လုပ်ဆောင်သည်။

### ၁၁.၆.၅ .PROBE- Save device node currents, device power dissipation, or differential voltages between arbitrary nodes

`probe` command သည် user သတ်မှတ်ထားသော device nodes များတွင် current တိုင်းတာခြင်းကို enable လုပ်သည်။ Voltage sources များကို အလိုအလျောက်ထည့်ပေးသည်။ `.probe alli` သို့မဟုတ် `.probe I(device)` စသည့်ပုံစံများဖြင့် သုံးသည်။ Differential voltage measurements အတွက် VCVS များထည့်ပေးပြီး `vd_` prefix ဖြင့် vector များထုတ်ပေးသည်။ Power dissipation measurement ကိုလည်း `.probe p(device)` ဖြင့် ရရှိနိုင်သည်။

### ၁၁.၆.၆ par('expression')- Algebraic expressions for output

`.four`, `.plot`, `.print`, `.save`, `.measure` lines များတွင် B source syntax ဖြင့် algebraic expressions များကို output vectors များသို့ ထည့်နိုင်သည်။ expression ကို internal voltage node တစ်ခုဖြင့် အစားထိုးသည်။

### ၁၁.၆.၇ .width

print-out or plot ၏ width ကို `.width out=256` ဖြင့် သတ်မှတ်သည်။

## ၁၁.၇ စက်ပစ္စည်း terminal များမှ current ကို တိုင်းတာခြင်း

### ၁၁.၇.၁ .probe command ကို အသုံးပြုခြင်း

`.probe` command (11.6.5) ဖြင့် device currents များကို တိုင်းတာနိုင်သည်။

### ၁၁.၇.၂ Series တွင် voltage source တစ်ခုထည့်ခြင်း

current တိုင်းတာရန် dc voltage 0 ရှိသော voltage source ကို series ထည့်နိုင်သည်။ `vmeas#branch` အဖြစ် current ကိုရရှိသည်။

### ၁၁.၇.၃ 'savecurrents' option ကို အသုံးပြုခြင်း

`.options savecurrents` သည် internal current data များကို `@dev[i]` ပုံစံဖြင့် save လုပ်သည်။ AC simulation တွင်မရပါ၊ memory များစွာသုံးနိုင်သည်။ MOS devices အတွက် `savecurrents_bsim3/4` options များရှိသည်။
