# ၈။ Mixed-Mode နှင့် XSPICE ဖြင့် Behavioral Modeling

Ngspice သည် behavioral modeling နှင့် mixed-mode (analog နှင့် digital) မော်ဒယ်လုပ်ခြင်းအတွက် XSPICE extensions များကို အကောင်အထည်ဖော်ထားသည်။ XSPICE framework တွင် ၎င်းကို code level modeling ဟုခေါ်ဆိုသည်။ Behavioral modeling သည် C ဖြင့် ရေးသားထားသော analog functionality များထည့်ရန် နည်းလမ်းတစ်ခုပေးသောကြောင့် အကျိုးရှိနိုင်သည်။ ပိုမိုပြောင်းလွယ်ပြင်လွယ်ရှိသည်မှာ သင့်ကိုယ်ပိုင် မော်ဒယ်များကို သတ်မှတ်နိုင်ပြီး ရှိပြီးသား ngspice functionality အားလုံးနှင့် ပေါင်းစပ်အသုံးပြုနိုင်ခြင်းဖြစ်သည်။ Digital နှင့် mixed mode simulation သည် digital အပိုင်းကို event driven နည်းဖြင့် သရုပ်ဖော်ခြင်းဖြင့် သိသိသာသာ မြန်ဆန်စေသည်။

## ၈.၁ Code Model Element & .MODEL Cards

### ၈.၁.၁ Syntax
XSPICE code model instance cards များသည် 'A' ဖြင့် စတင်ပြီး၊ .MODEL card ဖြင့် မော်ဒယ်ကို သတ်မှတ်သည်။ Instance card ပုံစံ-
```
AXXXXXXX <port types> <nodes> MODELNAME
.MODEL MODELNAME modeltype (param1=val1 param2=val2 ...)
```
Port types များကို `%v`, `%i`, `%vd`, `%id`, `%g`, `%h`, `%d` စသည်ဖြင့် သတ်မှတ်နိုင်သည်။ Vector ports များကို `[]` ဖြင့်ဖော်ပြပြီး `null` က ချိတ်ဆက်မှုမရှိကြောင်း ဖော်ပြသည်။ Digital node များတွင် `~` ကို logic ပြောင်းလဲရန် သုံးနိုင်သည်။

### ၈.၁.၂ ဥပမာများ
Analog model `gain` ဥပမာ-
```spice
a1 1 2 amp
.model amp gain(gain=5.0)
```

### ၈.၁.၃ ဖိုင်ထည့်သွင်းမှုအတွက် ရှာဖွေရေးလမ်းကြောင်း
Code model များက ဖိုင်များ (ဥပမာ filesource, d_source, d_state) ကိုခေါ်သုံးပါက၊ အောက်ပါအတိုင်း ရှာဖွေသည်-
- ဆားကစ် input file path (`Infile_Path`)
- `NGSPICE_INPUT_DIR` environment variable
- လက်ရှိ directory

### ၈.၁.၄ Code model တည်နေရာနှင့် အကဲဖြတ်ခြင်း
XSPICE extensions အသုံးပြုရန် ngspice ကို `--enable-xspice` ဖြင့် compile လုပ်ရမည်။ Code model များကို shared libraries (`.cm`) အဖြစ် သိမ်းဆည်းပြီး startup တွင် `codemodel` command ဖြင့် load လုပ်သည်။

## ၈.၂ Analog Models
အောက်ပါ analog code models များပါဝင်သည်-

