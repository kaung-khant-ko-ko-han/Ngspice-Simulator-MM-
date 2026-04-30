# ၅။ Non-linear Dependent Source များ (Behavioral Sources)

ဤအခန်းတွင် ဖော်ပြထားသော non-linear dependent sources B (Chapt. 5.1)၊ E (5.2)၊ G (5.3) တို့သည် သင်္ချာဆိုင်ရာ expression တစ်ခုကို အကဲဖြတ်ခြင်းမှ ရလဒ် voltages သို့မဟုတ် currents များ ထုတ်လုပ်ခွင့်ပြုသည်။ အတွင်းပိုင်းတွင် E နှင့် G sources များကို ပိုမိုယေဘူယျကျသော B source အဖြစ်သို့ ပြောင်းလဲသည်။ ဤ source သုံးခုစလုံးကို behavioral modeling and analysis ကို မိတ်ဆက်ရန် အသုံးပြုနိုင်သည်။

## ၅.၁ Bxxxx- Nonlinear dependent source (ASRC)

### ၅.၁.၁ Syntax နှင့် အသုံးပြုပုံ

**ယေဘူယျပုံစံ-**
```
BXXXXXX n+ n- <i=expr> <v=expr> <tcl=value> <tc2=value>
+ <temp=value> <dtemp=value>
```
**ဥပမာများ-**
```spice
B1 0 1 I=cos(v(1))+sin(v(2))
B2 0 1 V=ln(cos(log(v(1,2)^2)))-v(3)^4+v(2)^v(1)
B3 3 4 I=17
B4 3 4 V=exp(pi^i(vdd))
B5 2 0 V = V(1) < {Vlow} ? {Vlow} :
+ V(1) > {Vhigh} ? {Vhigh} : V(1)
```
n+ သည် positive node ဖြစ်ပြီး၊ n- သည် negative node ဖြစ်သည်။ V နှင့် I parameters များ၏တန်ဖိုးများသည် device ဖြတ်သန်းစီးဆင်းသော voltages နှင့် currents များကို အသီးသီး ဆုံးဖြတ်ပေးသည်။ I ကိုပေးပါက device သည် current source ဖြစ်ပြီး၊ V ကိုပေးပါက device သည် voltage source ဖြစ်သည်။ ဤ parameters များထဲမှ တစ်ခုနှင့် တစ်ခုသာ ပေးရမည်။ Instance parameters အားလုံးကို chapter 27.3.1 တွင် ဖော်ပြထားသည်။

temperature behavior အတွက် ရိုးရှင်းသော model ကို အောက်ပါဖော်မြူလာဖြင့် အကောင်အထည်ဖော်သည်-
\[
I(T) = I(\mathrm{TNOM})\left(1 + TC_1(T - TNOM) + TC_2(T - TNOM)^2\right)
\]
\[
V(T) = V(\mathrm{TNOM})\left(1 + TC_1(T - \mathrm{TNOM}) + TC_2(T - \mathrm{TNOM})^2\right)
\]
အထက်ဖော်မြူလာတွင် \(T\) သည် instance temperature ကိုကိုယ်စားပြုပြီး ၎င်းကို temp keyword သုံး၍ အတိအလင်းသတ်မှတ်နိုင်သည် သို့မဟုတ် circuit temperature နှင့် dtemp ကိုသုံး၍ တွက်ချက်နိုင်သည်။ temp နှင့် dtemp နှစ်ခုလုံးကို သတ်မှတ်ပါက နောက်တစ်ခုကို လျစ်လျူရှုသည်။

nonlinear source ၏ small-signal AC behavior သည် DC operating point ရှိ source ၏ derivative (သို့မဟုတ် derivatives) နှင့် ညီမျှသော proportionality constant ပါသည့် linear dependent source (သို့မဟုတ် sources) ဖြစ်သည်။ V နှင့် I အတွက် ပေးထားသော expressions များသည် system အတွင်းရှိ voltage sources များမှ voltages နှင့် currents များ၏ မည်သည့် function မဆို ဖြစ်နိုင်သည်။

အောက်ပါ single real variable ၏ function များကို သတ်မှတ်ထားသည်-
- Trigonometric functions: cos, sin, tan, acos, asin, atan
- Hyperbolic functions: cosh, sinh, acosh, asinh, atanh
- Exponential and logarithmic: exp, ln, log, log10 (ln, log with base e, log10 with base 10)
- Other: abs, sqrt, u, u2, uramp, floor, ceil, i
- Functions of two variables: min, max, pow, **, pwr, ^
- Functions of three variables: a ? b:c

