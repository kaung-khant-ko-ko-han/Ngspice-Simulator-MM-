# ၁၃။ အပြန်အလှန်တုံ့ပြန်နိုင်သော စကားပြန် (Interactive Interpreter)

## ၁၃.၁ နိဒါန်း

ngspice တွင် သရုပ်ဖော်မှုစီးဆင်းချက် (ထည့်သွင်းမှု၊ သရုပ်ဖော်မှု၊ ထုတ်လွှတ်မှု) ကို batch mode တွင် dot commands (Chapt. 11 နှင့် 12.4.1 ကိုကြည့်ပါ) ဖြင့် ထိန်းချုပ်နိုင်သည်။ သို့သော် ngspice တွင် ပိုမိုစွမ်းဆောင်နိုင်သော ထိန်းချုပ်မှုအစီအစဉ်တစ်ခုရှိပြီး ရှေးရိုးအရ “Interactive Interpreter” ဟုခေါ်ဆိုသော်လည်း ၎င်းထက်များစွာပိုပါသည်။ ဤအင်္ဂါရပ်ကို အသုံးပြုရန် နည်းလမ်းများစွာရှိသည်- input console သို့ commands များကို ရိုက်ထည့်၍ အမှန်တကယ် အပြန်အလှန်တုံ့ပြန်သည့်နည်း၊ သို့မဟုတ် command sequences များကို scripts အဖြစ် သို့မဟုတ် quasi batch mode တွင် သင့် input deck ၏တစ်စိတ်တစ်ပိုင်းအဖြစ် run နိုင်သည်။

သင် interactive simulation session မှ ရရှိပြီးသား data များကို ပြုပြင်ရန် expressions၊ functions (13.2) သို့မဟုတ် commands (13.5) များကို input console သို့ ရိုက်ထည့်နိုင်သည်။

Commands၊ functions နှင့် control structures (13.6) များ၏ sequences များကို script (13.8) တစ်ခုအဖြစ် file တစ်ခုထဲသို့ စုစည်း၍ ၎င်းကို interactive ngspice session ၏ console input သို့ file name ကို ရိုက်ထည့်ခြင်းဖြင့် activate လုပ်နိုင်သည်။

နောက်ဆုံးတွင် အသုံးအဝင်ဆုံးမှာ netlist နှင့် dot commands များအပြင် input file သို့ script တစ်ခုကို ထည့်သွင်းခြင်းဖြစ်သည်။ ၎င်းကို script ကို `.control ... .endc` ဖြင့် ဝန်းရံခြင်းဖြင့် လုပ်ဆောင်သည် (12.4.3 နှင့် 13.8.8 တွင် ဥပမာကိုကြည့်ပါ)။ ဤအင်္ဂါရပ်သည် ထိန်းချုပ်မှုရွေးချယ်စရာများစွာကို enable လုပ်ပေးသည်။ သင်သည် internal (13.7) နှင့် အခြား variables များ သတ်မှတ်နိုင်သည်၊ simulation တစ်ခုစတင်နိုင်သည်၊ simulation output ကို အကဲဖြတ်နိုင်သည်၊ ဤ data များကိုအခြေခံ၍ simulation အသစ်တစ်ခုစတင်နိုင်သည်၊ နောက်ဆုံးတွင် data များကို output လုပ်ရန် (graphically သို့မဟုတ် output files များသို့) ရွေးချယ်စရာများစွာကို အသုံးချနိုင်သည်။

## ၁၃.၂ Expression များ၊ Function များနှင့် Constants များ

Ngspice သည် data များကို vectors ပုံစံဖြင့် သိမ်းဆည်းသည်- time, voltage, etc. Vector တစ်ခုစီတွင် type တစ်ခုရှိပြီး vectors များကို ၎င်းတို့၏ types များနှင့် ကိုက်ညီသော နည်းလမ်းများဖြင့် algebraic စနစ်တကျ ပေါင်းစပ်၍ operation လုပ်နိုင်သည်။ Vectors များကို simulation တစ်ခု၏ output အဖြစ်၊ data file (output raw file) ကို ပြန်ဖတ်သည့်အခါ (load command 13.5.48 ဖြင့် ngspice)၊ သို့မဟုတ် initial data-file ကို ngnutmeg ထဲသို့ တိုက်ရိုက် load လုပ်သည့်အခါ ဖန်တီးသည်။ ၎င်းတို့ကို let command (13.5.45) ဖြင့်လည်း ဖန်တီးနိုင်သည်။