- **၈.၂.၁ Gain:** simple gain block (input offset, gain, output offset ဖြင့်)။ DC, AC, Transient တို့တွင် အလုပ်လုပ်သည်။
- **၈.၂.၂ Summer:** 2-to-N input ports များ၏ ပေါင်းလဒ်။ input offsets နှင့် gains များကိုသတ်မှတ်နိုင်သည်။
- **၈.၂.၃ Multiplier:** 2-to-N inputs များ၏မြှောက်လဒ်။ AC analysis တွင် input တစ်ခုသာ AC အချက်ပြဖြစ်ရန်လိုသည်။
- **၈.၂.၄ Divider:** two-quadrant divider။ numerator နှင့် denominator၊ gain၊ offset များပါရှိသည်။
- **၈.၂.၅ Limiter:** Gain block ကဲ့သို့ဖြစ်ပြီး output ကို upper/lower limits များဖြင့် ကန့်သတ်သည်။ smoothing ပြုလုပ်နိုင်သည်။
- **၈.၂.၆ Controlled Limiter:** Limiter နှင့်ဆင်တူသော်လည်း limit values များကို control inputs များဖြင့် ထိန်းချုပ်သည်။
- **၈.၂.၇ PWL Controlled Source:** piecewise linear transfer function ကိုဖော်ပြသည်။ x_array, y_array ဖြင့် coordinate points များပေးရသည်။
- **၈.၂.၈ PWL Time Controlled Source with optional edge smoothing:** အချိန်ကို input အဖြစ်အသုံးပြုသော PWL source။ smoothing options ပါသည်။
- **၈.၂.၉ Filesource (PWL sourced from file):** waveform data ကို file မှဖတ်ယူသော PWL source။
- **၈.၂.၁၀ Multi_input_PWL_block:** analog AND/OR gate ကဲ့သို့သော function။ smallest/largest input ကိုယူပြီး PWL output ထုတ်သည်။
- **၈.၂.၁၁ Analog Switch:** control voltage ပေါ်မူတည်၍ resistance ပြောင်းလဲသော switch။ logarithmically သို့မဟုတ် linearly ပြောင်းနိုင်သည်။
- **၈.၂.၁၂ Alternative Analog Switch:** Analog switch ၏ PSPICE-compatible ဗားရှင်း။
- **၈.၂.၁၃ Zener Diode:** breakdown voltage, breakdown current, series resistance ပါသော zener diode model။
- **၈.၂.၁၄ Current Limiter:** operational amplifier output stage ကဲ့သို့ current limiting ပြုလုပ်ပေးသည်။
- **၈.၂.၁၅ Hysteresis Block:** input/output hysteresis ပါသော buffer။
- **၈.၂.၁၆ Differentiator:** time-derivative block (နှင့်ဆိုင်သော အချက်ပြ၏ differential ကိုထုတ်ပေးသည်)။
- **၈.၂.၁၇ Integrator:** time-integration block (ပေါင်းလဒ်)။ truncation error checking ပါဝင်သည်။
- **၈.၂.၁၈ S-Domain Transfer Function:** Laplace transform ကိုအသုံးပြုသော transfer function။ numerator/denominator polynomials ကိုသတ်မှတ်နိုင်သည်။
- **၈.၂.၁၉ PWL Transfer Function:** frequency-dependent complex transfer function (AC analysis only)။ Touchstone file မှလည်း ဖတ်နိုင်သည်။
- **၈.၂.၂၀ Slew Rate Block:** output slope (rising/falling) ကို ကန့်သတ်သည်။
- **၈.၂.၂၁ Inductive Coupling:** magnetic circuit အတွက် `lcouple` model။ inductor current ကို mmf အဖြစ်ပြောင်းသည်။
- **၈.၂.၂၂ Magnetic Core:** magnetic core model (PWL သို့မဟုတ် hysteresis mode)။ `lcouple` နှင့်တွဲသုံးသည်။
- **၈.၂.၂၃ Controlled Sine Wave Oscillator:** frequency ကို control voltage ဖြင့် ထိန်းချုပ်နိုင်သော sine wave oscillator။
- **၈.၂.၂၄ Controlled Triangle Wave Oscillator:** triangle wave oscillator၊ frequency နှင့် duty cycle ကို control voltage ဖြင့် ထိန်းချုပ်သည်။
- **၈.၂.၂၅ Controlled Square Wave Oscillator:** square wave oscillator၊ duty cycle, rise/fall times ကိုထိန်းချုပ်နိုင်သည်။
- **၈.၂.၂၆ Controlled One-Shot:** pulse width ကို control voltage ဖြင့် ထိန်းချုပ်သော monostable multivibrator။
- **၈.၂.၂၇ Capacitance Meter:** node တစ်ခုရှိ total capacitance ကိုတိုင်းတာပြီး scaled output ထုတ်ပေးသည်။
- **၈.၂.၂၈ Inductance Meter:** node တစ်ခုရှိ total inductance ကိုတိုင်းတာသည်။
- **၈.၂.၂၉ Memristor:** memristor device model (rmin, rmax, rinit, vt, alpha, beta)။
- **၈.၂.၃၀ 2D table model:** 2-dimensional lookup table (inx, iny, out)။ linear interpolation, ဖိုင်မှ data များကိုဖတ်သည်။
- **၈.၂.၃၁ 3D table model:** 3-dimensional lookup table (inx, iny, inz, out)။ BSIM model ကဲ့သို့ data များကို table အနေဖြင့် မော်ဒယ်လုပ်နိုင်သည်။
- **၈.၂.၃၂ Simple Diode Model:** linear I(V) regions များဖြင့် ရိုးရှင်းသော diode model (ron, roff, vfwd, vrev)။
- **၈.၂.၃၃ Analog delay:** delay line (fixed or voltage-controlled delay)။ buffer ဖြင့်လုပ်ဆောင်သည်။
- **၈.၂.၃၄ Potentiometer:** 3-terminal variable resistor (position, total resistance, log/linear)။

## ၈.၃ Hybrid Models
Analog နှင့် digital domain များကြား ဘာသာပြန်ပေးသော မော်ဒယ်များ။

