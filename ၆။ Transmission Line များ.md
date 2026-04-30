# ၆။ Transmission Line များ

Ngspice သည် မူရင်း SPICE3f5 ၏ ဆုံးရှုံးမှုရှိ/မရှိ သွယ်တန်းလိုင်း (transmission line) မော်ဒယ်များနှင့် KSPICE မှ စတင်မိတ်ဆက်ခဲ့သော မော်ဒယ်နှစ်မျိုးလုံးကို အကောင်အထည်ဖော်ထားသည်။ နောက်ပိုင်းမော်ဒယ်များသည် ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်းများ (lossy transmission lines) ၏ transient analysis ကို ပိုမိုကောင်းမွန်အောင် ပြုလုပ်ပေးသည်။ SPICE မော်ဒယ်များသည် ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်းများကို သရုပ်ဖော်ရန် state-based approach ကို အသုံးပြုသည်နှင့် မတူဘဲ၊ KSPICE သည် recursive convolution method ကို အသုံးပြု၍ ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်းများနှင့် coupled multiconductor line systems များကို သရုပ်ဖော်သည်။ မည်သည့် arbitrary transfer function ၏ impulse response ကိုမဆို function ၏ Pade approximations မှ recursive convolution ကို ဆင်းသက်ခြင်းဖြင့် ဆုံးဖြတ်နိုင်သည်။ သွယ်တန်းလိုင်းတစ်ခုချင်းစီ၏ characteristics နှင့် multiconductor line တစ်ခုချင်းစီ၏ modal functions များကို သရုပ်ဖော်ရန် ဤချဉ်းကပ်နည်းကို အသုံးပြုထားသည်။ ဤနည်းလမ်းသည် ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်း သရုပ်ဖော်မှုအတွက် SPICE3f5 ထက် တစ်ဆင့်မှ နှစ်ဆင့်အထိ မြန်ဆန်ကြောင်း သက်သေပြထားသည်။

## ၆.၁ Lossless Transmission Line များ

**ယေဘူယျပုံစံ:**
```
TXXXXXXX N1 N2 N3 N4 Z0=VALUE <TD=VALUE>
+ <F=FREQ <NL=NRLMEN>> <IC=V1, I1, V2, I2>
```
**ဥပမာ:**
```spice
T1 1 0 2 0 Z0=50 TD=10NS
```
n1 နှင့် n2 သည် port 1 ရှိ node များဖြစ်ပြီး၊ n3 နှင့် n4 သည် port 2 ရှိ node များဖြစ်သည်။ z0 သည် characteristic impedance ဖြစ်သည်။ လိုင်း၏အလျားကို ပုံစံနှစ်မျိုးဖြင့် ဖော်ပြနိုင်သည်။ ထုတ်လွှင့်မှုနှောင့်နှေးချိန် td ကို တိုက်ရိုက်သတ်မှတ်နိုင်သည် (ဥပမာ td=10ns)။ တစ်နည်းအားဖြင့် ကြိမ်နှုန်း f တစ်ခုကို nl နှင့်အတူ ပေးနိုင်သည်။ nl သည် ကြိမ်နှုန်း f တွင် လိုင်းအတွင်းရှိ လှိုင်းအလျားနှင့် ဆက်စပ်သော သွယ်တန်းလိုင်း၏ normalized electrical length ဖြစ်သည်။ ထို့နောက် ထုတ်လွှင့်မှုနှောင့်နှေးချိန်ကို \(t_d = nl / f\) အဖြစ် တွက်ချက်သည်။ ကြိမ်နှုန်းကို သတ်မှတ်ပြီး nl ကို ချန်လှပ်ပါက 0.25 ဟု ယူဆသည် (ဆိုလိုသည်မှာ ကြိမ်နှုန်းသည် quarter-wave frequency ဟု ယူဆသည်)။ လိုင်းအလျားကို ဖော်ပြရန် ဤပုံစံနှစ်မျိုးစလုံးကို စိတ်ကြိုက်ဖြစ်သည်ဟု ဖော်ပြထားသော်လည်း နှစ်ခုထဲမှ တစ်ခုကို သတ်မှတ်ရမည်။

ဤ element အတွက် .model line မလိုအပ်ပါ။