convergence အကြောင်းပြချက်များအတွက် 'exp' function သည် ၎င်း၏ argument အတွက် 14 ကန့်သတ်ချက်ရှိပြီး၊ ထိုတန်ဖိုးထက်ကျော်လွန်ပါက linearly တိုးမည်။ 'u' function သည် unit step function ဖြစ်ပြီး၊ argument သည် သုညထက်ကြီးပါက တစ်၊ သုညတွင် 0.5၊ သုညအောက်ငယ်ပါက သုညတန်ဖိုး ရှိသည်။ 'u2' function သည် argument သည် သုညအောက်ငယ်ပါက သုည၊ တစ်ထက်ကြီးပါက တစ်၊ ဤကန့်သတ်ချက်များကြားတွင် argument ၏တန်ဖိုးကို ယူသည့် တန်ဖိုး ပြန်ပေးသည်။ 'uramp' function သည် unit step ၏ integral ဖြစ်သည်- input x အတွက် x သည် သုညအောက်ငယ်ပါက သုည၊ သို့မဟုတ် x သည် သုညနှင့် ညီ သို့မဟုတ် ကြီးပါက x ဖြစ်သည်။ ဤ function သုံးခုသည် piece-wise non-linear functions များကို synthesizing လုပ်ရာတွင် အသုံးဝင်သော်လည်း convergence ဆိုးကျိုးသက်ရောက်နိုင်သည်။

i(xyz) function သည် device instance xyz ၏ ပထမ node မှတစ်ဆင့် current ကို ပြန်ပေးသည်။

အောက်ပါ standard operators များကို သတ်မှတ်ထားသည်- +, -, *, /, ^, unary -

Logical operators: !=, <>, >=, <=, ==, >, <, ||, &&, !

Ternary function ကို a ? b : c ဟု သတ်မှတ်သည်၊ ဆိုလိုသည်မှာ IF a, THEN b, ELSE c ဖြစ်သည်။ parser က အခြား tokens များနှင့် ခွဲခြားနိုင်စေရန် '?' ၏ရှေ့တွင် space တစ်ခုထားရန် သေချာပါစေ။

B source functions pow, **, ^, နှင့် pwr တို့သည် x1 တွင် undefined regions များကို ရှောင်ရှားရန် အထူးဂရုစိုက်ရန် လိုအပ်သည်၊ အဘယ်ကြောင့်ဆိုသော် ၎င်းတို့သည် သာမန် mathematical usage (နှင့် chapt. 2.11.5 တွင် ဖော်ပြထားသော functions) နှင့် ကွဲပြားသောကြောင့်ဖြစ်သည်။

y = pow(x1,x2), x1**x2, နှင့် x1^x2 အားလုံး (y = x1^x2 ကိုဖော်ပြသည်) ကို အောက်ပါအတိုင်း ဖြေရှင်းသည်-
\[
y = \mathrm{pow(fabs(x1),x2)}
\]
ဤနေရာတွင် pow သည် standard C math library function ဖြစ်သည်။

y = pwr(x1,x2) ကို အောက်ပါအတိုင်း ဖြေရှင်းသည်-
if x1 < 0.0: y = (-pow(-x1,x2))
else: y = (pow(x1,x2))

ဤနေရာတွင် pow သည်လည်း standard C math library function ဖြစ်သည်။

**ဥပမာ- Ternary function**
```spice
* B source test Clamped voltage source
* C. P. Basso "Switched-mode power supplies", New York, 2008
.param Vhigh = 4.6
.param Vlow = 0.4
Vin1 1 0 DC 0 PWL(0 0 lu 5)
Bcl 2 0 V = V(1) < Vlow ? Vlow : V(1) > Vhigh ? Vhigh : V(1)
.control
unset askquit
tran 5n lu
plot V(2) vs V(1)
.endc
.end
```
log, ln, or sqrt ၏ argument သည် သုညအောက်နည်းသွားပါက argument ၏ absolute value ကို အသုံးပြုသည်။ divisor တစ်ခု သုညဖြစ်သွားပါက သို့မဟုတ် log or ln ၏ argument သုညဖြစ်သွားပါက error ဖြစ်ပေါ်မည်။ partial derivative တစ်ခုအတွင်းရှိ function တစ်ခု၏ argument သည် ထို function undefined ဖြစ်သော region တစ်ခုသို့ ဝင်ရောက်သည့်အခါ အခြားပြဿနာများ ဖြစ်ပေါ်နိုင်သည်။

Parameters များကို အထက်ပါဥပမာတွင်ပြထားသည့် {Vlow} ကဲ့သို့ အသုံးပြုနိုင်သည်။ Parameters များကို circuit setup လုပ်ချိန်တွင် အကဲဖြတ်မည်ဖြစ်ပြီး၊ V(1) ကဲ့သို့သော vectors များကို simulation အတွင်း အကဲဖြတ်မည်။

Expression ထဲသို့ time ကိုရယူရန် constant current source တစ်ခုမှ current ကို capacitor တစ်ခုဖြင့် integrate လုပ်ပြီး ရလာသော voltage ကို အသုံးပြုနိုင်သည် (capacitor ဖြတ်သန်း initial voltage ကို သတ်မှတ်ရန် မမေ့ပါနှင့်)။

