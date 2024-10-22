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

## Exercice : Lister les utilisateurs
### Script `listeuser.sh`

<pre>
  awk -F: '$3 > 100 { print $1 }' /etc/passwd;
</pre>

## Exercice : Mon utilisateur existe t’il
### Script `ifuser.sh`
<pre>
  #!/bin/bash

# Vérifier si au moins un argument a été passé
if [ $# -ne 1 ]; then
    echo "Veuillez fournir un nom d'utilisateur ou un UID."
    exit 1
fi

input="$1"

# Vérifier si l'entrée est un UID (numérique)
if [[ "$input" =~ ^[0-9]+$ ]]; then
    # Vérifier si l'UID existe
    if id -u "$input" >/dev/null 2>&1; then
        echo "$input existe."
    fi
else
    # Vérifier si le login existe
    if id "$input" >/dev/null 2>&1; then
        uid=$(id -u "$input")
        echo "L'utilisateur \"$input\" existe avec l'UID : $uid"
    fi
fi

</pre>

## Exercice : Creation utilisateur
### Script `createuser.sh`
<pre>
  #!/bin/bash

# Vérifie si root
if [ "$USER" != "root" ]; then
    echo "Exécutez en tant que root."
    exit 1
fi

# Vérifie si l'utilisateur existe
user_exists() {
    id "$1" &>/dev/null
}

# Questions
read -p "Login : " login
read -p "Nom : " nom
read -p "Prénom : " prenom
read -p "UID : " uid
read -p "GID : " gid
read -p "Commentaires : " commentaires

# Vérifie si l'utilisateur existe déjà
if user_exists "$login"; then
    echo "L'utilisateur '$login' existe."
    exit 1
fi

# Vérifie si le home existe
home_dir="/home/$login"
if [ -d "$home_dir" ]; then
    echo "Home '$home_dir' existe."
    exit 1
fi

# Crée l'utilisateur
useradd -m -d "$home_dir" -u "$uid" -g "$gid" -c "$commentaires" "$login"

# Vérifie la création
if [ $? -eq 0 ]; then
    echo "Utilisateur '$login' créé."
else
    echo "Échec de la création."
fi

</pre>

## Exercice : Lecture au clavier
### Script `visualiser_fichiers.sh`

<pre>
  #!/bin/bash

# Vérifie si un répertoire a été passé en argument
if [ $# -eq 0 ]; then
    echo "Veuillez fournir un répertoire."
    exit 1
fi

# Vérifie si l'argument est un répertoire
if [ ! -d "$1" ]; then
    echo "L'argument doit être un répertoire."
    exit 1
fi

# Parcourt tous les fichiers du répertoire spécifié
for fichier in "$1"/*; do
    # Vérifie si c'est un fichier
    if [ -f "$fichier" ]; then
        # Vérifie si le fichier est de type texte
        if file "$fichier" | grep -q "texte"; then
            # Pose la question à l'utilisateur
            read -p "Voulez-vous visualiser le fichier $(basename "$fichier") ? (o/n) " reponse
            if [[ "$reponse" == "o" || "$reponse" == "O" ]]; then
                more "$fichier"  # Lance more pour afficher le fichier
            fi
        fi
    fi
done

</pre>

## Exercice : Appréciation
### Script `appreciation.sh`

<pre>
  #!/bin/bash

while true; do
    # Demande à l'utilisateur de saisir une note
    read -p "Saisissez une note (ou appuyez sur 'q' pour quitter) : " input

    # Vérifie si l'utilisateur veut quitter
    if [ "$input" == "q" ]; then
        echo "Programme terminé."
        exit 0
    fi

    # Vérifie si l'entrée est un nombre
    if ! [[ "$input" =~ ^[0-9]+(\.[0-9]+)?$ ]]; then
        echo "Veuillez saisir une note valide ou 'q' pour quitter."
        continue
    fi

    # Convertit l'entrée en nombre
    note=$(echo "$input" | awk '{printf "%.1f", $1}')

    # Affiche le message en fonction de la note
    if (( $(echo "$note >= 16" | bc -l) )); then
        echo "Très bien"
    elif (( $(echo "$note >= 14" | bc -l) )); then
        echo "Bien"
    elif (( $(echo "$note >= 12" | bc -l) )); then
        echo "Assez bien"
    elif (( $(echo "$note >= 10" | bc -l) )); then
        echo "Moyen"
    else
        echo "Insuffisant"
    fi
done

</pre>
