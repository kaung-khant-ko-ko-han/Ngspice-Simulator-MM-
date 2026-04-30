# ၁၂။ Ngspice စတင်ခြင်း

## ၁၂.၁ နိဒါန်း

Ngspice တွင် simulator နှင့် data analysis and plotting အတွက် front-end တစ်ခု ပါဝင်သည်။ Simulator အတွက် input မှာ circuit analysis and output control အတွက် commands များ အပါအဝင် netlist file တစ်ခု ဖြစ်သည်။ Interactive ngspice သည် PC သို့မဟုတ် workstation display တစ်ခုပေါ်တွင် simulation မှ data များကို plot လုပ်နိုင်သည်။

Ngspice (နှင့် MacOS၊ Cygwin၊ BSD၊ Solaris အစရှိသော OS များ) ကို Linux ပေါ်တွင် ပုံမှန်အားဖြင့် run ရန် နည်းလမ်းမှာ options များနှင့် အနည်းဆုံး netlist file တစ်ခုကို parameter အဖြစ် ပေးပို့သော console command ဖြစ်သည်။ Netlists များစွာကို concatenate လုပ်၍ တစ်ခုတည်းအဖြစ် ဆက်ဆံသည်၊ ပထမ file သည် parameters (13.8) ပါသော pure script တစ်ခု ဖြစ်သည့်အခါမှလွဲ၍ ဖြစ်သည်။

Ngspice သည် environment variable `DISPLAY` ရရှိနိုင်ပါက plotting အတွက် X Window System ကို အသုံးပြုသည်။ (MacOS တွင် X11 server တစ်ခု ဦးစွာ install လုပ်ရမည်။) မဟုတ်ပါက console mode (graphical မဟုတ်) interface ကို အသုံးပြုသည်။ Workstation တစ်ခုပေါ်တွင် X ကို အသုံးပြုနေပါက `DISPLAY` variable ကို သတ်မှတ်ပြီးသား ဖြစ်သင့်သည်။ သင်သည် ngspice သို့မဟုတ် ngnutmeg ကို run နေသော system နှင့် မတူသော system တစ်ခုပေါ်တွင် graphics ကို display လုပ်လိုပါက `DISPLAY` သည် `machine:0.0` ပုံစံ ဖြစ်သင့်သည်။

MS Windows GUI version ngspice တွင် native graphics interface (Chapt. 14.1 ကိုကြည့်ပါ) ရှိသည်။

front-end ကို `ngnutmeg` ဟူသော အမည်ဖြင့် သီးခြား 'stand-alone' program အဖြစ် run နိုင်သည်။ ngnutmeg သည် ngspice ၏ subset တစ်ခုဖြစ်ပြီး data evaluation အတွက်သာ ရည်ရွယ်ကာ historical အကြောင်းပြချက်များအတွက် (Linux, Mingw) compile လုပ်နိုင်ဆဲဖြစ်သည်။ Ngnutmeg သည် `ngspice -r` သို့မဟုတ် interactive ngspice session အတွင်း `write` command မှ ဖန်တီးသော 'raw' data output file ကို ဖတ်ရှုလိမ့်မည်။

## ၁၂.၂ Ngspice ရယူနိုင်သည့်နေရာ

Ngspice ၏ လက်ရှိ distribution ကို ngspice download web page မှ download လုပ်နိုင်သည်။ Linux သို့မဟုတ် MS Windows အတွက် installation ကို top level directory တွင်ရှိသော `INSTALL` file တွင် ဖော်ပြထားသည်။ Compile လုပ်ခြင်းဆိုင်ရာ ညွှန်ကြားချက်များအတွက် ဤလက်စွဲ၏ Chapt. 28 ကိုလည်း ကြည့်နိုင်သည်။

အမှန်တကယ် development လုပ်နေသော source code ကို စစ်ဆေးလိုပါက Git Source Code Management (SCM) tool ကိုအသုံးပြု၍ သိမ်းဆည်းထားသော ngspice source code repository ကို ကြည့်နိုင်သည်။ Git repository ကို Git web page တွင် browse လုပ်နိုင်ပြီး individual files များ download လုပ်ရန်အတွက်လည်း အသုံးဝင်သည်။ သို့သော် console window (Linux, CYGWIN or MSYS/MINGW) မှ command တစ်ခုထုတ်၍ source code trees အားလုံးအပါအဝင် repository တစ်ခုလုံးကို download (သို့မဟုတ် clone) လုပ်နိုင်သည်-
```
git clone git://git.code.sf.net/p/ngspice/ngspice
```
Git ကို install လုပ်ထားရန် လိုအပ်သည်။ ထို့နောက် source tree တစ်ခုလုံးကို `<current directory>/ngspice` တွင် ရရှိနိုင်သည်။ Compilation နှင့် local installation ကို `INSTALL` (သို့မဟုတ် Chapt. 28) တွင် ထပ်မံဖော်ပြထားသည်။ သင်၏ local repository သို့ SourceForge မှ recent changes များကို update လုပ်ရန် ngspice directory ထဲသို့ ဝင်၍ `git pull` ဟုရိုက်ပါ။ `git pull` သည် သင်၏ working directory ရှိ modified files များကို overwrite မလုပ်ပါ။ သင်၏ local changes များကို ဦးစွာဖျက်လိုပါက `git reset --hard` ကို run နိုင်သည်။

