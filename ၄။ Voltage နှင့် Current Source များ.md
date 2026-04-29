# ၄။ Voltage နှင့် Current Source များ

## ၄.၁ Voltage သို့မဟုတ် Current အတွက် Independent Source များ

**ယေဘူယျပုံစံ-**
```
VXXXXXXX N+ N- <<DC> DC/TRAN VALUE> <AC <ACMAG <ACPHASE>>>
+ <DISTOF1 <F1MAG <F1PHASE>>> <DISTOF2 <F2MAG <F2PHASE>>>
IYYYYYYY N+ N- <<DC> DC/TRAN VALUE> <AC <ACMAG <ACPHASE>>>
+ <DISTOF1 <F1MAG <F1PHASE>>> <DISTOF2 <F2MAG <F2PHASE>>>
```
**ဥပမာများ-**
```spice
VCC 10 0 DC 6
VIN 13 2 0.001 AC 1 SIN(0 1 1MEG)
ISRC 23 21 AC 0.333 45.0 SFFM(0 1 10K 5 1K)
VMEAS 12 9
VCARRIER 1 0 DISTOF1 0.1 -90.0
VMODULATOR 2 0 DISTOF2 0.01
IIN1 1 5 AC 1 DISTOF1 DISTOF2 0.001
```
n+ နှင့် n- သည် positive နှင့် negative nodes များဖြစ်သည်။ voltage sources များကို ground လုပ်ရန်မလိုအပ်ကြောင်း သတိပြုပါ။ positive current ကို positive node မှ source မှတစ်ဆင့် negative node သို့ စီးဆင်းသည်ဟု ယူဆသည်။ positive value ရှိသော current source သည် current ကို n+ node မှ source မှတစ်ဆင့် n- node သို့ စီးဆင်းစေရန် တွန်းအားပေးသည်။ Voltage sources များကို circuit excitation အတွက်အသုံးပြုခြင်းအပြင် ngspice အတွက် 'ammeters' များအဖြစ်လည်း ဆောင်ရွက်သည်၊ ဆိုလိုသည်မှာ current တိုင်းတာရန်အတွက် zero valued voltage sources များကို circuit အတွင်းသို့ ထည့်သွင်းနိုင်သည်။ ၎င်းတို့သည် short-circuits ကိုကိုယ်စားပြုသောကြောင့် circuit လည်ပတ်မှုအပေါ် သက်ရောက်မှုမရှိပါ။

DC/TRAN သည် source ၏ dc and transient analysis value ဖြစ်သည်။ source value သည် dc and transient analyses နှစ်ခုလုံးအတွက် သုညဖြစ်ပါက၊ ဤတန်ဖိုးကို ချန်လှပ်နိုင်သည်။ source value သည် time-invariant (ဥပမာ power supply) ဖြစ်ပါက၊ တန်ဖိုး၏ရှေ့တွင် DC စာလုံးများကို စိတ်ကြိုက်ထည့်နိုင်သည်။

AC keyword နှင့် ၎င်း၏တန်ဖိုး ACMAG (နှင့် စိတ်ကြိုက်တန်ဖိုး ACPHASE) သည် voltage သို့မဟုတ် current source အား ac simulation တစ်ခုတွင် small signal source ဖြစ်လာစေရန် ရည်ရွယ်သည့်အခါ လိုအပ်သည်။ ACMAG သည် ac magnitude ဖြစ်ပြီး ACPHASE သည် ac phase ဖြစ်သည်။ ထို့နောက် voltage သို့မဟုတ် current source သည် node အားလုံးအတွက် reference တစ်ခုဖြစ်လာမည်။ simulation ပြီးနောက်ရရှိသော small signal node amplitude တန်ဖိုးအားလုံးကို reference ACMAG ဖြင့် စားထားသည်။ ထို့ကြောင့် ပုံမှန် ACMAG တန်ဖိုးသည် unity ဖြစ်နိုင်သည်။ တိုင်းတာထားသော phase အားလုံးကို ACPHASE ဖြင့် ပြောင်းလဲထားသည်။ ACPHASE ကို ချန်လှပ်ပါက၊ သုညဟု ယူဆသည်။ source သည် ac small-signal input မဟုတ်ပါက AC keyword နှင့် ac values များကို ရှောင်ရှားသင့်သည်။