ဤ element သည် propagating mode တစ်ခုတည်းကိုသာ မော်ဒယ်လုပ်ကြောင်း သတိပြုပါ။ အကယ်၍ actual circuit တွင် node လေးခုစလုံး သီးခြားဖြစ်နေပါက mode နှစ်ခု excite ဖြစ်နိုင်သည်။ ထိုကဲ့သို့သော အခြေအနေကို သရုပ်ဖော်ရန် transmission-line element နှစ်ခု လိုအပ်သည်။ (နောက်ထပ် ရှင်းလင်းချက်အတွက် Chapt. 17.7 ရှိ ဥပမာကိုကြည့်ပါ။) (စိတ်ကြိုက်) initial condition specification တွင် transmission line ports တစ်ခုချင်းစီရှိ voltage နှင့် current တို့ပါဝင်သည်။ initial conditions များသည် .TRAN control line ပေါ်တွင် UIC option ကို သတ်မှတ်မှသာ သက်ရောက်ကြောင်း သတိပြုပါ။

အကောင်အထည်ဖော်မှုအသေးစိတ်များကြောင့် ဆုံးရှုံးမှုမရှိသော သွယ်တန်းလိုင်း (lossless transmission line) ထက် ဆုံးရှုံးမှုသုညရှိသော ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်း (lossy transmission line) က ပိုမိုတိကျနိုင်ကြောင်း သတိပြုပါ။

## ၆.၂ Lossy Transmission Line များ

**ယေဘူယျပုံစံ:**
```
OXXXXXXX n1 n2 n3 n4 mname
```
**ဥပမာ:**
```spice
O23 1 0 2 0 LOSSYMOD
.model LOSSYMOD ltra rel=1 r=12.45 g=0 l=8.972e-9 c=0.468e-12
+ len=16 steplimit compactrel=1.0e-3 compactabs=1.0e-14
OONNECT 10 5 20 5 INTERCONNECT
```
၎င်းသည် single conductor lossy transmission lines များအတွက် two-port convolution model တစ်ခုဖြစ်သည်။ n1 နှင့် n2 သည် port 1 ရှိ node များဖြစ်ပြီး၊ n3 နှင့် n4 သည် port 2 ရှိ node များဖြစ်သည်။ အကောင်အထည်ဖော်မှုအသေးစိတ်များကြောင့် ဆုံးရှုံးမှုသုညရှိသော ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်းသည် ဆုံးရှုံးမှုမရှိသော သွယ်တန်းလိုင်းထက် ပိုမိုတိကျနိုင်ကြောင်း သတိပြုပါ။

### ၆.၂.၁ Lossy Transmission Line Model (LTRA)

Uniform RLC/RC/LC/RG transmission line model (ယခုနောက်ပိုင်း LTRA model ဟုခေါ်ဆိုသည်) သည် uniform constant-parameter distributed transmission line တစ်ခုကို မော်ဒယ်လုပ်သည်။ RC နှင့် LC cases များကို URC နှင့် TRA models များသုံး၍လည်း မော်ဒယ်လုပ်နိုင်သည်၊ သို့သော် ပိုမိုသစ်သော LTRA model သည် အခြားများထက် များသောအားဖြင့် ပိုမိုမြန်ဆန်ပြီး တိကျသည်။ LTRA model ၏လုပ်ဆောင်မှုသည် transmission line ၏ impulse responses များကို ၎င်း၏ inputs များနှင့် convolution လုပ်ခြင်းအပေါ် အခြေခံသည် ([8] ကိုကြည့်ပါ)။ LTRA model သည် parameters အတော်များများကို ယူသည်၊ အချို့မှာ မဖြစ်မနေပေးရမည်ဖြစ်ပြီး အချို့မှာ စိတ်ကြိုက်ဖြစ်သည်။