Expression ဆိုသည်မှာ vectors နှင့် scalars (scalar သည် length 1 ရှိသော vector) များပါဝင်သော algebraic formula တစ်ခုဖြစ်ပြီး အောက်ပါ operations များပါဝင်သည်- `+ - * / ^ % ,`။ `%` သည် modulo operator ဖြစ်ပြီး၊ comma operator တွင် အဓိပ္ပာယ်နှစ်မျိုးရှိသည်- user definable function တစ်ခု၏ argument list တွင်ရှိပါက arguments များကို ခွဲခြားရန် ဆောင်ရွက်သည်။ မဟုတ်ပါက `x, y` သည် `x + j(y)` နှင့် အဓိပ္ပာယ်တူသည်။ ထို့အပြင် logical operations `&` (and), `|` (or), `!` (not) နှင့် relational operations `<, >, >=, <=, =, <>` (not equal) တို့လည်း ရရှိနိုင်သည်။ Algebraic expression တစ်ခုတွင် အသုံးပြုပါက ၎င်းတို့သည် C တွင်ကဲ့သို့ အလုပ်လုပ်ပြီး 0 သို့မဟုတ် 1 တန်ဖိုးများ ထုတ်လုပ်သည်။

Function များတွင် `mag(vector)`၊ `ph(vector)`၊ `cph(vector)`၊ `unwrap(vector)`၊ `j(vector)`၊ `real(vector)`၊ `imag(vector)`၊ `conj(vector)`၊ `db(vector)`၊ `log10(vector)`၊ `ln(vector)`၊ `exp(vector)`၊ `abs(vector)`၊ `sqrt(vector)`၊ `sin(vector)`၊ `cos(vector)`၊ `tan(vector)`၊ `atan(vector)`၊ `sinh(vector)`၊ `cosh(vector)`၊ `tanh(vector)`၊ `atanh(vector)`၊ `floor(vector)`၊ `ceil(vector)`၊ `norm(vector)`၊ `mean(vector)`၊ `avg(vector)`၊ `stddev(vector)`၊ `group_delay(vector)`၊ `vector(number)`၊ `cvector(number)`၊ `unitvec(number)`၊ `length(vector)`၊ `interpolate(plot.vector)`၊ `integ(vector)`၊ `deriv(vector)`၊ `vecd(vector)`၊ `vecmin(vector)`၊ `minimum(vector)`၊ `vecmax(vector)`၊ `maximum(vector)`၊ `fft(vector)`၊ `ifft(vector)`၊ `sortorder(vector)`၊ `timer(vector)`၊ `clock(vector)` နှင့် statistical functions များဖြစ်သော `rnd(vector)`၊ `sgauss(vector)`၊ `sunif(vector)`၊ `poisson(vector)`၊ `exponential(vector)` တို့ပါဝင်သည်။

အသုံးပြုနိုင်သော predefined constants များမှာ `pi`၊ `e`၊ `c` (speed of light)၊ `i` (sqrt(-1))၊ `kelvin` (absolute zero in centigrade)၊ `echarge` (electron charge)၊ `boltz` (Boltzmann's constant)၊ `planck` (Planck's constant)၊ `yes`၊ `no`၊ `TRUE`၊ `FALSE` တို့ဖြစ်ပြီး အားလုံးကို MKS units ဖြင့် ပေးထားသည်။ `.csparam` (2.13) ကဲ့သို့သော circuit setup အတွင်း ထပ်ဆောင်း constants များကို ထုတ်လုပ်နိုင်သည်။

## ၁၃.၃ Plot များ

မည်သည့် analysis မှ output vectors များကိုမဆို plots များတွင် သိမ်းဆည်းသည်၊ ၎င်းသည် ရှေးရိုး SPICE အယူအဆတစ်ခုဖြစ်သည်။ Plot တစ်ခုသည် vectors အုပ်စုတစ်ခုဖြစ်သည်။ ပထမ `tran` command သည် plot `tran1` အတွင်း vectors များစွာ ထုတ်လုပ်မည်။ နောက်ဆက်တွဲ `tran` command သည် ၎င်း၏ vectors များကို `tran2` တွင် သိမ်းဆည်းမည်။ ထို့နောက် `linearize` command သည် `tran2` မှ vectors အားလုံးကို linearize လုပ်ပြီး `tran3` တွင် သိမ်းဆည်းကာ ၎င်းသည် current plot ဖြစ်လာသည်။ `fft` သည် plot `spec1` ကို ထုတ်လုပ်မည်ဖြစ်ပြီး ယခု current plot ဖြစ်သည်။ `display` command သည် current plot ရှိ vectors အားလုံးကို အမြဲပြသမည်။ `echo $plots` ဖြင့် ယခုထိ ထုတ်လုပ်ထားသော plots အားလုံးကို စာရင်းပြုလုပ်သည်။ `setplot` သည် current plot ကိုပြောင်းရန် ခွင့်ပြုသည်။ Plot အသစ်များကို `setplot new` ဖြင့် ဖန်တီးနိုင်ပြီး `set curplottitle`၊ `set curplotname`၊ `set curplotdate` တို့ဖြင့် title၊ name၊ date တို့ကို သတ်မှတ်နိုင်သည်။