## ၁၂.၃ Ngspice စတင်ရန်အတွက် Command line options

**Command Synopsis:**
```
ngspice [ -o logfile] [ -r rawfile] [ -b ] [ -i ] [ input files ]
```
အသုံးမပြုတော့သော (obsolete) ngnutmeg ကို အောက်ပါအတိုင်း ခေါ်နိုင်သည်-
```
ngnutmeg [ - ] [ datafile ... ]
```
data file သည် standard ngspice rawfile ဖြစ်သည်။

အောက်တွင် options များကို ဖော်ပြထားသည်-

| Option | Long option | Meaning |
|--------|-------------|---------|
| - | | Don't try to load the default data file ("rawspice.raw") if no other files are given (ngnutmeg only, obsolete). |
| -n | --no-spiceinit | Don't try to source the file .spiceinit upon start-up. Normally ngspice seeks to find it according to the search folder sequence described in 12.6. |
| -t TERM | --terminal=TERM | The program is being run on a terminal with mfb name term (obsolete). |
| -b | --batch | Run in batch mode. Ngspice reads the default input source (e.g. keyboard) or reads the given input file and performs the analyses specified; output is either Spice2-like line-printer plots ("ascii plots") or a ngspice rawfile. See the following section for details. Note that if the input source is not a terminal (e.g. using the IO redirection notation of "<") ngspice defaults to batch mode (-i overrides). This option is valid for ngspice only. |
| -s | --server | Run in server mode. This is like batch mode, except that a temporary rawfile is used and then written to the standard output, preceded by a line with a single "@", after the simulation is done. This mode is used by the ngspice daemon. This option is valid for ngspice only. Example for using pipes from the console window: `cat adder.cir|ngspice -s|more` |
| -i | --interactive | Run in interactive mode. This is useful if the standard input is not a terminal but interactive mode is desired. Command completion is not available unless the standard input is a terminal, however. This option is valid for ngspice only. |
| -r FILE | --rawfile=FILE | Use rawfile as the default file into which the results of the simulation are saved. This option is valid for ngspice only. |
| -p | --pipe | Allow a program (e.g., xcircuit) to act as a GUI frontend for ngspice through a pipe. Thus ngspice will assume that the input pipe is a tty and allow running in interactive mode. |
| -o FILE | --output=FILE | All logs generated during a batch run (-b) will be saved in outfile. |
| -h | --help | A short help statement of the command line syntax. |
| -v | --version | Prints a version information. |
| -a | --autorun | Start simulation immediately, as if a control section `.control run .endc` had been added to the input file. |
| --soa-log=FILE | | output from Safe Operating Area (SOA) check. |
| -D | --define | Set a variable (13.8.1), to be used in a .control section. -D var1 will set a boolean variable named var1, -D var2=7 will set a variable with its value. |

Ngspice သို့ ထပ်ဆောင်း arguments များကို ngspice input files များအဖြစ် ယူဆပြီး ၎င်းတို့ကို ဖတ်၍ save လုပ်သည် (batch mode တွင် run နေပါက ၎င်းတို့ကို ချက်ချင်း run သည်)။ Ngspice သည် Spice3 (နှင့် Spice2 အများစု) input files များကို လက်ခံပြီး ASCII plots, Fourier analyses, and node printouts များကို `.plot`, `.four`, and `.print` cards များတွင် သတ်မှတ်ထားသည့်အတိုင်း output ထုတ်ပေးသည်။

ngnutmeg အတွက် ထပ်ဆောင်း arguments များကို binary or ASCII raw file format ဖြင့် data files များအဖြစ် ယူဆပြီး ngnutmeg ထဲသို့ load လုပ်သည်။ File သည် binary format ဖြစ်ပါက တစ်စိတ်တစ်ပိုင်းသာ ပြီးစီးနိုင်သည် (simulation မပြီးဆုံးမီ output ကို စစ်ဆေးရန် အသုံးဝင်သည်)။ File တစ်ခုတွင် မတူညီသော analyses များမှ data sets မည်မျှမဆို ပါဝင်နိုင်သည်။

## ၁၂.၄ စတင်ခြင်း ရွေးချယ်မှုများ

### ၁၂.၄.၁ Batch mode

Chapt. 17.6 တွင်ပြထားသော adder-mos.cir တွင်သိမ်းထားသည့် Four-Bit binary adder MOS circuit ကို ဥပမာအဖြစ်ကြည့်ပါ။ simulaion ကို အောက်ပါအတိုင်း ချက်ချင်းစတင်နိုင်သည်-
```
ngspice -b -r adder.raw -o adder.log adder-mos.cir
```
ngspice သည် စတင်မည်၊ `.tran` command အတိုင်း simulate လုပ်မည်၊ output data များကို rawfile `adder.raw` တွင် သိမ်းမည်။ Comments, warnings and info messages များကို log file `adder.log` သို့ ပို့မည်။ Batch mode operation အတွက် commands များကို Chapt. 11 တွင် ဖော်ပြထားသည်။

