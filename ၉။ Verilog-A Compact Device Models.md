# ၉။ Verilog-A Compact Device Models

## ၉.၁ နိဒါန်း

ယနေ့ခေတ်တွင် စက်ပစ္စည်းပုံစံသစ်များကို Verilog-A code (Verilog-AMS ၏ analog subset) အဖြစ် ထုတ်ပြန်ကြသည်။ လူသိများသော ဥပမာများမှာ BSIMBULK, BSIMCMG, PSP, HiSIM သို့မဟုတ် HICUM တို့ဖြစ်သည်။ Si2 CMC ဝဘ်စာမျက်နှာတွင် အများပြည်သူသုံး ရရှိနိုင်သော စက်ပစ္စည်းပုံစံ ၂၀ ကျော်ကို ဖော်ပြထားသည်။ ထိုပုံစံများတွင် SOI, FinFet, multi-gate နှင့် high voltage transistors ကဲ့သို့သော ခေတ်မီ MOS စက်ပစ္စည်းများ၊ high speed SiGe bipolar transistors, HEMTs နှင့် ရှုပ်ထွေးသော diodes နှင့် resistors များ ပါဝင်သည်။ ngspice သည် ၎င်း၏ ပေါင်းစပ်ထားသော OSDI interface နှင့် Verilog-A စက်ပစ္စည်းပုံစံများကို dynamically loadable libraries အဖြစ်သို့ ဘာသာပြန်ပေးသော OpenVAF compiler မှတစ်ဆင့် ဤပုံစံများအားလုံးကို ရရှိနိုင်စေသည်။ အသုံးပြုသူကိုယ်တိုင်သတ်မှတ်ထားသော Verilog-A ပုံစံများကိုလည်း compile လုပ်၍ ngspice ထဲသို့ load လုပ်နိုင်သည်။ လက်ရှိတွင် Linux နှင့် MS Windows ကို ထောက်ပံ့ထားပြီး macOS အတွက် OSDI/OpenVAF မရရှိသေးပါ။ ဤအလွန်ကောင်းမွန်သော ပံ့ပိုးမှုများအတွက် SemiMod GmbH ကို ကျေးဇူးတင်ရှိပါသည်။

## ၉.၂ OSDI/OpenVAF

OSDI သည် simulator-ကင်းလွတ်သော စက်ပစ္စည်းပုံစံများအတွက် interface တစ်ခုဖြစ်သည်။ ngspice ဗားရှင်း ၃၉ မှစ၍ ဤ interface ကိုဝန်ဆောင်မှုပေးရန်နှင့် compile လုပ်ထားသော shared library စက်ပစ္စည်းပုံစံများနှင့် ဆက်သွယ်ရန် ပေါင်းစပ်ထားသော adapter တစ်ခုပါဝင်သည်။ Shared library ပုံစံများကို runtime တွင် `osdi` သို့မဟုတ် `pre_osdi` (13.5.58 ကိုကြည့်ပါ) .control language commands များဖြင့် dynamically ချိတ်ဆက်သည်။

OpenVAF သည် Verilog-A compact device model files များကို OSDI interface နှင့် ကိုက်ညီသော shared libraries အဖြစ် compile လုပ်ပေးသည်။ ပုံစံဖော်ပြချက်များသည် standard Verilog-AMS LRM 2.x ကို လိုက်နာရမည်။ ngspice-42 မှစ၍ small signal noise simulation (11.3.4) ကို အကောင်အထည်ဖော်ထားသည်။ သို့သော် noise simulation ကို Sparse 1.3 matrix solver တွင်သာ ရရှိပြီး KLU (11.1.1 ကိုကြည့်ပါ) တွင် မရှိပါ။ အခြားကန့်သတ်ချက်များ ရှိနိုင်သည်။ နောက်ထပ် အချက်အလက်များအတွက် OpenVAF ဝဘ်စာမျက်နှာများကို တိုင်ပင်ပါ။

## ၉.၃ OpenVAF ပုံစံများ ဖန်တီးခြင်းနှင့် အသုံးပြုခြင်း

### ၉.၃.၁ သရုပ်ဖော်ခြင်းအတွက် ပြင်ဆင်ခြင်း

ngspice တွင် Verilog-A ပုံစံများကို သရုပ်ဖော်ရန် အဆင့်ငါးဆင့်ရှိသည်- OSDI interface ဖြင့် ngspice ကို ရယူခြင်း သို့မဟုတ် compile လုပ်ခြင်း၊ VA-ပုံစံကို OpenVAF ဖြင့် compile လုပ်ခြင်း၊ သင့်တော်သော ပုံစံ parameter set တစ်ခု ပြင်ဆင်ခြင်း၊ compile လုပ်ထားသော ပုံစံကို ngspice ထဲသို့ load လုပ်ခြင်း ... နှင့် simulation စတင်ခြင်း။