## ၁၃.၄ Command Interpretation (အမိန့်ပေးချက် အနက်ပြန်ခြင်း)

### ၁၃.၄.၁ Console ပေါ်တွင်
ngspice console window (သို့မဟုတ် Windows GUI) တွင် 13.5 မှ မည်သည့် command ကိုမဆို တိုက်ရိုက်ရိုက်ထည့်နိုင်သည်။ command sequence တစ်ခုအတွင်း Input/output redirection ကို ရရှိနိုင်သည် (ဥပမာအတွက် Chapt. 13.8.9 ကိုကြည့်ပါ)။

### ၁၃.၄.၂ Script များ
command တစ်ခုအနေဖြင့် word တစ်ခုကို ရိုက်ထည့်၍ ထို name ဖြင့် built-in command မရှိပါက `sourcepath` စာရင်းရှိ directories များကို အစဉ်လိုက် ရှာဖွေမည်။ file တွေ့ရှိပါက ၎င်းကို input file အဖြစ် ဖတ်ရှုမည် (source လုပ်သကဲ့သို့)။ ထိုသို့သော file များသည် interpreter commands များသာ ပါဝင်သော pure script ဖြစ်လေ့ရှိသည်။

### ၁၃.၄.၃ Circuit ဖိုင်သို့ ထည့်သွင်းခြင်း
Chapt. 13.5 တွင် ဖော်ပြထားသော commands များကို invoke လုပ်ရန် အသုံးအများဆုံးနည်းလမ်းမှာ circuit input file သို့ `.control ... .endc` section တစ်ခု ထည့်ခြင်းဖြစ်သည် (12.4.3 ကိုကြည့်ပါ)။

## ၁၃.၅ Commands (အမိန့်ပေးချက်များ)

Commands များကို `*` (standard ngspice တွင်သာ) နှင့် `**` (shared ngspice တွင်သာ) တို့ဖြင့် မှတ်သားထားသည်။

ဤတွင်ဖော်ပြသော command တိုင်းသည် ngspice ၏ simulation နှင့် data manipulation ဆိုင်ရာ ကျယ်ပြန့်သော လုပ်ဆောင်ချက်များကို ကိုယ်စားပြုသည်။ အသေးစိတ်ကို manual တွင် ကြည့်ရှုနိုင်ပါသည်။

### ၁၃.၅.၁ Ac
AC small-signal frequency response analysis လုပ်ဆောင်သည်။

### ၁၃.၅.၂ Alias
Command အတွက် alias တစ်ခု ဖန်တီးသည်။

### ၁၃.၅.၃ Alter
Device သို့မဟုတ် model parameter တန်ဖိုးကို ပြောင်းလဲသည်။

### ၁၃.၅.၄ Altermod
Model parameter(s) ကို ပြောင်းလဲသည်။

### ၁၃.၅.၅ Alterparam
Global parameter တစ်ခု၏ တန်ဖိုးကို ပြောင်းလဲသည်။ `reset` လိုအပ်သည်။

### ၁၃.၅.၆ Asciiplot
Old-style character plots သုံး၍ တန်ဖိုးများကို plot လုပ်သည်။

### ၁၃.၅.၇ Aspice*
Asynchronous ngspice run တစ်ခု စတင်ပြီး ပြီးဆုံးသောအခါ data ကို load လုပ်သည်။

### ၁၃.၅.၈ Bg_ctrl**
`bg_run` မပြီးဆုံးမချင်း control commands များကို ဆိုင်းငံ့ထားသော thread တစ်ခု ဖန်တီးသည်။