### ၁၂.၄.၂ Interactive mode

အကယ်၍ သင်သည်
```
ngspice
```
ဟုခေါ်ပါက၊ ngspice သည် စတင်မည်၊ `spinit` (12.5) နှင့် `.spiceinit` (12.6, ရှိပါက) ကို load လုပ်မည်၊ ထို့နောက် သင်၏ manual input ကို စောင့်ဆိုင်းနေမည်။ 13.5 တွင် ဖော်ပြထားသော commands များ မည်သည့်အမိန့်ကိုမဆို ရွေးချယ်နိုင်သော်လည်း၊ ၎င်းတို့အနက် အများအပြားမှာ circuit တစ်ခုကို `source adder-mos.cir` ဖြင့် load လုပ်ပြီးမှသာ အသုံးဝင်သည်-
```
ngspice 1 -> source adder-mos.cir
```
အခြားသူများမှာ simulation ကို ပြီးမြောက်ရန် လိုအပ်သည် (ဥပမာ plot)-
```
ngspice 2 -> run
ngspice 3 -> plot allv
```
အကယ်၍ circuit file တစ်ခုကို parameter အဖြစ် ထည့်၍ ngspice ကို command line မှ ခေါ်ပါက-
```
ngspice adder-mos.cir
```
ngspice သည် စတင်မည်၊ circuit file ကို load လုပ်မည်၊ circuit ကို parse လုပ်မည် (analysis and output control အတွက် dot commands (Chapt. 11 ကိုကြည့်ပါ) သာပါဝင်သော အထက်ပါ circuit file နှင့် အတူတူပင်)။ ngspice သည် ထို့နောက် သင်၏ input ကိုသာ စောင့်ဆိုင်းနေမည်။ `run` command ကို ထုတ်ခြင်းဖြင့် simulation ကို စတင်နိုင်သည်။ simulation ပြီးဆုံးပြီးနောက် Chapt. 13.5 တွင် ဖော်ပြထားသော commands များဖြင့် data များကို ခွဲခြမ်းစိတ်ဖြာနိုင်သည်။

### ၁၂.၄.၃ Control mode (control file သို့မဟုတ် control section ပါသော Interactive mode)

သင်၏ input file `adder-mos.cir` သို့ အောက်ပါ control section ကို ထည့်ပါက-
```
ngspice adder-mos.cir
```
ဟု command line မှ ခေါ်ပြီး ngspice စတင်၊ simulation ပြုလုပ်၊ ထို့နောက် ချက်ချင်း plotting ပြုလုပ်သည်ကို ကြည့်ရှုနိုင်သည်။

**Control section:**
```spice
*
ADDER - 4 BIT ALL-NAND-GATE BINARY ADDER
.control
save vcc#branch
run
plot vcc#branch
rusage all
.endc
```
Chapt. 13.5 တွင် ဖော်ပြထားသော မည်သည့် suitable command ကိုမဆို control section သို့ ထည့်နိုင်သည့်အပြင် Chapt. 13.6 တွင် ဖော်ပြထားသော control structures များကိုလည်း ထည့်နိုင်သည်။ Control section ကို အောက်ပါအတိုင်း ပြောင်းလဲခြင်းဖြင့် Batch-like behavior ကို ရယူနိုင်သည်-

**Batch-like behavior ပါသော Control section:**
```spice
.control
save vcc#branch
run
write adder.raw vcc#branch
quit
.endc
```
ဤ control section ကို file တစ်ခု (ဥပမာ `adder-start.sp`) တွင် ထည့်ပြီး `.include adder-start.sp` လိုင်းကို သင့် input file `adder-mos.cir` သို့ ထည့်ခြင်းဖြင့် batch-like behavior ကို ရရှိနိုင်သည်။ အောက်ပါဥပမာတွင် input file မှ `.tran ...` လိုင်းကို control section မှပေးသော `tran` command ဖြင့် override လုပ်သည်။

**`.tran` command ကို override လုပ်သော Control section:**
```spice
.control
save vcc#branch
tran 1n 500n
plot vcc#branch
rusage time
.endc
```
`.control` section အတွင်းရှိ Commands များကို ၎င်းတို့ ဖော်ပြထားသည့် အစဉ်အတိုင်း လုပ်ဆောင်ပြီး circuit ကို ဖတ်ပြီး parse လုပ်ပြီးမှသာ လုပ်ဆောင်သည်။ circuit parsing မတိုင်မီ command တစ်ခုကို execute လုပ်လိုပါက command ၏ရှေ့တွင် prefix `pre_` (13.5.57) ကို သုံးနိုင်သည်။

သတိပေးချက်တစ်ခုမှာ- သင့် circuit file တွင် ဤကဲ့သို့သော control section (`.control ... .endc`) ပါဝင်ပါက ngspice ကို batch mode ( `-b` parameter ဖြင့်) မစတင်သင့်ပါ။ ရလဒ်သည် ခန့်မှန်းမရနိုင်ပေ။

