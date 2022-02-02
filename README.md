
{
# Enrofloxacin2
## Barekely Madona codes for seven compartments BPPK model for enrofloxacin in calves
### 9 compartments: liver, kidney, lungs, muscle, fat, intestine, arterial blood, venous blood and the rest of the body
}

{A PBPK model for enrofloxacin and its metabolite ciprofloxacin in cattle:
Model written by ASHENAFI BEYI, in March 2021, 
adapted from initial model coded in acsIX by Lin et al. (2016) Scientific Reports 6:27907}


METHOD RK4

STARTTIME = 0
STOPTIME = 48 ; Data of plasma and fecal samples collected upto 48 hrs is available for model validation
DT = 0.005 ; 0.00001
DTOUT = 0.1

; Physiological parameters

; Blood flow rates
QCC = 9.09 ; cardiac output (L/h/kg) (Lin et al., 2020; Table 16)

;Fraction of blood flow to organs (unitless, percent)
QLC = 0.30 ;    Fraction of blood flow to the liver (Lin et al., 2020; Table 24; Hepatic artery = 0.04 and Portal vein = 0.28)
   ; QLhC = 0.04 ;  Fraction of blood flow to the liver hepatic artery (Lin et al., 2020; Table 24)?
   ; QLpC = 0.28 ;  Fraction of blood flow to the liver portal vein (from intestine, Lin et al., 2020; Table 24)?
QKC = 0.10 ;    Fraction of blood flow to the kidneys (Lin et al., 2020; Table 24)
QFC = 0.08 ;    Fraction of blood flow to the fat (Lin et al., 2016 SS Table 2, search for calves)
QMC = 0.18 ;    Fraction of blood flow to the muscle (Lin et al., 2016 SS Table 2, search for calves)
QLuC = 0.46 ;   Fraction of blood flow to the lungs (Lin et al., 2020; Table 24);
QItC = 0.11 ;   Fraction of blood flow to intestine (Lin et al., 2020; in Table 24 for GIT, search for intestines alone)
QRC = 1-QLC-QKC-QFC-QMC-QItC ;   Fraction of blood flow to the rest of the body (1-QLC-QKC-QFC-QMC-QItC), no QLuC?

; Tissue volumes
BW = 118 ;        Body weight (kg) (median of 35 calves in FQ AMR cattle study, ISU); need to be scaled up to their age at ENR adminstration (3 weeks)
VAC = 0.0180 ;    Fractional arterial blood (Computed from Lin et al., 2020 based on Lin et al., 2016*2), plasma=0.45*VVC
VVC = 0.0511 ;    Fractional venous blood (Computed from Lin et al., 2020 based on Lin et al., 2016*)
VbloodC = 0.0691; Fractional Blood volume, VbloodC=VAC+VVC (Lin et al., 2020; Table 7 )
VLC = 0.0287 ;    Fractional liver tissue (Lin et al., 2020; Table 7)
VKC = 0.0039 ;    Fractional kidney tissue (Lin et al., 2020; Table 7)
VFC = 0.0695 ;    Fractional fat tissue (Lin et al., 2020; Table 7)
VMC = 0.339 ;     Fractional muscle tissue (Lin et al., 2020; Table 7)
VLuC = 0.0123 ;   Fractional lung tissue in calves (Lin et al., 2020; Table 7)
VItC = 0.0239 ;   Fractional intestine volumes in calves (Lin et al., 2020; Table 7; the whole intestine or LI alone?)
VRC = 1-VLC-VKC-VFC-VMC-VLuC-VItC-VbloodC ; Fractional rest of body (1-VLC-VKC-VFC-VMC-VLuC-VIC-VbloodC = 0.5231), absent in Lecture10.5? 

; Mass Transfer parameters (Chemical-specific parameters)
; Chemical molecular weight, PubChem
MW = 359.39 ; g/mol, enrofloxacin ; 1 mole parent compound is metabolized to produce 1 mole of metabolite
MW1 = 331.34 ; g/mol, ciprofloxacin
MWmol = 2.78 ; umol/mg, enrofloxacin
MWmg = 0.36 ; mg/umol, enrofloxacin
MW1mol = 3.02 ; umol/mg, ciprofloxacin
MW1mg = 0.33 ; mg/umol, ciprofloxacin