### ၁၃.၅.၉ Bg_halt**
`bg_run` ဖြင့် စတင်ထားသော run တစ်ခုကို ရပ်တန့်သည်။

### ၁၃.၅.၁၀ Bg_run**
Background thread တွင် input file မှ analysis ကို run လုပ်သည်။

### ၁၃.၅.၁၁ Bug
Ngspice bug tracker အတွက် URL ကို ထုတ်ပေးသည်။

### ၁၃.၅.၁၂ Cd
Directory ပြောင်းလဲသည်။

### ၁၃.၅.၁၃ Cdump
Screen ပေါ်သို့ control flow ကို dump လုပ်သည်။

### ၁၃.၅.၁၄ Circbyline
Circuit တစ်ခုကို line by line ထည့်သွင်းသည်။

### ၁၃.၅.၁၅ Codemodel
XSPICE code model library တစ်ခုကို load လုပ်သည်။

### ၁၃.၅.၁၆ Compose
Vector တစ်ခုကို ဖန်တီးသည် (values များ၊ linear/logarithmic/uniform/Gaussian distribution စသည်ဖြင့်)။

### ၁၃.၅.၁၇ Cutout
Tran plot တစ်ခုရှိ vector အားလုံး၏ section တစ်ခုကို ဖြတ်ထုတ်ပြီး plot အသစ်တစ်ခု ဖန်တီးသည်။

### ၁၃.၅.၁၈ Dc
DC-sweep analysis လုပ်ဆောင်သည်။

### ၁၃.၅.၁၉ Define
Function တစ်ခု သတ်မှတ်သည်။

### ၁၃.၅.၂၀ Deftype
Vector သို့မဟုတ် plot အတွက် type အသစ် (unit ပါ) သတ်မှတ်သည်။

### ၁၃.၅.၂၁ Delete
Saved nodes, parameters, breakpoints, traces များကို ဖယ်ရှားသည်။

### ၁၃.၅.၂၂ Destroy
Output data set (plot) များ၏ memory ကို လွှတ်ပေးသည်။

### ၁၃.၅.၂၃ Devhelp
Simulator တွင် ရရှိနိုင်သော devices များနှင့် ၎င်းတို့၏ parameters များအကြောင်း information ပြသသည်။

### ၁၃.၅.၂၄ Diff
Plots နှစ်ခုရှိ vectors များကို နှိုင်းယှဉ်သည်။

### ၁၃.၅.၂၅ Display
သိထားသော vectors များ၏ အမည်၊ length၊ type၊ real/complex ကို print ထုတ်သည်။

### ၁၃.၅.၂၆ Echo
စာသားကို print ထုတ်သည်။

### ၁၃.၅.၂၇ Edit*
လက်ရှိ circuit ကို editor တွင်ဖွင့်၍ ပြင်ဆင်ပြီး ပြန်ဖတ်သည်။

### ၁၃.၅.၂၈ Edisplay
Event driven nodes အားလုံး၏ စာရင်းကို print ထုတ်သည်။

### ၁၃.၅.၂၉ Eprint
Event driven node တစ်ခုကို print ထုတ်သည်။ အချိန်နှင့် node တန်ဖိုး table ထုတ်ပေးသည်။

### ၁၃.၅.၃၀ Eprvcd
Event driven nodes များကို VCD (Value Change Dump) format ဖြင့် file သို့ dump လုပ်သည်။ GTKWave ကဲ့သို့သော viewer ဖြင့် ကြည့်နိုင်သည်။

### ၁၃.၅.၃၁ Esave
Event node outputs အချို့ကိုသာ save လုပ်၍ memory ချွေတာသည်။

### ၁၃.၅.၃၂ Fclose
`fopen` သို့မဟုတ် `fread` ဖြင့်ဖွင့်ထားသော file ကို ပိတ်သည်။

### ၁၃.၅.၃၃ FFT
Transient analysis result vectors များ၏ fast Fourier transform ကို forward direction ဖြင့် တွက်ချက်သည်။ Windowing functions ကို `specwindow` variable ဖြင့် သတ်မှတ်နိုင်သည်။

### ၁၃.၅.၃၄ Fopen
စာသားဖိုင်တစ်ခုကို ဖွင့်၍ numeric handle ပြန်ပေးသည်။

### ၁၃.၅.၃၅ Fourier
Transient analysis output vector(s) ပေါ်တွင် Fourier analysis လုပ်ဆောင်သည်။ 10 multiples (သို့မဟုတ် `nfreqs` variable အတိုင်း) ကို တွက်ချက်ပြီး printout နှင့် vectors များ ထုတ်ပေးသည်။

