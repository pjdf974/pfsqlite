pfsqlite
========

Simple sqlite acces

Simple interface between sqlite3 and node.js
(tested on windows 7 and xp)

The sqlite3.exe must be in directory node_modules/pfsqlite/sqlite_exe

Exemple 1:

var Sql = require("pfsqlite");

var sCr = "create table pays (idx integer primary key autoincrement,"+
		" nom text)";

var sIn = "insert into pays values (null, 'ile');"+
		"insert into pays values (null, 'rempart');"+
		"insert into pays values (null, 'piton');"+
		"insert into pays values (null, 'riviere');"+
		"insert into pays values (null, 'etang')";

var db = new Sql();

db.execute(sCr, function(error, result){
	console.log("Création de la base :", error, result);
	if(error) throw(error);
	db.executescript(sIn, function(error, result){
		console.log("Insertions dans la base :", error, result);
		if(error)throw(error);
		db.execute("select * from pays", function(error, result){
			if(error)throw(error);
			console.log(result);
		});
	});
});

Exemple 2 :

var Sql = require("pfsqlite");

var liste = "create table exemple (texte text);"+
        "insert into exemple values ('Ce script');"+
		"insert into exemple values ('ne permet qu');"+
		"insert into exemple values ('une utilisation');"+
		"insert into exemple values ('simplifiée du');"+
		"insert into exemple values ('shell sqlite3');"+
		"select * from exemple";

var db = new Sql();

db.executescript(liste, function(error, result){
	if(error)throw(error);
	console.log(result);
});


METHODS :

. execute(request, callback): executes the query sql `request`
and calls the callback function (takes two parameters:
'error' and 'result')

. ExecuteScript(liste_of_request, callback): identical to execute,
but can receive multiple requests in succession.
Attention, only the result of the last query will be returned.

At the end of each execution, the method gives the function
callback results in tabular form and, when there
more results: `EOF`

Notes:

1-pfsqlite does not support the creation of databases in memory

2-pfsqlite gives no access to command line sqlite3