## ၁၂.၅ Standard configuration file spinit

စတင်ချိန်တွင် ngspice သည် ၎င်း၏ configuration file `spinit` ကို ဖတ်သည်။ `spinit` ကို ngspice executable တည်ရှိရာ နေရာနှင့် ဆက်စပ်သော path တစ်ခုဖြစ်သည့် `..\share\ngspice\scripts` တွင် တွေ့နိုင်သည်။ Environment variable `SPICE_SCRIPTS` ကို `spinit` တည်ရှိရာ path တစ်ခုသို့ သတ်မှတ်ခြင်းဖြင့် path ကို override လုပ်နိုင်သည်။ Windows အတွက် Ngspice သည် `spinit` ကို `ngspice.exe` တည်ရှိရာ directory တွင်လည်း ထပ်မံရှာဖွေမည်။ `spinit` ကို ရှာမတွေ့ပါက သတိပေးချက် message ထုတ်ပေးသော်လည်း ngspice ဆက်လက်လုပ်ဆောင်သည်။

`spinit` တွင် Chapt. 13.5 မှ commands များဖြင့် ရေးသားထားသော script တစ်ခုပါဝင်ပြီး ၎င်းကို ngspice စတင်ချိန်တွင် run သည်။ Aliases များ သတ်မှတ်နိုင်သည်။ ကြယ်ပွင့် '*' သည် line တစ်ကြောင်းကို comment out လုပ်သည်။ ngspice မှ အသုံးပြုပါက `spinit` သည် XSPICE code models များကို ngspice executable တည်ရှိသည့် လက်ရှိ directory နှင့် ဆက်စပ်သော path တစ်ခုမှလည်းကောင်း၊ OpenVAF compiled compact devices models များကိုလည်း load လုပ်လိမ့်မည်။ Absolute paths များကိုလည်း သတ်မှတ်နိုင်သည်။

libraries အတွက် standard path (အထက်ပါ standard spinit သို့မဟုတ် CYGWIN နှင့် Linux အောက်ရှိ `/usr/local/lib/spice`) သည် မလုံလောက်ပါက codemodel search path ကို `/usr/lib64/spice` သို့ သတ်မှတ်ရန် `./configure --prefix=/usr --libdir=/usr/lib64` options များကို ထည့်နိုင်သည်။

**Standard spinit ၏အနှစ်သာရများ:**
- `alias exit quit` နှင့် `alias acct rusage all` ကဲ့သို့သော aliases များ သတ်မှတ်ခြင်း။
- OpenMP threads အရေအတွက် သတ်မှတ်ခြင်း (`set num_threads=8`)။
- Shared mode ဟုတ်/မဟုတ် ပေါ်မူတည်၍ `interactive` mode ကို သတ်မှတ်ခြင်း။
- OSDI management မရှိပါက `unset osdi_enabled` ကို comment out လုပ်ခြင်း။
- XSPICE ကို enable လုပ်ထားပါက code models များ (`spice2poly.cm`, `analog.cm`, `digital.cm`, `xtradev.cm`, `xtraevt.cm`, `table.cm`) ကို load လုပ်ခြင်း။
- OSDI ကို enable လုပ်ထားပါက OpenVAF/OSDI models များ (ဥပမာ `BSIMBULK107.osdi`) ကို load လုပ်ခြင်း။

ngspice shared library ကို အသုံးပြုသည့်အခါ အထူးဂရုစိုက်ရမည်။ Windows OS အောက်တွင် `ngspice.dll` ကို အသုံးပြုပါက အထက်တွင်ပြထားသည့်အတိုင်း code models များအတွက် relative paths များကို အသုံးပြုခြင်းသည် standard ဖြစ်သည်။ သို့သော် path သည် dll နှင့်မဟုတ်ဘဲ calling program နှင့် ဆက်စပ်သည်။ `ngspice.dll` နှင့် calling program တို့သည် directory တူညီပါက ၎င်းသည် အဆင်ပြေသည်။ `ngspice.dll` ကို မတူညီသော directory တစ်ခုတွင် ထားရှိပါက Chapt. 28.2 ကို စစ်ဆေးပါ။

## ၁၂.၆ User defined configuration file .spiceinit

`spinit` အပြင် သင် (personal) file `.spiceinit` ကို သတ်မှတ်၍ current directory သို့မဟုတ် home directory တွင် ထားနိုင်သည်။ `.spiceinit` အတွက် ပုံမှန်ရှာဖွေမှုအစဉ်မှာ- user provided directory (env. variable `SPICE_USERINIT_DIR`), current directory, HOME (Linux) and then USERPROFILE (Windows) ဖြစ်သည်။

