# Enrofloxacin2
# PBPK-enrofloxacin1
## Barekely Madona codes for seven caompartments BPPK model for enrofloxacin in calves
### 7 compartments: liver, kidney, lungs, muscle, fat, intestine and the rest of the body
#### Two submodels: parent drug/enrofloxacin (seven compartments) and metabolite/ciprofloxacin (muscle and fat excluded)

METHOD RK4

STARTTIME = 0
STOPTIME = 50 # Data of plasma and fecal samples collected upto 48 hrs is available for model validation
DT = 0.001
DTOUT = 0.1

; Physiological parameters

; Tissue volumes
BW = 118 ; Body weight (kg) (median of 35 calves in FQ AMR cattle study, ISU)
VAC = 0.0104 ;    Fractional arterial blood (Lin et al., 2016 SS Table 2, update by original references); plasma?
VVC = 0.0296 ;    Fractional venous blood (Lin et al., 2016 SS Table 2, update by original references)
VbloodC = 0.0691 ; Fractional Blood volume, Is VbloodC different from VAC/VVC? IS it plasma? (Lin et al., 2020; Table 7)
VLC = 0.0287 ;     Fractional liver tissue (Lin et al., 2020; Table 7)
VKC = 0.0039 ;     Fractional kidney tissue (Lin et al., 2020; Table 7)
VFC = 0.0695 ;     Fractional fat tissue (Lin et al., 2020; Table 7)
VMC = 0.339 ;      Fractional muscle tissue (Lin et al., 2020; Table 7)
VLuC = 0.0123 ;    Fractional lung tissue in calves (Lin et al., 2020; Table 7)
VIC = 0.0239 ;     Fractional intestine volumes in calves (Lin et al., 2020; Table 7; the whole intestine or LI alone?)
VRC = 0.4536 ;     Fractional rest of body (1-VLC-VKC-VFC-VMC-VLuC-VIC-VbloodC)

; Blood flow rates
QCC = 9.09 ; cardiac output (L/h/kg) (Lin et al., 2020; Table 16)

;Fraction of blood flow to organs (unitless, percent)
;QLC = 0.30 ;  Fraction of blood flow to the liver (Lin et al., 2020; Table 24; Hepatic artery = 0.04 and Portal vein = 0.28)
QLhC = 0.04 ;  Fraction of blood flow to the liver hepatic artery (Lin et al., 2020; Table 24)
QLpC = 0.28 ;  Fraction of blood flow to the liver portal vein (from intestine, Lin et al., 2020; Table 24)
QKC = 0.10 ;   Fraction of blood flow to the kidneys (Lin et al., 2020; Table 24)
QFC = 0.08 ;   Fraction of blood flow to the fat (Lin et al., 2016 SS Table 2, search for calves)
QMC = 0.18 ;   Fraction of blood flow to the muscle (Lin et al., 2016 SS Table 2, search for calves)
QLuC = 0.46 ;  Fraction of blood flow to the lungs (Lin et al., 2020; Table 24);
QIC = 0.11 ;   Fraction of blood flow to the GI tract (Lin et al., 2020; Table 24, search for intestines alone)
QRC = 0.21 ;   Fraction of blood flow to the rest of the body (1-QLhC-QLpC-QKC-QFC-QMC-QGC)

; Mass Transer parameters (Chemical-specific parameters)
; Chemical molecular weight, PubChem
MW = 359.39 ; g/mol, enrofloxacin ; 1 mole parent compounn is metabolized to produce 1 mole of metabolite
MW1 = 331.34 ; g/mol, ciprofloxacin
MWmol= 2.78 ; umol/mg, enrofloxacin
MWmg= 0.36 ; mg/umol, enrofloxacin
MW1mol = 3.02 ; umol/mg, ciprofloxacin
MW1mg = 0.33 ; mg/umol, ciprofloxacin