Non-linear resistors, capacitors, and inductors များကို nonlinear dependent source ဖြင့် synthesize လုပ်နိုင်သည်။ Nonlinear resistors, capacitors and inductors များကို nonlinear dependent source ဖြင့် အကောင်အထည်ဖော်ထားသော variables များ၏ change ဖြင့် ၎င်းတို့၏ linear counterparts များဖြင့် အကောင်အထည်ဖော်သည်။ အောက်ပါ subcircuit သည် nonlinear capacitor တစ်ခုကို အကောင်အထည်ဖော်လိမ့်မည်-

**ဥပမာ- Non linear capacitor**
```spice
.Subckt nlcap pos neg
* Bx: calculate f(input voltage)
Bx 1 0 v = f(v(pos,neg))
* Cx: linear capacitance
Cx 2 0 1
* Vx: Ammeter to measure current into the capacitor
Vx 2 1 DC 0Volts
* Drive the current through Cx back into the circuit
Fx pos neg Vx 1
.ends
```
f(v(pos,neg)) အတွက် ဥပမာ-
```spice
Bx 1 0 V = v(pos,neg) * v(pos,neg)
```
Non-linear resistors သို့မဟုတ် inductors များကို အလားတူပုံစံဖြင့် ဖော်ပြနိုင်သည်။ ဤ template ကိုအသုံးပြုထားသော nonlinear resistor အတွက် ဥပမာတစ်ခုကို အောက်တွင်ပြထားသည်။

**ဥပမာ- Non linear resistor**
```spice
* use of 'hertz' variable in nonlinear resistor
*
.param rbase = 1k
* some tests
B1 1 0 V = hertz * v(33)
B2 2 0 V = v(33) * hertz
b3 3 0 V = 6.283e3/(hertz+6.283e3) * v(33)
V1 33 0 DC 0 AC 1
* Translate R1 10 0 R = '1k/sqrt(HERTZ)' to B source
.Subckt nIres pos neg rb = rbase
* Bx: calculate f(input voltage)
Bx 1 0 v = - 1 / {rb} / sqrt(HERTZ) * v(pos, neg)
* Rx: linear resistance
Rx 2 0 1
* Vx: Ammeter to measure current into the resistor
Vx 2 1 DC 0Volts
* Drive the current through Rx back into the circuit
Fx pos neg Vx 1
.ends
Xres 33 10 nIres rb=1k
*Rres 33 10 1k
Vres 10 0 DC 0
.control
define check(a,b) vecmax(abs(a - b))
ac lin 10 100 1k
* some checks
print v(1) v(2) v(3)
if check(v(1), frequency) < 1e-12
echo "INFO: ok"
end
plot vres#branch
.end
.end
```

### ၅.၁.၂ Special B-Source Variables time, temper, hertz

Special variables time နှင့် temper တို့ကို transient analysis တစ်ခုတွင် ရရှိနိုင်ပြီး၊ actual simulation time နှင့် circuit temperature ကို ထင်ဟပ်စေသည်။ temper သည် circuit temperature ကို degree C ဖြင့် ပြန်ပေးသည် (2.14 ကိုကြည့်ပါ)။ hertz variable ကို AC analysis တစ်ခုတွင် ရရှိနိုင်သည်။ time သည် AC analysis တွင် သုညဖြစ်ပြီး၊ hertz သည် transient analysis အတွင်း သုညဖြစ်သည်။ hertz variable ကို အသုံးပြုခြင်းသည် large circuit တစ်ခုရှိပါက CPU time အချို့ ကုန်ကျနိုင်သည်၊ အဘယ်ကြောင့်ဆိုသော် frequency တစ်ခုချင်းစီအတွက် AC response ကို မတွက်ချက်မီ operating point ကို ဆုံးဖြတ်ရသောကြောင့်ဖြစ်သည်။

### ၅.၁.၃ par('expression')

B source syntax ကို output lines များဖြစ်သော .plot တွင်လည်း output အတွက် algebraic expressions များအဖြစ် အသုံးပြုနိုင်သည် (Chapt.11.6.6 ကိုကြည့်ပါ)။

### ၅.၁.၄ Piecewise Linear Function: pwl

B source အမျိုးအစားနှစ်ခုစလုံးတွင် network variable တစ်ခု၏ piece-wise linear dependency ပါဝင်နိုင်သည်-
**ဥပမာ- pwl_current**
```spice
Bdio 1 0 I = pwl(v(A), 0, 0, 33, 10m, 100, 33m, 200, 50m)
```
v(A) သည် independent variable x ဖြစ်သည်။ နောက်မှလိုက်သော တန်ဖိုးအတွဲတစ်ခုချင်းစီသည် x,y functional relation ကိုဖော်ပြသည်- ဤဥပမာတွင် node A voltage 0V တွင် 0A current ထုတ်လုပ်သည် - နောက်တွဲတွင် node A ပေါ်တွင် 33V ရှိသောအခါ node 1 သို့ ground မှ 10mA စီးဆင်းသည်၊ စသည်ဖြင့်။