| အမည် | Parameter | Units/Type | Default | ဥပမာ |
|------|-----------|------------|---------|------|
| R | resistance/length | Ω/unit | 0.0 | 0.2 |
| L | inductance/length | H/unit | 0.0 | 9.13e-9 |
| G | conductance/length | mhos/unit | 0.0 | 0.0 |
| C | capacitance/length | F/unit | 0.0 | 3.65e-12 |
| LEN | length of line | unit | no default | 1.0 |
| REL | breakpoint control | arbitrary unit | 1 | 0.5 |
| ABS | breakpoint control | | 1 | 5 |
| NOSTEPLIMIT | don't limit time-step to less than line delay | flag | not set | set |
| NO CONTROL | don't do complex time-step control | flag | not set | set |
| LININTERP | use linear interpolation | flag | not set | set |
| MIXEDINTERP | use linear when quadratic seems bad | flag | not set | set |
| COMPACTREL | special reltol for history compaction | RELTOL | 1.0e-3 | |
| COMPACTABS | special abtol for history compaction | ABSTOL | 1.0e-9 | |
| TRUNCNR | use Newton-Raphson method for time-step control | flag | not set | set |
| TRUNCDONTCUT | don't limit time-step to keep impulse-response errors low | flag | not set | set |

ယခုအချိန်ထိ အကောင်အထည်ဖော်ထားသော လိုင်းအမျိုးအစားများမှာ-
- RLC (series loss သာရှိသော uniform transmission line)
- RC (uniform RC line)
- LC (lossless transmission line)
- RG (distributed series resistance and parallel conductance သာရှိသော)

အခြားပေါင်းစပ်မှုများသည် မှားယွင်းသောရလဒ်များ ထွက်ပေါ်စေမည်ဖြစ်ပြီး မစမ်းသပ်သင့်ပါ။ လိုင်း၏အလျား LEN ကို သတ်မှတ်ရမည်။ NOSTEPLIMIT သည် RLC case တွင် time-step ကို line delay ထက်နည်းအောင် ကန့်သတ်ခြင်း default restriction ကို ဖယ်ရှားပေးသည့် flag တစ်ခုဖြစ်သည်။ NO CONTROL သည် RLC နှင့် RC cases များတွင် convolution error criteria အပေါ်အခြေခံ၍ time-step ကို default ကန့်သတ်ခြင်းကို တားဆီးသည့် flag ဖြစ်သည်။ ၎င်းသည် simulation ကို မြန်ဆန်စေသော်လည်း အချို့ကိစ္စများတွင် ရလဒ်များ၏တိကျမှုကို လျှော့ချနိုင်သည်။ LININTERP သည် သတ်မှတ်ပါက delayed signals များကို တွက်ချက်ရန်အတွက် default quadratic interpolation အစား linear interpolation ကို အသုံးပြုမည့် flag တစ်ခုဖြစ်သည်။ MIXEDINTERP သည် quadratic interpolation မသင့်လျော်ကြောင်း ဆုံးဖြတ်ရန် metric တစ်ခုကိုအသုံးပြုပြီး ထိုသို့ဖြစ်ပါက linear interpolation ကိုသုံးမည့် flag ဖြစ်သည်။ မဟုတ်ပါက default quadratic interpolation ကိုသုံးသည်။ TRUNCDONTCUT သည် impulse-response related quantities များ၏တွက်ချက်မှုတွင် errors များကို ကန့်သတ်ရန် time-step ကို default ဖြတ်တောက်ခြင်းကို ဖယ်ရှားပေးသည့် flag တစ်ခုဖြစ်သည်။ COMPACTREL နှင့် COMPACTABS တို့သည် convolution အတွက် သိမ်းဆည်းထားသော အတိတ်တန်ဖိုးများ၏ history ကို compact လုပ်ခြင်းကို ထိန်းချုပ်သည့် quantity များဖြစ်သည်။ ဤတန်ဖိုးများ ကြီးလျှင် တိကျမှုနိမ့်သော်လည်း simulation speed များသောအားဖြင့် တိုးတက်သည်။ ၎င်းတို့ကို .OPTIONS section တွင်ဖော်ပြထားသော TRYTOCOMPACT option နှင့်အတူ အသုံးပြုရမည်။ TRUNCNR သည် time-step control routines များတွင် သင့်လျော်သော time-step ကိုဆုံးဖြတ်ရန် Newton-Raphson iterations များကို အသုံးပြုမည့် flag တစ်ခုဖြစ်သည်။ default သည် ယခင် time-step ကို ထက်ဝက်ဖြတ်ခြင်းဖြင့် trial and error procedure ဖြစ်သည်။ REL နှင့် ABS တို့သည် breakpoints များ၏ setting ကို ထိန်းချုပ်သည့် quantity များဖြစ်သည်။

