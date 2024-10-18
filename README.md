# Compte Rendu 
## Exercice : paramètres
### Script `analyse.sh`

<pre>
  #!/bin/bash

  echo "Bonjour, vous avez rentré $# parametre ";
  echo "Le nom du script est $0";
  
  if [ $# -ge 3 ]; then 
          echo "Le 3eme parametre est $3";
  else 
          echo "Le 3eme parametre est pas fourni";
  fi
  echo "Voici la listedes parametres $@";

</pre>
## Exercice : vérification du nombre de paramètres
### script `concat.sh`

<pre>
  #!/bin/bash
  
  if [ $# -ne 2 ]; then // verifie si on a pas passsé strictement 2 parametres
          echo "Erreur; Vous devez rentrer 2 parametre";
          exit 1;
  fi // si 2 parametre : 
  CONCAT="$1$2"; // concatination 
  echo "La concatenation des deux mots est: $CONCAT";

</pre>
## Exercice : argument type et droits
### Script `test-fichier.sh`