အလားတူ voltage sources အတွက်လည်း ဖြစ်နိုင်သည်-
**ဥပမာ- pwl_voltage**
```spice
Blimit b 0 V = pwl(v(1), -4, 0, -2, 2, 2, 4, 4, 5, 6, 5)
```
pwl definition တွင် independent variable ၏ monotony ကို စစ်ဆေးသည် - non-monotonic x entries များသည် program execution ကို ရပ်တန့်စေမည်။ v(1) ကို controlling current source ဖြင့် အစားထိုးနိုင်သည်၊ သို့မဟုတ် time (transient simulations အတွက်) ဖြင့် အစားထိုးနိုင်သည်။ v(1) ကို expression တစ်ခုဖြင့်လည်း အစားထိုးနိုင်သည်၊ ဥပမာ \(-2*i(V_{in})\)။ တန်ဖိုးအတွဲများသည် parameters များလည်း ဖြစ်နိုင်ပြီး၊ ၎င်းတို့ကို .param statement တစ်ခုဖြင့် ကြိုတင်သတ်မှတ်ထားရမည်။ ဤ options အားလုံးကို အသုံးပြုထားသော pwl function အတွက် ဥပမာကို အောက်တွင်ပြထားသည်။

**<center>Figure 5.1: pwl (piece-wise linear) B source</center>**

**ဥပမာ- pwl function in B source**
```spice
Demonstrates usage of the pwl function in an B source (ASRC)
* Also emulates the TABLE function with limits

.param x0 = -4 y0 = 0
.param x1 = -2 y1 = 2
.param x2 = 2 y2 = -2
.param x3 = 4 y3 = 1
.param xx0 = x0 - 1
.param xx3 = x3 + 1

Vin ctrl 0 DC=0V
R1 ctrl 0 2

* no limits outside of the tabulated x values
* (continues linearily)
Btest2 2 0 I = pwl(v(ctrl), 'x0', 'y0', 'x1', 'y1', 'x2', 'y2', 'x3', 'y3')

* like TABLE function with limits:
Btest3 3 0 I = (v(ctrl) < 'x0') ? 'y0' : (v(ctrl) < 'x3')
+ ? pwl(v(1), 'x0', 'y0', 'x1', 'y1', 'x2', 'y2', 'x3', 'y3') : 'y3'

* more efficient and elegant TABLE function with limits
* (voltage controlled):
Btest4 4 0 I = pwl(v(ctrl),
+ 'xx0', 'y0', 'x0', 'y0',
+ 'x1', 'y1',
+ 'x2', 'y2',
+ 'x3', 'y3', 'xx3', 'y3')

* more efficient and elegant TABLE function with limits
* (controlled by current):
Btest5 5 0 I = pwl(-2+ i(Vin),
+ 'xx0', 'y0', 'x0', 'y0',
+ 'x1', 'y1',
+ 'x2', 'y2',
+ 'x3', 'y3', 'xx3', 'y3')

Rint2 2 0 1
Rint3 3 0 1
Rint4 4 0 1
Rint5 5 0 1

.control
dc Vin -6 6 0.2
plot v(2) v(3) v(4)-0.5 v(5)+0.5
.endc
.end
```
ထူးခြားချက်တစ်ခု- controlling input (v(1) သို့မဟုတ် \(-2*i(V_{in})\)) သည် ပေးထားသော limits များ၏အပြင်ဘက်တွင် ရှိနေသည့်အခါ ဘာဖြစ်မည်နည်း။ ဥပမာ အထက်ပါဥပမာတွင် x0 ထက်ငယ်သည် သို့မဟုတ် x3 ထက်ကြီးသည် ဖြစ်နေလျှင်။ ပေးထားသော range ၏အပြင်ဘက်တွင် y values အသစ်များကို output curve ၏ slope ကိုတိုးချဲ့ခြင်းဖြင့် တွက်ချက်ထားသော x,y pairs များကို ထည့်သွင်းခြင်းဖြင့် ဆုံးဖြတ်မည်၊ ဥပမာ (y3 - y2) / (x3 - x2) ဖြင့်၊ ဥပမာ Btest2 မှ v(2) တွင် မြင်ရသည့်အတိုင်းဖြစ်သည်။ function ကို ကန့်သတ်လိုပါက နောက်ဆုံး y value ကို ထိန်းသိမ်းထားရန်၊ ဥပမာ y3၊ သင် အနည်းငယ် တိုးချဲ့ထားသော x နှင့် y ကို constant ထားသော နောက်ထပ် point (x,y pair) တစ်ခု ထည့်ရမည်၊ ဥပမာ x3 + 1, y3။

ဥပမာ pwl နှင့် behavioral resistor ကိုအသုံးပြုသည့်အခါ ၎င်းသည် အရေးကြီးလာသည်။ အောက်ပါဥပမာတွင် RR1 သည် (နှင့်ကျော်လွန်၍) 0 သို့ လျင်မြန်စွာ ရွေ့လျားသွားပြီး၊ ၎င်းသည် unphysical ဖြစ်ကာ transient simulation ပျက်ကွက်စေသည်၊ အဘယ်ကြောင့်ဆိုသော် RR1 မှတစ်ဆင့် current သည် unbounded ဖြစ်သောကြောင့်ဖြစ်သည်။ RR2 သည် 15.1ms,1 couple ဖြင့်ပေးထားသော limit ဖြင့် ထိုသို့သော malfunctioning ကို ရှောင်ရှားသည်။