DISTOF1 နှင့် DISTOF2 တို့သည် independent source တွင် F1 နှင့် F2 frequencies များ၌ distortion inputs များရှိကြောင်း သတ်မှတ်သည့် keywords များဖြစ်သည် ( .DISTO control line ၏ဖော်ပြချက်ကိုကြည့်ပါ)။ keywords များ၏နောက်တွင် စိတ်ကြိုက် magnitude နှင့် phase များထည့်နိုင်သည်။ magnitude နှင့် phase တို့၏ default values များမှာ 1.0 နှင့် 0.0 အသီးသီးဖြစ်သည်။

မည်သည့် independent source ကိုမဆို transient analysis အတွက် time-dependent value တစ်ခု သတ်မှတ်နိုင်သည်။ source တစ်ခုကို time-dependent value သတ်မှတ်ပါက၊ time-zero value ကို dc analysis အတွက် အသုံးပြုသည်။ independent source function ကိုးမျိုးရှိသည်-

- pulse
- exponential
- sinusoidal
- piece-wise linear
- single-frequency FM
- AM
- transient noise
- random voltages or currents
- external data (ngspice shared library ဖြင့်သာ)
- နှင့် RF port

source values များမှလွဲ၍ parameters များကို ချန်လှပ်ပါက သို့မဟုတ် သုညသတ်မှတ်ပါက၊ ပြထားသော default values များကို ယူဆသည်။ TSTEP သည် printing increment ဖြစ်ပြီး TSTOP သည် final time ဖြစ်သည် - ရှင်းလင်းချက်အတွက် .TRAN control line ကိုကြည့်ပါ။

### ၄.၁.၁ Pulse

**ယေဘူယျပုံစံ-**
```
PULSE(V1 V2 TD TR TF PW PER NP)
```
**ဥပမာ-**
```spice
VIN 3 0 PULSE(-1 1 2NS 2NS 2NS 50NS 100NS 5)
```
| အမည် | Parameter | Default Value | Units |
|------|-----------|---------------|-------|
| V1 | Initial value | - | V, A |
| V2 | Pulsed value | - | V, A |
| TD | Delay time | 0.0 | sec |
| TR | Rise time | TSTEP | sec |
| TF | Fall time | TSTEP | sec |
| PW | Pulse width | TSTOP | sec |
| PER | Period | TSTOP | sec |
| NP | Number of Pulses *) | unlimited | - |

repetition count သို့မဟုတ် phase offset မပါသော single pulse ကို အောက်ပါဇယားဖြင့် ဖော်ပြသည်-

| Time | Value |
|------|-------|
| 0 | V1 |
| TD | V1 |
| TD+TR | V2 |
| TD+TR+PW | V2 |
| TD+TR+PW+TF | V1 |
| TSTOP | V1 |

ကြားရှိအချက်များကို linear interpolation ဖြင့် ဆုံးဖြတ်သည်။

*) NP ကို 0 သို့မဟုတ် ချန်လှပ်ပါက unlimited pulses ဟုဆိုလိုသည်။ compatibility mode (၁၂.၁၄.၁ ကိုကြည့်ပါ) တွင် ngbehavior = xs ဟု .spiceinit ၌ သတ်မှတ်ထားပါက၊ 8th parameter သည် pulse signal ၏ phase (in degrees) ဖြစ်ပြီး၊ ၎င်းသည် pulse sequence ၏ forward running (pos. value) သို့မဟုတ် delay (neg. value) ကိုဖြစ်ပေါ်စေသည်။

### ၄.၁.၂ Sinusoidal

