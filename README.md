# in-office

template html5

<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8">

  <title>The HTML5 Herald</title>
  <meta name="description" content="The HTML5 Herald">
  <meta name="author" content="SitePoint">

  <link rel="stylesheet" href="css/styles.css?v=1.0">

</head>

<body>
  <script src="js/scripts.js"></script>
</body>
</html>

1. preparare i dati vettoriali nel db
2. mappa: visualizzazione layer esterno con simbolo su edificio + visualizzazione layer interno
Stili di visualizzaione: 
i desk non disponibili sono grigi 
i desk occupati sono rossi
i desk liberi sono verdi

edificio --> internal --> world
(un edificio può avere più internals e ad ogni internal è collegato un mondo, un world può essere collegato anche ad un oggetto del world)

3. internals consultabile e navigabile
4. gestione utenti
5. tool per occupare room e desk con utenti
6. agganciare web socket
7. stanza riunioni collegata a google calendar?
8. gestione bagno occupato/libero
9. strumento per disegnare stanze  e scrivanie, dopo aver caricato la piantina  
10. sistema reattivo. il server ogni due minuti controlla se ci sono delle variazioni nei turni relativi a internals con sessioni attive. Se ci sono aggiorna i client
- una room può avere un numero massimo di users. Se il numero massimo non è settatosi fa riferimento al numero di desk disponibili ( il desk può essere disponibile o non disponibile)
https://jsfiddle.net/user2314737/Lfzrqvet/
https://leaflet.github.io/Leaflet.draw/docs/examples/full.html


Primo rilascio: punto 1,2,3 popolare il db con i dati di turnazione e gestire lo stile




----------------------------------- query nel db per ottenere geoJson
select json_build_object(
    'type', 'FeatureCollection',
    'features', json_agg(ST_AsGeoJSON(t.*)::json)
    )
from (select id, type, name, concat(name, ' ', type) as description, ST_Transform(geom::geometry, 4326) from ufficio_dump2
     ) as t(id,  type, name,  description, geom);