`.spiceinit` ကို `spinit` ပြီးနောက် သို့သော် အခြား input file တစ်ခုခု မဖတ်မီ ဖတ်ပြီး execute လုပ်မည်။ ၎င်းတွင် ထပ်ဆောင်း scripts များ၊ variables များ သတ်မှတ်ခြင်း၊ သို့မဟုတ် `spinit` တွင်ပေးထားသော commands များကို override လုပ်ရန် Chapt.13.5 မှ commands များ ပါဝင်နိုင်သည်။ ဥပမာ `set filetype=ascii` သည် ထုံးစံအတိုင်း အသုံးပြုသော compact binary format အစား output data file (rawfile) တွင် ASCII output ကို ဖြစ်ပေါ်စေမည်။ `set ngdebug` သည် ထပ်ဆောင်း debug output များစွာကို ထုတ်ပေးမည်။ Script ၏ အခြား contents များဖြစ်သော plotting preferences များကိုလည်း ဤနေရာတွင် ထည့်သွင်းနိုင်သည်။ ngspice စတင်ချိန်တွင် command line option `-n` ကို အသုံးပြုပါက ဤဖိုင်ကို လျစ်လျူရှုမည်။

PDKs မှ MOS transistor data များဖြင့် IC designs များကို simulation လုပ်ရန်အတွက် `.spiceinit` ဥပမာများ၊ PSPICE-compatible behavioural models များ simulation လုပ်ရန် ဥပမာများကို manual တွင် ဖော်ပြထားသည်။ MS Windows ရှိ editor အချို့သည် အမည်၏ရှေ့တွင် dot ပါသော ဖိုင်များကို save မလုပ်နိုင်ပါ။ `.spiceinit` အတွက် alternative name မှာ `spice.rc` ဖြစ်သည်။

## ၁၂.၇ Environmental variables

### ၁၂.၇.၁ Ngspice specific variables

- `SPICE_LIB_DIR`, `SPICE_EXEC_DIR`, `SPICE_BUGADDR`, `SPICE_EDITOR`, `SPICE_ASCIIRAWFILE`, `SPICE_NEWS`, `SPICE_HELP_DIR`, `SPICE_HOST`, `SPICE_SCRIPTS`, `SPICE_PATH`, `NGSPICE_MEAS_PRECISION`, `SPICE_NO_DATASEG_CHECK`, `NGSPICE_INPUT_DIR`, `NGSPICE_OSDI_DIR`, `SPICE_USERINIT_DIR` စသည်တို့ဖြင့် ngspice ၏ လုပ်ဆောင်ချက်ကို ထိန်းချုပ်နိုင်သည်။

### ၁၂.၇.၂ Common environment variables

`TERM`, `LINES`, `COLS`, `DISPLAY`, `HOME`, `PATH`, `EDITOR`, `SHELL`, `POSIXLY_CORRECT` စသည့် common variables များကိုလည်း ngspice က အသိအမှတ်ပြုသည်။

## ၁၂.၈ Memory အသုံးပြုမှု

Batch option (`-b`) နှင့် rawfile output (`-r rawfile`) ဖြင့် စတင်သော Ngspice သည် simulation data အားလုံးကို memory တွင် မသိမ်းဘဲ rawfile ထဲသို့ ချက်ချင်း သိမ်းဆည်းမည်။ ထို့ကြောင့် အလွန်ကြီးမားသော circuits များကို simulate လုပ်နိုင်ပြီး ngspice စတင်ချိန်တွင် တောင်းဆိုသော memory သည် circuit size ပေါ်မူတည်သော်လည်း simulation အတွင်း တိုးမလာပါ။

Interactive mode သို့မဟုတ် control section ဖြင့် interactively စတင်ပါက data အားလုံးကို နောက်ပိုင်း evaluation အတွက် memory တွင် သိမ်းထားသည်။ ကြီးမားသော circuit သည် Gigabytes ပမာဏပင် memory ကုန်သွားနိုင်သည်။ `save <nodes>` command သည် memory usage ကို လျှော့ချရန် အကူအညီပေးသည်။ `INTERP` option (11.1.4) ကိုလည်း ရွေးချယ်နိုင်သည်။

## ၁၂.၉ Simulation အချိန်

ကြီးမားသော circuits များကို simulate လုပ်ခြင်းသည် CPU time အတော်အတန် ကြာနိုင်သည်။ အရေးကြီးပါက ngspice ကို speed အတွက် optimization flags များဖြင့် compile လုပ်သင့်သည်။ Linux, MINGW, CYGWIN, and macOS အောက်တွင် compilation scripts များကို ngspice distribution ၏ main directory တွင် ကြည့်ပါ။ MS Visual Studio အောက်တွင် releaseOMP သို့မဟုတ် release configurations ကို ရွေးချယ်ရမည်။

CPU time ကို setup period, KLU solver, device evaluation (OpenMP) နှင့် data evaluation တို့က ကုန်ဆုံးစေသည်။ XSPICE ကို enable လုပ်ထားပါက transient simulation အတွက် CPU time ကို နှစ်ဆ သို့မဟုတ် သုံးဆပင် မြင့်တက်စေနိုင်သည်။ `xtrtol` variable ကို သုံး၍ trtol ကို 7 သို့ reset လုပ်ကာ အမြန်နှုန်းကို မြှင့်နိုင်သော်လည်း precision or convergence issues များ သတိထားရမည်။