**ယေဘူယျပုံစံ-**
```
SIN(VO VA FREQ TD THETA PHASE)
```
**ဥပမာ-**
```spice
VIN 3 0 SIN(0 1 100MEG 1NS 1E10)
```
| အမည် | Parameter | Default Value | Units |
|------|-----------|---------------|-------|
| VO | Offset | - | V, A |
| VA | Amplitude | - | V, A |
| FREQ | Frequency | 1/TSTOP | Hz |
| TD | Delay | 0.0 | sec |
| THETA | Damping factor | 0.0 | 1/sec |
| PHASE | Phase | 0.0 | degrees |

waveform ၏ပုံစံကို အောက်ပါဖော်မြူလာဖြင့် ဖော်ပြသည်-
\[
V(t) =
\begin{cases}
V0 & 0\leq t< TD\\
V0 + VA e^{-(t-TD)THETA} \sin (2\pi \cdot FREQ\cdot (t-TD) + PHASE) & TD\leq t< TSTOP.
\end{cases}
\]

### ၄.၁.၃ Exponential

**ယေဘူယျပုံစံ-**
```
EXP(V1 V2 TD1 TAU1 TD2 TAU2)
```
**ဥပမာ-**
```spice
VIN 3 0 EXP(-4 -1 2NS 30NS 60NS 40NS)
```
| အမည် | Parameter | Default Value | Units |
|------|-----------|---------------|-------|
| V1 | Initial value | - | V, A |
| V2 | pulsed value | - | V, A |
| TD1 | rise delay time | 0.0 | sec |
| TAU1 | rise time constant | TSTEP | sec |
| TD2 | fall delay time | TD1+TSTEP | sec |
| TAU2 | fall time constant | TSTEP | sec |

waveform ၏ပုံစံကို အောက်ပါဖော်မြူလာဖြင့် ဖော်ပြသည်။ V21 = V2 - V1, V12 = V1 - V2:
\[
V(t) =
\begin{cases}
V1 & 0\leq t< TD1,\\
V1 + V21\left(1 - e^{\frac{(t-TD1)}{TAU1}}\right) & TD1\leq t< TD2,\\
V1 + V21\left(1 - e^{\frac{(t-TD1)}{TAU1}}\right) + V12\left(1 - e^{\frac{(t-TD2)}{TAU2}}\right) & TD2\leq t< TSTOP.
\end{cases}
\]

### ၄.၁.၄ Piece-Wise Linear

**ယေဘူယျပုံစံ-**
```
PWL(T1 V1 < T2 V2 T3 V3 T4 V4 ...> < r = value > < td = value >)
```
**ဥပမာ-**
```spice
VCLOCK 7 5 PWL(0 -7 10NS -7 11NS -3 17NS -3 18NS -7 50NS -7)
+ r = 0 td = 15NS
```
တန်ဖိုးအတွဲတစ်ခုချင်းစီ \((T_i, V_i)\) သည် အချိန် \(T_i\) တွင် source ၏တန်ဖိုးသည် \(V_i\) (in Volts or Amps) ဖြစ်ကြောင်း သတ်မှတ်သည်။ ကြားရှိအချိန်များအတွက် source ၏တန်ဖိုးကို input values များပေါ်တွင် linear interpolation ကိုအသုံးပြု၍ ဆုံးဖြတ်သည်။ parameter r သည် repeat time point ကိုသတ်မှတ်သည်။ r ကို -1 သို့မဟုတ် မပေးပါက \((T_i, V_i)\) တန်ဖိုးများအတွဲလိုက်ကို တစ်ကြိမ်သာထုတ်ပေးပြီး နောက်ဆုံးတန်ဖိုးတွင် output ရပ်သွားသည်။ r = 0 ဖြစ်ပါက၊ အချိန် 0 မှ အချိန် \(T_n\) အထိ sequence တစ်ခုလုံးကို အဆုံးမဲ့ ထပ်ခါထပ်ခါ လုပ်သည်။ r = 10ns ဖြစ်ပါက 10ns နှင့် 50ns ကြားရှိ sequence ကို အဆုံးမဲ့ ထပ်ခါထပ်ခါ လုပ်သည်။ r value သည် PWL sequence ၏ time points T1 မှ Tn ထဲမှ တစ်ခုဖြစ်ရမည်။ td ပေးပါက PWL sequence တစ်ခုလုံးကို td တန်ဖိုးဖြင့် delay လုပ်သည်။ ကျေးဇူးပြု၍ သတိပြုပါ- လက်ရှိတွင် r နှင့် td ကို voltage source ဖြင့်သာ ရရှိနိုင်ပြီး current source ဖြင့် မရနိုင်သေးပါ။