Simulation speed ကို တိုးမြှင့်ရန် အများဆုံးစမ်းသပ်သင့်သော option မှာ REL ဖြစ်သည်။ default တန်ဖိုး 1 သည် တိကျမှုရှုထောင့်မှ များသောအားဖြင့် စိတ်ချရသော်လည်း တစ်ခါတစ်ရံ တွက်ချက်ချိန်ကို တိုးစေသည်။ 2 ထက်ကြီးသော တန်ဖိုးသည် breakpoints အားလုံးကို ဖယ်ရှားပြီး circuit ၏ ကျန်အပိုင်းများ၏ သဘာဝအပေါ်မူတည်၍ တိကျမှုရှုထောင့်မှ စိတ်ချရမည်မဟုတ်ကြောင်း သတိပြုရင်း စမ်းသပ်သင့်သည်။

circuit သည် sharp discontinuities များ မပြသနိုင်ဟု မျှော်လင့်ပါက breakpoints များကို လုံးဝဖယ်ရှားနိုင်သည်။ 0 နှင့် 1 ကြားရှိတန်ဖိုးများသည် ပုံမှန်အားဖြင့် မလိုအပ်သော်လည်း breakpoints များစွာ သတ်မှတ်ရန်အတွက် အသုံးပြုနိုင်သည်။

.OPTIONS card တစ်ခုတွင် TRYTCOMPACT option ကို သတ်မှတ်ထားသည့်အခါ COMPACTREL ကိုလည်း စမ်းသပ်နိုင်သည်။ တရားဝင် range မှာ 0 နှင့် 1 ကြားဖြစ်သည်။ ကြီးမားသောတန်ဖိုးများသည် simulation ၏တိကျမှုကို များသောအားဖြင့် လျှော့ချသော်လည်း အချို့ကိစ္စများတွင် speed ကိုတိုးတက်စေသည်။ .OPTIONS card တွင် TRYTCOMPACT ကို မသတ်မှတ်ပါက history compaction ကို မကြိုးစားဘဲ တိကျမှုမြင့်မားသည်။

NO CONTROL, TRUNCDONTCUT နှင့် NOSTEPLIMIT တို့သည်လည်း တိကျမှုကို စွန့်လွှတ်၍ speed ကိုတိုးမြှင့်လေ့ရှိသည်။

## ၆.၃ Uniform Distributed RC Line များ

**ယေဘူယျပုံစံ:**
```
UXXXXXXX n1 n2 n3 mname l=len <n=lumps>
```
**ဥပမာ:**
```spice
U1 1 2 0 URCMOD L=50U
.model URCMOD URC CPERL=100p RPERL=100k FMAX=10G
URC2 1 12 2 UMODL l=1MIL N=6
```
n1 နှင့် n2 သည် RC line ချိတ်ဆက်သည့် element node နှစ်ခုဖြစ်ပြီး၊ n3 သည် capacitances များချိတ်ဆက်ထားသော node ဖြစ်သည်။ mname သည် model name ဖြစ်ပြီး၊ len သည် RC line ၏အလျား (မီတာဖြင့်) ဖြစ်သည်။ lumps ကို သတ်မှတ်ပါက RC line ကို မော်ဒယ်လုပ်ရာတွင် အသုံးပြုမည့် lumped segments အရေအတွက်ဖြစ်သည် (ဤ parameter ကို ချန်လှပ်ပါက လုပ်ဆောင်မည့်အရာအတွက် model description ကိုကြည့်ပါ)။

### ၆.၃.၁ Uniform Distributed RC Model (URC)

URC model ကို 1974 ခုနှစ်တွင် L. Gertzberg မှ အဆိုပြုခဲ့သော model မှ ဆင်းသက်သည်။ ဤ model ကို internally generated nodes များပါဝင်သော lumped RC segments များ၏ network တစ်ခုအဖြစ် URC line ကို subcircuit type expansion ဖြင့် အကောင်အထည်ဖော်သည်။ RC segments များသည် URC line ၏အလယ်သို့ တိုးလာသော geometric progression တွင်ရှိပြီး \(K\) သည် proportionality constant ဖြစ်သည်။ URC line device အတွက် မသတ်မှတ်ပါက အသုံးပြုသော lumped segments အရေအတွက်ကို အောက်ပါဖော်မြူလာဖြင့် ဆုံးဖြတ်သည်-