## ၁၂.၁၀ Multi-core processors များပေါ်တွင် Ngspice ကို OpenMP သုံး၍ အသုံးပြုခြင်း

### ၁၂.၁၀.၁ နိဒါန်း

ယနေ့ခေတ် computers များတွင် core တစ်ခုထက်ပိုသော CPU များ ပါလာသောကြောင့် ngspice က multi-core processors များ၏ အားသာချက်ကို ယူနိုင်အောင် မြှင့်တင်ခြင်းသည် အသုံးဝင်သည်။

Transistors နှင့် BSIM3 model များကို အဓိကသုံးသော circuits များတွင် CPU time ၏ ၂/၃ ခန့်သည် model equations များကို အကဲဖြတ်ရာတွင် ကုန်ဆုံးသည်။ ထို့ကြောင့် ထို functions များကို parallelize လုပ်သင့်သည်။ Matrix solving သည် CPU time ၏ 10% မှ 50% ခန့်သာ ကြာသောကြောင့် matrix solver တွင် parallel processing သည် တစ်ခါတစ်ရံ secondary interest သာဖြစ်သည်။

နောက်ထပ် alternative မှာ CUSPICE (ngspice-27 ကိုအခြေခံ) ဖြစ်ပြီး NVIDIA GPUs များပေါ်တွင် massively parallel run ရန်ဒီဇိုင်းထုတ်ထားသည်။

### ၁၂.၁၀.၂ အတွင်းပိုင်းများ

OpenMP ကိုအသုံးပြု၍ BSIM3 version 3.3.0 model ကို paralleling အတွက် ဥပမာအဖြစ် ပထမဆုံးရွေးချယ်ခဲ့သည်။ BSIM3load() function သည် for-loop ကို run ရန် array နှင့် wrapper function ကိုသုံးပြီး instance တစ်ခုချင်းစီအတွက် BSIM3LoadOMP() function ကို evaluate လုပ်ရန် OpenMP parallel for loop ကို အသုံးပြုသည်။ Synchronization အတွက် temporary per-thread memory ကိုသုံးသည်။

### ၁၂.၁၀.၃ ရလဒ်အချို့

627 CMOS inverters, BSIM4.7, 45nm, 200ns simulation တစ်ခုတွင် 8 cores (i9 9900K) ဖြင့် speed up 2x ကျော်ရရှိသည်။ Dual core i7 notebook တွင်ပင် 1.5x improvement ရရှိသည်။ 8 thread အထက် သုံးပါက hyperthreading အတွက် optimized မဖြစ်၍ improve များများမရ။

### ၁၂.၁၀.၄ အသုံးပြုပုံ

OpenMP ကို BSIM3 (v3.3.0, 3.2.4), BSIM4 (v4.5, 4.6.5, 4.7, 4.8), B4SOI (v4.4) နှင့် OSDI models များတွင် installed လုပ်ထားသည်။ Compilation တွင် default enable ဖြစ်သည်။ Threads အရေအတွက်ကို `set num_threads=4` ဖြင့် spinit သို့မဟုတ် .spiceinit တွင် သတ်မှတ်ရမည်။ မသတ်မှတ်ပါက default 2 ဖြစ်သည်။

နောက်ဆုံးတွင်၊ အတိအကျ model version ကိုရွေးချယ်ရန် သေချာရမည်။ Compilation တွင် OpenMP ကို disable လုပ်ပါက standard models များသာ ရရှိသည်။ `num_threads=1` ဖြင့် OpenMP ကိုသုံးပါက CPU time disadvantage အနည်းငယ် (~3%) သာရှိသည်။

### ၁၂.၁၀.၅ အကိုးအကားစာပေ

[1] R.K. Perng, T.-H. Weng, and K.-C. Li: "On Performance Enhancement of Circuit Simulation Using Multithreaded Techniques", IEEE International Conference on Computational Science and Engineering, 2009, pp. 158-165

## ၁၂.၁၁ Server mode option -s

တစ်စုံတစ်ခုသော program သည် SPICE input ကို console သို့ write နိုင်သည်။ ဤ output ကို '|' ဖြင့် ngspice သို့ redirect လုပ်သည်။ `-s` option ဖြင့် ခေါ်သော ngspice သည် ၎င်း၏ output ကို console သို့ write ပြီး ၎င်းကို '|' ဖြင့် receiving program သို့ redirect လုပ်သည်။ အောက်ပါ ရိုးရှင်းသော ဥပမာတွင် `cat` သည် input file ကိုဖတ်၍ ၎င်း၏ content ကို console သို့ print ထုတ်သည်၊ ၎င်းကို ပထမ pipe ဖြင့် ngspice သို့ redirect လုပ်ကာ ngspice က ၎င်း၏ output (raw file နှင့်ဆင်တူသည်) ကို ဒုတိယ pipe ဖြင့် `less` သို့ လွှဲပြောင်းသည်။
```
cat input.cir|ngspice -s|less
```
MS Windows အောက်တွင် ဤ server mode အသုံးပြုရန်အတွက် ngspice ကို console application (Chapt. 28.2.4 ကိုကြည့်ပါ) အဖြစ် compile လုပ်ရမည်။

