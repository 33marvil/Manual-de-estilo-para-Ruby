# Preludio

> Estilo es lo que separa lo bueno de lo grandioso. <br/>
> -- Bozhidar Batsov

Una cosa que siempre me ha disgustado como desarrollador en Ruby es que los desarrolladores en Python tienen una genial referencia de estilo de programación ([PEP-8](http://www.python.org/dev/peps/pep-0008/)) y nosotros nunca tuvimos una guía oficial, documentando el estilo del código en Ruby y las mejores practicas. Yo creo que el estilo importa. También creo que tan finos compañeros, como nosotros los desarrolladores de Ruby, debemos ser muy capaces de producir éste codiciado documento.

Ésta guía comenzó su vida como los lineamientos internos para la escritura de código en Ruby de nuestra compañía (escrita por mí, con sinceridad). En algún punto decidí que el trabajo que estaba haciendo podría ser de interés para los miembros de la comunidad de Ruby en general y que el mundo tenia poca necesidad de otro lineamiento interno de una compañía. Pero el mundo ciertamente podría beneficiarse de un conjunto de practicas, expresiones y prescripciones de estilo para la programación en Ruby que es encausado y sancionado por la comunidad.

Desde la concepción de la guía he recibido mucha retroalimentación por parte de los miembros de la excepcional comunidad de Ruby alrededor del mundo. ¡Gracias por su apoyo! Juntos podemos construir un recurso en beneficio de todos y cada uno de los desarrolladores en Ruby ahí afuera.

Por cierto, si estás trabajando con Rails tal vez te gustaría revisar la [Guía de estilo para Ruby on Rails 3](https://github.com/bbatsov/rails-style-guide) como complemento.

# La guía de estilo de Ruby

Ésta guía de estilo de Ruby recomienda las mejores practicas para que programadores de Ruby del mundo real puedan escribir código que pueda ser mantenido por otros programadores de Ruby en el mundo real. Una guía de estilo que refleja los usos en el mundo real, es utilizada, y una guía de estilo que se apega a un ideal que ha sido rechazado por las personas a las que se supone debería ayudar, se arriesga a quedar en el olvido -  sin importar que tan buena es.

La guía está dividida en varias secciones que agrupan convenciones relacionadas. He intentado agregar el razonamiento detrás de cada una de ellas (a menos que sea muy obvio, en cuyo caso las he omitido).

No saque las convenciones de la nada - la mayoría están basadas en mi extensa carrera como ingeniero de software profesional, retroalimentación y sugerencias de miembros de la comunidad entorno a Ruby, así como varias fuentes que son altamente estimadas como recursos para la programación en Ruby, tales como ["Programming Ruby 1.9"](http://pragprog.com/book/ruby3/programming-ruby-1-9) y

La guía aun es un trabajo en progreso - algunas convenciones no tienen ejemplos en código, otras no tienen ejemplos que lo ilustren con suficiente claridad. Mientras esos detalles son arreglados - sólo mantenlos en mente por ahora.

Puedes generar una copia en PDF o HTML de está guía usando [Transmuter](https://github.com/TechnoGate/transmuter).

## Tabla de contenidos

* [Estructura del código](#estructura-del-código)
* [Sintaxis](#sintaxis)
* [Identificadores](#identificadores)
* [Comentarios](#comentarios)
* [Anotaciones](#anotaciones)
* [Clases](#clases)
* [Excepciones](#excepciones)
* [Colecciones](#colecciones)
* [Cadenas](#cadenas)
* [Expresiones regulares](#expresiones-regulares)
* [Literales percentiles](#literales-percentiles)
* [Metaprogramación](#metaprogramación)
* [Misceláneos](#misceláneos)

## Estructura del código

> Casi todos están convencidos de que cada estilo a excepción del suyo es feo e
> imposible de leer. Deja fuera el "a excepción del suyo" y probablemente tengan
> razón... <br/>
> -- Jerry Coffin (sobre identación)

* Usa `UTF-8` como la codificación de los archivos del código fuente.
* Usa dos **espacios** por nivel de identación.

  ```Ruby
  # no recomendado - cuatro o más espacios
  def some_method
      do_something
  end

  # recomendado
  def some_method
    do_something
  end
  ```

* Usa terminadores de líneas con estilo Unix (LF). Los usuarios de BSD, Solaris, Linux y OSX no deben de tener problemas, pero los usuarios de Windows deben de tener cuidado.)
    * Si usas Git tal vez quisieras agregar la siguiente opción de configuración para proteger tu proyecto de terminaciones de línea estilo Windows (CR+LF) que pudieran colarse.

      ```$ git config --global core.autocrlf true```

* Usa espacios alrededor de operadores (`+`, `-`, `*`, `/`), después de las comas, los `:`, los `;`, alrededor de `{` y antes de `}`. El espacio en blanco puede ser (en su mayoría) irrelevante, pero su uso apropiado es muy importante para escribir código que se lea fácilmente.

  ```Ruby
  sum = 1 + 2
  a, b = 1, 2
  1 > 2 ? true : false; puts 'Helo'
  [1, 2, 3].each { |e| puts e }
  ```

    La única excepción es cuando se usa el operador para exponentes:

    ```Ruby
    # no recomendado
    e = M * c ** 2

    # recomendado
    e = M * c**2
    ```

* No usar espacios antes de `(`, `[` o después de `]`, `)`.

  ```Ruby
  some(arg).other
  [1, 2, 3].length
  ```

* Identa los `when` entre `case` y `end`.

  ```Ruby
  case
    when song.name == 'Trololo'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
  end
  ```

* Usa lineas vacías dentro de los métodos para dividirlos en párrafos lógicos.

  ```Ruby
  def some_method
    data = initialize(options)

    data.manipulate!

    data.result
  end

  def some_method
    result
  end
  ```

* Identa y alinea los parámetros de la llamada a un método si se extienden en varias lineas.

  ```Ruby
  # no recomendado: la linea es demasiado larga
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
  end

  # recomendado
  def send_mail(source)
    Mailer.deliver(
      to:      'bob@example.com',
      from:    'us@example.com',
      subject: 'Important message',
      body:    source.text
    )
  end
  ```

* Usa RDoc y sus convenciones para documentar las APIs. No coloques una linea vacía entre un comentario y una definición.
* Mantén las lineas en un tamaño de 80 caracteres o menos.
* Evita el uso innecesario de espacios en blanco al final de las lineas.

## Sintaxis

* Prefiere el uso de paréntesis en la definición de un método cuando éste reciba argumentos.

  ```Ruby
  def some_method_with_arguments(arg1, arg2)
    # cuerpo omitido
  end

  def some_method
    # cuerpo omitido
  end
  ```

* Evita el uso del ciclo `for` a menos que tengas una buena razón. A diferencia de los iteradores, las variables definidas dentro de `for` son visibles fuera de él.

  ```Ruby
  arr = [1, 2, 3]

  # no recomendado
  for elem in arr do
    puts elem
  end

  # recomendado
  arr.each { |elem| puts elem }
  ```

* Las expresiones `if x: ...`, `if x; ...`, `when x: ...` y `when x; ...` fueron removidas en Ruby 1.9, por lo tanto...

    * Utiliza el operador ternario en vez de `if x: ...`

      ```Ruby
      # no recomendado
      result = if some_condition: something else something_else end

      # recomendado
      result = some_condition ? something : something_else
      ```

    * Utiliza la expresión alternativa `when x then ...` en vez de `when x: ...` en instrucciones de una línea.

      ```Ruby
      genre = case year
        when 1850..1889 then 'Blues'
        when 1890..1909 then 'Ragtime'
        when 1910..1929 then 'New Orleans Jazz'
        when 1930..1939 then 'Swing'
        when 1940..1950 then 'Bebop'
        else 'Jazz'
      end
      ```

* Si utilizas el operador ternario, procura sólo tener una expresión por rama. Esto también quiere decir que debe evitarse el uso de anidaciones dentro de los operadores ternarios. Tampoco lo expandas sobre varias lineas. Es preferible utilizar construcciones `ìf/else` en esos casos.

  ```Ruby
  # no recomendado
  some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

  # recomendado
  if some_condition
    nested_condition ? nested_something : nested_something_else
  else
    something_else
  end
  ```

* Procura utilizar `&&/||` para expresiones booleanas, y `and/or` para flujo de control. (Regla del pulgar: Si tienes que usar paréntesis, estás utilizando los operadores equivocados).

  ```Ruby
  # expresión booleana
  if some_condition && some_other_condition
    do_something
  end

  # flujo de control
  document.saved? or document.save!
  ```

* Favorece el uso de los modificadores `if/unless` cuando tengas un cuerpo de una sola línea. Otra buena alternativa es el uso de `and/or`.

  ```Ruby
  # no recomendado
  if some_condition
    do_something
  end

  # recomendado
  do_something if some_condition

  # otra buena opción
  some_condition and do_something
  ```

* Favorece el uso de `unless` sobre `if` para condiciones negativas (o utiliza `or`).

  ```Ruby
  # no recomendado
  do_something if !some_condition

  # recomendado
  do_something unless some_condition

  # otra buena opción
  some_condition or do_something
  ```

* Procura no usar `unless` con `else`. Reescribe la instrucción con el caso positivo primero.

  ```Ruby
  # no recomendado
  unless success?
    puts 'failure'
  else
    puts 'success'
  end

  # recomendado
  if success?
    puts 'success'
  else
    puts 'failure'
  end
  ```

* Si la condición de un `if/unless/while` es sencilla, se lee más claro si no se rodea con paréntesis, a menos que la condición contenga una asignación.

  ```Ruby
  # no recomendado
  if (x > 10)
    # cuerpo omitido
  end

  # recomendado
  if x > 10
    # cuerpo omitido
  end

  # recomendado cuando hay asignaciones
  if (x = self.next_value)
    # cuerpo omitido
  end
  ```

* Favorece el uso de los modificadores `while/until` cuando tengas un cuerpo de una sola línea.

  ```Ruby
  # no recomendado
  while some_condition
    do_something
  end

  # recomendado
  do_something while some_condition
  ```

* Procura utilizar `until` sobre `while` para condiciones negativas.

  ```Ruby
  # no recomendado
  do_something while !some_condition

  # recomendado
  do_something until some_condition
  ```

* Es mejor omitir los paréntesis alrededor de parámetros en métodos que pertenecen un DSL interno (como Rake, Rails, RSpec...), métodos que tienen un estado de "palabra clave" en Ruby (como attr_reader, puts...) y métodos de acceso a atributos. De no ser así, es mejor rodear los parámetros del método con paréntesis.

  ```Ruby
  class Person
    attr_reader :name, :age

    # omitido
  end

  temperance = Person.new('Temperance', 30)
  temperance.name

  puts temperance.age

  x = Math.sin(y)
  array.delete(e)
  ```

* Si el primer argumento de un método comienza con un paréntesis abierto, usa otro juego de paréntesis para invocar el método. Por ejemplo, escribe `f((3 + 2) + 1)`.

* Procura no dejar espacios entre el nombre del método y el paréntesis de apertura.

  ```Ruby
  # no recomendado
  f (3 + 2) + 1

  # recomendado
  f(3 + 2) + 1
  ```

* Prefiere `{...}` para bloques de una sola línea y expresiones encadenadas. Es es mejor usar `do...end` para bloques de varias líneas, flujo de control y definiciones de métodos. (Evita usar expresiones encadenadas en varias líneas. Pregúntate si no pueden ser separadas en métodos de una forma elegante).

  ```Ruby
  names = ['Bozhidar', 'Steve', 'Sarah']

  # recomendado
  names.each { |name| puts name }

  # no recomendado
  names.each do |name|
    puts name
  end

  # recomendado
  names.select { |name| name.start_with?('S') }.map { |name| name.upcase }

  # no recomendado
  names.select do |name|
    name.start_with?('S')
  end.map { |name| name.upcase }
  ```

* Evita usar `self` donde no es requerido.

  ```Ruby
  # no recomendado
  def ready?
    if self.last_reviewed_at > self.last_updated_at
      self.worker.update(self.content, self.options)
      self.status = :in_progress
    end
    self.status == :verified
  end

  # recomendado
  def ready?
    if last_reviewed_at > last_updated_at
      worker.update(content, options)
      self.status = :in_progress
    end
    status == :verified
  end
  ```

* Es importante que evites hacer sombra a los métodos con variables locales a menos que sean equivalentes.

  ```Ruby
  class Foo
    attr_accessor :options

    # bien
    def initialize(options)
      self.options = options
      # options y self.options son equivalentes aquí
    end

    # no recomendado
    def do_something(options = {})
      unless options[:when] == :later
        output(self.options[:message])
      end
    end

    # recomendado
    def do_something(params = {})
      unless params[:when] == :later
        output(options[:message])
      end
    end
  end
  ```

* Mejora la legibilidad al dejar espacios alrededor del operador de asignación en los valores por defecto para los parámetros de un método.

  ```Ruby
  # no recomendado
  def some_method(arg1=:default, arg2=nil, arg3=[])
    # do something...
  end

  # recomendado
  def some_method(arg1 = :default, arg2 = nil, arg3 = [])
    # do something...
  end
  ```

* Evita las continuaciones de linea (\\) cuando no son requeridas. Es mejor si estructuras tu código para evitar su uso de ser posible.

  ```Ruby
  # no recomendado
  result = 1 - \
           2
  ```

* Usar el valor retornado de una asignación esta bien, pero rodea la asignación con paréntesis.

  ```Ruby
  # no recomendado
  if v = array.grep(/foo/) ...

  # bien - muestra la intención de uso de la asignación
  if (v = array.grep(/foo/)) ...

  # bien - muestra el uso de la asignación y tiene una precedencia correcta
  if (v = self.next_value) == 'hello' ...
  ```

* Está bien utilizar `||=` para inicializar variables, excepto si se trata de variables booleanas. (Considera lo que puede pasar si el valor es `false`).

  ```Ruby
  # asina Bozhidar como el nombre, sólo si es nulo o falso
  name ||= 'Bozhidar'

  # no recomendado - sería asignado verdadero aun si es falso
  enabled ||= true

  # recomendado
  enabled = true if enabled.nil?
  ```

* Utiliza la sintaxis literal de Ruby 1.9 para los hashes. (A menos que estés trabajando con una versión que no la soporte, claro). De igual forma, utiliza la nueva sintaxis literal para lambda.

  ```Ruby
  # no recomendado
  hash = { :one => 1, :two => 2 }

  # recomendado
  hash = { one: 1, two: 2 }

  # no recomendado
  lambda = lambda { |a, b| a + b }
  lambda.call(1, 2)

  # recomendado
  lambda = ->(a, b) { a + b }
  lambda.(1, 2)
  ```

* Usa un guión bajo `_` para los parámetros no usados en un bloque.

  ```Ruby
  # no recomendado
  result = hash.map { |k, v| v + 1 }

  # recomendado
  result = hash.map { |_, v| v + 1 }
  ```

* Evita utilizar las variables especiales estilo Perl (como `$0-9`, `$``, etc). Son bastante crípticas y su uso fuera de scripts de una sola línea no es bien visto.

* Para recordar algunas de las recomendaciones sobre la sintaxis del código, ejecuta el interprete con la opción `-w`. Así generara advertencias cuando encuentre alguna practica no recomendada.

## Identificadores

> Las únicas dificultades reales al programar son la validación del cache y el
> nombrar cosas. <br/>
> -- Phil Karlton

* Utiliza `snake_case` para métodos y variables.
* Utiliza `CamelCase` para clases y módulos. (Deja en mayúsculas los acrónimos como HTTP, RFC y XML).
* Utiliza `SCREAMING_SNAKE_CASE` para otras constantes.
* Los nombres de métodos predicados (métodos que devuelven un valor booleano) deben terminar con un signo de interrogación: `?` (por ejemplo `Array#empty?`).
* Los nombres de métodos potencialmente peligrosos (por ejemplo, métodos que modifican los argumentos o `self`, que no ejecutan los terminadores como lo hace `exit`, etc.) deben terminar con una marca de exclamación `!` si existe una versión segura de ese método *peligroso*.

  ```Ruby
  # no recomendado - no hay una versión segura del método
  class Person
    def update!
    end
  end

  # bien
  class Person
    def update
    end
  end

  # recomendado
  class Person
    def update!
    end

    def update
    end
  end
  ```

* Define el método seguro en términos del método peligroso cuando sea posible.

  ```Ruby
  class Array
    def flatten_once!
      res = []

      each do |e|
        [*e].each { |f| res << f }
      end

      replace(res)
    end

    def flatten_once
      dup.flatten_once!
    end
  end
  ```

* Cuando uses `reduce` en bloques cortos, nombra los argumentos como `|a, e|` (acumulador, elemento).
* Cuando definas operadores binarios, nombra el argumento como `other`.

  ```Ruby
  def +(other)
    # cuerpo omitido
  end
  ```

* Prefiere `map` sobre `collect`, `find` sobre `detect`, `select` sobre `find_all`, `reduce` sobre `inject` y `size` sobre `length`. Escribirás menos. :-)