\[
N = \frac{\log\left|F_{\mathrm{max}}\frac{R}{L} 2\pi L^2\left|\frac{(K - 1)}{K}\right|^2\right|}{\log K}
\]

URC line ကို ISPERL parameter ကို သုညမဟုတ်သောတန်ဖိုး မပေးမချင်း resistor နှင့် capacitor segments များဖြင့်သာ တင်းကြပ်စွာ ဖွဲ့စည်းထားသည်။ ISPERL parameter ကိုပေးပါက capacitors များကို အစားထိုးထားသော capacitance နှင့် ညီမျှသော zero-bias junction capacitance နှင့် သွယ်တန်းလိုင်း၏ တစ်မီတာလျှင် ISPERL amps ၏ saturation current ရှိသော reverse biased diodes များဖြင့် အစားထိုးပြီး စိတ်ကြိုက် series resistance မှာ တစ်မီတာလျှင် RSPERL ohms နှင့် ညီမျှသည်။

| အမည် | Parameter | Units | Default | ဥပမာ |
|------|-----------|-------|---------|------|
| K | Propagation Constant | - | 1.5 | 1.2 |
| FMAX | Maximum Frequency of interest | Hz | 1.0 G | 6.5 Meg |
| RPERL | Resistance per unit length | Ω/m | 1000 | 10 |
| CPERL | Capacitance per unit length | F/m | 10e-15 | 1 p |
| ISPERL | Saturation Current per unit length | A/m | 0 | |
| RSPERL | Diode Resistance per unit length | Ω/m | 0 | |

## ၆.၄ KSPICE Lossy Transmission Line များ

SPICE3 သည် ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်းများကို သရုပ်ဖော်ရန် state-based approach ကို အသုံးပြုသည်နှင့် မတူဘဲ၊ KSPICE သည် recursive convolution method ကို အသုံးပြု၍ ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်းများနှင့် coupled multiconductor line systems များကို သရုပ်ဖော်သည်။ arbitrary transfer function တစ်ခု၏ impulse response ကို function ၏ Pade approximations မှ recursive convolution ကို ဆင်းသက်ခြင်းဖြင့် ဆုံးဖြတ်နိုင်သည်။ Ngspice သည် သွယ်တန်းလိုင်းတစ်ခုချင်းစီ၏ characteristics နှင့် multiconductor line တစ်ခုချင်းစီ၏ modal functions များကို သရုပ်ဖော်ရန် ဤချဉ်းကပ်နည်းကို အသုံးပြုထားသည်။ ဤနည်းလမ်းသည် ဆုံးရှုံးမှုရှိသော သွယ်တန်းလိုင်း သရုပ်ဖော်မှုအတွက် သိသိသာသာ မြန်ဆန်ကြောင်း ပြသထားသည်။ အောက်ပါ model နှစ်ခုသည် transient simulation ကိုသာ ထောက်ပံ့မည်ဖြစ်ပြီး AC ကို မထောက်ပံ့ကြောင်း ကျေးဇူးပြု၍ သတိပြုပါ။

နောက်ထပ် ရရှိနိုင်သော စာရွက်စာတမ်းများ-
- S. Lin and E. S. Kuh, 'Pade Approximation Applied to Transient Simulation of Lossy Coupled Transmission Lines,' Proc. IEEE Multi-Chip Module Conference, 1992, pp. 52-55.
- S. Lin, M. Marek-Sadowska, and E. S. Kuh, 'SWEC: A StepWise Equivalent Conductance Timing Simulator for CMOS VLSI Circuits,' European Design Automation Conf., February 1991, pp. 142-148.
- S. Lin and E. S. Kuh, 'Transient Simulation of Lossy Interconnect,' Proc. Design Automation Conference, Anaheim, CA, June 1992, pp. 81-86.

### ၆.၄.၁ Single Lossy Transmission Line (TXL)

