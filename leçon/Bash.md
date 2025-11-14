## Introduction
- **Bash**( Bourne Again SHell): c'est le shell le plus utilisé dans les systèmes Unix
- **Zsh**: version étendu de Bash avec plus de fonctionnalités
- Un script bash doit toujours commencer par `#!/bin/bash`. Ça s'appel le **shebang**
- Avant de lancer un script, il faut d'abord le rendre exécutable avec `chmod`

## Les variables
Les variables shell sont créés en leurs assignant une valeur avec `=`

```
#!/bin/bash 
PRICE_PER_APPLE=5 
MyFirstLetters=ABC 
greeting='Hello world!' 
echo "Price per apple: $PRICE_PER_APPLE" 
echo "My first letters: $MyFirstLetters" 
echo "Greeting: $greeting"
```

`PRICE_PER_APPLE`: integer
`MyFirstLetters`: string
`greeting`: string avec des espaces 

### Référencement des variables

```
#!/bin/bash 
PRICE_PER_APPLE=5 
MyFirstLetters=ABC 
greeting='Hello world!' 

# Escaping special characters 
echo "The price of an Apple today is: \$HK $PRICE_PER_APPLE" 

# Avoiding ambiguity 
echo "The first 10 letters in the alphabet are:${MyFirstLetters}DEFGHIJ" 

# Preserving whitespace 
echo $greeting 
echo "$greeting"
```

- le symbole `$` est echapé pour s'afficher
- Les `{}` sont utilisés pour définir clairement le nom de la variables
- Les dernières montrent la différence entre l'utilisation des `"` et quand il n'y en a pas 

### Substitutions des commandes
Ça permet d'utiliser la sortie d'un commande comme variable
```
# Command substitution 
CURRENT_DATE=$(date +"%Y-%m-%d") 
echo "Today's date is: $CURRENT_DATE" 

FILES_IN_DIR=$(ls) 
echo "Files in the current directory:" 
echo "$FILES_IN_DIR" 

UPTIME=$(uptime -p) 
echo "System uptime: $UPTIME"
```
`$(date +"%Y-%m-%d")`: commande variabilisée pour afficher la date
`$(ls)`: commande variabilisée pour afficher le contenu d'un répertoire

### Opération arithmétique
On utilise la syntaxe `$((expression))` pour faire de l'arithmétique

```
#!/bin/bash 
X=10 
Y=5 
# Addition 
SUM=$((X + Y)) 
echo "Sum of $X and $Y is: $SUM" 
# Subtraction 
DIFF=$((X - Y)) 
echo "Difference between $X and $Y is: $DIFF" 
# Multiplication 
PRODUCT=$((X * Y)) 
echo "Product of $X and $Y is: $PRODUCT" 
# Division 
QUOTIENT=$((X / Y)) 
echo "Quotient of $X divided by $Y is: $QUOTIENT" 
# Modulus (remainder) 
REMAINDER=$((X % Y)) echo "Remainder of $X divided by $Y is: $REMAINDER" 
# Increment 
X=$((X + 1)) 
echo "After incrementing, X is now: $X" 
# Decrement 
Y=$((Y - 1)) 
echo "After decrementing, Y is now: $Y"
```

### variables d'environnements
Les variables d'environnement sont un type de variable accessible à tous les processus exécutés dans la session shell actuelle.

```
#!/bin/bash 
# Displaying some common environment variables 
echo "Home directory: $HOME" 
echo "Current user: $LOGNAME" 
echo "Shell being used: $SHELL" 
echo "Current PATH: $PATH" 

# Creating a new environment variable 
export MY_VARIABLE="Hello from my variable" 

# Displaying the new variable 
echo "My new variable: $MY_VARIABLE" 

# Creating a child process to demonstrate variable scope 
bash -c 'echo "MY_VARIABLE in child process: $MY_VARIABLE"' 

# Removing the environment variable 
unset MY_VARIABLE 

# Verifying the variable is unset 
echo "MY_VARIABLE after unsetting: $MY_VARIABLE"
```


## Transmission des arguments

C'est le fait de passer les arguments lors de l'exécution du script

```
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3" 

```

Lors de l'exécution on doit entrer donc : `./arguments.sh hello world example`
La sortie est:
```
Script name: ./arguments.sh
First argument: hello
Second argument: world
Third argument: example
```