စက်ပိတ်ခြင်းအန္တရာယ်ကို ကာကွယ်ရန် Linux တွင် `ctrl Z` ကို ဤနေရာတွင် မသုံးနိုင်ပါ၊ `ctrl D` ကို install လုပ်ရန် patch တစ်ခု အကဲဖြတ်ဆဲဖြစ်သည်။

## ၁၂.၁၂ Pipe mode option -p

Program တစ်ခုသည် ngspice commands (13.5 ကိုကြည့်ပါ) အစုတစ်ခုကို console သို့ write နိုင်သည်။ ဤ output ကို '|' ဖြင့် ngspice သို့ redirect လုပ်သည်။ `-p` option ဖြင့် ခေါ်သော ngspice သည် commands များကို ချက်ချင်း execute လုပ်ပြီး ထို့နောက် ထွက်သွားသည်။ အောက်ပါ ရိုးရှင်းသော ဥပမာတွင် `cat` သည် input file ကိုဖတ်၍ ၎င်း၏ content ကို console သို့ print ထုတ်သည်၊ ၎င်းကို pipe ဖြင့် ngspice သို့ redirect လုပ်ပြီး ngspice က commands များကို execute လုပ်သည်။
```
cat pipe-circuit.cir | ngspice -p
```
MS Windows အောက်တွင် ဤ pipe mode အသုံးပြုရန်အတွက် ngspice ကို console application အဖြစ် compile လုပ်ရမည်။

Raw file `pcir.raw` တွင် နောက်ဆုံး simulation results များ ပါဝင်မည်။

## ၁၂.၁၃ Input, output fifos များမှတစ်ဆင့် Ngspice ထိန်းချုပ်ခြင်း

Bash script သည် အခြား thread တစ်ခုတွင် pipe mode (`-p`) ဖြင့် ngspice ကို launch လုပ်၍ ngspice input သို့ commands အချို့ကို write ကာ `tran` command ဖြင့် ngspice ကို run စေပြီး output ကိုဖတ်ကာ console ပေါ်သို့ print ထုတ်သည်။

## ၁၂.၁၄ Compatibility (သဟဇာတဖြစ်မှု)

ngspice သည် UC Berkeley မှ spice3f5 ၏ တိုက်ရိုက်ဆင်းသက်လာမှုဖြစ်ပြီး ၎င်း၏ ယခင် version ၏ commands များအားလုံးကို အမွေဆက်ခံသည်။ Commercial variants များသည် netlist syntax ကို ပြောင်းလဲကြသည်။ ngspice တွင် ဤ dialects များကို အသိအမှတ်ပြုသော features အချို့ပါဝင်သည်။ Compatibility mode ကို `ngbehavior` variable ဖြင့် သတ်မှတ်နိုင်သည်။

### ၁၂.၁၄.၁ Compatibility mode

Variable `ngbehavior` သည် compatibility mode ကိုသတ်မှတ်သည်။ Default အားဖြင့် compatibility mode မရွေးချယ်ထားပါ။ `set ngbehavior=ltpsa` သည် PSPICE နှင့် LTSPICE compatibility ကို netlist တစ်ခုလုံးအတွက် သတ်မှတ်သည်။ Flag 'a' ကို အောက်ပါ flags များနှင့် ပေါင်းစပ်နိုင်သည်။ `set ngbehavior=ps` (without 'a') သည် PSPICE compatibility ကို `.include` command ဖြင့် ထည့်သော libraries များအတွက်သာ သတ်မှတ်မည်။

ရရှိနိုင်သော compatibility flags များ-
- a: complete netlist transformed
- ps: PSPICE compatibility
- hs: HSPICE compatibility
- spe: Spectre compatibility
- lt: LTSPICE compatibility
- s3: Spice3 compatibility
- ll: all (currently not used)
- ki: KiCad compatibility
- eg: EAGLE compatibility
- mc: for 'make check'

### ၁၂.၁၄.၂ Missing functions

သင့် input file တွင် အောက်ပါအတိုင်း function definitions များ ထည့်နိုင်သည်-
```
.func LIMIT(x,a,b) {min(max(x, a), b)}
.func PWR(x,a) {abs(x) ** a}
.func PWRS(x,a) {sgn(x) * PWR(x,a)}
.func stp(x) {u(x)}
```

### ၁၂.၁၄.၃ Devices (E Source with LAPLACE, VSwitch အစရှိသည်)

E Source with LAPLACE ကို s_xfer code model ဖြင့် အစားထိုးနိုင်သည်။ VSwitch ကို aswitch code model ဖြင့် အစားထိုးနိုင်သည်။ XSPICE option ကို enabled လုပ်ထားရမည်။

### ၁၂.၁၄.၄ Controls and commands

`.lib` command ကို PSPICE compatibility mode တွင် `.inc` သို့မဟုတ် အခြားနည်းဖြင့် အစားထိုးနိုင်သည်။ ` .incpslt` command က 'pslt' compatibility mode ဖြင့် include လုပ်သည်။ `.step` အတွက် script (Chapt. 13.8.8) ကိုသုံးနိုင်သည်။

