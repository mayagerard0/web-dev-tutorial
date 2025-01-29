# Web Dev 

## El internet

El internet es una red de computadoras conectadas entre sí que hablan un protocolo común. Pueden estar conectadas mediante cables, (incluyendo enormes cables submarinos que conectan Europa y Asia con América) antenas satelitales, wifi, etc.

El protocolo común que hablan las computadoras conectadas al internet se conoce como IP (`internet protocol`, no confundir con las direcciones ip).

Cada computadora conectada al internet tiene asociada una dirección ip pública. Hay dos tipos de direcciones de ip, conocidas como ipv4 e ipv6. La mayoría de las computadoras actuales utilizan direcciones ipv4, aunque direcciones ipv6 son cada vez más comunes. Una dirección de tipo ipv4 es secuencia de cuatro [bytes](##Glosario:Byte), típicamente representada como cuatro números entre 0 y 255 separados por puntos. (Ej: `142.250.65.142` es la dirección ip de Google).

El objetivo de estas direcciones es identificar de forma única las computadoras participantes en la red y permitirles intercambiar mensajes utilizando el protocolo de internet.

Ninguna computadora particular está conectada físicamente a todas las demás. Esto sería físicamente impráctico y económicamente restrictivo. En cambio, cada computadora está conectada a una cantidad pequeña de compañeros inmediatos. Para enviar un mensaje a un destino lejano, el mensaje es segmentado en paquetes y cada uno de estos es enviado dando saltos entre nodos adyacentes de la red. Existen computadoras especiales en el internet, llamadas routers, cuyo objetivo es retransmitir paquetes y ayudarlos a llegar a su destino apropiado.


```

          Origen
        ┌──────────┐
        │123.3.5.2 │
        └──────┬───┘      Destino
               │         ┌───────────────┐
               ▼         │142.250.65.142 │
             ┌──────┐    └────┬──────────┘
     ┌───────┤      │         │
     │       │      │         │
     │       └──────┘         │
     ▼         Router         │
   ┌─────┐                 ┌──┴──┐
   │     │                 │     │
   │     │ ──────────────► │     │
   └─────┘                 └─────┘
   Router                  Router

```

El protocolo de internet, por diseño, no garantiza que todos los paquetes enviados serán entregados, ni que paquetes consecutivos llegarán en el orden en el que fueron enviados. Por ello, existen protocolos de comunicación que operan 'sobre' o 'por encima' de ip y proporcionan garantías adicionales necesarias para las operaciones usuales de los usuarios de internet.

### Una analogía sobre protocolos encimados

Imaginemos que [Alice](##Glosario) quiere enviar cartas a Bob, quien vive a unas cuadras de distancia. Alice envía cartas a través del servicio de correos, empleando un protocolo estándar para todos usuarios del servicio (Colocar la carta en un sobre, pegarle una estampilla, escribir el destinatario claramente en la parte frontal del sobre, etc).

Desafortunadamente, el servicio de correos local es intermitente y puede perder cartas o entregarlas en un orden incorrecto. Esto es particularmente preocupante para Alice, quien le está escribiendo una novela epistolar a Bob y requiere de un orden preciso y ninguna carta faltante.

Para solucionar el problema, Alice y Bob convienen un protocolo interno entre ellos. Cada carta de Alice llevará un número secuencial claramente escrito en la esquina superior derecha; esto permite identificar cartas fuera de orden. 
Además, cada vez que Bob reciba una entrega, responderá con una carta propia con con formato: "Recibí carta #X en buen estado". Alice esperará 5 días después de un envío la confirmación de Bob, y si no la obtiene, asumirá que la carta más reciente ha sido perdidad y reenviará una copia. 

El protocolo que Alice y Bob desarrollaron opera 'por encima' del servicio de correos, es decir, utiliza al servicio de correos como canal de comunicación, pero proporciona garantías adicionales. 

De forma similar, por encima del protocolo de internet existen protocolos (llamados usualmente protocolos de transporte) como por ejemplo TCP e UDP, que proporcionan mecanismos adicionales de seguridad e integridad de la información.

Es interesante mencionar que el protocolo de Bob y Alice es relativamente independente del servicio de correos como medio de tranporte. Podrían reemplazarlo por palomas, mensajeros de relevo o señales de humo y operaría de forma similar con garantías similares (aunque sería más lento y menos eficiente). 

Este tipo de 'encimado de protocolos' es característico del internet moderno. El internet es conceptualizado frecuentemente por libros de texto como un modelo de 7 capas, donde el ip es la capa 3 y TCP ó UDP abarcan ambos las capas 4 y 5. 

El mismo protocolo de internet se encuentra por encima de protocolos de capa 2 (conocida como `data-link layer`) como Ethernet 


### TCP y HTTP 

TCP (`Transport Control Protocol`), a diferencia de ip, es un protocolo de sesión. Esto quiere decir que antes de enviar cualquier mensage, es necesario establecer una sesión entre los interlocutores. La sesión se establece mediante un proceso conocido como `tcp handshake` (Usualmente el sistema operativo se encarga de efectuar este proceso).

![Tcp handshake](https://upload.wikimedia.org/wikipedia/commons/9/98/Tcp-handshake.svg)

Para establecer una sesión usando el protocolo TCP, se requiere una dirección ip pública (pues TCP opera sobre IP) y un número de puerto. Este número de puerto es un entero de 16 [Bits](##Glosario) (entre 0 y 65535). Existen números de puertos asociados comunmente a ciertos servicios.

| Puerto | Servicio    |
|--------|-------------|
|     22 | SSH         |
|     23 | TELNET      |
|     25 | SMTP (Mail) |
|     80 | HTTP        |
|    443 | HTTPS       |

HTTP (`Hypertext Transfer Protocol`) es un protocolo de capa 7 que opera por encima de TCP. Es el protocolo usado comunmente por navegadores y servidores para transferir datos de páginas de internet.

A diferencia de los protocolos anteriores, HTTP es `plain text`, es decir, utiliza caracteres ASCII estándar. Esta particularidad lo vuelve especialmente susceptible a ataques [Man in the Middle](##Glosario) y por ello se inventó el protocolo HTTPS, una versión segura y encriptada de HTTP.

HTTP funciona con un modelo Petición - Respuesta. El cliente efectúa una petición para obtener (`GET`), modificar (`PUT`), añadir (`POST`) o eliminar `(DELETE)` recursos en el servidor. El servidor a su vez responde con un código numérico estándar y los datos pedidos (si aplica y el cliente tiene los permisos necesarios).

Por ejemplo, la petición HTTP que un navegador efectúa cuando se introduce `www.google.com` en la barra de búsqueda es:

```
          recurso
             │
      verbo  │  version del protocolo
         │   │     │
         ▼   ▼     ▼
         GET / HTTP/1.1     ┌─tipo de respuesta esperada
         Accept: */*   ◄────┘            (* significa cualquiera)
         Accept-Encoding: gzip, deflate
         Connection: keep-alive
         Host: www.google.com
         User-Agent: chrome
                        ▲
                        │
                    tipo de cliente

```

El navegador envía esta petición a la dirección ip de `www.google.com` y el puerto 80, y espera la respuesta. Una respuesta típica es:

```
               ┌────── Estatus
               ▼
   HTTP/1.1 200 OK
   Cache-Control: private, max-age=0    Tipo de respuesta
   Content-Encoding: gzip                  │  y longitud
   Content-Length: 6211    ◄───────────────┘
   Content-Type: text/html; charset=ISO-8859-1
   Date: Mon, 11 Jul 2022 02:15:28 GMT  ◄─────── Fecha y hora

           . . . otros headers . . .

   ┌──────────────────────────────────────────┐
   │  <html>                                  │  ◄────────────┐
   │   <head> <title> Google </title>         │               │
   │                                          │      Contenido de la pagina web
   │        . . . . . . . . . . . . . .       │         (usualmente en html)
   │                                          │
   └──────────────────────────────────────────┘
```

El servidor HTTP está en completa libertad de decidir cómo interpretar una petición y de qué responder. Esta flexibilidad es la que permite que HTTP sea empleado para transferir información en casi todos los sitios web. Por ejemplo, `Youtube` internamente envía una petición HTTP con verbo `POST` cada vez que un usuario preciona `Like` al url `https://www.youtube.com/youtubei/v1/like`. Sus servidores están programados para interpretar estas peticiones apropiadamente y modificar sus bases de datos internas para guardar el `Like`.


HTTP tiene [varios](https://http.cat) códigos posibles de status para una respuesta: 

| Rango     | Significado        |
|-----------|--------------------|
| 100 - 199 | Información        |
| 200 - 299 | Éxito              |
| 300 - 399 | Redirección        |
| 400 - 499 | Error del cliente  |
| 500 - 599 | Error del servidor |

### UDP

UDP (`User Datagram Protocol`) es el otro Protocolo de Transporte sobre IP más común. A diferencia de TCP, UDP no garantiza entregar todos los sus paquetes (llamados datagramas), pero permite transmitir datos a mayor velocidad y con menor [latencia](##Glosario). Es común en aplicaciones que requieren transmitir una gran cantidad de datos pero pueden aceptar pérdidas menores de información, como streaming de videos o videojuegos. 

### DNS

Para efectuar una petición IP (y consecuentemente UDP, TCP ó HTTP) se requiere conocer la dirección ip del destino, pero en ocasiones el cliente sólo conoce el nombre del dominio (por ejemplo `google.com` ó `wikipedia.org`) del servidor. Se requiere entonces de un servicio adicional para asociar dominios a direcciones ip. DNS (`domain name service`) es un protocolo (que opera sobre UDP) diseñado para esta función.

El navegador antes de comunicarse con `google.com` emite una petición DNS para averiguar su dirección ip. Cuando se compra un dominio, lo que se compra realmente es el registro de una asociación dominio - dirección ip en el sistema DNS.

### Otros Recursos 
* [Modelo OSI de capas del internet](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/)
* [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)

## Los lenguajes de la web
### HTML
HTML (`Hyper Text Markup Language`) es un lenguaje diseñado para especificar la estructura y contenido de una página web. Es usualmente enviado en respuestas de servidores HTTP. Los navegadores leen y procesan[^1] páginas el HTML y despliegan el documento especificado (sujeto a modificaciones de estilo por hojas de CSS). 
Un documento de HTML está compuesto por elementos anidados de la forma:

[^1]: Los navegadores modernos leen el HTML de forma secuencial y producen el documento gráfico (y el DOM) de forma gradual. Esto permite mostrar contenido inmediatamente cuando el HTML es de gran tamaño. Además, son generalmente flexibles en cuanto a la interpretación del HTML y tratan de corregir errores conforme avanzan.

```
<tag attributo="valor"> 
    .... ccontenido ....
</tag>
```

El HTML puede contener elementos que no se despliegan visualmente, pero transmiten información al navegador sobre, por ejemplo, el idioma del documento, el título de la página y el ícono a emplear en bookmarks. 

Ejemplo de una página completa pequeña:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title> Mi página </title>
  </head>
  <body>
  <h1> Hello world! </h1> 
  </body>
</html>
```


### CSS
CSS (`Cascading Style Sheets`) es un lenguaje que acompaña al HTML y modifica el estilo visual del documento. Se utiliza para decorar y cambiar la organización visual de los elementos de HTML.

El navegador interpreta tags de HTML `<link rel="stylesheet" href= URL >` como una instrucción para realizar una petición HTTP `GET` al URL, que debe responder con un documento de CSS para aplicar al documento.

Las hojas de CSS están compuestas por 'reglas' de formato. Una regla especifica un objectivo a quien aplica la regla mediante un `selector` y una serie de directivas para darle formato. Por ejemplo:

```
h1 {
  color: blue;
  font-size: 20px;
} 

```

indica que a todos los elementos con tag `<h1>` se les debe colorear de azul y aplicar un tamaño de letra de 20 pixeles.
Los selectores pueden ser tags como `h1`, clases de elementos como `.clickable-button`, elementos particulares con cierto `id` como: `#my-button`, entre otros.

Es frecuente que más de una regla aplique a un elemento dado. En ese caso, existe un algoritmo (el algoritmo de 'cascada') para determinar el aspecto final del elemento, de acuerdo a las especificidades de los selectores.

Además de especificar hojas enteras de CSS a aplicar mediante `<link>`, es posible especificar estilos particulares para un elemento usando el atributo `style`:

```
<div style="background-color: blue; color:white" > Hello world! </div> 
 
```


### Javascript
Javascript es un lenguaje de programación general (aunque no siempre lo fue[^2]) que es interpretado por los navegadores para crear elementos interactivos. Javascript permite crear aplicaciones complejas en páginas web, como hojas de cálculo, videojuegos y editores de texto.

[^2]: Javascript fue diseñado para proveer interactividad a las páginas web. El nombre está inspirado por Java, que al momento de su creación era el lenguaje de programación más popular entre los desarrolladores, pero no existe ninguna relación entre los lenguajes (además de una sintaxis similar, inspirada en ambos casos por el lenguaje C). La especificación inicial de Javascript fue creada en una semana por Brendan Eich en 1995 para el navegador Netscape. Desde entonces, ha cobrado importancia y complejidad, y es considerado por muchos el lenguaje de programación más usado en la actualidad.

Uno de los componentes principales de un navegador web es su `motor de javascript`, que se encarga de [interpretar](##Glosario) las instrucciones de Javascript.
Los motores de javascript son piezas complejas de software continuamente mejoradas para garantizar un buen rendimiento[^3].

[^3]: Los motores de Javascript modernos utilizan intérpretes JIT (Just in time). Durante la ejecución, toman notas sobre los segmentos de códigos más ejecutados. Cuando una sección pasa cierto umbral, el intérprete se detiene, compila el código a instrucciones de máquina y en ejecuciones posteriores emplea estas instrucciones compiladas altamente optimizadas para ahorrar tiempo.

El navegador interpreta tags de HTML `<script src= URL > </src>` como una instrucción para realizar una petición HTTP `GET` al URL, que debe responder con un documento de javascript para [interpretarlo](##Glosario) inmediatamente.

#### El DOM 

Cuando el navegador recibe el HTML de una página, mientras construye el documento para desplegar gráficamente, crea una copia del documento accesible a través de un [API](##Glosario) de javascript llamada DOM (`Document Object Model`). Scripts de javascript pueden despues emplear funciones como `document.getElementById` y `document.querySelector` para acceder a estas copias y usarlas como 'asas' para modificar el documento de forma programátcia. Por ejemplo, si el HTML contiene el elemento `<div id="my-div"/>`, es posible emplear javascript para introducir contenido dentro del div: 
    
    let myDiv = document.getElementById("my-div")
    myDiv.innerText = "Texto introducido"

Esta interacción entre el HTML y Javascript mediada por el DOM es la que permite crear las interfaces complejas comunes en la web moderna.

## Glosario
Alice y Bob
: Alice y Bob son los nombres estándares que se les da a interlocutores en las áreas de teoría de la información, criptografía y a veces física.

API
: `Application Programming Interface`. Un conjunto de funciones, clases, objetos, etc. que se hacen disponibles a un lenguaje de programación para dar acceso a cierta funcionaidad externa. Por ejemplo, un kit de robótica pued proveer una `API` de Javascript, para controlar el robot usando funciones como `moveLeft()` o `turnOff()`. Los navegadores proveen una miríada de `API`s para dar acceso a módulos como una webcam, la localización del cursor, el giroscopio de un celular, etc.

Bit 
: La unidad mínima de información. 1 ó 0.

Byte
: 8 bits consecutivos. Si interpretamos un byte como un número binario, un byte puede representar un entero entre 0 y 2^8 = 256.
Ejemplo: ``` 00000110 ~>  6``` 
Casi todas las computadoras modernas transfieren y guardan datos usando el byte como unidad fundamental.

Interpretar
: En el contexto de lenguajes de programación, un intérprete es un programa que toma como `input` un programa en un lenguaje de programación dado y ejecuta sus instrucciones de forma secuencial.

Latencia
: Tiempo que pasa entre el envío de un mensaje y su recepción.

Man in the middle
: Un tipo de ataque en el que un tercero malicioso inspecciona un canal de comunicación que se cree privado 