### ၄.၁.၅ Single-Frequency FM

**ယေဘူယျပုံစံ-**
```
SFFM(VO VA FM MDI FC TD PHASEM PHASEC)
```
**ဥပမာ-**
```spice
V1 12 0 SFFM(0 2 20 45 1k 1m 0 0)
```
| အမည် | Parameter | Default value | Units |
|------|-----------|---------------|-------|
| VO | Offset | - | V, A |
| VA | Amplitude | - | V, A |
| FM | Modulating frequency | 5/TSTOP | Hz |
| MDI | Modulation index | 90 | |
| FC | Carrier frequency | 500/TSTOP | Hz |
| TD | Signal delay | 0.0 | s |
| PHASEM | Modulation signal phase | 0.0 | degrees |
| PHASEC | Carrier signal phase | 0.0 | degrees |

waveform ကို အောက်ပါညီမျှခြင်းဖြင့် ဖော်ပြသည်-
\[
V(t) = V_O + V_A\cdot \sin (2\pi \cdot FC\cdot (t - TD) + MDI\sin (2\pi \cdot FM\cdot (t - TD) + PHASEM) + PHASEC)
\]
\(t > TD\) အတွက်၊ မဟုတ်ပါက \(V(t) = 0\)။

MDI ကို \(0 < = MDI < = FC / FM\) အထိ ကန့်သတ်ထားသည်။ VO နှင့် VA ကို အမြဲပေးရမည်။

### ၄.၁.၆ Amplitude modulated source (AM)

**ယေဘူယျပုံစံ-**
```
AM(VO VMO VMA FM FC TD PHASEM PHASEC)
```
**ဥပမာ-**
```spice
V1 12 0 AM(0.5 2 1.8 20K 5MEG 1m)
```
| အမည် | Parameter | Default value | Units |
|------|-----------|---------------|-------|
| VO | Overall offset | - | V, A |
| VMO | Modulation signal offset | - | V, A |
| VMA | Modulation signal amplitude | 1 | V, A |
| FM | Modulation signal frequency | 5/TSTOP | Hz |
| FC | Carrier signal frequency | 500/TSTOP | Hz |
| TD | Overall delay | 0.0 | s |
| PHASEM | Modulation signal phase | 0.0 | degrees |
| PHASEC | Carrier signal phase | 0.0 | degrees |

waveform ကို အောက်ပါညီမျှခြင်းဖြင့် ဖော်ပြသည်-
\[
V(t) = VO + (VMO + VMA\cdot \sin (2\pi \cdot FM\cdot (t - TD) + PHASEM)) \cdot \sin (2\pi \cdot FC\cdot (t - TD) + PHASEC)
\]
\(t > TD\) အတွက်၊ မဟုတ်ပါက \(V(t) = 0\)။

VO နှင့် VMO ကို အမြဲပေးရမည်။

Modulation depth ကို \(VMA / VMO\) ဖြင့်ပေးပြီး 0 နှင့် 1 ကြားပြောင်းလဲခြင်းဖြင့် standard amplitude modulated signal ကိုရရှိသည်။ VMO သည် signal သို့ overall multiplier အဖြစ်လည်းဆောင်ရွက်သည်။ အခြားတစ်ဖက်တွင် VMO ကို 0 သတ်မှတ်နိုင်ပြီး၊ double side band and suppressed carrier ပါသော signal ကိုရယူနိုင်သည်။