Pour aller plus loin
```
#!/bin/bash

if [ $# -eq 0 ]; then
echo "No arguments provided."
elif [ $# -eq 1 ]; then
echo "One argument provided: $1"
elif [ $# -eq 2 ]; then
echo "Two arguments provided: $1 and $2"
else
echo "More than two arguments provided:"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
echo "Total number of arguments: $#"
fi
```

- `$#`: variable spéciale qui contient le nbr d'arguments passés dans le script
- `[ $# -eq 0 ]`: check si le nbr d'arguments est égal à 0
- `-eq`: equal to
- `-lt`: less than
- `-gt`: greater than

Nous allons modifier notre script pour parcourir touts les arguments passés avec `$@`

```
#!/bin/bash

echo "Total number of arguments: $#"
echo "All arguments:"
count=1
for arg in "$@"; do
echo "Argument $count: $arg"
count=$((count + 1))
done
```

`$@`: variable spécial qui contient tout les arguments passés dans le scripts.
Voici le résultat
```
labex:project/ $ ./arguments.sh apple banana cherry date
Total number of arguments: 4
All arguments:
Argument 1: apple
Argument 2: banana
Argument 3: cherry
Argument 4: date
```

## Array
Pour initialiser des tableaux vides:

```
#!/bin/bash

# Initialize empty arrays
NUMBERS=()
STRINGS=()
NAMES=()
```

Pour ajouter des elements :

```
# Add elements to NUMBERS array
NUMBERS+=(1)
NUMBERS+=(2)
NUMBERS+=(3)

# Add elements to STRINGS array
STRINGS+=("hello")
STRINGS+=("world")

# Add elements to NAMES array
NAMES+=("John")
NAMES+=("Eric")
NAMES+=("Jessica")
```

`+=`: opérateur utilisé pour ajouter un éléments à un tableau

Pour avoir le nombre des éléments dans un tableau:

```
# Get the number of elements in the NAMES array
NumberOfNames=${#NAMES[@]}
```

Accéder à un éléments d'un tableau:

```
# Access the second name in the NAMES array
second_name=${NAMES[1]}
```

Afficher les tableaux et les variables:

```
# Print the arrays and variables
echo "NUMBERS array: ${NUMBERS[@]}"
echo "STRINGS array: ${STRINGS[@]}"
echo "The number of names listed in the NAMES array: $NumberOfNames"
echo "The second name on the NAMES list is: $second_name"
```

### Exo

```
#!/bin/bash  

# Define arrays for each cargo bay's inventory
forward_bay=()
midship_bay=()
aft_bay=()

# Add elements to forward_bay array
forward_bay+=("Space Suits")
forward_bay+=("Oxygen Tanks")
forward_bay+=("Kits")

# Add elements to midship_bay array
midship_bay+=("Food Supplies")
midship_bay+=("Water Containers")
midship_bay+=("Medical Equipment")

# Add elements to aft_bay array
aft_bay+=("Spare Parts")
aft_bay+=("Fuel Cells")
aft_bay+=("Medical Equipment")

# Check if an argument is provided
if [ $# -eq 0 ]; then
echo "Please specify a cargo bay: forward, midship, or aft"
exit 1
fi
  
# Display inventory based on the argument
if [ "$1" = "forward" ]; then
echo "Forward Bay Inventory:"
echo "1. ${forward_bay[0]}"
echo "2. ${forward_bay[1]}"
echo "3. ${forward_bay[2]}"

elif [ "$1" = "midship" ]; then
echo "Midship Bay Inventory:"
echo "1. ${midship_bay[0]}"
echo "2. ${midship_bay[1]}"
echo "3. ${midship_bay[2]}"

elif [ "$1" = "aft" ]; then
echo "Aft Bay Inventory:"
echo "1. ${aft_bay[0]}"
echo "2. ${aft_bay[1]}"
echo "3. ${aft_bay[2]}"

else
echo "Invalid cargo bay. Choose forward, midship, or aft."
exit 1
fi
```

```
labex:project/ $ ./cargo_manifest.sh 
Please specify a cargo bay: forward, midship, or aft
labex:project/ $ ./cargo_manifest.sh forward
Forward Bay Inventory:
1. Space Suits
2. Oxygen Tanks
3. Kits
labex:project/ $ ./cargo_manifest.sh midship
Midship Bay Inventory:
4. Food Supplies
5. Water Containers
6. Medical Equipment
labex:project/ $ ./cargo_manifest.sh aft    
Aft Bay Inventory:
7. Spare Parts
8. Fuel Cells
9. Medical Equipment
labex:project/ $ 
```

