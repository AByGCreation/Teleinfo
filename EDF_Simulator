/**
Générateur de trame teleinformation EDF

V0.2 - Amelioration possible, rendre dynamique l'increment des HC,HP,... pb de concatenation char-int
V0.1 - Script permettant de generer une trame identique à celle d'un compteur EDF (option HC)

-- AByGCreation 2015 --

**/

#define start 0x02
#define startLine 0x0A
#define SP 0x20
#define stopLine 0x0D

void setup() 
{
  /**
  initialisation de la sortie TX serie hardware de maniere à envoyer à 
  1200bauds - 7bits - parité paire - 1 bit de stop 
   **/
 Serial.begin(1200, SERIAL_7E1);

}

void loop() 
{
  generateTeleinfo();
  delay (1000);
}

/**
 Calcul de la Checksum 
 
 Pour le calcul de la checksum, il suffir d'aller voir sur la doc de la teleinfo
 ( http://norm.edf.fr/pdf/HN44S812emeeditionMars2007.pdf ) page 11
 
 " La "checksum" est calculée sur l'ensemble des caractères allant du début du champ étiquette 
   à la fin du champ donnée, caractère SP inclus. On fait tout d'abord la somme des codes ASCII 
   de tous ces caractères. 
   Pour éviter d'introduire des fonctions ASCII (00 à 1F en hexadécimal), on ne conserve que les 
   six bits de poids faible du résultat obtenu (cette opération se traduit par un ET logique 
   entre la somme précédemment calculée et 03Fh). Enfin, on ajoute 20 en hexadécimal. 
   Le résultat sera donc toujours un caractère ASCII imprimable (signe, chiffre,lettre majuscule) 
   allant de 20 à 5F en hexadécimal. "
 
 
**/

char chksum(char *buff)
{
  int i;
  unsigned int sum = 0;

    for (i=0; i<strlen(buff); i++) 
    {
      sum = sum + buff[i];
    }
    sum = (sum & 0x3F) + 0x20;
    
    return(sum);
}



void generateTeleinfo()
{

/**
Exemple de Trame à envoyé

ADCO 524563565245 /
OPTARIF HC.. <
ISOUSC 20 8
HCHC 001065963 _
HCHP 001521211 '
PTEC HC.. S
IINST 001 I
IMAX 008 2
PMAX 06030 3
PAPP 01250 +
HHPHC E 0
MOTDETAT 000000 B
PPOT 00 #

**/

// on place la trame à envoyer dans un tableau/pointeur    
    char* trame[] =  { "ADCO 014881982491"
                      ,"OPTARIF HC.."
                      ,"ISOUSC 60"
                      //,"BASE 040302197"
                      ,"HCHC 039264886"
                      ,"HCHP 026709387"
                      ,"PTEC HP.."
                      ,"IINST 003"
                      ,"IMAX 049"
                      ,"PMAX 09030"
                      ,"PAPP 01250"
                      ,"HHPHC A"
                      ,"MOTDETAT 000000"
                      ,"PPOT 00"                      
                     };
// on vient ici boucler sur l'ensemble des lignes du tableau

    for (int i = 0; i < (sizeof(trame)/sizeof(char*)); i++) 
    {
      construct_trame(trame[i]);
    }
}

void construct_trame(char* datas)
{
     char* buff[] = {datas};
    
     Serial.write(startLine);
     Serial.print(buff[0]);
     Serial.write(SP);
     Serial.write(chksum(buff[0]));
     Serial.write(stopLine); 
}