### ၄.၁.၇ Transient noise source

**ယေဘူယျပုံစံ-**
```
TRNOISE(NA NT NALPHA NAMP RTSAM RTSCAPT RTSEMT)
```
**ဥပမာများ-**
```spice
VNoiw 1 0 DC 0 TRNOISE(20n 0.5n 0 0)   ; white
VNoilof 1 0 DC 0 TRNOISE(0 10p 1.1 12p)  ; 1/f
VNoiwlof 1 0 DC 0 TRNOISE(20 10p 1.1 12p) ; white and 1/f
IALL 10 0 DC 0 trnoise(1m 1u 1.0 0.1m 15m 22u 50u) ; white, 1/f, RTS
```
Transient noise သည် (low frequency) transient noise injection and analysis ကိုခွင့်ပြုသော စမ်းသပ်ဆဲ feature တစ်ခုဖြစ်သည်။ အသေးစိတ်ရှင်းပြချက်အတွက် Chapt. 11.3.11 ကိုကြည့်ပါ။ NA သည် Gaussian noise rms voltage amplitude ဖြစ်ပြီး၊ NT သည် sample values များကြားရှိအချိန် (breakpoints များကို ဤတန်ဖိုး၏ multiples များတွင် enforce လုပ်မည်) ဖြစ်သည်။ NALPHA (frequency dependency အတွက် exponent) နှင့် NAMP (rms voltage or current amplitude) တို့သည် 1/f noise အတွက် parameters များဖြစ်သည်။ RTSAM သည် random telegraph signal amplitude၊ RTSCAPT သည် trap capture time ၏ exponential distribution ၏ mean ဖြစ်ပြီး RTSEMT သည် ၎င်း၏ emission time mean ဖြစ်သည်။ White Gaussian, 1/f, နှင့် RTS noise တို့ကို statement တစ်ခုတည်းတွင် ပေါင်းစပ်နိုင်သည်။

| အမည် | Parameter | Default value | Units |
|------|-----------|---------------|-------|
| NA | Rms noise amplitude (Gaussian) | - | V, A |
| NT | Time step | - | sec |
| NALPHA | 1/f exponent | 0 < α < 2 | - |
| NAMP | Amplitude (1/f) | - | V, A |
| RTSAM | Amplitude | - | V, A |
| RTSCAPT | Trap capture time | - | sec |
| RTSEMT | Trap emission time | - | sec |

NT နှင့် RTSAM ကို 0 သတ်မှတ်ပါက၊ noise option TRNOISE ... ကို လျစ်လျူရှုသည်။ ထို့ကြောင့် သင်သည် individual voltage source VNOI ၏ noise contribution ကို အောက်ပါ command ဖြင့် ပိတ်နိုင်သည်-
```
alter @vnoi[trnoise] = [0 0 0 0]   ; no noise
alter @vrtst[trnoise] = [0 0 0 0 0 0 0] ; no noise
```
alert command အတွက် Chapt. 13.5.3 ကိုကြည့်ပါ။

TRNOISE noise sources အားလုံးကို ပိတ်ရန် `set notnoise` ကို သင်၏ .spiceinit file (သင်၏ simulations အားလုံးအတွက်) သို့မဟုတ် နောက် run သို့မဟုတ် tran command ရှေ့တွင် သင်၏ control section ထဲသို့ ထည့်နိုင်သည်။ `unset notnoise` command သည် noise sources အားလုံးကို ပြန်လည်အသက်ဝင်စေမည်။

Noise generators များကို independent voltage (vsrc) and current (iscr) sources များအတွင်းသို့ အကောင်အထည်ဖော်ထားသည်။

### ၄.၁.၈ Random voltage source