; partition coefficients (PC, tissue:plasma)
; Parent drug, enrofloxacin
PL = 4.3 ;   Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK = 5.5 ;   Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF = 0.53 ;  Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM = 1.09 ;  Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu = 4.3 ;  Lung:plasma PC (Lin et al., 2016 SS Table 4)
PIt = 4.3 ;   Intestine:plasma, assumed to be similar to that of liver?
PR = 1.5 ;   Rest of body:plasma (Lin et al., 2016 SS Table 4) Is PR affected by the number of compartments? e.g. whether GIT is included or not?

; main metabolite, ciprofloxacin
PL1 = 4.3 ;   Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK1 = 5.5 ;   Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF1 = 0.53 ;  Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM1 = 1.09 ;  Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu1 = 4.3 ;  Lung:plasma PC (Lin et al., 2016 SS Table 4)
PIt1 = 4.3 ;   Intestine:plasma , assumed to be similar to that of liver?
PR1 = 1.5 ;   Rest of body:plasma  (Lin et al., 2016 SS Table 4)


;Kinetic constants
; Oral absorption and fecal elimination rate constants (for parent compound)
  ; We do not have oral exposure in the ENR cattle model
Kst= 0.0 ; /h, gastric emptying rate constant
Ka= 0.0 ; /h, intestinal absorption rate constant
Kfeces= 0.0 ; /h, intestinal transit rate constant (fecal elimination rate constant)

;Percentage of plasma protein binding (unitless) Davis et al., 2007
PB = 0.46 ;        ENR percentage of plasma protein binding (unitless)  (Lin et al., 2016 SS Table 4)
PB1 = 0.19 ;       CIP percentage of plasma protein binding (unitless) (Lin et al., 2016 SS Table 4s)

; Metabolic rate constants
Ksc = 0.06 ;       SC absorpation rate constant (/h) (Lin et al., 2016 SS Table 4)

Frac = 0.55 ;      Fraction of ENR metabolized to CIP (unitless),  (Lin et al., 2016 SS Table 4)
KmC = 0.06 ;       Hepatic metabolic rate [/(h*kg)] (Lin et al., 2016 SS Table 4)

KurineC = 0.15 ;   ENR urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4) 
Kurine1C = 1.99 ;  CIP urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4)

; SC infusion rate constants
Timesc = 0.00 ; SC injection/infusion (h)

; IV infusion/injection rate constants
Timeiv= 0.01 ; h, IV infusion/injection time

; IM absorption rate constants
Kim = 0.7 ; /h, intramuscular absorption rate constant

;?
{; Billiary elimination rate constant??
KfecesC = 0.01 ; ENR L/h/kg (Lin et al., 2016 SS Table 4, 0.01 if for swine)
KfecesC1 = 0.01 ; CIP L/h/kg It was not considered for CIP Lin et al 2016 but for unrinary excretion both ENR and CIP were included.
}

; Parameters for exposure scenario
PDOSEsc = 7.5;12.5   mg/kg Low dose and high dose
PDOSEiv = 0 ;        mg/kg
PDOSEim = 0 ;        mg/kg
PDOSEoral = 0 ;      mg/kg

; Cardiac output and blood flows to tissues (L/h)
QC = QCC*BW ; Cardiac output
QL = QLC*QC ; blood flow to the Liver
QK = QKC*QC ; blood flow to kidney
QF = QFC*QC ; blood flow to Fat
QLu = QLuC*QC ; blood flow to Lung
QM = QMC*QC ; blood flow to Muscle
QIt = QItC*QC ; Intestine
QR = QC-QL-QK-QF-QM-QIt ; blood flow to the rest of the body