#### ၉.၃.၁.၁ OpenVAF ရယူခြင်း
OpenVAF ကို MS Windows သို့မဟုတ် Linux အတွက် https://openvaf.semimod.de/download/ မှ single executable တစ်ခုချင်းအဖြစ် download လုပ်၍ user defined directory တစ်ခုထဲသို့ ကူးယူနိုင်သည်။ OpenVAF ကို ကိုယ်တိုင် compile လုပ်ခြင်းမှာ ဖြစ်နိုင်သော်လည်း ၎င်း၏ ရှုပ်ထွေးသော လုပ်ဆောင်ချက်ကြောင့် မထောက်ခံပါ။

#### ၉.၃.၁.၂ Verilog-A compact ပုံစံများ
Verilog-A compact device models များကို si2 CMC standard compact model page သို့မဟုတ် device modelling web sites များ (ဥပမာ BSIM သည် UC Berkeley, HiSIM သည် Hiroshima University, PSP သည် CEA-Leti, HICUM သည် TU Dresden) မှ ရရှိနိုင်သည်။ အခြားများလည်း ရှိသည်။ Verilog-AMS ၏ LRM 2.x standard ကို လိုက်နာသော user provided သို့မဟုတ် user defined ပုံစံများကိုလည်း compile လုပ်နိုင်သည်။ (ဥပမာ PSP102, EKV2.6 တို့ကဲ့သို့) အားလုံးသော public models များ မလိုက်နာပါ။

အများပြည်သူသုံး Verilog-A compact ပုံစံအများစုပါဝင်သော github repository VA-Models ရှိသည်။ ထိုပုံစံများကို LRM 2.4.0 နှင့် စစ်ဆေးပြီး ngspice simulation အတွက် ပြင်ဆင်ထားသည်။

#### ၉.၃.၁.၃ ngspice ပြင်ဆင်ခြင်း
OSDI interface ကို ngspice တွင် ထည့်သွင်းရန် `--enable-osdi` configure flag ဖြင့် compile လုပ်ပါ။ Distribution မှ MSVC Windows version ngspice.exe တွင် ဤ interface ပါဝင်ပြီးသားဖြစ်သည်။

#### ၉.၃.၁.၄ ပုံစံများကို compile လုပ်ခြင်း
အခြေခံကျသော နည်းလမ်းမှာ openvaf executable နှင့် Verilog-A model (ဥပမာ bsimbulk.va) ကို directory တစ်ခုထဲထည့်ပြီး console window မှ `openvaf bsimbulk.va` ဟုခေါ်ဆိုခြင်းဖြစ်သည်။ ထို့နောက် compiled shared library `bsimbulk.osdi` ကို ngspice ထဲသို့ load လုပ်ရန် အဆင်သင့်ဖြစ်လာသည်။

*.osdi ဖိုင်များကို မည်သည့်နေရာတွင်ထားရန်- သင့်စိတ်ကြိုက် directory တစ်ခုခုတွင် ထားနိုင်ပြီး `osdi` သို့မဟုတ် `pre-osdi` commands များ (9.3.1.6) ကို absolute သို့မဟုတ် relative path ဖြင့် ရှေ့တွင်ထည့်နိုင်သည်။ အမြဲတမ်းတည်နေရာအတွက် bulk model install အနေဖြင့် `libs/ngspice` (XSPICE code model libs *.cm များတွေ့ရသည့်နေရာ) သို့ ထည့်ရန် အကြံပြုသည်။ ngspice က compiled and linked shared library files (*.osdi) များကို အလွယ်တကူရှာဖွေနိုင်ရန် environment variable `NGSPICE_OSDI_DIR` ကို သုံးနိုင်သည်။

OpenVAF compiler ၏ နောက်ထပ် ရွေးချယ်စရာများအတွက် `openvaf --help` ကိုကြည့်ပါ။

ngspice/examples/osdi တွင် ပေးထားသော example netlists များအတွက် သင့်လျော်သော *.osdi ပုံစံများ ပြုလုပ်ရန် ရိုးရှင်းစေရန်၊ သက်ဆိုင်ရာ Verilog-A ပုံစံများနှင့် script တိုများ (Linux နှင့် Windows) ကို ကျွန်ုပ်တို့၏ release directory မှ VAforOSDI.7z အဖြစ် download ရယူနိုင်သည်။