- **၈.၃.၁ Digital-to-Analog Node Bridge (dac_bridge):** digital state ကို analog voltage သို့ပြောင်းသည် (out_low, out_high, out_undef, t_rise/t_fall)။
- **၈.၃.၂ Analog-to-Digital Node Bridge (adc_bridge):** analog voltage ကို digital state သို့ပြောင်းသည် (in_low, in_high)။
- **၈.၃.၃ Bidirectional Analog/Digital Node Bridge (bidi_bridge):** bidirectional bridge၊ digital I/O နှင့် analog transconductance port ပါ။ Schmitt trigger အဖြစ်လည်းသုံးနိုင်သည်။
- **၈.၃.၄ Controlled Digital Oscillator (d_osc):** digital clock output, frequency ကို control voltage ဖြင့် ထိန်းချုပ်သည်။
- **၈.၃.၅ Node bridge from digital to real with enable (d_to_real):** digital input ကို real (floating point) value သို့ပြောင်းသည်။
- **၈.၃.၆ A Z**-1 block working on real data (real_delay): event-driven real data အတွက် unit delay (Z^-1)။
- **၈.၃.၇ A gain block for event-driven real data (real_gain):** real node အတွက် gain, offset, delay ပါသော block။
- **၈.၃.၈ Node bridge from real to analog voltage (real_to_v):** real value ကို analog voltage သို့ပြောင်းသည်။
- **၈.၃.၉ Controlled PWM Oscillator (d_pwm):** duty cycle ကို control voltage ဖြင့် ထိန်းချုပ်သော digital oscillator။

## ၈.၄ Digital Models
Event-driven digital simulation အတွက် standard logic gates နှင့် sequential blocks များ။

- **၈.၄.၁ Buffer:** 1-bit buffer (time-delayed copy)
- **၈.၄.၂ Inverter:** 1-bit inverter
- **၈.၄.၃ And:** n-input AND gate
- **၈.၄.၄ Nand:** n-input NAND gate
- **၈.၄.၅ Or:** n-input OR gate
- **၈.၄.၆ Nor:** n-input NOR gate
- **၈.၄.၇ Xor:** n-input XOR gate
- **၈.၄.၈ Xnor:** n-input XNOR gate
- **၈.၄.၉ Tristate:** tristate buffer (enable input, delay)
- **၈.၄.၁၀ Pullup:** digital pullup resistor (output HI_IMPEDANCE ကို ONE ဖြစ်စေသည်)
- **၈.၄.၁၁ Pulldown:** digital pulldown resistor
- **၈.၄.၁၂ D Flip Flop:** positive-edge-triggered D flip-flop (asynchronous set/reset)
- **၈.၄.၁၃ JK Flip Flop:** JK flip-flop (edge-triggered, async set/reset)
- **၈.၄.၁၄ Toggle Flip Flop:** T flip-flop (toggles on clock edge)
- **၈.၄.၁၅ Set-Reset Flip Flop:** SR flip-flop (clocked)
- **၈.၄.၁၆ D Latch:** D-type latch (level-sensitive)
- **၈.၄.၁၇ Set-Reset Latch:** SR-type latch (level-sensitive, enable)
- **၈.၄.၁၈ State Machine:** digital state machine (state transition file `state_file` ဖြင့်သတ်မှတ်သည်)
- **၈.၄.၁၉ Frequency Divider:** programmable digital frequency divider
- **၈.၄.၂၀ RAM:** M-wide, N-deep RAM (address, data_in/out, write_en, select)
- **၈.၄.၂၁ Digital Source:** digital signal source (input file `input_file` မှဖတ်သည်)
- **၈.၄.၂၂ LUT:** n-input, 1-output lookup table (table_values string)
- **၈.၄.၂၃ General LUT:** n-input, m-output lookup table (vector outputs, independent delays)
- **၈.၄.၂၄ D_process:** external process (C executable) နှင့် ဆက်သွယ်သော မော်ဒယ်
- **၈.၄.၂၅ d_cosim:** shared library မှ digital model ကို load လုပ်သော ကွန်တိန်နာ (Verilator, Icarus Verilog တို့နှင့် တွဲသုံးနိုင်သည်)

## ၈.၅ Event driven simulation အတွက် Predefined Node Types
- **၈.၅.၁ Digital Node Type:** '0', '1', 'U' state နှင့် 's', 'r', 'z', 'u' strength (12 states)
- **၈.၅.၂ Real Node Type:** double-precision floating point ဒေတာအတွက် event-driven node
- **၈.၅.၃ Int Node Type:** integer ဒေတာအတွက် event-driven node
- **၈.၅.၄ (Digital) Input/Output:** digital node များ၏ input/output အတွက် `d_source`, `filesource` စသည်များ။ VCD file ထုတ်လုပ်ရန် `eprvcd` အမိန့်ပေးချက်။

## ၈.၆ Bridging device များ အလိုအလျောက် ထည့်သွင်းခြင်း
Analog နှင့် digital node များ နာမည်တူပါက bridge ကို အလိုအလျောက်ထည့်ပေးသည်။ `auto_adc`, `auto_dac`, `auto_bidi` ဟူသော default models များရှိပြီး `set auto_bridge_d_*` variables များဖြင့် override လုပ်နိုင်သည်။ `family` parameter ကိုလည်း bridge ထိန်းချုပ်ရန် သုံးနိုင်သည်။ ဤအလိုအလျောက်စနစ်ကို `auto_bridge=0` ဖြင့် ပိတ်နိုင်သည်။
