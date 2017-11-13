+++
author = ""
categories = []
date = "2017-11-13T15:47:37+01:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "scala basic steps"
type = "post"

+++

# Podstawy

### Operacje matematyczne

~~~Scala
15 + 3 * 2 / 2 -7

"To jest przykładowy tekst"
~~~

## Zmienne i wartości

~~~Scala
var variable = 5
variable = 7
variable
~~~

~~~Scala
val value = 5
value = 7
value
~~~

## Komentarze

~~~Scala
// Some comment

/*
Other comment
*/
~~~

## Liczby
~~~Scala
val bigInt = BigInt("821735918723912749821723948572334280704")
bigInt + 1

val bigDecimal = BigDecimal("25.09102349571921928308213601287562173888012")
bigDecimal + 0.00000000000000000000000001

bigDecimal + bigInt.toDouble
bigDecimal + BigDecimal(bigInt)
~~~

# Operatory
~~~Scala
+ - * / %
~~~

~~~Scala
++ --
~~~

~~~Scala
+= i -= *= /=
~~~

# Importowanie klas
~~~Scala
import scala.math._
abs(-8)
cbrt(8)

ceil(8.12)
round(8.12)
round(8.89)
floor(8.89)

pow(2,2)
sqrt(4)

exp(0.6931471805599453)
log(2)
log10(10)
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~

~~~Scala
~~~






# Legenda
* https://www.scala-lang.org
* http://www.scala-lang.org/api/current/
* http://www.scala-lang.org/api/current/scala/math/BigDecimal.html
* http://www.scala-lang.org/api/current/scala/math/