#### ၉.၃.၁.၅ ပုံစံ parameters များ ပြင်ဆင်ခြင်း
အခန်း ၂.၅ အရ စက်ပစ္စည်းပုံစံတစ်ခုစီအတွက် model parameter set ကို .model line တစ်ခုတွင် စုစည်းထားသည်။ သို့သော် OSDI ပုံစံများတွင် `model type` သည် ပုံစံများကို တစ်ခုနှင့်တစ်ခု ခွဲခြားရန် အခန်းကဏ္ဍမှပါဝင်သည်။ TYPE parameter က NMOS (TYPE=1) သို့မဟုတ် PMOS (TYPE=-1)၊ NPN (TYPE=1) သို့မဟုတ် PNP (TYPE=-1) ကို ဆုံးဖြတ်သည်။

ဥပမာ bsimbulk ပုံစံအတွက် `modeltype` ကို BSIMBULK.va Verilog-A model file ရှိ `module bsimbulk(d, g, s, b, t);` line မှ သတ်မှတ်သည်။ .model line အတွက် modeltype ကို ရယူရန် *.va file တွင် module name, nodes အရေအတွက်နှင့် ၎င်းတို့၏အဓိပ္ပါယ်များကို ရှာဖွေရပါမည်။

ပုံစံ parameter set တစ်ခုကို ပြင်ဆင်ရန် သင့်လျော်သော model parameter set ကိုရွေးချယ်ပြီး version နှင့် level parameters များကို comment out လုပ်ကာ type parameter ကိုထည့်ကာ modeltype ကို Verilog-A module name သို့ပြောင်းပါ။

#### ၉.၃.၁.၆ ngspice netlist ပြင်ဆင်ခြင်း
compile လုပ်ထားသော ပုံစံ (ဥပမာ bsimbulk.osdi) ကို ngspice ထဲသို့ load လုပ်ရမည်။ spinit မှတစ်ဆင့် osdi commands များထည့်သွင်းထားပါက ngspice စတင်ချိန်တွင် အလိုအလျောက် load နိုင်သည်။

မည်သည့် directory တွင်မဆိုရှိသော *.osdi ဖိုင်များကို .control section (12.4.3) အတွင်းမှ `pre_osdi` command ဖြင့် local အသုံးပြုနိုင်သည်။

OSDI စက်ပစ္စည်းများအတွက် reference designator မှာ N ဖြစ်သည်။ Instance line ပုံစံမှာ-
```
Ndevname node1 ... nodex mname pname1=pval1 pname2=pval2 ...
```
ဥပမာ-
```spice
Np1 z a vdd vdd BSIMBULK_osdi_P l=0.1u w=1u
Nn1 z a vss vss BSIMBULK_osdi_N l=0.1u w=0.5u
```
NMOS နှင့် PMOS စက်ပစ္စည်းများကို ၎င်းတို့၏ သက်ဆိုင်ရာ model names များဖြင့် ရွေးချယ်သည်။ node အရေအတွက်နှင့် အခန်းကဏ္ဍကို VA code ရှိ module statement (ဥပမာ module bsimbulk(d, g, s, b, t);) တွင် သတ်မှတ်ထားသည်။ Instance parameters များ (l, w, as စသည်) ကို VA code က သတ်မှတ်ထားသည့်အတိုင်း ခွင့်ပြုသည်။

#### ၉.၃.၁.၇ သရုပ်ဖော်မှု run လုပ်ခြင်း
ပုံမှန်အတိုင်း simulation ကို run နိုင်သည်။ ngspice စတင်စဉ် spinit မှတစ်ဆင့် OSDI libraries များကို load လုပ်သောအခါ Batch mode ကို အထူးထောက်ပံ့သည်။

### ၉.၃.၂ Ngspice နှင့်အတူ ဖြန့်ဝေသော OSDI/OpenVAF နမူနာများ

ngspice/examples/osdi folder တွင် example input netlists အများအပြားရှိသည်။ bsimbulk-local မှလွဲ၍ ကျန်အားလုံးသည် spinit မှညွှန်ပြသော folder တစ်ခုရှိ *.osdi installation ကို bulk model install အဖြစ် အသုံးပြုသည်။ bsimbulk-local သည် bsimbulk.osdi ၏ local copy ကို လိုအပ်သည်။

Bsimbulk, bsimbulk-local, bsimcmg, mixed-models, and psp103 နမူနာ folders များတွင် MOS စက်ပစ္စည်းများ၏ dc characteristics, CMOS inverters, CMOS ring oscillators, 15,000 transistors ပါသော 7552_ann benchmark CMOS circuit နှင့် အခြား passives များစွာ ပါဝင်သည်။ Hicuml0, mextram တွင် bipolar စက်ပစ္စည်းများ၏ output characteristics, Gummel-plot နှင့် circuits အချို့ပါဝင်ပြီး r2_cmc သည် အထူး resistor ပုံစံတစ်ခုဖြစ်သည်။
