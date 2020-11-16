## Proyectocovid


// Esta es la estructura de la base de datos.
> db.covid.findOne()
{     
        "_id" : "001",
        "continente" : "America",
        "pais" : "EEUU",
        "habitantes" : "328000000",
        "total_contagios" : "50913451",
        "curados" : "33289404",
        "fallecidos" : "1263089",
        "status" : "Y"      
}

//La siguiente mostraremos de cuantos paises tenemos la informacion.
> db.covid.count()
39 
// 39 paises por todo el mundo 

//------------------------------------------------------------------------------------------

//La Primera consulta es sobre nuestro pais y sobre datos que mas nos importan. Una consulta totalmente filtrada.
>db.covid.findOne({"pais": "España"},
                {"total_contagios": 1, "curados": 1, "fallecidos": 1,  "_id": 0})
{"total_contagios" : 1381218, "curados" : 150376, "fallecidos" : 39345}

//Esta consulta es especifica de campo, se ejecuta el FindOne y incluyendo campos con ":1" o  ":0", mostramos solo esos resultados de la tabla.

//------------------------------------------------------------------------------------------

//Esta consulta es a nivel Europeo sobre contagios, curados y fallefimientos. Ordenados alfabeticamente.
> db.covid.find({"continente": "Europa"},
                {"pais": 1 ,"total_contagios": 1, "curados": 1, "fallecidos": 1, "_id": 0})
                .sort({pais:1})  
{ "pais" : "Alemania", "total_contagios" : 689146, "curados" : 443621, "fallecidos" : 11408 }
{ "pais" : "Belgica", "total_contagios" : 503182, "curados" : "null", "fallecidos" : 13216 }    
{ "pais" : "Chequia", "total_contagios" : 417181, "curados" : 244005, "fallecidos" : 5028 }     
{ "pais" : "Dinamarca", "total_contagios" : 55892, "curados" : 41232, "fallecidos" : 747 }      
{ "pais" : "España", "total_contagios" : 1381218, "curados" : 150376, "fallecidos" : 39345 }    
{ "pais" : "Filandia", "total_contagios" : 17887, "curados" : 12700, "fallecidos" : 326 }       
{ "pais" : "Francia", "total_contagios" : 1807479, "curados" : "null", "fallecidos" : 40987 }   
{ "pais" : "Inglaterra", "total_contagios" : 1213363, "curados" : "null", "fallecidos" : 49238 }
{ "pais" : "Italia", "total_contagios" : 960373, "curados" : 345289, "fallecidos" : 41750 }     
{ "pais" : "Noruega", "total_contagios" : 24975, "curados" : 11863, "fallecidos" : 285 }
{ "pais" : "Paises_Bajos", "total_contagios" : 414745, "curados" : "null", "fallecidos" : 8043 }
{ "pais" : "Polonia", "total_contagios" : 593592, "curados" : 230661, "fallecidos" : 8375 }
{ "pais" : "Rusia", "total_contagios" : 1817109, "curados" : "null", "fallecidos" : 31161 }
{ "pais" : "Suecia", "total_contagios" : 146461, "curados" : "null", "fallecidos" : 6022 }
{ "pais" : "Suecia", "total_contagios" : 146461, "curados" : "null", "fallecidos" : 6022 }
{ "pais" : "Turquia", "total_contagios" : 369831, "curados" : 340286, "fallecidos" : 10972 }
{ "pais" : "Ucrania", "total_contagios" : 479197, "curados" : 214657, "fallecidos" : 8756 }

//En esta consutla llamamos a todos los paises europeo haciendo un find de continentes y añadiendo los otros datos de interes y lo ordenamos con el .sort.

//------------------------------------------------------------------------------------------

//Esta consulta la hacemos para llamar a todos los paises actuamente investigando la vacuna. Lo hacemos mediantes "status" CON $regex.