; partition coefficients (PC, tissue:plasma)
; Parent drug, enrofloxacin
PL = 4.3 ;   Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK = 5.5 ;   Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF = 0.53 ;  Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM = 1.09 ;  Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu = 4.3 ;  Lung:plasma PC (Lin et al., 2016 SS Table 4)
PI = 4.3 ;   Intestine:plasma, assumed to be similar to that of liver
PR = 1.5 ;   Rest of body:plasma (Lin et al., 2016 SS Table 4) Is PR affected by the number of compartments? e.g. whether GIT is included or not?

; main metabolite, ciprofloxacin
PL1 = 4.3 ;   Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK1 = 5.5 ;   Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF1 = 0.53 ;  Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM1 = 1.09 ;  Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu1 = 4.3 ;  Lung:plasma PC (Lin et al., 2016 SS Table 4)
PI1 = 4.3 ;   Intestine:plasma , assumed to be similar to that of liver
PR1 = 1.5 ;   Rest of body:plasma  (Lin et al., 2016 SS Table 4)

KmC = 0.06 ; Hepatic metabolic rate [/(h*kg)] (Lin et al., 2016 SS Table 4)

;Kinetic constants
;Percentage of plasma protein binding (unitless) Davis et al., 2007
PB = 0.46 ;        ENR percentage of plasma protein binding (unitless)  (Lin et al., 2016 SS Table 4)
PB1 = 0.19 ;       CIP percentage of plasma protein binding (unitless) (Lin et al., 2016 SS Table 4s)

; Metabolic rate constants
Ksc = 0.06 ;       SC absorpation rate constant (/h) (Lin et al., 2016 SS Table 4)
Frac = 0.55 ;      Fraction of ENR metabolized to CIP (unitless),  (Lin et al., 2016 SS Table 4)

KurineC = 0.15 ;   ENR urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4) 
Kurine1C = 1.99 ;  CIP urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4)

; SC infusion rate constants
Timesc = 0.00 ; SC injection/infusion (h)

; IV infusion/injection rate constants
Timeiv= 0.01 ; h, IV infusion/injection time

; IM absorption rate constants
Kim = 0.7 ; /h, intramuscular absorption rate constant

; Billiary elimination rate constant??
KfecesC = 0.01 ; ENR L/h/kg (Lin et al., 2016 SS Table 4, 0.01 if for swine)
KfecesC1 = 0.01 ; CIP L/h/kg It was not considered for CIP Lin et al 2016 but for unrinary excretion both ENR and CIP were included.

; Parameters for exposure scenario
PDOSEsc = 7.5;12.5 (mg/kg) Low dose and high dose
PDOSEEiv = 0 ; mg/kg
PDOSEim = 0 ; mg/kg
PDOSEoral = 0 ; mg/kg

; Cardiac output and blood flows to tissues (L/h)
QC = QCC*BW ; Cardiac output
QL = QLC*QC ; Liver
QK = QKC*QC ; kidney
QF = QFC*QC ; Fat
QLu = QLuC*QC ; Lung
QM = QMC*QC ; Muscle
QI = QIC*QC ; Intestine
QR = QRC*QC ; Rest of body

; Tissue Volume (L)
VL = VLC*BW ; Liver
VK = VKC*BW ; Kidney
VF = VFC*BW ; Fat
VM = VMC*BW ; Muscle
VLu = VLuC*BW ; Lung
VI = VIC*BW ; Intestine
VR = VRC*BW ; Rest of the body

; Dosing amounts (mg converted to umol)
DOSEoral=PDOSEoral*BW*MWmol; umol
DOSEiv=PDOSEiv*BW*MWmol; umol
DOSEim=PDOSEim*BW*MWmol; umol
DOSEsc=PDOSEsc*BW*MWmol; umol