### ၁၃.၅.၃၆ Fread
ဖွင့်ထားသော file မှ line တစ်ကြောင်းဖတ်၍ string variable တစ်ခုသို့ ထည့်ပေးသည်။

### ၁၃.၅.၃၇ Getcwd
လက်ရှိ working directory ကို print ထုတ်သည်။

### ၁၃.၅.၃၈ Gnuplot
Gnuplot ကိုအသုံးပြု၍ data တွက်ချက်ရန်နှင့် ပုံထုတ်ရန် ခွင့်ပြုသည်။

### ၁၃.၅.၃၉ Hardcopy
Plot တစ်ခုကို file သို့ save လုပ်သည် (postscript or svg format)။

### ၁၃.၅.၄၀ Help
Ngspice commands များ၏ အကျဉ်းချုပ်ကို print ထုတ်သည် (spice3f5 style)။

### ၁၃.၅.၄၁ History
ယခင် commands များကို ပြန်လည်သုံးသပ်ပြီး history substitutions ပြုလုပ်နိုင်သည်။

### ၁၃.၅.၄၂ Inventory
Loaded netlist တွင် device instance အရေအတွက်ကို print ထုတ်သည်။

### ၁၃.၅.၄၃ Iplot*
Ngspice run နေစဉ် node values များကို incrementally plot လုပ်သည်။

### ၁၃.၅.၄၄ Jobs*
Active asynchronous ngspice jobs များကို list လုပ်သည်။

### ၁၃.၅.၄၅ Let
Expression တစ်ခု၏တန်ဖိုးဖြင့် vector အသစ်တစ်ခု ဖန်တီးသည် သို့မဟုတ် ရှိပြီးသား vector ၏ element ကို modify လုပ်သည်။

### ၁၃.၅.၄၆ Linearize
Current plot ရှိ vectors များကို equidistant time scale ပေါ်သို့ interpolate လုပ်ပြီး plot အသစ် ဖန်တီးသည်။

### ၁၃.၅.၄၇ Listing
လက်ရှိ circuit ၏ listing ကို print ထုတ်သည် (logical, physical, deck, expand, runnable, param)။

### ၁၃.၅.၄၈ Load
Rawfile data (binary or ascii) ကို load လုပ်သည်။

### ၁၃.၅.၄၉ Mc_source
Circuit netlist ကို internal storage မှ ပြန်လည် load လုပ်သည်။ `alterparam` နှင့် တွဲသုံးသည်။

### ၁၃.၅.၅၀ Meas
Simulation data ပေါ်တွင် တိုင်းတာမှုများ ပြုလုပ်သည်။ .meas(ure) နှင့် ဆင်တူပြီး vector ထဲတွင် result ကိုလည်း သိမ်းပေးသည်။

### ၁၃.၅.၅၁ Mdump
Matrix values များကို file သို့မဟုတ် console သို့ dump လုပ်သည်။

### ၁၃.၅.၅၂ Mrdump
Matrix right hand side values များကို file သို့မဟုတ် console သို့ dump လုပ်သည်။

### ၁၃.၅.၅၃ Noise
Noise analysis လုပ်ဆောင်သည် (11.3.4 ကိုကြည့်ပါ)။

### ၁၃.၅.၅၄ Op
Operating point analysis လုပ်ဆောင်သည်။

### ၁၃.၅.၅၅ Option
Ngspice option တစ်ခုကို သတ်မှတ်သည် (11.1 ကိုကြည့်ပါ)။

### ၁၃.၅.၅၆ Plot*
Display (graphics terminal) ပေါ်တွင် vectors သို့မဟုတ် expressions များကို plot လုပ်သည်။ Multiple plot styles, axes များ၊ log scales များကို ထောက်ပံ့သည်။

### ၁၃.၅.၅၇ Pre_<command>
Circuit ကို parse မလုပ်မီ command တစ်ခုကို execute လုပ်သည်။

### ၁၃.၅.၅၈ Pre_OSDI
*.osdi compact device model shared library ကို load လုပ်သည်။

### ၁၃.၅.၅၉ Print
Expression များ၏တန်ဖိုးများကို print ထုတ်သည် (col or line format)။

### ၁၃.၅.၆၀ Psd
Transient analysis result များ၏ single sided power spectral density ကို တွက်ချက်သည်။

