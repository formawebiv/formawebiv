# XSD

**XSD** (*XML Schema Definition*) es un lenguaje, también llamado simplemente ***XML Schema\***, que sirve para definir la estructura de un documento XML, permitiendo su validación.

En los siguientes enlaces se puede consultar la *W3C Recommendation* de XSD, en la cual está basado este tutorial de introducción a XML Schema:

- [XML Schema Part 0: Primer](http://www.w3.org/TR/xmlschema-0/)
- [XML Schema Part 1: Structures](http://www.w3.org/TR/xmlschema-1/)
- [XML Schema Part 2: Datatypes](http://www.w3.org/TR/xmlschema-2/)

Los contenidos de este tutorial, están redactados asumiendo que el lector ya posee conocimientos previos de XML. De no ser así, se recomienda consultar el [tutorial de XML](https://www.abrirllave.com/xml/).



# Validación de un documento XML con XSD (XML Schema)



**24**

**EJEMPLO** Se quiere almacenar una lista de marcadores de páginas web, guardando de cada uno de ellos su nombre, una descripción y su URL. Para ello, se ha escrito el siguiente documento XML (***"marcadores.xml"\***) asociado al archivo ***"marcadores.xsd"\***:

<?xml version="1.0" encoding="UTF-8"?> <marcadores xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="marcadores.xsd">    <pagina>       <nombre>Abrirllave</nombre>       <descripcion>Tutoriales de informática.</descripcion>       <url>http://www.abrirllave.com/</url>    </pagina>    <pagina>       <nombre>Wikipedia</nombre>       <descripcion>La enciclopedia libre.</descripcion>       <url>http://www.wikipedia.org/</url>    </pagina>    <pagina>       <nombre>W3C</nombre>       <descripcion>World Wide Web Consortium.</descripcion>       <url>http://www.w3.org/</url>    </pagina> </marcadores>

- Para vincular un esquema a un documento XML, es obligatorio que este último haga referencia al espacio de nombres **http://www.w3.org/2001/XMLSchema-instance**. Para ello, habitualmente se utiliza el prefijo **xsi**.
- El atributo **noNameSchemaLocation** permite referenciar a un archivo con la definición de un esquema que no tiene ningún espacio de nombres asociado. En este caso, dicho archivo es ***"marcadores.xsd"\***.

El esquema XML guardado en ***"marcadores.xsd"\*** y que permita validar el documento XML ***"marcadores.xml"\*** podría ser:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  <xs:element name="marcadores">    <xs:complexType>      <xs:sequence>        <xs:element name="pagina" maxOccurs="unbounded">          <xs:complexType>            <xs:sequence>              <xs:element name="nombre" type="xs:string"/>              <xs:element name="descripcion" type="xs:string"/>              <xs:element name="url" type="xs:string"/>             </xs:sequence>          </xs:complexType>        </xs:element>      </xs:sequence>    </xs:complexType>  </xs:element> </xs:schema>
```

Para estar bien formado, un esquema XML tiene que cumplir las mismas reglas de sintaxis que cualquier otro documento XML.



Por otra parte, hay que tener en cuenta que, en todos los esquemas XML, el elemento raíz es "schema". Ahora bien, para escribirlo, es muy común utilizar el prefijo **xsd** o **xs**.

Con **xmlns:xs="http://www.w3.org/2001/XMLSchema"** se ha indicado que:

- Los elementos y tipos de datos utilizados en el esquema pertenecen al espacio de nombres **http://www.w3.org/2001/XMLSchema**.
- Dichos elementos y tipos de datos deben llevar el prefijo **xs** (**xs:schema**, **xs:element**, **xs:complexType**, **xs:string**...).

Fíjese también que:

- Los elementos "marcadores" y "página" son de tipo complejo (**complexType**), ya que, contienen a otros elementos.
- **sequence** indica que los elementos hijo deben aparecer, en el documento XML, en el mismo orden en el que sean declarados en el esquema.
- Los elementos "nombre", "descripción" y "url" son de tipo simple (**string** en este caso) y no pueden contener a otros elementos.
- Mediante **maxOccurs="unbounded"** se ha indicado que pueden aparecer ilimitados elementos "página" en el documento XML.

| Ejercicio                                                    |
| ------------------------------------------------------------ |
| [Validar un documento XML](https://www.abrirllave.com/xsd/ejercicio-validar-un-documento-xml.php) |

## Definición de un espacio de nombres

**EJEMPLO** En el siguiente documento XML se ha definido un espacio de nombres escribiendo **xmlns:mar="http://www.abrirllave.com/marcadores"**:

```
<?xml version="1.0" encoding="UTF-8"?> <mar:marcadores xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.abrirllave.com/marcadores marcadores.xsd" xmlns:mar="http://www.abrirllave.com/marcadores">   <mar:pagina>      <mar:nombre>Abrirllave</mar:nombre>      <mar:descripcion>Tutoriales de informática.</mar:descripcion>      <mar:url>http://www.abrirllave.com/</mar:url>   </mar:pagina>   <mar:pagina>      <mar:nombre>Wikipedia</mar:nombre>      <mar:descripcion>La enciclopedia libre.</mar:descripcion>      <mar:url>http://www.wikipedia.org/</mar:url>   </mar:pagina>   <mar:pagina>      <mar:nombre>W3C</mar:nombre>      <mar:descripcion>World Wide Web Consortium.</mar:descripcion>      <mar:url>http://www.w3.org/</mar:url>   </mar:pagina> </mar:marcadores>
```

En el atributo **schemaLocation** se pueden escribir parejas de valores:

- En el primer valor de cada pareja, hay que hacer referencia a un espacio de nombres.
- En el segundo valor, se tiene que indicar la ubicación de un archivo donde hay un esquema de ese espacio de nombres.

En cuanto al archivo ***"marcadores.xsd"\***, ahora su código podría ser:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="http://www.abrirllave.com/marcadores" xmlns="http://www.abrirllave.com/marcadores" elementFormDefault="qualified">  <xs:element name="marcadores">    <xs:complexType>      <xs:sequence>        <xs:element name="pagina" maxOccurs="unbounded">          <xs:complexType>            <xs:sequence>              <xs:element name="nombre" type="xs:string"/>              <xs:element name="descripcion" type="xs:string"/>              <xs:element name="url" type="xs:string"/>             </xs:sequence>          </xs:complexType>        </xs:element>      </xs:sequence>    </xs:complexType>  </xs:element> </xs:schema>
```

- En el atributo **targetNamespace** se está indicando que los elementos definidos en este esquema ("marcadores", "página", "nombre", "descripción" y "url"), provienen del espacio de nombres **http://www.abrirllave.com/marcadores**.
- **xmlns="http://www.abrirllave.com/marcadores"** especifica que este es el espacio de nombres por defecto.
- El atributo **elementFormDefault="qualified"** indica que todos los elementos declarados localmente en el esquema tienen que estar calificados, es decir, tienen que pertenecer a un espacio de nombres. Por esta razón, en ***"marcadores.xml"\*** se han escrito con el prefijo **mar**.

## Elementos globales y locales en XSD

Los elementos pueden ser globales o locales:

- Los elementos globales son hijos directos del elemento raíz, **<xs:schema>** en este caso. En el ejemplo que estamos tratando, solamente existe un elemento global: **<xs:element name="marcadores">**.
- Los elementos locales son el resto de elementos.

Cuando se define un espacio de nombres, los elementos globales tienen que estar calificados, obligatoriamente.

## **elementFormDefault="unqualified"**

**EJEMPLO** En el supuesto de que el valor del atributo **elementFormDefault** fuese **"unqualified"**, para que ***"marcadores.xml"\*** fuese válido, se podría escribir algo similar a:

<?xml version="1.0" encoding="UTF-8"?> <mar:marcadores xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.abrirllave.com/marcadores marcadores.xsd" xmlns:mar="http://www.abrirllave.com/marcadores">    <pagina>       <nombre>Abrirllave</nombre>       <descripcion>Tutoriales de informática.</descripcion>       <url>http://www.abrirllave.com/</url>    </pagina>    <pagina>       <nombre>Wikipedia</nombre>       <descripcion>La enciclopedia libre.</descripcion>       <url>http://www.wikipedia.org/</url>    </pagina>    <pagina>       <nombre>W3C</nombre>       <descripcion>World Wide Web Consortium.</descripcion>       <url>http://www.w3.org/</url>    </pagina> </mar:marcadores>

- **"unqualified"** es el valor por defecto del atributo **elementFormDefault**.

## **elementFormDefault="qualified"**

**EJEMPLO** Si el atributo **elementFormDefault** se definiese **"qualified"**, también sería válido el siguiente documento XML:

<?xml version="1.0" encoding="UTF-8"?> <marcadores xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.abrirllave.com/marcadores marcadores.xsd" xmlns="http://www.abrirllave.com/marcadores">    <pagina>       <nombre>Abrirllave</nombre>       <descripcion>Tutoriales de informática.</descripcion>       <url>http://www.abrirllave.com/</url>    </pagina>    <pagina>       <nombre>Wikipedia</nombre>       <descripcion>La enciclopedia libre.</descripcion>       <url>http://www.wikipedia.org/</url>    </pagina>    <pagina>       <nombre>W3C</nombre>       <descripcion>World Wide Web Consortium.</descripcion>       <url>http://www.w3.org/</url>    </pagina> </marcadores>

Ahora, ningún elemento lleva prefijo, al igual que el espacio de nombres al que pertenecen: **xmlns="http://www.abrirllave.com/marcadores"**

## Validación de un sitemap XML

**EJEMPLO** Para validar el archivo ***"sitemap.xml"\*** que da solución al ejercicio de XML propuesto en el siguiente enlace:

- [www.abrirllave.com/xml/ejercicio-crear-un-sitemap-xml.php](http://www.abrirllave.com/xml/ejercicio-crear-un-sitemap-xml.php)

Obsérvese que, es necesario añadir el texto resaltado que se muestra a continuación:

<?xml version="1.0" encoding="UTF-8"?> <urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">    <url>       <loc>http://www.ejemplos-de-abrirllave.com/</loc>       <lastmod>2016-09-30</lastmod>       <priority>0.8</priority>    </url>    <url>       <loc>http://www.ejemplos-de-abrirllave.com/contactar.html</loc>       <lastmod>2016-09-30</lastmod>       <priority>0.3</priority>    </url>    <url>       <loc>http://www.ejemplos-de-abrirllave.com/productos/impresora.html</loc>       <lastmod>2016-09-30</lastmod>       <priority>0.5</priority>    </url>    <url>       <loc>http://www.ejemplos-de-abrirllave.com/productos/monitor.html</loc>       <lastmod>2016-09-30</lastmod>       <priority>0.5</priority>    </url>    <url>       <loc>http://www.ejemplos-de-abrirllave.com/productos/teclado.html</loc>       <lastmod>2016-09-30</lastmod>       <priority>0.5</priority>    </url> </urlset>

Al visualizar en un navegador web el código fuente del archivo ***"sitemap.xsd"\*** ubicado en:

- http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd

En pantalla se verá algo parecido a:

![Código fuente del archivo sitemap.xsd](https://www.abrirllave.com/xsd/images/codigo-sitemap-xsd.gif)

Nótese que, en el archivo ***"sitemap.xsd"\***, la etiqueta **<xsd:schema>** contiene los atributos **xmlns:xsd**, **targetNamespace**, **xmlns** y **elementFormDefault**.

```
<?xml version="1.0" encoding="UTF-8"?> <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"            targetNamespace="http://www.sitemaps.org/schemas/sitemap/0.9"            xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"            elementFormDefault="qualified">
```

| Ejercicios                                                   |
| ------------------------------------------------------------ |
| [Validar un sitemap XML](https://www.abrirllave.com/xsd/ejercicio-validar-un-sitemap-xml.php)[Mensaje entre personas](https://www.abrirllave.com/xsd/ejercicio-mensaje-entre-personas.php) |



# Elementos simples en XSD (XML Schema)





**Los elementos simples solamente pueden contener texto** (caracteres). Dicho de otro modo, los elementos simples no pueden contener a otro u otros elementos (hijos), ni tampoco pueden tener atributos. Ahora bien, el texto contenido en un elemento simple, puede ser de diferentes tipos de datos predefinidos en ***W3C XML Schema\*** o definidos por el usuario (programador).

Los tipos de datos predefinidos pueden ser primitivos (**string**, **boolean**, **decimal**...) o derivados de estos (**integer**, **ID**, **IDREF**...). Véase en el siguiente enlace, una imagen donde se puede ver la relación que existe entre todos ellos:

- https://goo.gl/lK2l2I

Para definir un elemento simple se puede utilizar la siguiente sintaxis:

```
<xs:element name="nombre_del_elemento" type="tipo_de_dato"/>
```

**EJEMPLO** Para los siguientes elementos XML:

```
<nombre>Elsa</nombre> <edad>23</edad>
```

Sus definiciones pueden ser:

```
<xs:element name="nombre" type="xs:string"/> <xs:element name="edad" type="xs:integer"/>
```

## Tipos de declaración de elementos simples (**fixed**, **default**)

Si se quiere indicar que un valor es fijo (**fixed**), se puede escribir, por ejemplo:

```
<xs:element name="mes" type="xs:string" fixed="agosto"/>
```

También, se puede especificar un valor por defecto (**default**), por ejemplo, tecleando:

```
<xs:element name="mes" type="xs:string" default="agosto"/>
```

| Ejercicios resueltos                                         |
| ------------------------------------------------------------ |
| [Definición de elementos simples](https://www.abrirllave.com/xsd/ejercicio-definicion-de-elementos-simples.php)[Puerta cerrada y ventana abierta](https://www.abrirllave.com/xsd/ejercicio-puerta-cerrada-y-ventana-abierta.php) |

# Atributos en XSD (XML Schema)



**7**

Para definir un atributo se puede emplear la siguiente sintaxis:

```
<xs:attribute name="nombre_del_atributo" type="tipo_de_dato"/>
```

**EJEMPLO** Para el elemento "curso" siguiente, donde aparece el atributo "grupo":

```
<curso grupo="B">2</curso>
```

Sus definiciones pueden ser:

```
<xs:element name="curso" type="xs:integer"/> <xs:attribute name="grupo" type="xs:string"/>
```

- Todos los atributos pueden tomar por valor tipos simples.
- Por otra parte, cuando un elemento tiene al menos un atributo –como es el caso del elemento "curso" en este ejemplo– dicho elemento se dice que es complejo.

## Tipos de declaración de atributos (**fixed**, **default**, **optional**, **required**)

Para indicar que el valor de un atributo es fijo (**fixed**), es posible escribir, por ejemplo:

```
<xs:attribute name="grupo" type="xs:string" fixed="B"/>
```

Para especificar el valor por defecto (**default**) de un atributo, se puede escribir:

```
<xs:attribute name="grupo" type="xs:string" default="B"/>
```

Para indicar que un atributo es obligatorio (**required**) escribirlo, se puede teclear:

```
<xs:attribute name="grupo" type="xs:string" use="required"/>
```

Por defecto, si no se indica nada, el atributo será opcional (**optional**).

| Ejercicio resuelto                                           |
| ------------------------------------------------------------ |
| [Fichas de personas](https://www.abrirllave.com/xsd/ejercicio-fichas-de-personas.php) |

# Restricciones (facetas) en XSD (XML Schema)



**23**

***XML Schema\*** permite definir restricciones a los posibles valores de los tipos de datos. Dichas restricciones se pueden establecer en diferentes aspectos, llamados facetas.

Dicho de otro modo, **las facetas permiten definir restricciones sobre los posibles valores de atributos o elementos.** Las facetas que pueden utilizarse son:

| Faceta                | Descripción                                                  |
| --------------------- | ------------------------------------------------------------ |
| **xs:length**         | Especifica una longitud fija.                                |
| **xs:minLength**      | Especifica una longitud mínima.                              |
| **xs:maxLength**      | Especifica una longitud máxima.                              |
| **xs:pattern**        | Especifica un patrón de caracteres admitidos.                |
| **xs:enumeration**    | Especifica una lista de valores admitidos.                   |
| **xs:whiteSpace**     | Especifica cómo se debe tratar a los posibles espacios en blanco, las tabulaciones, los saltos de línea y los retornos de carro que puedan aparecer. |
| **xs:maxInclusive**   | Especifica que el valor debe ser menor o igual que el indicado. |
| **xs:maxExclusive**   | Especifica que el valor debe ser menor que el indicado.      |
| **xs:minExclusive**   | Especifica que el valor debe ser mayor que el indicado.      |
| **xs:minInclusive**   | Especifica que el valor debe ser mayor o igual que el indicado. |
| **xs:totalDigits**    | Especifica el número máximo de dígitos que puede tener un número. |
| **xs:fractionDigits** | Especifica el número máximo de decimales que puede tener un número. |

Seguidamente, se muestran algunos ejemplos de restricciones definidas con una o más facetas:

## **xs:minExclusive** y **xs:maxInclusive**

**EJEMPLO** En el siguiente código se define un elemento llamado "mes" con la restricción de que el valor que tome no pueda ser menor que 1 ni mayor que 12:

```
<xs:element name="mes">   <xs:simpleType>      <xs:restriction base="xs:integer">         <xs:minInclusive value="1"/>         <xs:maxInclusive value="12"/>      </xs:restriction>   </xs:simpleType> </xs:element>
```

- **xs:simpleType** permite definir un tipo simple y especificar sus restricciones.
- **xs:restriction** sirve para definir restricciones de un **xs:simpleType** (como se ha hecho en este ejemplo). También sirve para definir restricciones de un **xs:simpleContent** o de un **xs:complexContent**. Estos elementos se estudiarán más adelante en este tutorial.
- En el atributo **base** se indica el tipo de dato a partir del cual se define la restricción.
- **xs:minInclusive** sirve para especificar que el valor debe ser mayor o igual que el indicado en su atributo **value**, (en este caso, mayor o igual que 1).
- **xs:maxInclusive** sirve para especificar que el valor debe ser menor o igual que el indicado en su atributo **value**, (en este caso, menor o igual que 12).

También se podría haber escrito:

```
<xs:element name="mes" type="numeroMes"/> <xs:simpleType name="numeroMes">   <xs:restriction base="xs:integer">      <xs:minInclusive value="1"/>      <xs:maxInclusive value="12"/>   </xs:restriction> </xs:simpleType>
```

Haciendo esto, el tipo **numeroMes** definido, podría ser utilizado por otros elementos, ya que, no está contenido en el elemento "mes".

| Ejercicios resueltos                                         |
| ------------------------------------------------------------ |
| [Edad entre 0 y 130 años](https://www.abrirllave.com/xsd/ejercicio-edad-entre-0-y-130.php)[Precios de tres dígitos](https://www.abrirllave.com/xsd/ejercicio-precios-de-tres-digitos.php) |

## **xs:enumeration**

**EJEMPLO** En el siguiente ejemplo se define un elemento llamado "color" con la restricción de que los únicos valores admitidos son: **"verde"**, **"amarillo"** y **"rojo"**.

```
<xs:element name="color">   <xs:simpleType>      <xs:restriction base="xs:string">         <xs:enumeration value="verde"/>         <xs:enumeration value="amarillo"/>         <xs:enumeration value="rojo"/>      </xs:restriction>   </xs:simpleType> </xs:element>
```

- **xs:enumeration** sirve para definir una lista de valores admitidos.

| Ejercicio resuelto                                           |
| ------------------------------------------------------------ |
| [Tipo de vehículo](https://www.abrirllave.com/xsd/ejercicio-tipo-de-vehiculo.php) |

## **xs:pattern**

**EJEMPLO** En el siguiente ejemplo se define un elemento llamado "letra" con la restricción de que el único valor admitido es una de las letras minúsculas de la **"a"** a la **"z"**:

```
<xs:element name="letra">   <xs:simpleType>      <xs:restriction base="xs:string">         <xs:pattern value="[a-z]"/>      </xs:restriction>   </xs:simpleType> </xs:element>
```

- **xs:pattern** sirve para definir un patrón de caracteres admitidos (en este caso se admite una única letra minúscula de la **"a"** a la **"z"**). **El valor del patrón tiene que ser una expresión regular.**

| Ejercicios resueltos                                         |
| ------------------------------------------------------------ |
| [Iniciales de personas famosas](https://www.abrirllave.com/xsd/ejercicio-iniciales-de-personas-famosas.php)[Iniciales al revés](https://www.abrirllave.com/xsd/ejercicio-iniciales-al-reves.php)[Respuestas admitidas](https://www.abrirllave.com/xsd/ejercicio-respuestas-admitidas.php)[Números y letras](https://www.abrirllave.com/xsd/ejercicio-numeros-y-letras.php)[Escribir expresiones regulares](https://www.abrirllave.com/xsd/ejercicio-escribir-expresiones-regulares.php)[Letras admitidas](https://www.abrirllave.com/xsd/ejercicio-letras-admitidas.php) |

## **xs:length**

**EJEMPLO** En el siguiente ejemplo se define un elemento llamado "clave" con la restricción de que su valor tiene que ser una cadena de, exactamente, doce caracteres:

```
<xs:element name="clave">   <xs:simpleType>      <xs:restriction base="xs:string">         <xs:length value="12"/>      </xs:restriction>   </xs:simpleType> </xs:element>
```

- **xs:length** sirve para especificar una longitud fija.

| Ejercicios resueltos                                         |
| ------------------------------------------------------------ |
| [Longitud fija de una clave](https://www.abrirllave.com/xsd/ejercicio-longitud-fija-de-una-clave.php)[Longitud mínima y máxima de una clave](https://www.abrirllave.com/xsd/ejercicio-longitud-minima-y-maxima-de-una-clave.php) |

## **xs:whiteSpace**

**EJEMPLO** En el siguiente ejemplo se define un elemento llamado "dirección" con la restricción de que los espacios en blanco, las tabulaciones, los saltos de línea y los retornos de carro que aparezcan en él, se deben mantener (**preserve**):

```
<xs:element name="direccion">   <xs:simpleType>      <xs:restriction base="xs:string">         <xs:whiteSpace value="preserve"/>      </xs:restriction>   </xs:simpleType> </xs:element>
```

- **xs:whiteSpace** sirve para especificar cómo se debe tratar a los posibles espacios en blanco, las tabulaciones, los saltos de línea y los retornos de carro que puedan aparecer.

En vez de **preserve** también se puede utilizar:

- **replace** para sustituir todas las tabulaciones, los saltos de línea y los retornos de carro por espacios en blanco.
- **collapse** para, después de reemplazar todas las tabulaciones, los saltos de línea y los retornos de carro por espacios en blanco, eliminar todos los espacios en blanco únicos y sustituir varios espacios en blanco seguidos por un único espacio en blanco.



# Extensiones en XSD (XML Schema)



**12**

**xs:extension** sirve para extender un elemento **simpleType** o **complexType**.

## **xs:extension** (**complexContent**)

**EJEMPLO** Dado el siguiente documento XML:

<?xml version="1.0" encoding="UTF-8"?> <fichas xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="fichas.xsd">    <ficha numero="1">       <nombre>Eva</nombre>       <edad>25</edad>       <ciudad>París</ciudad>       <pais>Francia</pais>    </ficha>    <ficha numero="2">       <nombre>Giovanni</nombre>       <edad>26</edad>       <ciudad>Florencia</ciudad>       <pais>Italia</pais>    </ficha> </fichas>

Y el archivo ***"fichas.xsd"\*** que permite validarlo:

<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">    <xs:element name="fichas">     <xs:complexType>       <xs:sequence>         <xs:element name="ficha" type="infoPersonaAmpliada"                     maxOccurs="unbounded"/>       </xs:sequence>     </xs:complexType>   </xs:element>    <xs:complexType name="infoPersonaAmpliada">     <xs:complexContent>       <xs:extension base="infoPersona">         <xs:sequence>           <xs:element name="ciudad" type="xs:string"/>           <xs:element name="pais" type="xs:string"/>         </xs:sequence>       </xs:extension>     </xs:complexContent>   </xs:complexType>    <xs:complexType name="infoPersona">     <xs:sequence>       <xs:element name="nombre" type="xs:string"/>       <xs:element name="edad" type="edadPersona"/>     </xs:sequence>     <xs:attribute name="numero" type="xs:integer"/>   </xs:complexType>    <xs:simpleType name="edadPersona">      <xs:restriction base="xs:integer">         <xs:minExclusive value="-1"/>         <xs:maxExclusive value="131"/>      </xs:restriction>   </xs:simpleType>  </xs:schema>

- Obsérvese que, **infoPersonaAmpliada** se basa en **infoPersona**, añadiéndole dos elementos: "ciudad" y "país".
- En cuanto a **xs:complexContent**, sirve para definir restricciones o extensiones a un tipo complejo (**complexType**).

| Ejercicio resuelto                                           |
| ------------------------------------------------------------ |
| [Información de persona ampliada](https://www.abrirllave.com/xsd/ejercicio-informacion-de-persona-ampliada.php) |

## **xs:extension** (**simpleContent**)

**xs:simpleContent** permite definir restricciones o extensiones a elementos que solo contienen datos, es decir, no contienen a otros elementos.

**EJEMPLO** El siguiente archivo ***"precios.xsd"\***:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  <xs:element name="precios">    <xs:complexType>      <xs:sequence>        <xs:element name="precio" maxOccurs="unbounded">          <xs:complexType>            <xs:simpleContent>              <xs:extension base="xs:decimal">                <xs:attribute name="moneda" type="xs:string"/>              </xs:extension>            </xs:simpleContent>          </xs:complexType>        </xs:element>      </xs:sequence>    </xs:complexType>  </xs:element> </xs:schema>
```

Permite validar el siguiente documento XML:

```
<?xml version="1.0" encoding="UTF-8"?> <precios xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="precios.xsd">   <precio moneda="Euro">5</precio>   <precio moneda="Dólar">6.2</precio>   <precio moneda="Libra esterlina">4.3</precio> </precios>
```

- Nótese que, utilizando **xs:extension**, al elemento "precio" se le ha incorporado el atributo **moneda**.

| Ejercicios resueltos                                         |
| ------------------------------------------------------------ |
| [Precios de artículos](https://www.abrirllave.com/xsd/ejercicio-precios-de-articulos.php)[Información de ubicaciones](https://www.abrirllave.com/xsd/ejercicio-informacion-de-ubicaciones.php)[Colores de muebles](https://www.abrirllave.com/xsd/ejercicio-colores-de-muebles.php) |



# Elementos complejos en XSD (XML Schema)





Un elemento es complejo (**complexType**) cuando contiene uno o más elementos y/o atributos. De entre las posibles combinaciones de elementos y/o atributos que puede contener un elemento complejo (1 elemento y 0 atributos, 1 elemento y 1 atributo, 1 elemento y varios atributos, 0 elementos y 1 atributo...) cabe destacar las siguientes:

- **Un elemento complejo puede estar vacío**, es decir, no contener elementos ni texto, pero sí tener al menos un atributo.
- **Un elemento complejo puede contener contenido mixto**, es decir, contener uno o más elementos, además de texto. Por otra parte, podría tener atributos, o no.

## Elemento vacío

**EJEMPLO** En el siguiente código se ha definido vacío el elemento "bola", no pudiendo contener ni otros elementos ni texto. Ahora bien, véase que sí tiene un atributo, llamado **numero**:

```
<xs:element name="bola">  <xs:complexType>    <xs:attribute name="numero" type="numeroDeBola"/>  </xs:complexType> </xs:element> <xs:simpleType name="numeroDeBola">   <xs:restriction base="xs:positiveInteger">      <xs:minInclusive value="1"/>      <xs:maxExclusive value="90"/>   </xs:restriction> </xs:simpleType>
```

- **xs:positiveInteger** indica que el valor del atributo **numero** debe ser un número entero mayor que cero.

| Ejercicio resuelto                                           |
| ------------------------------------------------------------ |
| [Números del bingo](https://www.abrirllave.com/xsd/ejercicio-numeros-del-bingo.php) |

## Contenido mixto

Fíjese que, en el siguiente código se ha definido el elemento "persona" de tipo complejo mixto (**mixed="true"**):

```
<xs:element name="persona">  <xs:complexType mixed="true">    <xs:sequence>      <xs:element name="nombre" type="xs:string"/>      <xs:element name="ciudad" type="xs:string"/>      <xs:element name="edad" type="xs:positiveInteger"/>    </xs:sequence>  </xs:complexType> </xs:element>
```

| Ejercicio resuelto                                           |
| ------------------------------------------------------------ |
| [Información de personas en contenido mixto](https://www.abrirllave.com/xsd/ejercicio-informacion-de-personas-en-contenido-mixto.php) |



# Indicadores en XSD (XML Schema)



**9**

Los indicadores permiten establecer cómo se van a escribir –o utilizar– los elementos en un documento XML. Hay siete tipos de indicadores que se pueden clasificar en:

- **Indicadores de orden:** secuencia (**sequence**), todo (**all**) y elección (**choice**).
- **Indicadores de ocurrencia:** **maxOccurs** y **minOccurs**.
- **Indicadores de grupo:** de elementos (**group**) y de atributos (**attributeGroup**).

## Indicadores de orden (**xs:sequence**, **xs:all**, **xs:choice**)

Mientras que **xs:sequence** sirve para especificar el orden en el que obligatoriamente deben aparecer los elementos hijo de un elemento, **xs:all** sirve para indicar que dichos elementos pueden aparecer en cualquier orden.

**EJEMPLO** El siguiente archivo ***"lugar.xsd"\***:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  <xs:element name="lugar">    <xs:complexType>      <xs:sequence>        <xs:element name="ciudad">          <xs:complexType>            <xs:all>              <xs:element name="nombre" type="xs:string"/>              <xs:element name="pais" type="xs:string"/>            </xs:all>          </xs:complexType>        </xs:element>      </xs:sequence>    </xs:complexType>  </xs:element> </xs:schema>
```

Permite validar el siguiente documento XML:

```
<?xml version="1.0" encoding="UTF-8"?> <lugar xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="lugar.xsd">   <ciudad>      <pais>Italia</pais>      <nombre>Florencia</nombre>   </ciudad> </lugar>
```

Por otra parte, **xs:choice** sirve para especificar que solamente se permite escribir uno de los elementos hijo. Por ejemplo, en este caso, se podría utilizar para indicar que habría que elegir entre escribir el "nombre" o escribir el "país" de la "ciudad", pero no ambos.

## Indicadores de ocurrencia (**maxOccurs**, **minOccurs**)

**maxOccurs** y **minOccurs** permiten establecer, respectivamente, el número máximo y mínimo de veces que puede aparecer un determinado elemento. El valor por defecto para **maxOccurs** y **minOccurs** es 1.

**EJEMPLO** Dado el siguiente documento XML ***"paises.xml"\***:

<?xml version="1.0" encoding="UTF-8"?> <paises xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="paises.xsd">    <pais>       <nombre>Argentina</nombre>       <ciudad>Buenos Aires</ciudad>       <ciudad>Rosario</ciudad>    </pais>    <pais>       <nombre>México</nombre>       <ciudad>Guadalajara</ciudad>       <ciudad>Monterrey</ciudad>       <ciudad>Cancún</ciudad>       <ciudad>Mérida</ciudad>       <ciudad>Ciudad de México</ciudad>    </pais>    <pais>       <nombre>Colombia</nombre>    </pais> </paises>

Considerando que se quiere especificar que:

- "país" pueda aparecer una o ilimitadas veces.
- "nombre" tenga que escribirse obligatoriamente, y solo una vez, dentro de "país".
- De cada "país" puedan escribirse de cero a cinco "ciudades".

El código del archivo ***"paises.xsd"\*** que permita validar ***"paises.xml"\***, podría ser:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  <xs:element name="paises">    <xs:complexType>      <xs:sequence>        <xs:element name="pais" maxOccurs="unbounded">          <xs:complexType>            <xs:sequence>              <xs:element name="nombre" type="xs:string"/>              <xs:element name="ciudad" type="xs:string"                          minOccurs="0" maxOccurs="5"/>            </xs:sequence>          </xs:complexType>        </xs:element>      </xs:sequence>    </xs:complexType>  </xs:element> </xs:schema>
```

## Indicadores de grupo (**xs:group**, **xs:attributeGroup**)

**xs:group** sirve para agrupar un conjunto de declaraciones de elementos relacionados.

**EJEMPLO** Dado el siguiente documento XML ***"personas.xml"\***:

<?xml version="1.0" encoding="UTF-8"?> <personas xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="personas.xsd">    <persona>       <nombre>Eva</nombre>       <edad>25</edad>       <pais>Francia</pais>       <telefono>999888777</telefono>    </persona>    <persona>       <nombre>Giovanni</nombre>       <edad>26</edad>       <pais>Italia</pais>       <telefono>111222333</telefono>    </persona> </personas>

Y el archivo ***"personas.xsd"\*** que permite validarlo:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">   <xs:element name="personas">    <xs:complexType>      <xs:sequence>        <xs:element name="persona" type="datosDePersona"                    maxOccurs="unbounded"/>      </xs:sequence>    </xs:complexType>  </xs:element>   <xs:complexType name="datosDePersona">    <xs:sequence>      <xs:group ref="datosBasicos"/>      <xs:element name="telefono" type="xs:string"/>    </xs:sequence>  </xs:complexType>   <xs:group name="datosBasicos">    <xs:sequence>      <xs:element name="nombre" type="xs:string"/>      <xs:element name="edad" type="xs:positiveInteger"/>      <xs:element name="pais" type="xs:string"/>    </xs:sequence>  </xs:group> </xs:schema>
```

- Obsérvese que, se ha definido el grupo **datosBasicos**, el cual ha sido incorporado a la definición del tipo complejo **datosDePersona**.

Del mismo modo, **attributeGroup** sirve para definir un grupo de atributos. Por ejemplo, para validar el siguiente documento XML:

```
<?xml version="1.0" encoding="UTF-8"?> <personas xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="personas.xsd">   <persona nombre="Eva" edad="25" pais="Francia"/>   <persona nombre="Giovanni" edad="26" pais="Italia"/> </personas>
```

Se puede escribir el siguiente código en el archivo ***"personas.xsd"\***:

```
<?xml version="1.0" encoding="UTF-8"?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">   <xs:element name="personas">    <xs:complexType>      <xs:sequence>        <xs:element name="persona" maxOccurs="unbounded">          <xs:complexType>            <xs:attributeGroup ref="datosDePersona"/>          </xs:complexType>        </xs:element>      </xs:sequence>    </xs:complexType>  </xs:element>   <xs:attributeGroup name="datosDePersona">    <xs:attribute name="nombre" type="xs:string"/>    <xs:attribute name="edad" type="xs:positiveInteger"/>    <xs:attribute name="pais" type="xs:string"/>  </xs:attributeGroup> </xs:schema>
```

- En este caso, se ha definido el grupo de atributos **datosDePersona**, el cual ha sido incorporado a la definición del elemento **persona**.

| Ejercicios resueltos                                         |
| ------------------------------------------------------------ |
| [Panel de vuelos](https://www.abrirllave.com/xsd/ejercicio-panel-de-vuelos.php)[Factura](https://www.abrirllave.com/xsd/ejercicio-factura.php)[Registro de conexiones](https://www.abrirllave.com/xsd/ejercicio-registro-de-conexiones.php)[Personal de departamentos](https://www.abrirllave.com/xsd/ejercicio-personal-de-departamentos.php) |

---

_.ref:_ https://www.abrirllave.com/xsd/validacion-de-un-documento-xml-con-xsd.php



https://drive.google.com/drive/folders/0B5H0la1TMfufVFJoV0w3LWRtanc?resourcekey=0-LYS8TLdq6gcdfGaz8KhkJA