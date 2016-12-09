<!-- $theme: gaia -->

# M07. Desenvolupament web en entorn servidor

## UF1. Desenvolupament web en entorn servidor
Autor: Sergi Coll

---

## Introducció al Llenguatge PHP

* **PHP** és un llenguatge per generar codi HTML.
* De fet, les sigles PHP signifiquen "***PHP: Hypertext Preprocessor"*** (definició recursiva).

Els arxius PHP combinen codi HTML i PHP:

* Els scripts **PHP** són processats per servidor (pot ser l'Apache)
	* Els scripts PHP són processats per servidor (pot ser l'Apache)
	* El resultat generat (codi HTML) s'envia al client.

---

## Introducció al Llenguatge PHP

![php](https://sdz-upload.s3.amazonaws.com/prod/upload/p1ch1_JavaScript%20client%20-%20New%20Page.png)

---

## Entorn client vs entorn servidor

* Un **llenguatge és entorn client** quan l'irtèrpret que ha d'executar els scripts és accessible des del client sense fer cap petició al servidor.
	* Per exemple, Javascript. 	
* Un **llenguatge és entorn servidor** quan l'execució dels scripts s'efectua al sevidor abans d'enviar la resposta al client.
	* Per exemple, PHP.
	* El client **no** rep el codi PHP, només rep el codi HTML generat al servidor.

---

## Requisits per a l'ús del llenguatge PHP
PHP requereix tenir instal·lat:
* Un programari de servidor que suporti el protocol HTTP i configurat per interactuar amb l'intèrpret PHP.
	* Per exemple, l'Apache.
* Un intèrpret de PHP.

---

## Treballant amb Apache i Linux

En Ubuntu o Debian podem instal·lar els paquets necessaris amb:

```
$ sudo apt-get install apache2 php5 mysql-server phpmyadmin
```
---

## Apache i Windows
Es recomana instal·lar el WAMPP o XAMPP for Windows.
http://www.apachefriends.org/en/xampp-windows.html

* Aquest paquet ho porta tot integrat: Apache, PHP5 i MySQL.
* Els arxius \*.php els haureu de ficar al directori htdocs (del Apache).

---

### El primer PHP
El llenguatge php s'insereix en un pàgina HTML.

```php
<html>
<body>
   <h1>Primer arxiu PHP</h1>
   <?php
      echo "<p>Hola, Món</p>";
   ?>
</body>
</html>
```
* Per indicar que incrustem codi PHP cal **obrir amb `<?php`** finalment **tancar amb** `?>`
* La **instrucció** `echo` imprimeix text a l'arxiu que rep el client (el text va entre cometes dobles).
* Les **instruccions acaben amb punt i coma ";"** (és molt important!)

---

* El client **MAI REP CODI PHP**.
* El resultat que ha de rebre el client és TOT HTML:

```html
<html>
<body>
   <h1>Primer arxiu PHP</h1>
   <p>Hola, Món.</p>
</body>
</html>
```
---

### Comentaris

Cal distingir els comentaris en HTML dels PHP.
```php
<html>
<body>
   <h1>Primer arxiu PHP</h1>
   <!-- Això és un comentari HTML -->

   <?php
      // Això és un comentari d'una línia
      echo "<p>Hola, Món</p>";

      /* Amb barra i asterisc podem fer
         comentaris de varies línies. */
   ?>
</body>
</html>
```
---

### Imprimir valors
* Tenim dues formes de mostrar valors `echo` i `print`
* La diferència és que `echo` no retorna cap valor i `print` retorna 1.

echo

```php
echo "<h2>PHP is Fun!</h2>";
```

print

```php
print "<h2>PHP is Fun!</h2>";
```

---
<!-- *template: invert -->

# Programació bàsica
## Variables i operadors

---

### Constants

* Per definir una constant utilitzem la instrucció  `define("NOMCONSTANT",valor)`

```php
<?php
   // Definicions
   define("PI", 3.1416);

   // mostrem per pantalla
   echo "<p>El número Pi és " . PI . "</p>";  
   // enlloc de "PI" es mostrarà "3.1416"
?>
```

**IMPORTANT**: Fixeu-vos en què per encadenar text (entre cometes) i les variables utilitzem el **punt** ```.```

---

### Variables

* En PHP les variables comencen amb el símbol de `$`

* PHP distingeix entre majúscules i minúscules. Les variables `$persona` i `$Persona` són diferents.
* **No cal declarar les variables.**
* **No cal definir el seu tipus.**
	* PHP converteix automàticament la variable al tipus correcte depenet del valor assignat.

```php
$nom="Sergi";		//Variable de tipus String
$edat=35;		//Variable de tipus Integer
$pes=61.5;		//Variable de tipus Float
$casat=true;		//Variable de tipus Boolena
```

---

### Variables
* Exemple canvi de tipus:

```php
$nomPersona = "Àlex";
$edat = "35";
// la variable edat és de tipus string

echo "$nomPersona té $edat anys<br>";

$edat = 34;
//Ara la variable edat és de tipus integer:

$edat++;
echo "$nomPersona té $edat anys";
```

---

### Funcions pels tipus de variables

* **gettype(variable)**: torna el tipus de dada de la variable.
* **settype(variable,tipus)**: converteix la variable al tipus indicat.
* **isset(variable)**: indica si la variable s'ha inicialitzat.
```php
$a = "10";
echo gettype($a); 	//Mostrarà String
settype($a,"integer");  //Convertim el string a number
echo gettype ($a);	//Mostrarà integer
settype($a,"number");	//Convertim el string a number
echo isset($b);		//Mostrarà false perquè la
			//variable no s'ha inicialitzat        
```
---

### Operadors
Permeten manipular el valor de les variables, realitzar operacions matemàtiques o comparar-les.
* Operadors aritmètics: +, -, *, /, % (Mòdul)
* Operadors d'increment i decrement `++$a` `$a++` `--$a` `$a--`
* Operadors d’assignació: =, +=, -=, .=
* Operadors de cadenes: **.** `"Hola" . "Món"`
* Operadors lògics: !(NEGACIÓ), && (AND), || (OR)
* Operadors condicionals: ==, !=, >, <, >=, <=


---
<!-- *template: invert -->

# Arrays

---

### Arrays

* Els **arrays** són colleccions ordenades d’elements.
* Cada element té un **valor** i és identificat per una **clau** que és única a l’array.


---

### Declaració d'un Array

```php
//Declaració amb constructor
$colors=array("verd","groc","vermell");
$colors=array(1=>"verd", 2=>groc, 3=>"vermell");

//Declaració explícita
$color[1]="verd";
$color[2]="groc";
$color[]="vermell";	//Si no posem índex,
			//s'assigna a la següent posició.
```

---

### Tipus de dades en un array

* A les caselles dels arrays podem guardar dades de qualsevol tipus.
* Els array en PHP podem guardar diferents tipus de dades en les caselles d'un mateix array.
	*  S’anomenen **arrays heterogenis**.
```php
a[0] = 1;
a[1] = "Hola";
a[2] = 0.75;
a[3] = true;

//Declaració resumida del array
$a = array(1, "Hola", 0.75, true);
```
---

### Array associatius

* També s'anomenen diccionaris o mapes.
* Són un conjunt d'elements **clau** - **valor**

```php
$a = array("id"=>1,"name"=>"Sayeed","age"=>24);
```

![php](http://programmingsphere.com/wp-content/uploads/2015/07/associative-array-in-PHP.jpg)

**Nota**: Per mostrar els valors d'un array associatiu es pot utilitzar la funció `print_r($a)`

---

### Funcions amb Arrays

`$a=array(‘Nom1’=>'Maria’,'Nom2’=>’Joan’,…);`

* **count($a)**: ens diu quants elements té l’array
* **array_key_exists('clau', $a)**: ens diu si existeix la clau en l'array.
```php
array_key_exists('Nom1', $a);	//retorna true
```
* **in_array(‘valor', $a)**: ens diu si existeix el valor en l'array.
```php
in_array('Maria', $a);	//retorna true
```

---

### Funcions amb Arrays

* **unset($a[‘clau’])**: ens elimina l'element que té la clau indicada
* **sort($a)**: ordena els valors de menor a majors.
* **rsort($a)**: ordena els valors de major a menor.

---

### Funcions amb Arrays

* **array_push($a,'valor1','valor2')**: afegeix un valor o més al final d'un array.
```php
array_push($a,'Dani', 'Raquel');
```
* **array_pop($a')**: elimina l'últim element de l'array i retorna el seu valor.
```php
$valor = array_pop($a);
```

---

### Arrays multidimensionals

* En **PHP**, els tots arrays són de una única dimensió.
	* Podem crear arrays multidimesionals creant arrays d’arrays, com si els elements de l'array fossin al seu torn altres arrays.

---

### Declaració Arrays multidimensionals

* Podem declarar arrays de **qualsevol dimensió**.
```php
//DECLARACIÓ ARRAY DUES DIMENSIONS
$ciutat1 = [20, 22, 18];
$ciutat2 = [25, 29, 23];
$ciutat3 = [15, 19, 15];
//guardem els array en un altre array
$temperatures = [$ciutat1,$ciutat2,$ciutat3];

//DECLARACIÓ ABREUJADA
$temperatures = array(array(20, 22, 18),array(25, 29, 23),array(15, 19, 15));
```

```php
//Accés a les dades
$temperatures[0][2]			//Retorna 18
$temperatures[2][1]			//Retorna 19
```

---
<!-- *template: invert -->


## Estructures de control de flux

---

### IF, ELSE

```php
if (condició) {
    //codi a executar si la condició és true;
} else {
   //codi a executar si la condició és false;
}
```
```php
if (condició) {
    //codi a executar si la condició és true;
} elseif (condició) {
    //codi a executar si la condició és true;
} else {
    //codi a executar si la condició és false;
}
```

---

### FOR

```
for (inicialització contador; condició de final ; increment contador) {
   //codi a executar;
}
```

```php
for  ($i=1; $i<5; $i++) {
  echo "<p>Iteració $i</p>";
}
```
---

### WHILE

```
while (condició) {
    //codi a executar;
}
```

---

### DO ... WHILE

```
do {
     //codi a executar;
} while (condició);
```

---

### SWITCH

```php
switch ($variable) {
    case valor1:
        //codi a executar si $variable==valor1;
        break;
    case valor2:
        //codi a executar si $variable==valor2;
        break;
    case valor3:
        //codi a executar si $variable==valor3;
        break;
    ...
    default:
        //codi a executar si $variable és diferent
        //a tots els valors;
}
```

---

<!-- *template: invert -->


## Formularis

---

### Formularis

```html
<form method="post" action="processa.php">
    <label for="name"/>
    <input type="text" name="nom"/>
    <input type="submit" value="Enviar">
</form>
```
* **method**: indica la forma d'enviament de la informació (POST o GET).
* **action**: indica el fitxer on s'enviaran les dades
* **name**: per cada camp del formulari indica el nom que l'identifica i que ens permetrà recuperar el valor introduït.
---

### Formularis. Processament

*  La pàgina destí rebrà un **array associatiu** (`$_REQUEST, $_POST o $_GET`) del qual el **name** del camp serà una **clau** que contidrà el **valor** entrat al formulari.
* El fitxer que recollirà la informació `processa.php` pot contenir el següent:
```php
<? php
    $nom = $_REQUEST['nom'];  //Obtenim el nom introduït
    echo "Hola, $nom!";       //Mostrem una salutació
?>
```
* Obtenim el valor entrat al formulari a partir de l'array associatiu `$_REQUEST` i indicant la clau **nom**.

---

### Formularis: POST i GET

* Quan un formulari envia dades ho pot fer mitjançant dos mètodes diferents:

	* **POST**:  els valors del formulari es transmeten de manera oculta (no aparèixen a la barra d'adreces del navegador).
	* **GET**:	els valors aperèixen a l'adreça URL.
	Per exemple:
    `http://localhost/processa.php/nom=jaume&curs=1`

---

### Formularis: GET

Característiques del mètode GET:

* Les dades són visibles en la URL.
  * No utilitzar per enviar dades sensibles (com passwords).
* Té limitació en la quantitat de dades a enviar.
  * Màxim 2000 caràcters.
* Les sol·licitud GET es guarden en l'historial del navegador.
* Les sol·licitud GET  poden ser *bookmarked*.

---

### Formularis: POST

Característiques del mètode POST:
* No hi ha límits en la quantitat d'informació a enviar.
* Les dades s'afegeixen en el cos de la petició HTTP.
* Les sol·licitud POST NO es guarden en l'historial del navegador.
* Les sol·licitud POST NO poden ser *bookmarked*.

---

<!-- *template: invert -->
## Sessions

---

### Protocol HTTP

* El  **protocol http** és un [protocol sense estat (*stateless*)](https://es.wikipedia.org/wiki/Protocolo_sin_estado).
* Es tracta cada petició com un transacció independent que no té relació amb cap altre petició anterior.
* S'han creat tècniques per mantenir informació entre peticions.
* Una d'aquestes tècniques són les [**cookies**](http://php.net/manual/es/features.cookies.php).

---

### Cookies

* Una *galeta* o ***cookie*** és un fitxer de text de mida limitada que permet guardar localment informacions diverses.
* La finalitat és guardar informació d'un visitant **en el seu ordinador** per poder utilitzar en futures visites.
  * Per exemple: data i hora última visita, nom d'usuaris, etc.
* Cal que el navegador tingui les **cookies habilitades**.

---
### Crear cookies

```php
setcookie(name, value, expire)
```
* El **temps d'expiració** s'expressa amb segons.
* Exemple:

```php
<? php
setcookie("user", "sergi", time() + (30 * 24 * 3600));
// Expiració = 30 dies (30d * 24h * 3600s)
?>
```
* **Important!!!** quan volguem enviar una cookie hem de començar el codi php just al començament del fitxer, abans de qualsevol etiqueta html o espai en blanc.

---

### Recuperar cookies
* Per recuperar els valors desats en **cookies** s'utilitzar l'array associatiu `$_COOKIE["nom_cookie"]`

```php
<?php
if(!isset($_COOKIE["user"])) {
    echo "La cookie amb nom 'user' no existeix!";
} else {
    echo "Benvingut " . $_COOKIE["user"];
}
?>
```

---
### Sessions
* PHP proporciona un altre mecanisme per mantenir la sessió : les **variables de sessió**.
* Són variables que estan disponibles a qualsevol lloc dels scripts i en totes les pàgines **mentre la sessió no s'acabi**. Un sessió es tanca quan:
  * L'usuari no tanqui el navegador.
  * Passa un cert temps (definit pel sistema) sense que l'usuari interaccioni.
* Les variables de sessió, a diferència de les cookies, **es guarden al servidor**.

---

### Creació de la sessió
pagina1.php
```html
<?php
// Start the session
session_start();
?>
<!DOCTYPE html>
<html>
<body>
<?php
// Set session variables
$_SESSION["favcolor"] = "green";
$_SESSION["favanimal"] = "cat";
echo "Session variables are set.";
?>
</body>
</html>
```

---

### Recuperació variables de sessió
pagina2.php
```html
<?php
// Restart de session
session_start();
?>
<!DOCTYPE html>
<html>
<body>
<?php
// Echo session variables that were set on previous page
echo "Favorite color is " . $_SESSION["favcolor"] . ".<br>";
echo "Favorite animal is " . $_SESSION["favanimal"] . ".";
?>
</body>
</html>
```
