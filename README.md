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
<pre>
#!/bin/bash

# Vérifier si un argument a été passé
if [ $# -eq 0 ]; then
    echo "Veuillez fournir un nom de fichier ou de répertoire."
    exit 1
fi

# Assigner le premier argument à une variable
file="$1"

# Vérifier si le fichier ou répertoire existe
if [ -e "$file" ]; then
    # Vérifier le type de fichier
    if [ -d "$file" ]; then
        echo "Le fichier $file est un répertoire"
    elif [ -f "$file" ]; then
        echo "Le fichier $file est un fichier ordinaire"
        
        # Vérifier si le fichier est vide ou non
        if [ -s "$file" ]; then
            echo "\"$file\" n'est pas vide"
        else
            echo "\"$file\" est vide"
        fi
    else
        echo "Le type de $file n'est pas reconnu"
    fi

    # Afficher les permissions d'accès pour l'utilisateur courant
    user_permissions=$(stat -c "%A" "$file")
    echo "\"$file\" est accessible par $(whoami) en $user_permissions"
else
    echo "Le fichier ou répertoire \"$file\" n'existe pas."
fi
</pre>

## Exercice : Afficher le contenu d’un répertoire
### Script `listedir.sh`
<pre>
#!/bin/bash

# Vérifier si un argument a été passé
if [ $# -eq 0 ]; then
    echo "Veuillez fournir un répertoire à lister."
    exit 1
fi
dir="$1"
#  le répertoire existe et est bien un répertoire
if [ -d "$dir" ]; then
    echo "####### fichiers dans $dir/"

    # Lister uniquement les fichiers dans le répertoire
    find "$dir" -maxdepth 1 -type f

    echo ""
    echo "####### répertoires dans $dir/"

    find "$dir" -maxdepth 1 -type d | grep -v "^$dir$"
else
    echo "Le répertoire \"$dir\" n'existe pas ou n'est pas un répertoire."
fi  
</pre>