**ဥပမာ- pwl function in behavioral resistor**
```spice
* pwl for behavioral R, transient sim
VU1 3 0 DC 9
RR1 3 0 R = pwl(time, 0, 1, 7m, 1, 8m, 1.19, 14m, 1.19, 15m, 1)
RR2 3 0 R = pwl(time, 0, 1, 7m, 1, 8m, 1.19, 14m, 1.19, 15m, 1,
+ 15.1m, 1)
.tran 100u 20m 0
.probe alli
.control
option nonint
run
display
set xbrushwidth=2
plot rr1#branch rr2#branch ylimit 7 17
.endc
.end
```

## ၅.၂ Exxxx- non-linear voltage source

### ၅.၂.၁ VOL

**ယေဘူယျပုံစံ-**
```
EXXXXXXX n+ n- vol='expr'
```
**ဥပမာ-**
```spice
E41 4 0 vol = 'V(3)*V(3) - 0ffs'
```
Expression သည် equation တစ်ခု သို့မဟုတ် node voltages သို့မဟုတ် branch currents (i(vm) ပုံစံဖြင့်) များပါဝင်သော expression တစ်ခုဖြစ်နိုင်ပြီး B source အတွက်ပေးထားသော Chapt. 5.1 တွင်ဖော်ပြထားသည့် အခြား terms များလည်းပါဝင်နိုင်သည်။ ၎င်းတွင် parameters (2.11.1) နှင့် special variables (5.1.2) တို့ ပါဝင်နိုင်သည်။

### ၅.၂.၂ VALUE

စိတ်ကြိုက် Syntax-
```spice
EXXXXXXX n+ n- value = 'expression'
```
ဥပမာ-
```spice
E41 4 0 value = 'V(3)*V(3) - 0ffs'
```
'=' sign သည် စိတ်ကြိုက်ဖြစ်သည်။

### ၅.၂.၃ TABLE

E- Source (Chapt. 5.2.3 ကိုကြည့်ပါ) နှင့် ဆင်တူသော syntax ဖြင့် tabulated listing မှ data entry ပြုလုပ်နိုင်သည်။

table မှ data entry အတွက် Syntax-
```
Exxx n1 n2 TABLE {expression} =
+ (x0, y0) (x1, y1) (x2, y2)
```
ဥပမာ (voltage control ဖြင့် current output ရှိသော simple comparator)-
```spice
ECMP 0 11 TABLE {V(10,9)} = (-5MV, 0V) (5MV, 5V)
R 11 0 1k
```

### ၅.၂.၄ POLY

Chapt. 5.5 ရှိ E- Source ကိုကြည့်ပါ။

### ၅.၂.၅ LAPLACE

လက်ရှိတွင် ngspice သည် LAPLACE option ဖြင့် တိုက်ရိုက် E- Source element ကို မပေးသော်လည်း၊ ၎င်းကို s_xfer (8.2.18 ကိုကြည့်ပါ) ဟုခေါ်သော XSPICE code model equivalent ဖြင့် အကောင်အထည်ဖော်ပြီး netlist ကို ပြန်လည်ရေးသားခြင်းဖြင့် အလိုအလျောက် invoke လုပ်သည်။ XSPICE option ကို enabled (28.1) လုပ်ထားရမည်။ AC (11.3.1) နှင့် transient analysis (11.3.10) ကို ထောက်ပံ့သည်။

အောက်ပါ E- Source-
```spice
ELOPASS 4 0 LAPLACE {V(1)} {5 * (s/100 + 1) / (s^2/42000 + s/60 + 1)}
```
ကို အောက်ပါအတိုင်း အစားထိုးနိုင်သည်-
```spice
AELOPASS 1 int_4 filter1
.model filter1 s_xfer(gain = 5 num_coeff = [{1/100} 1]
+ den_coeff = [{1/42000} {1/60} 1]
+ int_ic = [0 0])
ELOPASS 4 0 int_4 0 1
```
ဤတွင် input အဖြစ် node 1 ၏ voltage၊ intermediate output node int_4 နှင့် 'ELOPASS' အမည်ကို ဆက်လက်အသုံးပြုနိုင်ရန် buffer အဖြစ် E- source ပါဝင်သည်။

controlling expression သည် voltage node တစ်ခုထက် ပိုမိုရှုပ်ထွေးပါက၊ A- device သို့မ၀င်မီ expression ကိုအကဲဖြတ်ရန် B- Source (5.1) ကို ထည့်နိုင်သည်။

ရှုပ်ထွေးသော controlling expression ပါသော E- Source-
```spice
ELOPASS 4 0 LAPLACE {V(1)*v(2)} {10 / (s/6800 + 1)}
```
ကို အောက်ပါအတိုင်း အစားထိုးနိုင်သည်-
```spice
BELOPASS int_1 0 V = V(1)*v(2)
AELOPASS int_1 int_4 filter1
.model filter1 s_xfer(gain = 10
+ num_coeff = [1]
+ den_coeff = [{1/6800} 1]
+ int_ic = [0])
ELOPASS 4 0 int_4 0 1
```