;Single IV dosing to the venous
IVR=DOSEiv/timeiv
RIV=IVR(1.0-step(1,timeiv)
d/dt(AIV)=RIV
init AIV=0

; Single IM exposure
Rim=Kim*Aimsite
d/dt(Aim)=Rim
init Aim=0
Rimsite=-Kim*Aimsite
d/dt(Aimsite)=Rimsite
init Aimsite=Doseim

; Single Sc exposure
Rsc = Ksc*Ascsite
d/dt(Asc) = Rsc
init Asc = 0
Rscsite=-Ksc*Ascsite
Ascsite=Dosesc

Metabolic rate
Km=KmC*BW ; h-1

; Urinary elimination rate constant
Kurine = KurineC*BW ; ENR
Kurine1 = Kurine1C*BW ; CIP

; Fecal elimination rate constant
Kfeces = KfecesC*BW ; ENR
Kfeces1 = Kfeces1C*BW ; CIP

; CV = venous blood/plasma concentration
RV=QL*CVL+QK*CVK+QM*CVM+QF*CVF+Qrest*CVrest+Riv+Rim+Rsc-QC*CV
AV'=RV
initAV = 0
CV=AV/Vven
CVfree=CV*(1-PB)
CVbound=CV*PB
CVmg=CV*MWmg; Unit conversion from umol/L to mg/L (ug/g)
; CA = arterial blood/plasma concentration
RA=QC*CVLu-QC*CAfree
AA'=RA
initAA = 0
CA=AA/Vart
CAfree=CA*(1-PB)
CAbound=CA*PB
ABlood=AV+AA
; Parent compound in lung compartment
RALu=QC*(CV-CVLu)
ALu'=RALu
initALu= 0
CLu=ALu/VLu
CVLu=CLu/PLu




;ENR in blood compartment
CVHd = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QI*CVI+RscHd)/QC ; High dose
CVLd = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QI*CVI+RscLd)/QC ; Low dose

