# Fuzzy_logic
this repository contains the implementation of fuzzy_logic in python . 


/*
Example: A tip calculation FIS (fuzzy inference system)
Calculates tip based on 'vitesse' and 'distance'

If you want to about this example (and fuzzy logic), please
read Matlab's tutorial on fuzzy logic toolbox
http://www.mathworks.com/access/helpdesk/help/pdf_doc/fuzzy/fuzzy.pdf

Pablo Cingolani
pcingola@users.sourceforge.net
*/

FUNCTION_BLOCK tipper // Block definition (there may be more than one block per file)

VAR_INPUT // Define input variables
vitesse : REAL;
distance : REAL;
END_VAR

VAR_OUTPUT // Define output variable
freinage : REAL;
END_VAR

FUZZIFY vitesse // Fuzzify input variable 'vitesse': {'TresPetit', 'Petit', 'Moyen', 'Grand', 'TresGrand'}
TERM TresPetit := (0, 1) (35, 1) (45, 0) ;
TERM Petit := (35, 0) (45, 1) (75, 1) (85, 0) ;
TERM moyen := (75, 0) (85,1) (115,1) (125,0);
TERM Grand := (115, 0) (125,1) (155,1) (165,0);
TERM TresGrand := (165, 0) (200, 1);
END_FUZZIFY

FUZZIFY distance // Fuzzify input variable 'distance': { 'TresCourte', 'Courte', 'moyen' , 'Langue' , 'TresLangue' }
TERM TresCourte := (0, 1) (15, 1) (25, 0) ;
TERM Courte := (15, 0) (25, 1) (35, 1) (45, 1) ;
TERM moyen := (35, 0) (45,1) (55,1) (65,0);
TERM Langue := (55, 0) (65,1) (75,1) (85,0);
TERM TresLangue := (75, 0) (85, 1);
END_FUZZIFY

DEFUZZIFY freinage // Defzzzify output variable 'freinage' : {'TresTresDeucement', 'TresDeucement', 'Deucement', 'Normale', Fortement', 'TresFortement', 'TresTresFortement' }
TERM TresTresDeucement := (0,1) (10,1) (18,0);
TERM TresDeucement := (10,0) (18,1) (24,1) (32,0);
TERM Deucement := (24,0) (32,1) (38,1) (46,0);
TERM Normale := (38,0) (46,1) (52,1) (60,0);
TERM Fortement := (52,0) (60,1) (66,1) (75,0);
TERM TresFortement := (66,0) (75,1) (80,1) (88,0);
TERM TresTresFortement := (80,0) (88,1) (100,1);
METHOD : COG; // Use 'Center Of Gravity' defuzzification method
DEFAULT := 0; // Default value is 0 (if no rule activates defuzzifier)
END_DEFUZZIFY

RULEBLOCK No1
AND : MIN; // Use 'min' for 'and' (also implicit use 'max' for 'or' to fulfill DeMorgan's Law)
ACT : MIN; // Use 'min' activation method
ACCU : MAX; // Use 'max' accumulation method

RULE 1 : IF vitesse IS TresPetit AND distance is TresCourte THEN freinage IS Fortement;
RULE 2 : IF vitesse IS TresPetit AND distance is Courte THEN freinage IS Normale;
RULE 3 : IF vitesse IS TresPetit AND distance IS moyen THEN freinage is Deucement;
RULE 4 : IF vitesse IS TresPetit AND distance IS Langue THEN freinage is TresDeucement;
RULE 5 : IF vitesse IS TresPetit AND distance IS TresLangue THEN freinage is TresTresDeucement;
RULE 6 : IF vitesse IS Petit AND distance IS TresCourte THEN freinage is Deucement;
RULE 7 : IF vitesse IS Petit AND distance IS Courte THEN freinage is Normale;
RULE 8 : IF vitesse IS Petit AND distance IS moyen THEN freinage is Fortement;
RULE 9 : IF vitesse IS Petit AND distance IS Langue THEN freinage is TresFortement;
RULE 10 : IF vitesse IS Petit AND distance IS TresLangue THEN freinage is TresTresFortement;
RULE 11 : IF vitesse IS moyen AND distance is TresCourte THEN freinage IS TresTresDeucement;
RULE 12 : IF vitesse IS moyen AND distance is Courte THEN freinage IS TresDeucement;
RULE 13 : IF vitesse IS moyen AND distance IS moyen THEN freinage is TresFortement;
RULE 14 : IF vitesse IS moyen AND distance IS Langue THEN freinage is Deucement;
RULE 15 : IF vitesse IS moyen AND distance IS TresLangue THEN freinage is Fortement;
RULE 16 : IF vitesse IS Grand AND distance IS TresCourte THEN freinage is Normale;
RULE 17 : IF vitesse IS Grand AND distance IS Courte THEN freinage is Fortement;
RULE 18 : IF vitesse IS Grand AND distance IS moyen THEN freinage is Fortement;
RULE 19 : IF vitesse IS Grand AND distance IS Langue THEN freinage is Fortement;
RULE 20 : IF vitesse IS Grand AND distance IS TresLangue THEN freinage is Normale;
RULE 21 : IF vitesse IS TresGrand AND distance is TresCourte THEN freinage IS TresTresDeucement;
RULE 22 : IF vitesse IS TresGrand AND distance is Courte THEN freinage IS TresDeucement;
RULE 23 : IF vitesse IS TresGrand AND distance IS moyen THEN freinage is TresFortement;
RULE 24 : IF vitesse IS TresGrand AND distance IS Langue THEN freinage is Normale;
RULE 25 : IF vitesse IS TresGrand AND distance IS TresLangue THEN freinage is Normale;
END_RULEBLOCK

END_FUNCTION_BLOCK