### ၅.၂.၆ FREQ

လက်ရှိတွင် ngspice သည် FREQ option ဖြင့် တိုက်ရိုက် E- Source element ကို မပေးသော်လည်း ၎င်းကို xfer (8.2.19 ကိုကြည့်ပါ) ဟုခေါ်သော XSPICE code model equivalent ဖြင့် အကောင်အထည်ဖော်ပြီး netlist ကို ပြန်လည်ရေးသားခြင်းဖြင့် အလိုအလျောက် invoke လုပ်သည်။ XSPICE option ကို enabled (28.1) လုပ်ထားရမည်ဖြစ်ပြီး AC (11.3.1) analysis ကိုသာ ထောက်ပံ့သည်။

ဤ E- Source-
```spice
EXFER 1 0 FREQ {V(20,21)} = DB
+ (1.000000e+07Hz, 1.633257e-07, -1.859873e+01)
+ (1.025641e+08Hz, -4.165672e+00, -4.076855e+02)
+ (2.000000e+08Hz, -2.798303e-05, -7.519027e+02)
```
သည် simulation frequency (transfer function) ၏ complex-valued PWL function တစ်ခုဖြင့် input differential voltage (v(20, 21)) ကို မြှောက်ခြင်းဖြင့် ဆုံးဖြတ်သော complex voltage ကို ထုတ်လုပ်သည်။ DB keyword သည် ဒုတိယ column သည် gain (db) ဖြစ်ပြီး တတိယ column သည် phase (degrees) ဖြစ်ကြောင်း ညွှန်ပြသည်။ Alternative keywords များမှာ MAG (linear gain), RAD (phase in radians), DEG (phase in degrees, already the default) or R_I (real and imaginary parts) တို့ဖြစ်သည်။

### ၅.၂.၇ AND/OR/NAND/NOR

E- source ၏ ဤပုံစံသည် analog inputs and output ပါသော basic logic gates များ၏ ရိုးရှင်းသော behavioural implementations များကို ပေးသည်။ ၎င်းကို multi_input_pwl (8.2.10 ကိုကြည့်ပါ) ဟုခေါ်သော XSPICE code model ဖြင့် အကောင်အထည်ဖော်ပြီး netlist ကို ပြန်လည်ရေးသားခြင်းဖြင့် အလိုအလျောက် invoke လုပ်သည်။ XSPICE option ကို enabled (28.1) လုပ်ထားရမည်။

ဤ E- Source-
```spice
EAND out1 out0 and(2) in1 0 in2 0 (0.5, 0) (2.8, 3.3)
```
သည် differential input voltages များထဲမှ အသေးဆုံးကို ရွေးချယ်၍ PWL output function တစ်ခုကို ကျင့်သုံးခြင်းဖြင့် ဆုံးဖြတ်သော differential output voltage ကို ထုတ်လုပ်သည်။ ဤတွင် "and(2)" သည် logic function နှင့် PWL points အရေအတွက်ကို ဆုံးဖြတ်သည်- minimum input voltage 0.5 အောက်အတွက် output သုညဖြစ်ပြီး inputs 2.8 အထက်အတွက် 3.3၊ ၎င်းတို့ကြားတွင် linear ramp ဖြင့်။ အခြား function သုံးခုမှာ အလားတူဖြစ်သည်- "or" သည် အကြီးဆုံး input ကိုရွေးချယ်ပြီး "nand/nor" သည် PWL points များ၏အစဉ်ကို ပြောင်းပြန်လုပ်သည်။ points နှစ်ခုသာ ထောက်ပံ့သည်။

examples/digital/compare/adder_esource.cir တွင် နမူနာ circuit တစ်ခုကို တွေ့နိုင်သည်။

## ၅.၃ Gxxxx- non-linear current source

### ၅.၃.၁ CUR

**ယေဘူယျပုံစံ-**
```
GXXXXXX n+ n- cur='expr' <m=val>
```
**ဥပမာ-**
```spice
G51 55 225 cur = 'V(3) * V(3) - 0ffs'
```
Expression သည် equation တစ်ခု သို့မဟုတ် node voltages သို့မဟုတ် branch currents (i(vm) ပုံစံဖြင့်) များပါဝင်သော expression တစ်ခုဖြစ်နိုင်ပြီး B source အတွက်ပေးထားသော Chapt. 5.1 တွင်ဖော်ပြထားသည့် အခြား terms များလည်းပါဝင်နိုင်သည်။ ၎င်းတွင် parameters (2.11.1) နှင့် special variables (5.1.2) တို့ ပါဝင်နိုင်သည်။ m သည် output current သို့ စိတ်ကြိုက် multiplier တစ်ခုဖြစ်သည်။ val သည် numerical value သို့မဟုတ် အခြား parameters များကိုသာ (node voltages or branch currents မပါ) ရည်ညွှန်းချက်များပါဝင်သော 2.11.5 အရ expression တစ်ခုဖြစ်နိုင်သည်၊ အဘယ်ကြောင့်ဆိုသော် ၎င်းကို simulation မစတင်မီ အကဲဖြတ်သောကြောင့်ဖြစ်သည်။