;CIP in blood compartment
CV1Hd = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QI*CVI+RscHd)/QC ; High dose
CV1Ld = ((QL*CVL+QK*CVK+QLu*CVLu+QR*CVR+QI*CVI+RscLd)/QC ; Low dose

;ENR
RA= QC*(CV-CA) ; CV-venous blood, CA- arterial blood
d/dt(AA) = RA
init AA = 0
CA = AA/Vblood ; concetration in the artery

;CIP
RA1= QC*(CV1-CA1) ; CV-venous blood, CA- arterial blood
d/dt(AA) = RA1
init AA = 0
CA1 = AA1/Vblood ; concetration in the artery



; ENR in liver compartment
RL = QL*(CAfree-CVL) +RAO-Rmet ; Rate of ENR change in liver
d/dt(AL) = RL
init AL = 0
CL = AL/VL ; concetration in the liver (AL - amount in liver, VL - Volume of liver)
CVLmg = CL*MWmg ; concentration in liver venous blood

; Metabolism of ENR in liver compartment
Rmet=Km*CL*VL
Rmet1=Rmet*Frac
Rmet2=Rmet*(1-Frac)
Amet'=Rmet
initAmet= 0
Amet1'=Rmet1
initAmet1 = 0
Amet2'=Rmet2
initAmet2 = 0

; ENR in kidney compartment
RK=QK*(CAfree-CVK)-Rurine ; Rate of ENR change in kideny
d/dt(AK)=RK
initAK = 0
CK=AK/VK
CVK=CK/PK ; Concentration in kideny
Ckmg=Ck*MWmg ; Concentration in kideny venous blood

; Urinary excretion of parent compound
Rurine=Kurine*CVK
d/dt(Aurine)=Rurine
init Aurine= 0

; ENRO in lung compartment
RALu = QLu*(CV-CVLu) ; rate of ENR change in lung
d/dt(ALu) = RLu
init ALu = 0
CLu = ALu/VLu ; Concentration in lung
CVLu = CLu/PLu ; Concentration in lung venous blood

; ENRO in muscle compartment
RM = QM*(CAfree-CVM) ; Rate of ENR change in muscle
d/dt(AM) = RM
init AM = 0
CM = AM/VM ; Concentration in muscle
CVM = CM/PM ; Cocentration in muscle venous blood

; ENRO in fat compartment
RF = QF*(CAfree-CVF)
d/dt(AF) = RF
init AF = 0
CF = AF/VF
CVF = CF/PF ; Concentration venous blood of fat

; ENRO in intestine compartment
RI = QF*(CAfree-CVI)
d/dt(AI) = RI
init AI = 0
CI = AI/VI ; ENR concentration in intestine
CVI = CI/PI ; Concentration in intestinal venous blood

; ENRO in rest of the body
RR = QR*(CAfree-CVR)
d/dt(AR) = RR
init AR = 0
CR = AR/VR
CVR = AR/PR ; Concentration in venous blood of the rest of body

; Mass balance
Qbal=QC-QL-QK-QM-QF-QI-QR
Tmass=Ablood+AL+AK+AM+AF+AI+AR+Aurine+Amet
bal=AAO+AIV+AIM+ASC-Tmass

;.......Submodel for the marker resisue (CIP)..........
;CV1 = venous blood/plasma concentration of CIP

RV1=QL*CVL1+QK*CVK1+QM*CVM1+QF*CVF1+QI*CVI1+QR*CVR1-QC*CV1
d/dt(AV1)=RV1
init AV1 = 0
CV1=AV1/Vven
CV1free=CV1*(1-PB1)
CV1bound=CV1*PB1
CV1mg=CV1*MW1mg ; Unit conversion from umol/L to mg/L (ug/g)
CVtotalmg=CVmg+CV1mg ; Concentration of combined the parent drug and major metabolite

; CA1 = arterial blood/plasma concentration of the marker residue
RA1=QC*CVLu1-QC*CA1free
AA1'=RA1
initAA1 = 0
CA1=AA1/Vart
CA1free=CA1*(1-PB1)
CA1bound=CA1*PB1

ABlood1=AV1+AA1

; Marker residue in lung compartment
RALu1=QC*(CV1-CVLu1)
d/dt(ALu1)=RALu1
initAlu1 = 0
CLu1=ALu1/VLu
CVLu1=CLu1/PLu1

; CIProfloxacin
; CIP in liver compartment
RL1 = QL*(CA1free-CVL1)+Rmet1 ; Rate CIP change in venous blood
d/dt(AL1) = RL1
init AL1 = 0
CL1 = AL1/VL ; CIP Concentration in liver
CVL1 = CL1/PL1 ; CIP concentration in liver
CL1mg=CL1*MW1mg
CLtotalmg=CL1mg+CLmg ; concentration of combined ENR and CIP

; CIPRO in kidney compartment
RK1 = QK*(CA1free-CVK1)-Rurine1 ; Rate of CIP change in kideny
d/dt(AK1) = RK1
init AK1 = 0
CK1 = AK1/VK ; Concentration in kidney
CVK1 = CK1/PK1 ; Concentration in kidney venous blood
CKtotalmg=CK1mg+CKmg ; concentration of ENR and CIP in kidney

; Urinary excretion of the marker residue
Rurine1=Kurine1*CVK1
d/dt (Aurine1)=Rurine1
init Aurine1 = 0

; Marker residue in muscle compartment
RM1=QM*(CA1free-CVM1)
d/dt(AM1)=RM1
initAM1 = 0
CM1=AM1/VM
CVM1=CM1/PM1
CM1mg=CM1*MW1mg
CMtotalmg=CM1mg+CMmg ; Concentration of combined the parent drug and major metabolite

; Marker residue in fat compartment
RF1=QF*(CA1free-CVF1)
d/dt(AF1)=RF1
initAF1 = 0
CF1=AF1/VF
CVF1=CF1/PF1

; CIPRO in intestine compartment
RI1 = QF*(CA1free-CVI1)+Rmet1
d/dt(AI1) = RI1
init AI1 = 0
CI1 = AI1/VI ; CIP Concentration in intestinal
CVI1 = CI1/PI1 ; CIP Concentration in intestinal venous blood
CItotalmg=CI1mg+CImg ; concentrations of ENR and CIP in intestine

; CIPRO in rest of body compartment
RR1 = QR*(CA1free-CVR1) ; Rate of CIP change in rest of body
d/dt(AR1) = RR1
init AR1 = 0
CR1 = AR1/VR ; CIP concentration rest of the body
CVR1 = CR1/PR1 ; CIP Concentration in venous blood of the rest of body

;Mass balance
Tmass1=ABlood+AL1+AK1+AM1+AF1+AI1+AR1+ALu1+Aurine1
Bal1=Amet1-Tmass1





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