**ယေဘူယျပုံစံ:**
```
YXXXXXXX N1 0 N2 0 mname <LEN=LENGTH>
```
**ဥပမာ:**
```spice
Y1 1 0 2 0 ymod LEN=2
.MODEL ymod tx1 R=12.45 L=8.972e-9 G=0 C=0.468e-12 length=16
```
n1 နှင့် n2 သည် port နှစ်ခု၏ node များဖြစ်သည်။ စိတ်ကြိုက် instance parameter len သည် လိုင်း၏အလျားဖြစ်ပြီး [unit] ၏ multiples ဖြင့် ဖော်ပြနိုင်သည်။ ပုံမှန်အားဖြင့် unit ကို မီတာဖြင့် ပေးသည်။ len သည် specific instance အတွက်သာ model parameter length ကို override လုပ်လိမ့်မည်။

TXL model သည် parameters အတော်များများကို ယူသည်-

| အမည် | Parameter | Units/Type | Default | ဥပမာ |
|------|-----------|------------|---------|------|
| R | resistance/length | Ω/unit | 0.0 | 0.2 |
| L | inductance/length | H/unit | 0.0 | 9.13e-9 |
| G | conductance/length | mhos/unit | 0.0 | 0.0 |
| C | capacitance/length | F/unit | 0.0 | 3.65e-12 |
| LENGTH | length of line | unit | no default | 1.0 |

Model parameter length ကို unit ၏ multiple အဖြစ် သတ်မှတ်ရမည်။ ပုံမှန်အားဖြင့် unit ကို [m] ဖြင့် ပေးသည်။ Transient simulation အတွက်သာ။

### ၆.၄.၂ Coupled Multiconductor Line (CPL)

CPL multiconductor line model သည် သီအိုရီအရ RLGC model နှင့်ဆင်တူသော်လည်း frequency dependent loss (skin effect နှင့် frequency-dependent dielectric loss) မပါဝင်ပါ။ Ngspice တွင် coupled lines 8 ခုအထိ ထောက်ပံ့သည်။

**ယေဘူယျပုံစံ:**
```
PXXXXXXX N11 N12...NIX GND1 N01 N02...NOX GND2 mname <LEN=LENGTH>
```
**ဥပမာ:**
```spice
P1 in1 in2 0 b1 b2 0 PLINE
.model PLINE CPL length={Len}
+ R=1 0 1
+ L={L11} {L12} {L22}
+ G=0 0
+ C={C11} {C12} {C22}
.param Len=1 Rs=0
+ C11=9.143579E-11 C12=-9.78265E-12 C22=9.143578E-11
+ L11=3.83572E-7 L12=8.26253E-8 L22=3.83572E-7
```
n11 ... nix သည် port 1 ၏ node များဖြစ်ပြီး gnd1 နှင့်၊ no1 ... nox သည် port 2 ၏ node များဖြစ်ကာ gnd2 နှင့်။ စိတ်ကြိုက် instance parameter len သည် လိုင်း၏အလျားဖြစ်ပြီး [unit] ၏ multiples ဖြင့် ဖော်ပြနိုင်သည်။ ပုံမှန်အားဖြင့် unit ကို မီတာဖြင့် ပေးသည်။ len သည် specific instance အတွက်သာ model parameter length ကို override လုပ်လိမ့်မည်။

CPL model သည် parameters အတော်များများကို ယူသည်-

| အမည် | Parameter | Units/Type | Default | ဥပမာ |
|------|-----------|------------|---------|------|
| R | resistance/length | Ω/unit | 0.0 | 0.2 |
| L | inductance/length | H/unit | 0.0 | 9.13e-9 |
| G | conductance/length | mhos/unit | 0.0 | 0.0 |
| C | capacitance/length | F/unit | 0.0 | 3.65e-12 |
| LENGTH | length of line | unit | no default | 1.0 |

RLGC parameters အားလုံးကို Maxwell matrix form ဖြင့် ပေးရသည်။ R နှင့် G matrices များအတွက် diagonal elements များကို သတ်မှတ်ရမည်ဖြစ်ပြီး၊ L နှင့် C matrices များအတွက် lower or upper triangular elements များကို သတ်မှတ်ရမည်။ Parameter LENGTH သည် scalar ဖြစ်ပြီး mandatory ဖြစ်သည်။ Transient simulation အတွက်သာ။