; Tissue Volume (L)
VL = VLC*BW ; Liver
VK = VKC*BW ; Kidney
VF = VFC*BW ; Fat
VM = VMC*BW ; Muscle
VLu = VLuC*BW ; Lung
VIt = VItC*BW ; Intestine
VBlood=VBloodC*BW ; Blood
VartC = 0.26*VBloodC; Fraction blood as arterial, Leavens et al. 2014
VvenC = 0.74*VBloodC; Fraction blood as venous, Leavens et al. 2014
Vven = VvenC*BW
Vart = VartC*BW
VR = BW-VL-VK-VM-VF-VLu-VBlood-VIt; Rest of body

; Dosing amounts (mg converted to umol)
DOSEoral = PDOSEoral*BW*MWmol; umol
DOSEiv = PDOSEiv*BW*MWmol; umol
DOSEim = PDOSEim*BW*MWmol; umol
DOSEsc = PDOSEsc*BW*MWmol; umol

; Multiple oral dosing using the PULSE/EXPOSURE function, not required for my project
tlen= 0.001 ; Length of exposure, oral, iv, im, or sc(h/day)
tinterval= 24 ; Varied dependent on the exposure paradigm (h)
Dstart= 0.0 ; Initiation day of exposure (day)
Dstop= 1.0 ; Termination day of exposure (day)
MAXT = 1.0 ; maximum comm. interval
CINTC = 0.1 ; Communication interval
CINT = CINTC ; Communication interval

Tsim=TSTOP*24 ; Tstopin hours
DS=Dstart*24 ; Initiation time point of exposure (h)
Doff=(Dstop-Dstart)*24 ; Exposure duration (h)
TimeOn=Dstart*24 ; Initiation time point of exposure (h)
TimeOff=Dstop*24+tlen ; Termination time point of exposure (h)

Exposure=PULSE(0,tinterval,tlen)*PULSE(DS,Tsim,Doff)
RDOSEoral=(DOSEoral/tlen)*Exposure
RAST=RDOSEoral-Kst*AST
d/dt(AST)=RAST
init AST = 0
RAI=Kst*AST-Ka*AI-Kfeces*AI
Rfeces=Kfeces*AI
d/dt(Afeces) = Rfeces
init Afeces= 0
d/dt(AI)=RAI
init AI = 0
RAO = Ka*AI
d/dt(AAO)=RAO
init AAO = 0

;Single IV dosing to the venous
IVR = DOSEiv/timeiv
RIV = IVR*(1.0-step(1,timeiv))
d/dt(AIV) = RIV
init AIV = 0

; Dosing, repeated doses
tinterval = 24 ; varied dependent on the exposure paradigm (h)
Tdoses = 1     ; times for mutiple oral gavage
dosingperiod = if time < Tdoses*tinterval-DT then 1 else 0

; Dosing, IM, Intramuscular
Rinputim = pulse(DOSEim,0,tinterval)*dosingperiod
Rpenim = Rinputim
Rim = Kim*Amtsiteim
d/dt(Absorbim) = Rim
init Absorbim = 0
d/dt(Amtsiteim) = Rpenim-Rim
init Amtsiteim = 0

;Dosing, SC, subcutaneous
Rinputsc = pulse(DOSEsc,0,tinterval)*dosingperiod
Rpensc = Rinputsc
Rsc = Ksc*Amtsitesc
d/dt(Absorbsc) = Rsc
init Absorbsc = 0
d/dt(Amtsitesc) = Rpensc-Rsc
init Amtsitesc = 0

; Metabolic rate
Km = KmC*BW ; h-1, first order

; Urinary elimination rate constant
Kurine = KurineC*BW ; ENR
Kurine1 = Kurine1C*BW ; CIP

;?
{; Fecal elimination rate constant
Kfeces = KfecesC*BW ; ENR
Kfeces1 = Kfeces1C*BW ; CIP
}



; ...........ENR submodel............................
; CV = venous blood/plasma concentration
RV = QL*CVL+QK*CVK+QM*CVM+QF*CVF+QIt*CVIt+QR*CVR+Riv+Rim+Rsc-QC*CV
d/dt(AV) = RV
init AV = 0

CV = AV/Vven
CVfree = CV*(1-PB)
CVbound = CV*PB
CVmg = CV*MWmg; Unit conversion from umol/L to mg/L (ug/g), what would the unit for calibration/validation data? mg/mL?