TRRANDOM option သည် ngspice random number generator မှ ဆင်းသက်လာသော statistically distributed voltage values များကို ထုတ်ပေးသည်။ ဤ values များကို circuit တစ်ခုအတွင်း transient simulation တွင် တိုက်ရိုက်အသုံးပြုနိုင်သည်၊ ဥပမာ specific noise voltage တစ်ခုထုတ်လုပ်ရန်၊ သို့သော် အထူးသဖြင့် ၎င်းတို့ကို behavioral sources (B, E, G sources 5, voltage controllable A sources 8, capacitors 3.3.9, inductors 3.3.13, or resistors 3.3.4) များ၏ control တွင် statistically varying device parameters များအပေါ် circuit dependence ကို simulate လုပ်ရန် အသုံးပြုနိုင်သည်။ Monte-Carlo simulation ကို simulation run တစ်ခုတည်းတွင် ကိုင်တွယ်နိုင်သည်။

**ယေဘူယျပုံစံ-**
```
TRRANDOM(TYPE TS <TD <PARAM1 <PARAM2>>>)
```
**ဥပမာများ-**
```spice
VR1 r1 0 dc 0 trrandom (2 10m 0 1) ; Gaussian with mean 0
V1 1 0 dc 0 trrandom (1 1u 0.5u 0.5 0.5) ; Uniform between 0 and 1
```
| TYPE | Distribution | PARAM1 | PARAM2 |
|------|--------------|--------|--------|
| 1 | Uniform | Mean | Span |
| 2 | Gaussian | Mean | Standard Deviation |
| 3 | Exponential | Mean | |
| 4 | Poisson | Mean | |
| 5 | Limit (binary) | Low value | High value |

TS သည် sample time ဖြစ်ပြီး TD သည် delay time ဖြစ်သည်။ Values များကို sample points များကြားရှိအချိန်များအတွက် linearly interpolate လုပ်သည်။

### ၄.၁.၉ External voltage or current input

Ngspice shared library mode (Chapter 15 ကိုကြည့်ပါ) တွင်၊ external voltage or current input ကို EXTERNAL keyword ဖြင့် သတ်မှတ်နိုင်သည်။ ၎င်းသည် simulator အား simulation ၏အဆင့်တိုင်းတွင် voltage or current value အတွက် caller သို့ callback ပြန်ခေါ်ရန် ညွှန်ကြားသည်။

**ယေဘူယျပုံစံ-**
```
VXXXXXXX N+ N- EXTERNAL
IYYYYYYY N+ N- EXTERNAL
```
**ဥပမာ-**
```spice
Vext 1 0 EXTERNAL
Iext 2 3 EXTERNAL
```
ဤ feature ကို shared ngspice interface မှတစ်ဆင့် ပံ့ပိုးထားပြီး၊ caller သည် simulation runtime အတွင်း arbitrary time-varying inputs များကို ထောက်ပံ့ခွင့်ပြုသည်။

### ၄.၁.၁၀ Arbitrary Phase Sources

Phase shift ကို independent source card တွင် နောက်ဆုံး parameter အဖြစ် သတ်မှတ်သည်။ အောက်ပါ နှစ်ခုလုံးအတွက် Phase shift ကို +45 degrees အဖြစ် သတ်မှတ်ထားသည်-
```spice
v1 1 0 0.0 sin(0 1 1k 0 0 45.0)
r1 1 0 1k
v2 2 0 0.0 pulse(-1 1 0 1e-5 1e-5e-4 1e-3 45.0)
r2 2 0 1k
```
sinusoidal source အတွက် phase parameter သည် ၄.၁.၂ တွင် ဖော်ပြထားသည့်အတိုင်း အလုပ်လုပ်သည်။ pulse source အတွက်၊ phase shift ကို degrees ဖြင့် နောက်ဆုံး parameter အဖြစ် သတ်မှတ်ပြီး၊ သတ်မှတ်ထားသော delay time TD နှင့် ပေါင်းထားသည်၊ ဆိုလိုသည်မှာ phase သည် additional delay (positive phase) သို့မဟုတ် advance (negative phase) ကို ပေးသည်။

