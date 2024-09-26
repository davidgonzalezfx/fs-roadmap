**Contexto:**
+ app: todoist.com
+ crear una tarea -> y ver el network tab para ver la URL y el body de la peticion HTPP (es decir, ver cómo se crea internamente una tarea)
+ eliminar una tarea -> y ver en el network tab el body de la petición HTTP (es decir, ver cómo se elimina internamente una tarea)
+ despues use ese `body/json` desde afuera de la app (en mi caso visual studio, pero se puede probar en programar como Postman o Insomnia) e intente modificar ciertos valores
+ lo que me di cuenta es que tienen un parametro `uuid` que parece ser especifico para cada tarea, sin embargo si se manda cualquier valor y desps se usa el `id` de cualquier tarea, la tarea se va a eliminar en la base de datos de todoist


**Alertas:**

+ Exponer la URL directa con la que se puede interactuar en la base de datos.
  + Idealmente las apps deberian tener un proxy que implemente CORS, es decir: si la URL del backends es www.ejemplo-database.com/api/v1, la app del front deberia crear un "puente" para que en los devtools se vea otra URL ej: www.ejemplo-front.com/web
  + Y ese proxy (www.ejemplo.com/web) se debe poder usar solamente desde la pagina web, no desde sitios externos (si no estoy mal eso se llaman CORS, que es como configurar desde que sitios web o apps, se pueden usar esas URLs)
+ al crear una tarea, cada accion de `crear una tarea` tiene un `uuid` especifico, sin embargo al eliminar, el parametro `uuid` no tiene ninguna validación o funcionalidad al eliminar

<details>
  <summary>Video</summary>
  https://github.com/user-attachments/assets/24099e17-1e5d-4965-b6ce-6c22d3ad2b7b
</details>


**Impacto**

+ Creeeoo que no significa una falla de seguridad tan grave porque para hacer lo que hice igual debo usar un token (que se supone cada cierto tiempo vence), y ese token lo saqué de la misma pagina web
+ Que pueda eliminar tareas desde afuera de todoist.com tampoco es tan grave, pero evidenemente el backend tiene uno falla en que el valor del `uuid` puede ser cualquier cosa y no debería ser así