; CA = arterial blood/plasma concentration
RA = QC*CVLu-QC*CAfree
d/dt(AA) = RA
init AA = 0
CA = AA/Vart
CAfree = CA*(1-PB)
CAbound = CA*PB

ABlood = AV+AA

; ENR in lung compartment
RALu = QC*(CV-CVLu)
d/dt(ALu) = RALu
init ALu= 0
CLu = ALu/VLu
CVLu = CLu/PLu

; ENR in liver compartment
RL = QL*(CAfree-CVL)+RAO-Rmet ; Rate of ENR change in liver
d/dt(AL) = RL
init AL = 0
CL = AL/VL ; concentration in the liver (AL - amount in liver, VL - Volume of liver)
CVL = CL/PL
CLmg = CL*MWmg ; concentration in liver venous blood

; Metabolism of ENR in liver compartment, first order metabolism rate because it does not saturate.
Rmet = Km*CL*VL
Rmet1 = Rmet*Frac
Rmet2 = Rmet*(1-Frac)
d/dt(Amet) = Rmet
init Amet= 0
d/dt(Amet1) = Rmet1
init Amet1 = 0
d/dt(Amet2) = Rmet2
init Amet2 = 0

; ENR in kidney compartment
RK = QK*(CAfree-CVK)-Rurine ; Rate of ENR change in kideny
d/dt(AK) = RK
init AK = 0
CK = AK/VK
CVK = CK/PK ; Concentration in kideny
Ckmg = Ck*MWmg ; Concentration in kideny venous blood

; Urinary excretion of parent compound
Rurine = Kurine*CVK
d/dt(Aurine) = Rurine
init Aurine = 0

; ENRO in muscle compartment
RM = QM*(CAfree-CVM) ; Rate of ENR change in muscle
d/dt(AM) = RM
init AM = 0
CM = AM/VM ; Concentration in muscle
CVM = CM/PM ; Cocentration in muscle venous blood
CMmg = CM*MWmg

; ENRO in fat compartment
RF = QF*(CAfree-CVF)
d/dt(AF) = RF
init AF = 0
CF = AF/VF
CVF = CF/PF ; Concentration of ENR in venous blood of fat

; ENRO in intestine compartment
RIt = QIt*(CAfree-CVIt)
d/dt(AIt) = RIt
init AIt = 0
CIt = AIt/VIt ; ENR concentration in intestine
CVIt = CIt/PIt ; Concentration in intestinal venous blood
CItmg = CIt*MWmg

; ENRO in rest of the body
RR = QR*(CAfree-CVR)
d/dt(AR) = RR
init AR = 0
CR = AR/VR
CVR = CR/PR ; Concentration in venous blood of the rest of body

; Mass balance
Qbal = QC-QL-QK-QM-QF-QIt-QR
Tmass = Ablood+AL+AK+AM+AF+AIt+AR+ALu+Aurine+Amet
Bal = AAO+AIV+Absorbim+Absorbsc-Tmass


;.......Submodel for the marker residue (CIP)..........

;CV1 = venous blood/plasma concentration of CIP
RV1 = QL*CVL1+QK*CVK1+QM*CVM1+QF*CVF1+QIt*CVIt1+QR*CVR1-QC*CV1
d/dt(AV1)=RV1
init AV1 = 0
CV1 = AV1/Vven
CV1free = CV1*(1-PB1)
CV1bound = CV1*PB1
CV1mg = CV1*MW1mg ; Unit conversion from umol/L to mg/L (ug/g)
CVtotalmg = CVmg+CV1mg ; Concentration of combined the parent drug and major metabolite

; CA1 = arterial blood/plasma concentration of the marker residue
RA1 = QC*CVLu1-QC*CA1free
d/dt(AA1) = RA1
init AA1 = 0
CA1 = AA1/Vart
CA1free = CA1*(1-PB1)
CA1bound = CA1*PB1

ABlood1 = AV1+AA1