### ၅.၃.၂ VALUE

စိတ်ကြိုက် syntax-
```spice
G51 55 225 value = 'V(3) * V(3) - 0ffs'
```
'=' sign သည် စိတ်ကြိုက်ဖြစ်သည်။

### ၅.၃.၃ TABLE

E- Source (Chapt. 5.2.3 ကိုကြည့်ပါ) နှင့် ဆင်တူသော syntax ဖြင့် tabulated listing မှ data entry ကို ရရှိနိုင်သည်။

table မှ data entry အတွက် Syntax-
```
Gxxx n1 n2 TABLE {expression} =
+ (x0, y0) (x1, y1) (x2, y2) <m=val>
```
ဥပမာ (voltage control ဖြင့် current output ရှိသော simple comparator)-
```spice
GCMP 0 11 TABLE {V(10,9)} = (-5MV, 0V) (5MV, 5V)
R 11 0 1k
```
m သည် output current သို့ စိတ်ကြိုက် multiplier တစ်ခုဖြစ်သည်။ val သည် numerical value သို့မဟုတ် အခြား parameters များကိုသာ (node voltages or branch currents မပါ) ရည်ညွှန်းချက်များပါဝင်သော 2.11.5 အရ expression တစ်ခုဖြစ်နိုင်သည်၊ အဘယ်ကြောင့်ဆိုသော် ၎င်းကို simulation မစတင်မီ အကဲဖြတ်သောကြောင့်ဖြစ်သည်။ '=' sign သည် keyword TABLE ၏နောက်တွင် လိုက်နိုင်သည်။

### ၅.၃.၄ POLY

Chapt. 5.5 ရှိ E- Source ကိုကြည့်ပါ။

### ၅.၃.၅ LAPLACE

equivalent code model အစားထိုးမှုအတွက် E- Source, Chapt. 5.2.5 ကိုကြည့်ပါ။

### ၅.၃.၆ FREQ

equivalent code model အစားထိုးမှုအတွက် E- Source, Chapt. 5.2.6 ကိုကြည့်ပါ။

### ၅.၃.၇ ဥပမာ

နမူနာ ဖိုင်ကို အောက်တွင်ဖော်ပြထားသည်။

```spice
VCCS, VCVS, non-linear dependency
.param Vi=1
.param Offs='0.01*Vi'
*VCCS depending on V(3)
B21 int1 0 V = V(3)*V(3)
G1 21 22 int1 0 1
* measure current through VCCS
vm 22 0 dc 0
R21 21 0 1
* new VCCS depending on V(3)
G51 55 225 cur = 'V(3)*V(3)-0ffs'
* measure current through VCCS
vm5 225 0 dc 0
R51 55 0 1
* VCVS depending on V(3)
B31 int2 0 V = V(3)*V(3)
E1 1 0 int2 0 1
R1 1 0 1
* new VCVS depending on V(3)
E41 4 0 vol = 'V(3)*V(3)-0ffs'
R4 4 0 1
* control voltage
V1 3 0 PWL(0 0 100u {Vi})
.control
unset askquit
tran 10n 100u uic
plot i(E1) i(E41)
plot i(vm) i(vm5)
.endc
.end
```

## ၅.၄ Behavioral source တစ်ခုကို debug လုပ်ခြင်း

B, E, G, sources များနှင့် behavioral R, C, L elements များသည် user defined models များ set up လုပ်ရန် အစွမ်းထက်သောကိရိယာများဖြစ်သည်။ ကံမကောင်းစွာဖြင့် ဤ models များကို debugging လုပ်ခြင်းသည် အလွန်အဆင်ပြေမှုမရှိပါ။

bug (log(-2)) ပါသော နမူနာ input file-
```spice
B source debugging

V1 1 0 1
V2 2 0 -2

E41 4 0 vol = 'V(1)*log(V(2))'

.control
tran 1 1
.endc
.end
```
အထက်ပါ input file သည် အောက်ပါ error message ကိုဖြစ်ပေါ်စေသည်-
```
Error: -2 out of range for log
```
ဤသေးငယ်သော ဥပမာတွင် bug ၏အကြောင်းရင်းနှင့်တည်နေရာမှာ သိသာသည်။ သို့သော်၊ behavior sources များစွာနှင့် log function ၏ occurrences များစွာကိုအသုံးပြုထားပါက debugging လုပ်ရန် မဖြစ်နိုင်သလောက်ဖြစ်သည်။

သို့သော် variable ngdebug (13.7 ကိုကြည့်ပါ) ကို (ဥပမာ file .spiceinit တွင်) သတ်မှတ်ထားပါက buggy parameter ၏တည်နေရာနှင့်တန်ဖိုးကို ဖော်ပြပေးမည့် (အနည်းငယ် အနီးကပ်စစ်ဆေးပြီးနောက်) ပိုမိုထင်ရှားသော error message ကို ထုတ်ပေးသည်။