### ၁၃.၅.၆၁ Quit
Ngspice မှ ထွက်ခွာသည်။

### ၁၃.၅.၆၂ Rehash
Internal hash tables များကို ပြန်လည်တွက်ချက်သည် (UNIX commands များအတွက်)။

### ၁၃.၅.၆၃ Remcir
လက်ရှိ circuit ကို circuits စာရင်းမှ ဖယ်ရှားသည်။

### ၁၃.၅.၆၄ Remzerovec
Current plot မှ length zero ရှိသော vectors များကို ဖယ်ရှားသည်။

### ၁၃.၅.၆၅ Reset
Analysis တစ်ခုကို ပယ်ဖျက်ပြီး circuit ကို initial state မှ ပြန်စရန် input file ကို re-parse လုပ်သည်။

### ၁၃.၅.၆၆ Reshape
Vector တစ်ခု၏ dimensionality သို့မဟုတ် dimensions များကို ပြောင်းလဲသည်။

### ၁၃.၅.၆၇ Resume
Stop သို့မဟုတ် interruption (control-C) ပြီးနောက် simulation ကို ဆက်လက်လုပ်ဆောင်သည်။

### ၁၃.၅.၆၈ Rspice*
Remote ngspice submission - remote host ပေါ်တွင် ngspice run လုပ်ပြီး data ကို load လုပ်သည်။

### ၁၃.၅.၆၉ Run
Input file မှ analysis (.ac, .op, .tran, .dc) ကို run လုပ်သည်။

### ၁၃.၅.၇၀ Rusage
Resource usage statistics (time, iterations, memory စသည်) ကို print ထုတ်သည်။

### ၁၃.၅.၇၁ Save
Output vectors အချို့ကိုသာ save လုပ်၍ memory ချွေတာသည်။

### ၁၃.၅.၇၂ Sens
Sensitivity analysis run လုပ်သည်။

### ၁၃.၅.၇၃ Set
Variable တစ်ခု၏တန်ဖိုးကို သတ်မှတ်သည်။ C-shell style variable expansion ($var) ကို ထောက်ပံ့သည်။

### ၁၃.၅.၇၄ Setcs
Case မပြောင်းလဲပဲ variable တစ်ခု၏တန်ဖိုးကို သတ်မှတ်သည်။

### ၁၃.၅.၇၅ Setcir
Current circuit ကို ပြောင်းလဲသည်။

### ၁၃.၅.၇၆ Setplot
Current plot ကို ပြောင်းလဲသည်။

### ၁၃.၅.၇၇ Setscale
Current plot အတွက် default scale vector ကို သတ်မှတ်သည်။

### ၁၃.၅.၇၈ Setseed
Random number generator အတွက် seed value ကို သတ်မှတ်သည်။

### ၁၃.၅.၇၉ Settype
Vector တစ်ခု၏ type (unit) ကို ပြောင်းလဲသည်။

### ၁၃.၅.၈၀ Shell
Operating system ၏ command interpreter ကို ခေါ်ယူသည်။

### ၁၃.၅.၈၁ Shift
List variable တစ်ခုကို left shift လုပ်သည်။

### ၁၃.၅.၈၂ Show
Selected devices များ၏ operating condition ကို table ပုံစံဖြင့် print ထုတ်သည်။

### ၁၃.၅.၈၃ Showmod
Model parameter values များကို print ထုတ်သည်။

### ၁၃.၅.၈၄ Snload
Snapshot file (snsave ဖြင့်ထုတ်ထားသော) ကို load လုပ်သည်။

### ၁၃.၅.၈၅ Snsave
Simulation အခြေအနေကို file သို့ save လုပ်၍ နောက်မှ ပြန်စရန် ခွင့်ပြုသည်။

### ၁၃.၅.၈၆ Source
Ngspice input file (circuit netlist) ကို ဖတ်သည်။

### ၁၃.၅.၈၇ Sp
S-Parameter Analysis လုပ်ဆောင်သည် (11.3.8 ကိုကြည့်ပါ)။

### ၁၃.၅.၈၈ Spec
Frequency domain plot တစ်ခု (Fourier transform) ကို slower algorithm ဖြင့် ဖန်တီးသည်။

### ၁၃.၅.၈၉ Status
Current breakpoints, traces, saved nodes များကို print ထုတ်သည်။

### ၁၃.၅.၉၀ Step
Fixed number of time-points ကို run လုပ်ပြီး stop လုပ်သည်။

