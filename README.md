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
  
</pre>
