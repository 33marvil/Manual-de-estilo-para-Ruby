# Preludio

> Estilo es lo que separa lo bueno de lo grandioso. <br/>
> -- Bozhidar Batsov

Una cosa que siempre me ha disgustado como desarrollador en Ruby es que los
desarrolladores en Python tienen una genial referencia de estilo de programación
([PEP-8](http://www.python.org/dev/peps/pep-0008/)) y nosotros nunca tuvimos una guía oficial, documentando el estilo del
código en Ruby y las mejores practicas. Yo creo que el estilo importa. También
creo que tan finos compañeros, como nosotros los desarrolladores de Ruby,
debemos ser muy capaces de producir éste codiciado documento.

Ésta guía comenzó su vida como los lineamientos internos para la escritura de
código en Ruby de nuestra compañía (escrita por mí, con sinceridad). En algún
punto decidí que el trabajo que estaba haciendo podría ser de interés para los
miembros de la comunidad de Ruby en general y que el mundo tenia poca necesidad
de otro lineamiento interno de una compañía. Pero el mundo ciertamente podría
beneficiarse de un conjunto de practicas, expresiones y prescripciones de estilo
para la programación en Ruby que es encausado y sancionado por la comunidad.

Desde la concepción de la guía he recibido mucha retroalimentación por parte de
los miembros de la excepcional comunidad de Ruby alrededor del mundo. ¡Gracias
por su apoyo! Juntos podemos construir un recurso en beneficio de todos y cada
uno de los desarrolladores en Ruby ahí afuera.

Por cierto, si estás trabajando con Rails tal vez te gustaría revisar la [Guía
de estilo para Ruby on Rails 3](https://github.com/bbatsov/rails-style-guide) como complemento.

# La guía de estilo de Ruby

Ésta guía de estilo de Ruby recomienda las mejores practicas para que
programadores de Ruby del mundo real puedan escribir código que pueda ser
mantenido por otros programadores de Ruby en el mundo real. Una guía de estilo
que refleja los usos en el mundo real, es utilizada, y una guía de estilo que se
apega a un ideal que ha sido rechazado por las personas a las que se supone
debería ayudar, se arriesga a quedar en el olvido -  sin importar que tan buena
es.

La guía está dividida en varias secciones que agrupan convenciones relacionadas.
He intentado agregar el razonamiento detrás de cada una de ellas (a menos que
sea muy obvio, en cuyo caso las he omitido).

No saque las convenciones de la nada - la mayoría están basadas en mi extensa
carrera como ingeniero de software profesional, retroalimentación y sugerencias
de miembros de la comunidad entorno a Ruby, así como varias fuentes que son
altamente estimadas como recursos para la programación en Ruby, tales como
["Programming Ruby 1.9"](http://pragprog.com/book/ruby3/programming-ruby-1-9) y

La guía aun es un trabajo en progreso - algunas convenciones no tienen ejemplos
en código, otras no tienen ejemplos que lo ilustren con suficiente claridad.
Mientras esos detalles son arreglados - sólo mantenlos en mente por ahora.

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

* Usa terminadores de líneas con estilo Unix (LF). Los usuarios de BSD, Solaris,
  Linux y OSX no deben de tener problemas, pero los usuarios de Windows deben de
  tener cuidado.)
    * Si usas Git tal vez quisieras agregar la siguiente opción de configuración
      para proteger tu proyecto de terminaciones de línea estilo Windows (CR+LF)
      que pudieran colarse.

      ```$ git config --global core.autocrlf true```

* Usa espacios alrededor de operadores (`+`, `-`, `*`, `/`), después de las comas,
  los `:`, los `;`, alrededor de `{` y antes de `}`. El espacio en blanco puede
  ser (en su mayoría) irrelevante, pero su uso apropiado es muy importante para
  escribir código que se lea fácilmente.

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

* Identa y alinea los parámetros de la llamada a un método si se extienden en
  varias lineas.

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

* Usa RDoc y sus convenciones para documentar las APIs. No coloques una linea
  vacía entre un comentario y una definición.
* Mantén las lineas en un tamaño de 80 caracteres o menos.
* Evita el uso innecesario de espacios en blanco al final de las lineas.

## Sintaxis

* Prefiere el uso de paréntesis en la definición de un método cuando éste
  reciba argumentos.

  ```Ruby
  def some_method_with_arguments(arg1, arg2)
    # cuerpo omitido
  end

  def some_method
    # cuerpo omitido
  end
  ```

* Evita el uso del ciclo `for` a menos que tengas una buena razón. A diferencia
  de los iteradores, las variables definidas dentro de `for` son visibles fuera
  de él.

  ```Ruby
  arr = [1, 2, 3]

  # no recomendado
  for elem in arr do
    puts elem
  end

  # recomendado
  arr.each { |elem| puts elem }
  ```

* Las expresiones `if x: ...`, `if x; ...`, `when x: ...` y `when x; ...` fueron
  removidas en Ruby 1.9, por lo tanto...

    * Utiliza el operador ternario en vez de `if x: ...`

      ```Ruby
      # no recomendado
      result = if some_condition: something else something_else end

      # recomendado
      result = some_condition ? something : something_else
      ```

    * Utiliza la expresión alternativa `when x then ...` en vez de `when x: ...`
      en instrucciones de una línea.

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

* Si utilizas el operador ternario, procura sólo tener una expresión por rama.
  Esto también quiere decir que debe evitarse el uso de anidaciones dentro de
  los operadores ternarios. Tampoco lo expandas sobre varias lineas. Es
  preferible utilizar construcciones `ìf/else` en esos casos.

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

* Procura utilizar `&&/||` para expresiones booleanas, y `and/or` para flujo de
  control. (Regla del pulgar: Si tienes que usar paréntesis, estás utilizando
  los operadores equivocados).

  ```Ruby
  # expresión booleana
  if some_condition && some_other_condition
    do_something
  end

  # flujo de control
  document.saved? or document.save!
  ```

* Favorece el uso de los modificadores `if/unless` cuando tengas un cuerpo de
  una sola línea. Otra buena alternativa es el uso de `and/or`.

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

* Favorece el uso de `unless` sobre `if` para condiciones negativas (o utiliza
  `or`).

  ```Ruby
  # no recomendado
  do_something if !some_condition

  # recomendado
  do_something unless some_condition

  # otra buena opción
  some_condition or do_something
  ```

* Procura no usar `unless` con `else`. Reescribe la instrucción con el caso
  positivo primero.

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

* Si la condición de un `if/unless/while` es sencilla, se lee más claro si no se
  rodea con paréntesis, a menos que la condición contenga una asignación.

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

* Favorece el uso de los modificadores `while/until` cuando tengas un cuerpo de
  una sola línea.

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

* Es mejor omitir los paréntesis alrededor de parámetros en métodos que
  pertenecen un DSL interno (como Rake, Rails, RSpec...), métodos que tienen un
  estado de "palabra clave" en Ruby (como attr_reader, puts...) y métodos de
  acceso a atributos. De no ser así, es mejor rodear los parámetros del método
  con paréntesis.

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

* Si el primer argumento de un método comienza con un paréntesis abierto,
  usa otro juego de paréntesis para invocar el método. Por ejemplo, escribe
  `f((3 + 2) + 1)`.

* Procura no dejar espacios entre el nombre del método y el paréntesis de
  apertura.

  ```Ruby
  # no recomendado
  f (3 + 2) + 1

  # recomendado
  f(3 + 2) + 1
  ```

* Prefiere `{...}` para bloques de una sola línea y expresiones encadenadas. Es
  es mejor usar `do...end` para bloques de varias líneas, flujo de control y
  definiciones de métodos. (Evita usar expresiones encadenadas en varias líneas.
  Pregúntate si no pueden ser separadas en métodos de una forma elegante).

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

* Es importante que evites hacer sombra a los métodos con variables locales a
  menos que sean equivalentes.

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

* Mejora la legibilidad al dejar espacios alrededor del operador de asignación
  en los valores por defecto para los parámetros de un método.

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

* Evita las continuaciones de linea (\\) cuando no son requeridas. Es mejor si
  estructuras tu código para evitar su uso de ser posible.

  ```Ruby
  # no recomendado
  result = 1 - \
           2
  ```

* Usar el valor retornado de una asignación esta bien, pero rodea la asignación
  con paréntesis.

  ```Ruby
  # no recomendado
  if v = array.grep(/foo/) ...

  # bien - muestra la intención de uso de la asignación
  if (v = array.grep(/foo/)) ...

  # bien - muestra el uso de la asignación y tiene una precedencia correcta
  if (v = self.next_value) == 'hello' ...
  ```

* Está bien utilizar `||=` para inicializar variables, excepto si se trata de
  variables booleanas. (Considera lo que puede pasar si el valor es `false`).

  ```Ruby
  # asina Bozhidar como el nombre, sólo si es nulo o falso
  name ||= 'Bozhidar'

  # no recomendado - sería asignado verdadero aun si es falso
  enabled ||= true

  # recomendado
  enabled = true if enabled.nil?
  ```

* Utiliza la sintaxis literal de Ruby 1.9 para los hashes. (A menos que estés
  trabajando con una versión que no la soporte, claro). De igual forma, utiliza
  la nueva sintaxis literal para lambda.

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

* Evita utilizar las variables especiales estilo Perl (como `$0-9`, `$``, etc).
  Son bastante crípticas y su uso fuera de scripts de una sola línea no es bien
  visto.

* Para recordar algunas de las recomendaciones sobre la sintaxis del código,
  ejecuta el interprete con la opción `-w`. Así generara advertencias cuando
  encuentre alguna practica no recomendada.

## Identificadores

> Las únicas dificultades reales al programar son la validación del cache y el
> nombrar cosas. <br/>
> -- Phil Karlton

* Utiliza `snake_case` para métodos y variables.
* Utiliza `CamelCase` para clases y módulos. (Deja en mayúsculas los acrónimos
  como HTTP, RFC y XML).
* Utiliza `SCREAMING_SNAKE_CASE` para otras constantes.
* Los nombres de métodos predicados (métodos que devuelven un valor booleano)
  deben terminar con un signo de interrogación: `?`
  (por ejemplo `Array#empty?`).
* Los nombres de métodos potencialmente peligrosos (por ejemplo, métodos que
  modifican los argumentos o `self`, que no ejecutan los terminadores como lo
  hace `exit`, etc.) deben terminar con una marca de exclamación `!` si existe
  una versión segura de ese método *peligroso*.

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

* Cuando uses `reduce` en bloques cortos, nombra los argumentos como `|a, e|`
  (acumulador, elemento).
* Cuando definas operadores binarios, nombra el argumento como `other`.

  ```Ruby
  def +(other)
    # cuerpo omitido
  end
  ```

* Prefiere `map` sobre `collect`, `find` sobre `detect`, `select` sobre `find_all`,
  `reduce` sobre `inject` y `size` sobre `length`. Escribirás menos. :-)

## Comments

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell

* Write self-documenting code and ignore the rest of this section. Seriously!
* Comments longer than a word are capitalized and use punctuation. Use [one
  space](http://en.wikipedia.org/wiki/Sentence_spacing) after periods.
* Avoid superfluous comments.

    ```Ruby
    # bad
    counter += 1 # increments counter by one
    ```

* Keep existing comments up-to-date. An outdated is worse than no comment
at all.

> Good code is like a good joke - it needs no explanation. <br/>
> -- Russ Olsen

* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Do or do not - there is no try. --Yoda)

## Annotations

* Annotations should usually be written on the line immediately above
  the relevant code.
* The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
* If multiple lines are required to describe the problem, subsequent
  lines should be indented two spaces after the `#`.

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

* In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Use `TODO` to note missing features or functionality that should be
  added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
* Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`
* Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.

## Classes

* When designing class hierarchies make sure that they conform to the
  [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle).
* Try to make your classes as
  [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))
  as possible.
* Always supply a proper `to_s` method for classes that represent
  domain objects.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* Use the `attr` family of functions to define trivial accessors or
mutators.

    ```Ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```
* Consider using `Struct.new`, which defines the trivial accessors,
constructor and comparison operators for you.

    ```Ruby
    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    class Person < Struct.new(:first_name, :last_name)
    end
    ````

* Consider adding factory methods to provide additional sensible ways
to create instances of a particular class.

    ```Ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

* Prefer [duck-typing](http://en.wikipedia.org/wiki/Duck_typing) over inheritance.

    ```Ruby
    # bad
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # good
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* Avoid the usage of class (`@@`) variables due to their "nasty" behavior
in inheritance.

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    As you can see all the classes in a class hierarchy actually share one
    class variable. Class instance variables should usually be preferred
    over class variables.

* Assign proper visibility levels to methods (`private`, `protected`)
in accordance with their intended usage. Don't go off leaving
everything `public` (which is the default). After all we're coding
in *Ruby* now, not in *Python*.
* Indent the `public`, `protected`, and `private` methods as much the
  method definitions they apply to. Leave one blank line above them.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private
      def private_method
        # ...
      end
    end

* Use `def self.method` to define singleton methods. This makes the methods
  more resistant to refactoring changes.

    ```Ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many singleton methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

## Exceptions

* Signal exceptions using the `fail` keyword. Use `raise` only when
  catching an exception and re-raising it (because here you're not failing, but explicitly and purposefully raising an exception).

    ```Ruby
    begin
      fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* Never return from an `ensure` block. If you explicitly return from a
  method inside an `ensure` block, the return will take precedence over
  any exception being raised, and the method will return as if no
  exception had been raised at all. In effect, the exception will be
  silently thrown away.

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Use *implicit begin blocks* when possible.

    ```Ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* Mitigate the proliferation of `begin` blocks via the use of
  *contingency methods* (a term coined by Avdi Grimm).

    ```Ruby
    # bad
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # good
    def with_io_error_handling
       yield
    rescue
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* Don't suppress exceptions.

    ```Ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # bad
    do_something rescue nil
    ```

* Don't use exceptions for flow of control.

    ```Ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # good
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* Put more specific exceptions higher up the rescue chain, otherwise
  they'll never be rescued from.

    ```Ruby
    # bad
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # good
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

* Release external resources obtained by your program in an ensure
block.

    ```Ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

* Favor the use of exceptions for the standard library over
introducing new exception classes.

## Collections

* Prefer literal array and hash creation notation (unless you need to
pass parameters to their constructors, that is).

    ```Ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* Prefer `%w` to the literal array syntax when you need an array of
strings.

    ```Ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

* Avoid the creation of huge gaps in arrays.

    ```Ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

* Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.
* Use symbols instead of strings as hash keys.

    ```Ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Avoid the use of mutable object as hash keys.
* Use the new 1.9 literal hash syntax in preference to the hashrocket
syntax.

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Rely on the fact that hashes in 1.9 are ordered.
* Never modify a collection while traversing it.

## Strings

* Prefer string interpolation instead of string concatenation:

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Prefer single-quoted strings when you don't need string interpolation or
  special symbols such as `\t`, `\n`, `'`, etc.

    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

* Don't use `{}` around instance variables being interpolated into a
  string.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad
      def to_s
        "#{@first_name} #{@last_name}"
      end

      # good
      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## Regular Expressions

* Don't use regular expressions if you just need plain text search in string:
  `string['text']`
* For simple constructions you can use regexp directly through string index.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Use non capturing groups when you don't use captured result of parenthesis.

    ```Ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* Avoid using $1-9 as it can be hard to track what they contain. Named groups
  can be used instead.

    ```Ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* Character classes have only few special characters you should care about:
  `^`, `-`, `\`, `]`, so don't escape `.` or brackets in `[]`.

* Be careful with `^` and `$` as they match start/end of line, not string endings.
  If you want to match the whole string use: `\A` and `\z` (not to be
  confused with `\Z` which is the equivalent of `/\n?\z/`).

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```

* Use `x` modifier for complex regexps. This makes them more readable and you
  can add some useful comments. Just be careful as spaces are ignored.

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

* For complex replacements `sub`/`gsub` can be used with block or hash.

## Percent Literals

* Use `%w` freely.

    ```Ruby
    STATES = %w(draft open closed)
    ```

* Use `%()` for single-line strings which require both interpolation
  and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```Ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

* Use `%r` only for regular expressions matching *more than* one '/' character.

    ```Ruby
    # bad
    %r(\s+)

    # still bad
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # good
    %r(^/blog/2011/(.*)$)
    ```

* Avoid `%q`, `%Q`, `%x`, `%s`, and `%W`.

* Prefer `()` as delimiters for all `%` literals.

## Metaprogramming

* Do not mess around in core classes when writing libraries. (Do not monkey
patch them.)

* The block form of `class_eval` is preferable to the string-interpolated form.
  - when you use the string-interpolated form, always supply `__FILE__` and `__LINE__`, so that your backtraces make sense:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` is preferable to `class_eval{ def ... }`

* When using `class_eval` (or other `eval`) with string interpolation, add a comment block showing its appearance if interpolated (a practice I learned from the rails code):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* avoid using `method_missing` for metaprogramming. Backtraces become messy; the behavior is not listed in `#methods`; misspelled method calls might silently work (`nukes.launch_state = false`). Consider using delegation, proxy, or `define_method` instead.  If you must, use `method_missing`,
  - be sure to [also define `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - only catch methods with a well-defined prefix, such as `find_by_*` -- make your code as assertive as possible.
  - call `super` at the end of your statement
  - delegate to assertive, non-magical methods:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## Misc

* Write `ruby -w` safe code.
* Avoid hashes as optional parameters. Does the method do too much?
* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.
* If you really have to, add "global" methods to Kernel and make them private.
* Use class instance variables instead of global variables.

    ```Ruby
    #bad
    $foo_bar = 1

    #good
    class Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* Avoid `alias` when `alias_method` will do.
* Use `OptionParser` for parsing complex command line options and
`ruby -s` for trivial command line options.
* Code in a functional way, avoiding mutation when that makes sense.
* Avoid needless metaprogramming.
* Do not mutate arguments unless that is the purpose of the method.
* Avoid more than three levels of block nesting.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.

# Contributing

Nothing written in this guide is set in stone. It's my desire to work
together with everyone interested in Ruby coding style, so that we could
ultimately create a resource that will be beneficial to the entire Ruby
community.

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

# Spread the Word

A community-driven style guide is of little use to a community that
doesn't know about its existence. Tweet about the guide, share it with
your friends and colleagues. Every comment, suggestion or opinion we
get makes the guide just a little bit better. And we want to have the
best possible guide, don't we?

Documento basado en ("The Ruby Style Guide"(https://github.com/bbatsov/ruby-style-guide)),
escrito originalmente por Bozhidar Batsov.