### ၄.၁.၁၁ RF Port

Voltage source VSRC ကို RF Port အဖြစ် သတ်မှတ်နိုင်သည်။ ထိုသို့လုပ်ရန်၊ အနည်းဆုံး နောက်ထပ် parameter နှစ်ခု လိုအပ်သည်။ ပထမမှာ portnum (integer) ဖြစ်ပြီး ၎င်းသည် VSRC ကို RF Port အဖြစ် သတ်မှတ်သည်။ RF ports များအဖြစ် သတ်မှတ်ထားသော VSRCs အားလုံး၏ Portnum သည် 1 မှစတင်ပြီး RF ports အရေအတွက်အထိ ရေတွက်ရမည်။ duplicate portnums များ မရှိရပါ။ ထို့နောက် Z0 (real) သည် internal impedance ကို သတ်မှတ်သည်။ မပေးပါက၊ ၎င်း၏ default value သည် 50Ohm ဖြစ်သည်။ RF ports တစ်ခု ကြေငြာသောအခါ၊ VSRC သည် Z0 Ohm series ပါသော VSRC ဖြစ်လာသည်။ ဤ extra resistor သည် simulations အားလုံးကို သက်ရောက်သည်။

**ယေဘူယျပုံစံ-**
```
VXXXXXXX N+ N- <DC> <AC> <other parameters> PORTNUM=portnum Z0=z0
```
**ဥပမာ-**
```spice
V1 1 0 dc 0 ac 1 PORTNUM=1 Z0=50
V2 2 0 dc 0 ac 1 PORTNUM=2 Z0=75
```
Command .sp (11.3.8) ဖြင့် S-parameter simulation အတွက် အနည်းဆုံး port နှစ်ခု လိုအပ်သည်။ portnum ကို မပေးပါက voltage source VSRC သည် ပုံမှန်အတိုင်း လုပ်ဆောင်သည်။

## ၄.၂ Linear Dependent Source များ

Ngspice သည် အောက်ပါညီမျှခြင်းလေးခုမှ တစ်ခုခုဖြင့် သွင်ပြင်လက္ခဏာပြသော linear dependent sources များကို circuits များတွင် ပါဝင်ခွင့်ပြုသည်-
\[
|i = gv| \quad v = ev \quad i = fi \quad v = hi
\]
၎င်းတို့တွင် \(g\), \(e\), \(f\), နှင့် \(h\) တို့သည် transconductance, voltage gain, current gain, နှင့် transresistance တို့ကိုအသီးသီးကိုယ်စားပြုသော constants များဖြစ်သည်။ Voltages သို့မဟုတ် currents အတွက် Non-linear dependent sources (B, E, G) များကို Chapt. 5 တွင် ဖော်ပြထားသည်။

### ၄.၂.၁ Gxxxx- Linear Voltage-Controlled Current Sources (VCCS)

**ယေဘူယျပုံစံ-**
```
GXXXXXXX N+ N- NC+ NC- VALUE <m=val>
```
**ဥပမာ-**
```spice
G1 2 0 5 0 0.1
```
n+ နှင့် n- သည် positive နှင့် negative nodes များဖြစ်သည်။ Current flow သည် positive node မှ source မှတစ်ဆင့် negative node သို့ဖြစ်သည်။ nc+ နှင့် nc- သည် positive နှင့် negative controlling nodes များဖြစ်သည်။ value သည် transconductance (in mhos) ဖြစ်သည်။ m သည် output current သို့ စိတ်ကြိုက် multiplier တစ်ခုဖြစ်သည်။ val သည် numerical value သို့မဟုတ် အခြား parameters များကိုရည်ညွှန်းချက်များပါဝင်သော 2.11.5 အရ expression တစ်ခုဖြစ်နိုင်သည်။ Instance parameters များကို chapt. 27.3.6 တွင် ဖော်ပြထားသည်။