bug (log(-2)) ပါသော input file အတွက် အသေးစိတ် error message-
```
Error: -2 out of range for log
calling PTeval, tree = (v0) * (log (v1))
d/dv0: log (v1)
d/dv1: (v0) * ((0.434294) / (v1))
values: var0 = 1 var1 = -2
```
variable strict_errorhandling (13.7 ကိုကြည့်ပါ) ကို သတ်မှတ်ထားပါက ngspice သည် ဤ message ပြီးနောက် ထွက်သွားသည်။ မသတ်မှတ်ပါက gmin နှင့် source stepping များကို စတင်နိုင်ပြီး၊ ပုံမှန်အားဖြင့် အောင်မြင်မှုမရှိပါ။

## ၅.၅ POLY Source များ

Polynomial sources များကို XSPICE option (Chapt. 28 ကိုကြည့်ပါ) enabled လုပ်ထားမှသာ ရရှိနိုင်သည်။

### ၅.၅.၁ E voltage source, G current source

**ယေဘူယျပုံစံ-**
```
EXXXX N+ N- POLY(ND) NC1+ NC1- (NC2+ NC2-...) P0 (P1...)
```
**ဥပမာ-**
```spice
ENONLIN 100 101 POLY(2) 3 0 4 0 0.0 13.6 0.2 0.005
```
POLY(ND) သည် polynomial ၏ dimensions အရေအတွက်ကို သတ်မှတ်သည်။ controlling nodes အတွဲများ၏အရေအတွက်သည် dimensions အရေအတွက်နှင့် ညီရမည်။

(N+) နှင့် (N-) nodes များသည် output nodes များဖြစ်သည်။ Positive current သည် (+) node မှ source မှတစ်ဆင့် (-) node သို့ စီးဆင်းသည်။

<NC1+> နှင့် <NC1-> တို့သည် အတွဲလိုက်ဖြစ်ပြီး controlling voltages အစုတစ်ခုကို သတ်မှတ်သည်။ node တစ်ခုသည် တစ်ကြိမ်ထက်ပို၍ ပေါ်လာနိုင်ပြီး၊ output နှင့် controlling nodes များသည် မတူညီရန် မလိုပါ။

ဥပမာသည် input voltages နှစ်ခု v(3,0) နှင့် v(4,0) တို့ဖြင့် control လုပ်သော voltage output ကိုထုတ်ပေးသည်။ Polynomial coefficients လေးခုပေးထားသည်။ Output ထုတ်လုပ်ရန် equivalent function မှာ-
```
0 + 13.6 * v(3) + 0.2 * v(4) + 0.005 * v(3) * v(3)
```
ယေဘူယျအားဖြင့် အောက်ပါအတိုင်း equation ကို သတ်မှတ်မည်-
- POLY(1): y = p0 + p1*X1 + p2*X1*X1 + p3*X1*X1*X1 + ...
- POLY(2): y = p0 + p1*X1 + p2*X2 + p3*X1*X1 + p4*X2*X1 + p5*X2*X2 + p6*X1*X1*X1 + p7*X2*X1*X1 + p8*X2*X2*X1 + p9*X2*X2*X2 + ...
- POLY(3): y = p0 + p1*X1 + p2*X2 + p3*X3 + p4*X1*X1 + p5*X2*X1 + p6*X3*X1 + p7*X2*X2 + p8*X2*X3 + p9*X3*X3 + ...

X1 သည် ပထမ input node pair ၏ voltage difference၊ X2 သည် ဒုတိယ pair စသည်ဖြင့်။ polynomial coefficients အားလုံးကို ခြေရာခံခြင်းသည် large polynomials များအတွက် အတော်လေး ပျင်းစရာကောင်းသည်။

### ၅.၅.၂ F voltage source, H current source

**ယေဘူယျပုံစံ-**
```
FXXXX N+ N- POLY(ND) V1 (V2 V3 ...) P0 (P1...)
```
**ဥပမာ-**
```spice
FNONLIN 100 101 POLY(2) VDD Vxx 0 0.0 13.6 0.2 0.005
```
POLY(ND) သည် polynomial ၏ dimensions အရေအတွက်ကို သတ်မှတ်သည်။ controlling sources များ၏အရေအတွက်သည် dimensions အရေအတွက်နှင့် ညီရမည်။

(N+) နှင့် (N-) nodes များသည် output nodes များဖြစ်သည်။ Positive current သည် (+) node မှ source မှတစ်ဆင့် (-) node သို့ စီးဆင်းသည်။

V1 (V2 V3 ...) တို့သည် controlling voltage sources များဖြစ်သည်။ Control variable သည် ဤ sources များမှ current ဖြစ်သည်။

P0 (P1...) တို့သည် 5.5.1 တွင် ဖော်ပြထားသည့်အတိုင်း coefficients များဖြစ်သည်။