### ၁၃.၅.၉၁ Stop
Breakpoint တစ်ခု သတ်မှတ်သည်။

### ၁၃.၅.၉၂ Strcmp
String နှစ်ခုကို နှိုင်းယှဉ်သည်။

### ၁၃.၅.၉၃ Strslice
String တစ်ခုမှ substring တစ်ခုကို ထုတ်ယူသည်။

### ၁၃.၅.၉၄ Strstr
String တစ်ခုထဲတွင် substring တစ်ခုကို ရှာဖွေသည်။

### ၁၃.၅.၉၅ Sysinfo
System information (OS, CPU, memory) ကို print ထုတ်သည်။

### ၁၃.၅.၉၆ Tf
Transfer function analysis run လုပ်သည် (output/input, input/output resistance)။

### ၁၃.၅.၉၇ Trace
Analysis step တိုင်းအတွက် node value ကို print ထုတ်သည်။

### ၁၃.၅.၉၈ Tran
Transient analysis လုပ်ဆောင်သည်။

### ၁၃.၅.၉၉ Transpose
Multi-dimensional data set တစ်ခုရှိ elements များကို swap လုပ်သည်။

### ၁၃.၅.၁၀၀ Unalias
Alias တစ်ခုကို ပြန်လည်ရုပ်သိမ်းသည်။

### ၁၃.၅.၁၀၁ Undefine
User-defined function ကို ဖျက်သည်။

### ၁၃.၅.၁၀၂ Unlet
Vector(s) ကို ဖျက်သည်။

### ၁၃.၅.၁၀၃ Unset
Variable တစ်ခု (သို့မဟုတ် `*` ဖြင့် အားလုံး) ကို ရှင်းလင်းသည်။

### ၁၃.၅.၁၀၄ Version
Ngspice version ကို print ထုတ်သည်။ Rawfile version ကိုလည်း စစ်ဆေးနိုင်သည်။

### ၁၃.၅.၁၀၅ Where
Transient or operating point analysis တွင် non-convergence ဖြစ်စေသော နောက်ဆုံး node သို့မဟုတ် device ကို print ထုတ်သည်။

### ၁၃.၅.၁၀၆ Wrdata
Data ကို file သို့ simple column table format ဖြင့် ရေးသားသည်။

### ၁၃.၅.၁၀၇ Write
Data ကို file သို့ Spice3f5 rawfile format ဖြင့် ရေးသားသည်။

### ၁၃.၅.၁၀၈ Wrnodev
Node voltage values များကို `.ic=xx` format ဖြင့် file သို့ ရေးသားသည်။

### ၁၃.၅.၁၀၉ Wrs2p
Two-port ၏ scattering parameters များကို Touchstone® Version 1 format ဖြင့် file သို့ ရေးသားသည်။

## ၁၃.၆ Control Structures (ထိန်းချုပ်မှု တည်ဆောက်ပုံများ)

- **While - End:** condition true ဖြစ်နေသရွေ့ loop လုပ်သည်။
- **Repeat - End:** အကြိမ်အရေအတွက် သို့မဟုတ် ထာဝရ loop လုပ်သည်။
- **Dowhile - End:** statements များကို execute လုပ်ပြီးမှ condition ကို စစ်ဆေးသည်။
- **Foreach - End:** list သို့မဟုတ် vector ထဲရှိ value တစ်ခုချင်းစီအတွက် loop လုပ်သည်။
- **If - Then - Else:** condition ပေါ်မူတည်၍ branch လုပ်သည်။
- **Label:** goto အတွက် အမှတ်အသားတစ်ခု သတ်မှတ်သည်။
- **Goto:** label ဆီသို့ control ကို transfer လုပ်သည်။
- **Continue:** loop ၏ condition test သို့ control ကို ပြန်ပို့သည်။
- **Break:** ချက်ချင်း loop မှ ထွက်သည်။

## ၁၃.၇ Internally predefined variables (အတွင်းပိုင်း ကြိုတင်သတ်မှတ်ထားသော variable များ)

ngspice ၏ လုပ်ဆောင်ချက်ကို `set` command ဖြင့် variables များ သတ်မှတ်၍ ထိန်းချုပ်နိုင်သည်။ Variables များသည် options (11.1) များနှင့်လည်း သက်ဆိုင်သည်။