; Marker residue in lung compartment
RALu1 = QC*(CV1-CVLu1)
d/dt(ALu1) = RALu1
init Alu1 = 0
CLu1 = ALu1/VLu
CVLu1 = CLu1/PLu1

; CIP in liver compartment
RL1 = QL*(CA1free-CVL1)+Rmet1 ; Rate of CIP change in venous blood
d/dt(AL1) = RL1
init AL1 = 0
CL1 = AL1/VL ; CIP Concentration in liver
CVL1 = CL1/PL1 ; CIP concentration in liver venous
CL1mg = CL1*MW1mg
CLtotalmg = CL1mg+CLmg ; concentration of combined ENR and CIP

; CIPRO in kidney compartment
RK1 = QK*(CA1free-CVK1)-Rurine1 ; Rate of CIP change in kideny
d/dt(AK1) = RK1
init AK1 = 0
CK1 = AK1/VK ; Concentration in kidney
CVK1 = CK1/PK1; Concentration in kidney venous blood
CK1mg = CK1*MW1mg
CKtotalmg = CK1mg+CKmg ; concentration of ENR and CIP in kidney

; Urinary excretion of the marker residue
Rurine1 = Kurine1*CVK1
d/dt(Aurine1) = Rurine1
init Aurine1 = 0

; CIP in muscle compartment
RM1 = QM*(CA1free-CVM1)
d/dt(AM1) = RM1
init AM1 = 0
CM1 = AM1/VM
CVM1 = CM1/PM1
CM1mg = CM1*MW1mg
CMtotalmg = CM1mg+CMmg ; Concentration of combined the parent drug and major metabolite

; Marker residue in fat compartment
RF1=QF*(CA1free-CVF1)
d/dt(AF1) = RF1
init AF1 = 0
CF1 = AF1/VF
CVF1 = CF1/PF1

; CIPRO in intestine compartment
RIt1 = QIt*(CA1free-CVIt1)
d/dt(AIt1) = RIt1
init AIt1 = 0
CIt1 = AIt1/VIt ; CIP Concentration in intestinal
CVIt1 = CIt1/PIt1 ; CIP Concentration in intestinal venous blood
CIt1mg=CIt1*MW1mg
CIttotalmg=CIt1mg+CItmg ; concentrations of ENR and CIP in intestine

; CIPRO in rest of body compartment
RR1 = QR*(CA1free-CVR1) ; Rate of CIP change in rest of body
d/dt(AR1) = RR1
init AR1 = 0
CR1 = AR1/VR ; CIP concentration rest of the body
CVR1 = CR1/PR1 ; CIP Concentration in venous blood of the rest of body

;Mass balance
Tmass1=ABlood1+AL1+AK1+AM1+AF1+AIt1+AR1+ALu1+Aurine1
Bal1=Amet1-Tmass1

TSTOP = 1080; h


; # RUNNING MASS BALANCE
; Double click the Graph window, choose QC, QL, QK, QM, QF, Qrest, VL, and VK in the Y Axes window, click remove (one by one); choose Bal, Bal1, CVmg, CV1mg, CVtotalmg, CLtotalmg, CKtotalmg, and CMtotalmgin the Variables window, add to the Y Axes window (one by one); click Run, hide CVmg, CV1mg, CVtotalmg, CLtotalmg, CKtotalmg, and CMtotalmg, click Run, you should see the graph below


; For biliary excretion refer to Module 7.3 lecture
; Fecal/biliary? excretion of ENR (The purpose is to know the concentration of ENR and CIP in the intestinal contents)
KbC = ? ; ENR bile elimination rate constant 
Kb = KbC*BW ; ENR elemination rate

; Fecal/biliary excretion of CIP
KbC1 = ? ; CIP bile elimination rate constant 
Kb1 = KbC1*BW ; CIP elimination rate

; Bile excretion of ENR
Rbile = Kb*CVL ; rate of bile excretion of ENR
Abile = integ(Rbile, 0) ; is it the same/similar with Afeces?

; Bile excretion of CIP
Rbile1 = Kb1*CVL ; rate of bile excretion of CIP
Abile1 = integ(Rbile1, 0)






