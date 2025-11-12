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