ထင်ရှားသော variable အချို့မှာ-
- `altshow`: show command အတွက် non-tabular format သုံးသည်။
- `appendwrite`: write command တွင် append mode သုံးရန်။
- `askquit`: quit မလုပ်မီ prompt လုပ်ရန်။
- `colorN`: plotting အတွက် အရောင်များ သတ်မှတ်ရန်။
- `debug`: debugging information print လုပ်ရန်။
- `editor`: edit command အတွက် editor သတ်မှတ်ရန်။
- `filetype`: rawfile format (ascii or binary)။
- `fourgridsize`, `gridsize`: interpolation points များ သတ်မှတ်ရန်။
- `hcopydev`, `hcopydevtype`, စသည်- hardcopy output အတွက်။
- `history`: history events အရေအတွက်။
- `ngbehavior`: compatibility mode (12.14) သတ်မှတ်ရန်။
- `num_threads`: OpenMP threads အရေအတွက်။
- `sourcepath`: source, .include, .lib အတွက် ရှာဖွေသည့် directories စာရင်း။
- `sqrnoise`: noise output units ကို V²/Hz သို့ပြောင်းရန်။
- `strict_errorhandling`: parse error တွင် ထွက်ခွာရန်။
- `xbrushwidth`, `xgridwidth`, `xfont` စသည်- plot အတွက် graphical options များ။

ပိုမိုပြည့်စုံသောစာရင်းနှင့် အသေးစိတ်ကို manual တွင် တွေ့နိုင်သည်။

## ၁၃.၈ Script များ

ngspice ကို command line တွင် input file ဖြင့် batch သို့မဟုတ် interactive mode တွင် စတင်သည်။ Input file တွင် netlist, model cards များအပြင် `.control ... .endc` section အတွင်း command script တစ်ခုလည်း ပါဝင်နိုင်သည်။ Scripting language သည် csh-like ဖြစ်သော်လည်း numerical data ကို `let` ဖြင့် vector များ ဖန်တီး၍ အသုံးပြုသည်။ Variable များကို `set` ဖြင့် သတ်မှတ်ပြီး `$var` ဖြင့် ရယူသည်။ `$&var` ဖြင့် vector မှ numeric value ကို string သို့ ပြောင်းသည်။

Script တစ်ခုသည်:
- Variables (set)
- Vectors (let, simulation output)
- Subcircuit access (xsub1.node) ကို အသုံးပြုနိုင်သည်။
- Commands (13.5) နှင့် control structures (13.6) များကို အသုံးပြုနိုင်သည်။

ဥပမာ scripts များမှာ `spectrum` (Fourier analysis by script)၊ random number generation and testing၊ parameter sweep (alter command ဖြင့် loop)၊ output redirection (`>`, `>>`) တို့ဖြစ်သည်။

## ၁၃.၉ Scattering parameters (S-parameters)

ngspice သည် S-parameters များကို `.sp` / `sp` commands များဖြင့် တိုက်ရိုက်တွက်ချက်ခြင်းနှင့် example script `examples/control_structs/s-param.cir` ဖြင့် two-port circuits များအတွက် တွက်ချက်ခြင်း နှစ်မျိုးလုံးကို ထောက်ပံ့သည်။ Heaviside transformation ကို အသုံးပြု၍ a and b waves များအဖြစ် voltage and current တို့မှ တွက်ချက်ပြီး S-parameters များကို ဆုံးဖြတ်သည်။ `wrs2p` command ဖြင့် Touchstone® format file အဖြစ် ထုတ်လွှတ်နိုင်သည်။

## ၁၃.၁၀ Shell variables များကို အသုံးပြုခြင်း

`shell` command (13.5.80) ဖြင့် shell command တစ်ခုကို execute လုပ်နိုင်ပြီး return value ကို print ထုတ်သည်။ Console app (Linux, Cygwin) တွင် backquote substitution (`` `command` ``) ကို သုံး၍ shell command ၏ output ကို ngspice variable ထဲသို့ ဖမ်းယူနိုင်သည်။

## ၁၃.၁၁ အထွေထွေ

C-shell type quoting (`'` and `"`), history substitutions, file globbing (`*`, `?`, `[]`) ကို `noglob` ဖြင့် ထိန်းချုပ်နိုင်သည်။ X Window System ဖြင့် cursor positioning, annotation များလည်း ပြုလုပ်နိုင်သည်။

## ၁၃.၁၂ Bugs များ

Function definitions တွင် argument list substitution နှင့် user-defined function အတွင်း `plot.vec` syntax သုံးသည့်အခါ problems ရှိနိုင်သည်။