### ၁၂.၁၄.၅ PSPICE Compatibility mode

`set ngbehavior=ps` or `psa` ဖြင့် သတ်မှတ်ပါက ngspice သည် PSPICE syntax မှ ngspice သို့ files များကို translate လုပ်သည်။ PSPICE device libraries များကို ဖတ်နိုင်စေသည်။

### ၁၂.၁၄.၆ LTSPICE Compatibility mode

`set ngbehavior=lt` or `lta` ဖြင့် LTSPICE syntax မှ ngspice သို့ translate လုပ်သည်။ Simple diode ကို sidiode code model (8.2.32) အဖြစ် အလိုအလျောက် ဘာသာပြန်သည်။ RKM code compatibility ကိုလည်း ထောက်ပံ့သည်။

### ၁၂.၁၄.၇ LTSPICE/PSPICE Compatibility mode

`set ngbehavior=ltps` or `ltpsa` ဖြင့် LTSPICE နှင့် PSPICE နှစ်မျိုးလုံး၏ device libraries or netlists များကို ဖတ်နိုင်သည်။

### ၁၂.၁၄.၈ KiCad Compatibility mode

KiCad သည် vector names များတွင် '/' ပါဝင်သည်။ `set ngbehavior=ki` ဖြင့် name ကို double quotes ဖြင့် ဝန်းရံပေးသည်။

### ၁၂.၁၄.၉ Spectre Compatibility mode

`set ngbehavior=spe` ဖြင့် Spectre compatibility mode enable လုပ်သည်။ MOS device ၏ `nf` parameter ကို multi-gate transistor အတွက် binning process တွင် သုံးနိုင်သည်။

### ၁၂.၁၄.၁၀ HSPICE Compatibility mode

`set ngbehavior=hs` ဖြင့် HSPICE compatible PDKs များကို `.lib` command ဖြင့် recursive ဖတ်နိုင်စေသည်။ `nf` flag ကိုလည်း enable လုပ်သည်။

## ၁၂.၁၅ Tests (စမ်းသပ်မှုများ)

ngspice distribution တွင် စမ်းသပ် input and output files များပါသော suite တစ်ခုပါရှိသည်။ `$ make check` ဖြင့် စမ်းသပ်မှုများ လုပ်ဆောင်နိုင်သည်။ CMC Regression test ကဲ့သို့သော complex models များအတွက် မတူညီသော ဗျူဟာများ လိုအပ်သည်။ `--enable-shortcheck` configure option က runtime ကို လျှော့ချနိုင်သည်။

## ၁၂.၁၆ Circuit netlist တစ်ခုကို debug လုပ်ရန် ကိရိယာများ

### ၁၂.၁၆.၁ options နှင့် initial conditions

Operation point ရှာဖွေရာတွင် အခက်အခဲရှိပါက `.nodeset` or `.ic` ဖြင့် critical nodes များအတွက် initial conditions သတ်မှတ်ခြင်း၊ op option parameters များ ပြောင်းလဲခြင်း၊ `RSHUNT` option ဖြင့် node များမှ ground သို့ resistor ထည့်ခြင်း၊ `RSERIES` option ဖြင့် inductor တစ်ခုချင်းစီသို့ series resistor ထည့်ခြင်းဖြင့် convergence ကို မြှင့်တင်နိုင်သည်။ Transient simulations များကို အခြား options များ (11.1.4) ဖြင့် ထိန်းချုပ်နိုင်သည်။

### ၁၂.၁၆.၂ set debug

`.spiceinit` တွင် `set debug` ဟုသတ်မှတ်ပါက `.spiceinit` နှင့် `.control` မှ run သော command တစ်ခုချင်းစီ၏ analysis ကို yield လုပ်သည်။

### ၁၂.၁၆.၃ set ngdebug

`set ngdebug` က `.spiceinit` တွင်သတ်မှတ်ပါက ထပ်ဆောင်း warning messages များ ပေးသည်။ current directory တွင် write access ရှိပါက debug files များ save လုပ်သည်။ Transient simulation အတွင်း speed monitor vector တစ်ခု ထုတ်ပေးသည်။

### ၁၂.၁၆.၄ အထွေထွေ

B source debugging (Chapt. 5.4)၊ compilation flags `--enable-ftedebug` သို့မဟုတ် `STEPDEBUG` ဖြင့် ထပ်ဆောင်း debug information ရယူနိုင်သည်။

## ၁၂.၁၇ Bugs နှင့် errors များကို report လုပ်ခြင်း

Ngspice သည် ရှုပ်ထွေးသော software တစ်ခုဖြစ်သည်။ Source code တွင် files 1500 ကျော်ပါဝင်သည်။ error report အတွက် ngspice bug tracker ကို အသုံးပြုရန် အကြံပြုသည်။ Version, Operating system, reproduction input file, actual vs expected output များ ထည့်သွင်းသင့်သည်။