> db.covid.find( { status: { $regex: /Y/ } } )
{ "_id" : "001", "continente" : "America", "pais" : "EEUU", "habitantes" : "328000000", "total_contagios" : "50913451", "curados" : "33289404", "fallecidos" : "1263089", "status" : "Y" }
{ "_id" : "004", "continente" : "Europa", "pais" : "Rusia", "habitantes" : "144000000", "total_contagios" : "1817109", "curados" : "null", "fallecidos" : "31161", "status" : "Y" }
{ "_id" : "005", "continente" : "Europa", "pais" : "Francia", "habitantes" : "67000000", "total_contagios" : "1807479", "curados" : "null", "fallecidos" : "40987", "status" : "Y" }
{ "_id" : "006", "continente" : "Europa", "pais" : "España", "habitantes" : "47000000", "total_contagios" : "1381218", "curados" : "150376", "fallecidos" : "39345", "status" : "Y" }
{ "_id" : "008", "continente" : "Europa", "pais" : "Inglaterra", "habitantes" : "67000000", "total_contagios" : "1213363", "curados" : "null", "fallecidos" : "49238", "status" : "Y" }
{ "_id" : "015", "continente" : "Europa", "pais" : "Alemania", "habitantes" : "83200000", "total_contagios" : "689146", "curados" : "443621", "fallecidos" : "11408", "status" : "Y" }
{ "_id" : "016", "continente" : "Europa", "pais" : "Polonia", "habitantes" : "38100000", "total_contagios" : "593592", "curados" : "230661", "fallecidos" : "8375", "status" : "Y" }
{ "_id" : "029", "continente" : "Asia", "pais" : "China", "habitantes" : "1393000000", "total_contagios" : "94677", "curados" : "75404", "fallecidos" : "4620", "status" : "Y" }
{ "_id" : "036", "continente" : "America", "pais" : "Canada", "habitantes" : "37590000", "total_contagios" : "268736", "curados" : "218400"," "fallecidos" : "10564", "status" : "Y" }

//En Europa Rusia, Francia, España, Inglaterra, Alemania, Polonia. America, EEUU, Canada. Asia, China.

//------------------------------------------------------------------------------------------

//Con la instruccion $or conseguimos filtrar con una u otra condicion, la cual nos traslada la informacion de los habitantes. 

> db.covid.find({$or:[{ status:"Y"},{habitantes:{$gte:100000000}}]})
{ "_id" : "001", "continente" : "America", "pais" : "EEUU", "habitantes" : "328000000", "total_contagios" : "50913451", "curados" : "33289404", "fallecidos" : "1263089", "status" : "Y" }
{ "_id" : "004", "continente" : "Europa", "pais" : "Rusia", "habitantes" : "144000000", "total_contagios" : "1817109", "curados" : "null", "fallecidos" : "31161", "status" : "Y" }
{ "_id" : "005", "continente" : "Europa", "pais" : "Francia", "habitantes" : "67000000", "total_contagios" : "1807479", "curados" : "null", "fallecidos" : "40987", "status" : "Y" }
{ "_id" : "006", "continente" : "Europa", "pais" : "España", "habitantes" : "47000000", "total_contagios" : "1381218", "curados" : "150376", "fallecidos" : "39345", "status" : "Y" }
{ "_id" : "008", "continente" : "Europa", "pais" : "Inglaterra", "habitantes" : "67000000", "total_contagios" : "1213363", "curados" : "null", "fallecidos" : "49238", "status" : "Y" }
{ "_id" : "015", "continente" : "Europa", "pais" : "Alemania", "habitantes" : "83200000", "total_contagios" : "689146", "curados" : "443621", "fallecidos" : "11408", "status" : "Y" }
{ "_id" : "016", "continente" : "Europa", "pais" : "Polonia", "habitantes" : "38100000", "total_contagios" : "593592", "curados" : "230661", "fallecidos" : "8375", "status" : "Y" }
{ "_id" : "029", "continente" : "Asia", "pais" : "China", "habitantes" : "1393000000", "total_contagios" : "94677", "curados" : "75404", "fallecidos" : "4620", "status" : "Y" }
{ "_id" : "036", "continente" : "America", "pais" : "Canada", "habitantes" : "37590000", "total_contagios" : "268736", "curados" : 218400, "fallecidos" : "10564", "status" : "Y" }

//Europa; Rusia, Francia, España, Inglaterra, Alemania, Polonia. America; EEUU, Canada. Asia; China. Estan investigando y tienen mas de 100M/Habitantes.

//------------------------------------------------------------------------------------------

//Esta consulta es un $Regex con filtro null.

"> db.covid.find( { curados: { $regex: /null/ }})  
{ "_id" : "002", "continente" : "Asia", "pais" : "India", "habitantes" : "1.353.000.000", "total_contagios" : "10.191.261", "curados" : "null", "fallecidos" : "238.116" }
{ "_id" : "004", "continente" : "Europa", "pais" : "Rusia", "habitantes" : "144.000.000", "total_contagios" : "1.817.109", "curados" : "null", "fallecidos" : "31.161" }
{ "_id" : "005", "continente" : "Europa", "pais" : "Francia", "habitantes" : "67.000.000", "total_contagios" : "1.807.479", "curados" : "null", "fallecidos" : "40.987" }
{ "_id" : "008", "continente" : "Europa", "pais" : "Inglaterra", "habitantes" : "67.000.000", "total_contagios" : "1.213.363", "curados" : "null", "fallecidos" : "49.238" }
{ "_id" : "018", "continente" : "Europa", "pais" : "Belgica", "habitantes" : "11.430.000", "total_contagios" : "503.182", "curados" : "null", "fallecidos" : "13.216" } 
{ "_id" : "024", "continente" : "Europa", "pais" : "Paises_Bajos", "habitantes" : "17.280.000", "total_contagios" : "414.745", "curados" : "null", "fallecidos" : "8.043" }
{ "_id" : "033", "continente" : "Europa", "pais" : "Suecia", "habitantes" : "10.230.000", "total_contagios" : "146.461", "curados" : "null", "fallecidos" : "6.022" }"   

//Esta consulta nos proporciona la informacion de los paises que no nos han brindados los datos de los curados que han sido, India, Rusia, Francia, Ingraterra, Belgica, Paises bajos, Suecia.

//------------------------------------------------------------------------------------------

//Esta consulta es mas larga y incluimos varias condiciones. Incluimos el $and despues el $or. los paises con mayores contagios de 5 millones o con menos  100 millones de habitantes y que no tenga ningun null en curados.

> db.covid.find({$and:[  {$or: [{total_contagios:{$gt : 5000000}},
                        {habitantes:{$lte: 100000000}}, 
                        {curados:{$not: /null/}}]}]})
{ "_id" : "001", "continente" : "America", "pais" : "EEUU", "habitantes" : "328000000", "total_contagios" : "50913451", "curados" : "33289404", "fallecidos" : "1263089", "status" : "Y" }
{ "_id" : "003", "continente" : "America", "pais" : "Brasil", "habitantes" : "210000000", "total_contagios" : "5675766", "curados" : "7959406", "fallecidos" : "127059", "status" : "N" }
{ "_id" : "006", "continente" : "Europa", "pais" : "España", "habitantes" : "47000000", "total_contagios" : "1381218", "curados" : "150376", "fallecidos" : "39345", "status" : "Y" }
{ "_id" : "007", "continente" : "America", "pais" : "Argentina", "habitantes" : "45000000", "total_contagios" : "1250486", "curados" : "1073564", "fallecidos" : "33907", "status" : "N" }
{ "_id" : "009", "continente" : "America", "pais" : "Colombia", "habitantes" : "50000000", "total_contagios" : "1149063", "curados" : "1047017", "fallecidos" : "32974", "status" : "N" }
{ "_id" : "010", "continente" : "America", "pais" : "Mexico", "habitantes" : "130000000", "total_contagios" : "967825", "curados" : "824355", "fallecidos" : "95027", "status" : "N" }
{ "_id" : "011", "continente" : "Europa", "pais" : "Italia", "habitantes" : "60000000", "total_contagios" : "960373", "curados" : "345289", "fallecidos" : "41750", "status" : "N" }
{ "_id" : "012", "continente" : "America", "pais" : "Peru", "habitantes" : "32000000", "total_contagios" : "923527", "curados" : "848346", "fallecidos" : "34943", "status" : "N" }
{ "_id" : "013", "continente" : "Africa", "pais" : "Sudafrica", "habitantes" : "58000000", "total_contagios" : "738525", "curados" : "680726", "fallecidos" : "19845", "status" : "N" }
{ "_id" : "014", "continente" : "Asia", "pais" : "Iran", "habitantes" : "81000000", "total_contagios" : "692949", "curados" : "525641", "fallecidos" : "38749", "status" : "N" }
{ "_id" : "015", "continente" : "Europa", "pais" : "Alemania", "habitantes" : "83200000", "total_contagios" : "689146", "curados" : "443621", "fallecidos" : "11408", "status" : "Y" }
{ "_id" : "016", "continente" : "Europa", "pais" : "Polonia", "habitantes" : "38100000", "total_contagios" : "593592", "curados" : "230661", "fallecidos" : "8375", "status" : "Y" }
{ "_id" : "017", "continente" : "America", "pais" : "Chile", "habitantes" : "18730000", "total_contagios" : "522879", "curados" : "498904", "fallecidos" : "14588", "status" : "N" }
{ "_id" : "019", "continente" : "Asia", "pais" : "Irak", "habitantes" : "38430000", "total_contagios" : "501733", "curados" : "432233", "fallecidos" : "11380", "status" : "N" }
{ "_id" : "020", "continente" : "Europa", "pais" : "Ucrania", "habitantes" : "41980000", "total_contagios" : "479197", "curados" : "214657", "fallecidos" : "8756", "status" : "N" }
{ "_id" : "021", "continente" : "Asia", "pais" : "Indonesia", "habitantes" : "267700000", "total_contagios" : "444348", "curados" : "339768", "fallecidos" : "6092", "status" : "N" }
{ "_id" : "022", "continente" : "Asia", "pais" : "Bangladesh", "habitantes" : "161400000", "total_contagios" : "421921", "curados" : "339768", "fallecidos" : "6092", "status" : "N" }
{ "_id" : "023", "continente" : "Europa", "pais" : "Chequia", "habitantes" : "10600000", "total_contagios" : "417181", "curados" : "244005", "fallecidos" : "5028", "status" : "N" }
{ "_id" : "025", "continente" : "Asia", "pais" : "Filipinas", "habitantes" : "106700000", "total_contagios" : "399749", "curados" : "361919", "fallecidos" : "7661", "status" : "N" }
{ "_id" : "026", "continente" : "Europa", "pais" : "Turquia", "habitantes" : "82000000", "total_contagios" : "369831", "curados" : "340286", "fallecidos" : "10972", "status" : "N" }

//Los paises con mayores contagios de 5 millones o con menos  100 millones d habitantes, que no tenga ningun null en curados.(Que nos ofeazcan los datos completos)"

//------------------------------------------------------------------------------------------

//Esta consulta es para filtrar con la negacion $nin.

> db.covid.find( { "continente": { $nin: [ "America", "Europa" ]}})
{ "_id" : "002", "continente" : "Asia", "pais" : "India", "habitantes" : "1353000000", "total_contagios" : "10191261", "curados" : "null", "fallecidos" : "238116", "status" : "N" }
{ "_id" : "013", "continente" : "Africa", "pais" : "Sudafrica", "habitantes" : "58000000", "total_contagios" : "738525", "curados" : "680726", "fallecidos" : "19845", "status" : "N" }
{ "_id" : "014", "continente" : "Asia", "pais" : "Iran", "habitantes" : "81000000", "total_contagios" : "692949", "curados" : "525641", "fallecidos" : "38749", "status" : "N" }
{ "_id" : "019", "continente" : "Asia", "pais" : "Irak", "habitantes" : "38430000", "total_contagios" : "501733", "curados" : "432233", "fallecidos" : "11380", "status" : "N" }
{ "_id" : "021", "continente" : "Asia", "pais" : "Indonesia", "habitantes" : "267700000", "total_contagios" : "444348", "curados" : "339768", "fallecidos" : "6092", "status" : "N" }
{ "_id" : "022", "continente" : "Asia", "pais" : "Bangladesh", "habitantes" : "161400000", "total_contagios" : "421921", "curados" : "339768", "fallecidos" : "6092", "status" : "N" }
{ "_id" : "025", "continente" : "Asia", "pais" : "Filipinas", "habitantes" : "106700000", "total_contagios" : "399749", "curados" : "361919", "fallecidos" : "7661", "status" : "N" }
{ "_id" : "027", "continente" : "Asia", "pais" : "Arabia_Saudi", "habitantes" : "33700000", "total_contagios" : "350984", "curados" : "337788", "fallecidos" : "5559", "status" : "N" }
{ "_id" : "028", "continente" : "Asia", "pais" : "Pakistan", "habitantes" : "212200000", "total_contagios" : "346476", "curados" : "319413", "fallecidos" : "7000", "status" : "N" }
{ "_id" : "029", "continente" : "Asia", "pais" : "China", "habitantes" : "1393000000", "total_contagios" : "94677", "curados" : "75404", "fallecidos" : "4620", "status" : "Y" }
{ "_id" : "030", "continente" : "Asia", "pais" : "Japon", "habitantes" : "126500000", "total_contagios" : "109000", "curados" : "96684", "fallecidos" : "1834", "status" : "N" }
{ "_id" : "031", "continente" : "Asia", "pais" : "Corea_Sur", "habitantes" : "51640000", "total_contagios" : "27653", "curados" : "25160", "fallecidos" : "485", "status" : "N" }
{ "_id" : "032", "continente" : "Asia", "pais" : "Marruecos", "habitantes" : "36600000", "total_contagios" : "260000", "curados" : "213000", "fallecidos" : "4356", "status" : "N" }
{ "_id" : "037", "continente" : "Africa", "pais" : "Libia", "habitantes" : "10230000", "total_contagios" : "69040", "curados" : "40780", "fallecidos" : "944", "status" : "N" }
{ "_id" : "039", "continente" : "Africa", "pais" : "Egipto", "habitantes" : "98420000", "total_contagios" : "109422", "curados" : "100439", "fallecidos" : "6380", "status" : "N" }

//Filtro para eliminar visualemtne los continentes Europeo y Americano que es la mayor parte de la base de datos, y asi poder observar mejor dicho paises.

//------------------------------------------------------------------------------------------

//Esta consulta es la misma que la anterior pero incluyendole una doble negacion de total contagio inferior a 900.000

> db.covid.find({$and:[{"continente": { $nin: [ "America", "Europa" ]}},
                {total_contagios:{$not:{$gt:900000}}}]})
"{ "_id" : "013", "continente" : "Africa", "pais" : "Sudafrica", "habitantes" : "58000000", "total_contagios" : "738525", "curados" : "680726", "fallecidos" : "19845", "status" : "N" }
{ "_id" : "014", "continente" : "Asia", "pais" : "Iran", "habitantes" : "81000000", "total_contagios" : "692949", "curados" : "525641", "fallecidos" : "38749", "status" : "N" }
{ "_id" : "019", "continente" : "Asia", "pais" : "Irak", "habitantes" : "38430000", "total_contagios" : "501733", "curados" : "432233", "fallecidos" : "11380", "status" : "N" }
{ "_id" : "021", "continente" : "Asia", "pais" : "Indonesia", "habitantes" : "267700000", "total_contagios" : "444348", "curados" : "339768", "fallecidos" : "6092", "status" : "N" }
{ "_id" : "022", "continente" : "Asia", "pais" : "Bangladesh", "habitantes" : "161400000", "total_contagios" : "421921", "curados" : "339768", "fallecidos" : "6092", "status" : "N" }
{ "_id" : "025", "continente" : "Asia", "pais" : "Filipinas", "habitantes" : "106700000", "total_contagios" : "399749", "curados" : "361919", "fallecidos" : "7661", "status" : "N" }
{ "_id" : "027", "continente" : "Asia", "pais" : "Arabia_Saudi", "habitantes" : "33700000", "total_contagios" : "350984", "curados" : "337788", "fallecidos" : "5559", "status" : "N" }
{ "_id" : "028", "continente" : "Asia", "pais" : "Pakistan", "habitantes" : "212200000", "total_contagios" : "346476", "curados" : "319413", "fallecidos" : "7000", "status" : "N" }
{ "_id" : "029", "continente" : "Asia", "pais" : "China", "habitantes" : "1393000000", "total_contagios" : "94677", "curados" : "75404", "fallecidos" : "4620", "status" : "Y" }
{ "_id" : "030", "continente" : "Asia", "pais" : "Japon", "habitantes" : "126500000", "total_contagios" : "109000", "curados" : "96684", "fallecidos" : "1834", "status" : "N" }
{ "_id" : "031", "continente" : "Asia", "pais" : "Corea_Sur", "habitantes" : "51640000", "total_contagios" : "27653", "curados" : "25160", "fallecidos" : "485", "status" : "N" }
{ "_id" : "032", "continente" : "Asia", "pais" : "Marruecos", "habitantes" : "36600000", "total_contagios" : "260000", "curados" : "213000", "fallecidos" : "4356", "status" : "N" }
{ "_id" : "037", "continente" : "Africa", "pais" : "Libia", "habitantes" : "10230000", "total_contagios" : "69040", "curados" : "40780", "fallecidos" : "944", "status" : "N" }
{ "_id" : "039", "continente" : "Africa", "pais" : "Egipto", "habitantes" : "98420000", "total_contagios" : "109422", "curados" : "100439", "fallecidos" : "6380", "status" : "N" }

//Con estaconsulta podemos ver como en el continente Asiatico y Africano, cuales son los paises con menos de 900.000 contagios 

//------------------------------------------------------------------------------------------

//Esta consulta es con dos $and y filtro de negacion y de menor e igual que.

db.covid.find({$and: [{"continente":{ $nin: ["Asia","Africa"]}}, 
                {$and: [{total_contagios:{$not: {$lte: "500.0000" }}}]}]})
{ "_id" : "010", "continente" : "America", "pais" : "Mexico", "habitantes" : "130.000.000", "total_contagios" : "967.825", "curados" : "824.355", "fallecidos" : "95.027" }
{ "_id" : "011", "continente" : "Europa", "pais" : "Italia", "habitantes" : "60.000.000", "total_contagios" : "960.373", "curados" : "345.289", "fallecidos" : "41.750" }
{ "_id" : "012", "continente" : "America", "pais" : "Peru", "habitantes" : "32.000.000", "total_contagios" : "923.527", "curados" : "848.346", "fallecidos" : "34.943" }
{ "_id" : "015", "continente" : "Europa", "pais" : "Alemania", "habitantes" : "83.200.000", "total_contagios" : "689.146", "curados" : "443.621", "fallecidos" : "11.408" }
{ "_id" : "016", "continente" : "Europa", "pais" : "Polonia", "habitantes" : "38.100.000", "total_contagios" : "593.592", "curados" : "230.661", "fallecidos" : "8.375" }
{ "_id" : "017", "continente" : "America", "pais" : "Chile", "habitantes" : "18.730.000", "total_contagios" : "522.879", "curados" : "498.904", "fallecidos" : "14.588" }
{ "_id" : "018", "continente" : "Europa", "pais" : "Belgica", "habitantes" : "11.430.000", "total_contagios" : "503.182", "curados" : "null", "fallecidos" : "13.216"} 

//Hemos eliminado en la busqueda Asia y America, y fitrando los continentes que nos queda y eliminando los paises con menos de 500.000 contagios.

//------------------------------------------------------------------------------------------

//Esta consulta es sobre un continente, y pidiendole solo 3 resultados.

> db.covid.find({"continente":"Africa"},
                {"pais": 1 ,"total_contagios": 1, "curados": 1, "fallecidos": 1, "_id": 0})
                .limit(3)
{ "pais" : "Sudafrica", "total_contagios" : 738525, "curados" : 680726, "fallecidos" : 19845 }
{ "pais" : "Libia", "total_contagios" : 69040, "curados" : 40780, "fallecidos" : 944 }
{ "pais" : "Egipto", "total_contagios" : 109422, "curados" : 100439, "fallecidos" : 6380 }

//Es consulta te de vuelve del continente africano los datos de contagios, curados y fallecido 3 paises y sin la ID. 

//------------------------------------------------------------------------------------------