### ၄.၂.၂ Exxxx- Linear Voltage-Controlled Voltage Sources (VCVS)

**ယေဘူယျပုံစံ-**
```
EXXXXXXX N+ N- NC+ NC- VALUE
```
**ဥပမာ-**
```spice
E1 2 3 14 1 2.0
```
n+ သည် positive node ဖြစ်ပြီး n- သည် negative node ဖြစ်သည်။ nc+ နှင့် nc- သည် positive နှင့် negative controlling nodes များဖြစ်သည်။ value သည် voltage gain ဖြစ်သည်။ Instance parameters များကို chapt. 27.3.7 တွင် ဖော်ပြထားသည်။

### ၄.၂.၃ Fxxxx- Linear Current-Controlled Current Sources (CCCS)

**ယေဘူယျပုံစံ-**
```
FXXXXXXX N+ N- VNAM VALUE <m=val>
```
**ဥပမာ-**
```spice
F1 13 5 VSENS 5 m=2
```
n+ နှင့် n- သည် positive နှင့် negative nodes များဖြစ်သည်။ Current flow သည် positive node မှ source မှတစ်ဆင့် negative node သို့ဖြစ်သည်။ vnam သည် controlling current စီးဆင်းသည့် voltage source ၏အမည်ဖြစ်သည်။ positive controlling current flow ၏ direction သည် vnam ၏ positive node မှ source မှတစ်ဆင့် negative node သို့ဖြစ်သည်။ value သည် current gain ဖြစ်သည်။ m သည် output current သို့ စိတ်ကြိုက် multiplier တစ်ခုဖြစ်သည်။ Instance parameters များကို chapt. 27.3.4 တွင် ဖော်ပြထားသည်။

### ၄.၂.၄ Hxxxx- Linear Current-Controlled Voltage Sources (CCVS)

**ယေဘူယျပုံစံ-**
```
HXXXXXXX N+ N- VNAM VALUE
```
**ဥပမာ-**
```spice
HX 5 17 VZ 0.5K
```
n+ နှင့် n- သည် positive နှင့် negative nodes များဖြစ်သည်။ vnam သည် controlling current စီးဆင်းသည့် voltage source ၏အမည်ဖြစ်သည်။ positive controlling current flow ၏ direction သည် vnam ၏ positive node မှ source မှတစ်ဆင့် negative node သို့ဖြစ်သည်။ value သည် transresistance (in ohms) ဖြစ်သည်။ Instance parameters များကို chapt. 27.3.5 တွင် ဖော်ပြထားသည်။

### ၄.၂.၅ Polynomial Source Compatibility

SPICE2G6 တွင်ရရှိနိုင်သော Dependent polynomial sources များကို XSPICE extension (21.1) ကိုအသုံးပြု၍ ngspice တွင် အပြည့်အဝထောက်ပံ့ထားသည်။ ဤ sources များကိုသတ်မှတ်ရာတွင်အသုံးပြုသောပုံစံကို Table 4.1 တွင်ပြထားသည်။ ၎င်း၏အသုံးပြုမှုအသေးစိတ်အတွက် Chapt. 5.5 ကိုကျေးဇူးပြု၍ကြည့်ပါ။

| Dependent Polynomial Sources | Source Type | Instance Card |
|------------------------------|-------------|----------------|
| POLYNOMIAL VCVS | E | XXXXXXXX N+ N- POLY(ND) NC1+ NC1- P0 (P1...) |
| POLYNOMIAL VCCS | G | XXXXXXXX N+ N- POLY(ND) NC1+ NC1- P0 (P1...) |
| POLYNOMIAL CCCS | F | XXXXXXXX N+ N- POLY(ND) VNAM1 !VNAM1...? P0 (P1...) |
| POLYNOMIAL CCVS | H | XXXXXXXX N+ N- POLY(ND) VNAM1 !VNAM1...? P0 (P1...) |

Table 4.1: Dependent Polynomial Sources
