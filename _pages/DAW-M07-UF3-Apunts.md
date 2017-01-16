---
title: "UF3. Accés a dades"
permalink: /DAW-M07-UF3-Apunts/
date: 2017-01-16T03:02:20+00:00
---

# M07. Desenvolupament web en entorn servidor
## UF3. Accés a dades
###### Autor: Sergi Coll

---

## Tècniques d'accés a dades
Disposem de diferents llibreries per connentar a BD:

* **MYSQL extension** (obsoleta)
	* Aquesta extensió ha estat "deprecated" el 2012.

* **MySQLi extension** ("i" improved)
	* Només funciona amb bases de dades MySQL.

* **PDO o PHP Data Object**
	* Pot treballar amb 12 diferents gestors de bases de dades.
	  * MySQL, SQLite, Oracle, MS SQL Server, etc.

---

## MYSQLi CONNEXIÓ A BD

```php
<?php
$servername= "localhost";
$username = "username";
$password = "password";

// Create connection
$conn= new mysqli($servername, $username, $password);

// Check connection
if($conn->connect_error) {
	die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully";

//Close connection
$conn->close();
?>
```
---

<!-- *template: invert -->

# PDO
# PHP Data Object

---

### Introducció a PDO

* **PDO** és un extensió de PHP que ens propociona una capa d'abstracció d'accés a dades.
  * Per tant, ens permet utilitzar les mateixes funcions per realitzar consultes independentment de la base de dades que estiguem utilitant.
* **PDO** ve amb **PHP 5.1**.
* Requereix característiques de OO (Orientació Objectes) del nucli de PHP 5.
  * Per tant, no funcionarà en versions anteriors a 5.1.

---

### PDO CONNEXIÓ A BD

```php
<?php
$servername= "localhost";
$username = "username";
$password = "password";

try{
	//creem una nova connexió instancinat l'objecte PDO
	$conn = new PDO("mysql:host=$servername;dbname=myDB",
			$username, $password);
  // establim el mode PDO error a exception per poder
  // recuperar les excepccions
  $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  echo "Connected successfully";
}
catch(PDOException $e)
{
	//Si falla la connexió amb la BD es mostra l'error.
	echo "Connection failed: " . $e->getMessage();
}
//Close connection
$conn=null;
?>
```


---

## PDO SELECT
```php
<?php
// sql to select a record
$sql = 'SELECT firstname, lastname, email FROM MyGuest ORDER BY firstname';

//obtenir les files de la BD
$rows = $conn->query($sql);	// use query() because results are returned

//recorrem cadauna de les files per mostrar les dades
foreach ($rows as $row) {
	 echo $row['firstname'] . "\t";
	 echo $row['lastname'] . "\t";
	 echo $row['email'] . "\n";
}
?>
```
---

## PDO INSERT

Si realitzem un INSERT en una taula amb un camp AUTO_INCREMENT, podem obtenir el ID de lúltim registre inserit.

```php
<?php
$sql= "INSERT INTO MyGuests(firstname, lastname, email)
VALUES ('John', 'Doe', 'john@example.com')";

// use exec() because no results are returned
$conn->exec($sql);
$last_id = $conn->lastInsertId();

echo "New record created successfully. ";
echo "Last inserted ID is: " . $last_id;
?>
```

---

## PDO INSERT


```php
<?php
$firstname = $_POST['firstname'];
$lastname = $_POST['lastname'];
$email = $_POST['email'];

$sql= "INSERT INTO MyGuests(firstname, lastname, email)
VALUES ($firstname, $lastname, $email)";

// use exec() because no results are returned
$conn->exec($sql);
echo "New record created successfully. ";
?>
```

---

## SQL INJECTION

* És un tècnica que hacking que permet injectar instruccions SQL a través d'una pàgina de la nostra aplicació web.
* Si tenim, per exemple, un formulari de login:


```php
<?php
// sql to select a record
$user = $_GET["user"];
$pass = $_GET["password"];
$sql = "SELECT * FROM users WHERE username='$user' AND password='$pass'";
$result = $conn->query($sql);

if ($result){
	echo "Usuari autenticat!"
}
```
---
## Exemple SQL injection
* Un perillós hacker omple un formulari amb aquestes dades:

![Formulari login](http://i.imgur.com/MMftsdg.png)

---

### **Exemple 1 SQL Injection:**
```php
<?php
	$user = "' OR 1=1 --"
	$pass = ""
	$sql = "SELECT * FROM users WHERE username='$user' AND password='$pass'";
?>
```
El **--** en MySQL indica que és un comentari .

```sql
SELECT * FROM users WHERE username='' OR 1=1 --' AND password=''
```
---

### **Exemple 2 SQL Injection:**

```php
<?php
	$user = "sergi"
	$pass = "' OR 1=1"
	$sql = "SELECT * FROM users WHERE username='$user' AND password='$pass'";
?>
```

```sql
SELECT * FROM users WHERE username='sergi' AND password='' OR 1=1;
```

---

## **Exemple 3 SQL Injection:**

```php
<?php
	$user = "'; DROP TALBE users --"
	$pass = ""
	$sql = "SELECT * FROM users WHERE username='$user' AND password='$pass'";
?>
```

```sql
SELECT * FROM users WHERE username=''; DROP TALBE users --' AND password=''
```

---

## PDO PREPARED STATEMENTS

* Una **instrucció preparada** es tracta d'una instrucció SQL pre-compilada que pot ser executada varies vegades només enviant les dades al servidor.
* Permet evitar el *SQL injection*.

---

## Exemple INSERT amb Prepared Statements
```php
<?php
  // prepare sql statement
	$stmt = $conn->prepare("INSERT INTO MyGuests (firstname, lastname, email)
					VALUES (:firstname, :lastname, :email)");

	// execute to insert a row
	$stmt->execute(array(':firstname'=>'John',
						':lastname'=>'Doe',
						':email'=>"john@example.com"));

	// execute to insert a row
	$stmt->execute(array(':firstname'=>'Mary',
						':lastname'=>'Moe',
						':email'=>"mary@example.comm"));
?>
```
---

## Exemple SELECT amb Prepared Statements
```php
<?php
  // 1. prepare sql statement
	$sql = 'SELECT firstname, lastname, email FROM MyGuest WHERE firstname = :firstname';
	$stmt = $conn->prepare($sql);

	// 2. execute to insert a row
	$stmt->execute(array(':firstname'=>'John'));

	// 3. get all rows
	$rows = $stmt->fetchAll();

	// 4. show rows
	foreach ($rows as $row) {
		echo $row['firstname'];
		echo $row['lastname'];
	}
?>
```
El métode fetch() es pot definir per tal que retorni un array, un objecte, una instància d’una classe, etc.

`$rows = $stmt->fetch(PDO::FETCH_ASSOC);`

Únicament cal passar com a paràmetre:
* PDO::FETCH_ASSOC: Retorna un array associatiu. És el valor per defecte.
* PDO::FETCH_BOTH
* PDO::FETCH_BOUND
* PDO::FETCH_CLASS
* PDO::FETCH_OBJ
---

## PDO DELETE

```php
<?php
	// sql to delete a record
	$sql= "DELETE FROM MyGuests WHERE id=3";

	// use exec() because no results are returned
	$conn->exec($sql);
	echo "Record deleted successfully";
?>
```

---

## PDO UPDATE
```php
<?php
	$sql= "UPDATE MyGuests SET lastname='Doe' WHERE id=2";
	// Prepare statement
	$stmt= $conn->prepare($sql);
	// execute the query
	$stmt->execute();
	// echo a message to say the UPDATE succeeded
	echo $stmt->rowCount() . " records UPDATED successfully";
?>
```
---

## MÉS INFO

* **MYSQLi**
W3Schools:[http://www.w3schools.com/php/php_mysql_intro.asp](http://www.w3schools.com/php/php_mysql_intro.asp)


* **PDO**
PHP.net: [http://php.net/manual/es/book.pdo.php](http://php.net/manual/es/book.pdo.php)
