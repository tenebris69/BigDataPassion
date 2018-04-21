+++
author = "Radosław Szmit"
categories = ["Java", "Wyzwania"]
date = "2018-04-21"
description = ""
featured = "java-kodolamacz.jpg"
featuredalt = ""
featuredpath = "/img/java"
linktitle = ""
title = "Wyzwanie: Podstawy języka Java"
type = "post"

+++

W poprzednim wyzwaniu przygotowaliśmy sobie środowisko do pracy i stworzyliśmy swój pierwszy program. Dzisiaj chcemy podnieść poprzeczkę i poznać podstawowe instrukcje wykorzystywane w języku Java!

Przed nami takie rzeczy jak:

* Zmienne i stałe
* Typy danych
* Operatory
* Instrukcje sterujące
* Pętle

Zaczynamy!

# Zmienne

W poprzednim programie wczytywaliśmy od użytkownika jego imię i nazwisko, a następnie wyświetlaliśmy te informacje. Nasz program wyglądał mniej więcej tak:
~~~java
package pl.kodolamacz;

import java.util.Scanner;

public class MyFirstJavaApplication {

    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);

        System.out.println("Proszę podaj swoje imię:");
        String forename = scanner.nextLine();

        System.out.println("Proszę podaj swoje nazwisko:");
        String surname = scanner.nextLine();

        scanner.close();

        System.out.println("Witaj " + forename + " " + surname);
    }

}
~~~

Aby taki program mógł zadziałać, musieliśmy w jakiś sposób zapamiętać sobie naszą wartość, czyli imię i nazwisko użytkownika. Potrzebujemy do tego czegoś w rodzaju "pudełka" na dane, gdzie będziemy mogli w każdej chwili coś włożyć, a potem pobrać i w razie potrzeby włożyć coś innego. Takie "pudełka" na dane w językach programowania nazywamy **zmiennymi** (ang. variable).

W naszym kodzie z wyzwania stworzyliśmy dwie zmienne, *forename* i *surname*:
~~~java
String forename = scanner.nextLine();
String surname = scanner.nextLine();
~~~

Nazwa zmiennej musi się zaczynać literą oraz może składać się z liter i cyfr. Za "literę" w Javie traktujemy dowolny znak Unicode (https://pl.wikipedia.org/wiki/Unikod), w tym także polskie znaki potocznie nazywane literami z ogonkami jak np. "ą". Przyjęto jednak zwyczajowo, że nasze **programy piszemy w języku angielskim**, ułatwia to współpracę z programistami z całego świata oraz unikamy problemów z kodowaniem.

W nazwach zmiennych możemy używać zarówno dużych i małych liter, trzeba jednak pamiętać, że wielkość liter ma znaczenie i zmienna *forename*, *Forename* czy *foreName* to zupełnie inne zmienne. Przyjęto, że w Javie stosujemy notację **camelCase** (https://pl.wikipedia.org/wiki/CamelCase), czyli nazwy zmiennych zaczynamy zawsze małą literą, zaś w przypadku nazw będących złożeniem kilku słów, każde kolejne słowo zaczyna się dużą literą, np. *myDogName*.

Dodatkowo nie można stosować w nazwach symboli taki jak "+", spacji oraz słów zarezerwowanych (https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html).

Więcej o "zwyczajach" stosowanych w języku Java, czyli tak zwanych *konwencjach*, możemy przeczytać tutaj: http://www.oracle.com/technetwork/java/codeconventions-150003.pdf. Oczywiście nie ma obowiązku ich przestrzegania, jest to jednak mocno zalecane i ułatwia współpracę z innymi programistami. Zdarza się także, że różne firmy tworzą własne reguły pisania kodu, gdybyśmy chcieli zobaczyć jak robi to firma Google wystarczy zajrzeć na stronę: https://google.github.io/styleguide/javaguide.html.

# Typy danych

Java jest językiem o **ścisłej kontroli typów** (https://pl.wikipedia.org/wiki/Typowanie_statyczne). Oznacza to, że każda zmienna ma z góry określony typ, czyli informację o tym co można do takiej zmiennej zapisać. Typy dzielimy na proste (ang. primitive) oraz złożone, czyli obiekty o których powiemy sobie więcej w kolejnych wyzwaniach.

Do typów prostych zaliczamy:

* byte - przechowuje liczby całkowite z zakresu od -128 do 127
* short - przechowuje liczby całkowite z zakresu od -32 768 do 32 767
* int - przechowuje liczby całkowite z zakresu -2<sup>31</sup> do 2<sup>31</sup>-1
* long - przechowuje liczby całkowite z zakresu -2<sup>63</sup> do 2<sup>63</sup>-1
* float - przechowuje liczby zmiennoprzecinkowe 32 bitowe zgodne ze standardem IEEE 754
* double - przechowuje liczby zmiennoprzecinkowe 64 bitowe zgodne ze standardem IEEE 754
* boolean - przechowuje wartość logiczną (prawda / fałsz, ang. true / false)
* char - przechowuje znak Unicode (16-bit, 0-65,535)

Więcej informacji o typach możemy znaleźć pod adresem: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

## Deklaracja

Zmienne w języku Java trzeba *deklarować*, czyli podać typ oraz nazwę zmiennej, np:
~~~java
double money;
int days;
boolean success;
~~~

Możemy też zadeklarować kilka zmiennych razem, gdy są tego samego typu:
~~~java
int days, months;
~~~
ale nie jest to zalecane, gdyż utrudnia czytanie programów.

## Inicjalizacja

Zmiennych które są tylko zadeklarowane nie możemy użyć, dlatego trzeba najpierw je *zainicjalizować*, czyli przypisać im jakąś wartość początkową:
~~~java
days = 5;
money = 50.6;
~~~

Najlepiej jest jednak deklarować zmienne i je inicjalizować w tej samej linijce jak poniżej:
~~~java
int days = 5;
double money = 50.6;
~~~
Jeśli nie jest to możliwe, należy deklarację napisać jak najbliżej w kodzie miejsca inicjalizacji, kod będzie łatwiejszy do analizowania.

W powyższym przykładzie powiedzieliśmy kompilatorowi, że nasza zmienna *days* ma mieć na początku wartość równą 5, zaś *money* wartość równą 50.6. Liczbę 5 oraz 50.6 w tym przypadku nazywamy *literałem* (ang. literal), czyli konkretną wartością zapisaną w naszym kodzie. Więcej przykładów takich literałów możemy znaleźć w dokumentacji na stronie: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

# Stałe

Wartość przechowywaną w zmiennej w każdej chwili możemy zmienić na inną (stąd też nazwa zmienna). Jednak czasami potrzebujemy powiedzieć kompilatorowi oraz innym programistom, że coś ma być "stałe". Wtedy warto skorzystać ze słówka *final*.

Poniższy kod jest prawidłowy:
~~~java
int days = 5;
days = 10;
~~~

zaś poniższy już nie:
~~~java
final int days = 5;
days = 10;
~~~

# Operatory

Na zmiennych możemy wykonywać różne operacje, np. arytmetyczne. Robimy to za pomocą operatorów.

Podstawowymi operatorami są znaki "+", "-", "*" oraz "/" oznaczające operację odpowiednio dodawania, odejmowania, mnożenia oraz dzielenia. Także znak "=" jest tak zwanym operatorem przypisania. Poniżej przykład użycia tych operatorów:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {

        double x = 10;
        double y = 2;

        double result = x + y;
        System.out.println(result);

        result = x - y;
        System.out.println(result);

        result = x * y;
        System.out.println(result);

        result = x / y;
        System.out.println(result);
    }

}
~~~

Czasami chcemy zwiększyć wartość naszej zmiennej bez konieczności zapisywania wyniku w innej zmiennej jak poniżej:
~~~java
int x = 2;
int y = x + 5;
~~~

Wolimy wtedy napisać:
~~~java
int x = 2;
x = x + 5;
~~~

Można to jednak jeszcze bardziej uprościć używając operatorów "+=", "-=", "*=", "/=". Dzięki nim zamiast pisać:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {

        double x = 10;
        double y = 2;

        x = x + y;
        System.out.println(x);

        x = x - y;
        System.out.println(x);

        x = x * y;
        System.out.println(x);

        x = x / y;
        System.out.println(x);
    }

}
~~~
możemy napisać zwięźlej:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {

        double x = 10;
        double y = 2;

        x += y;
        System.out.println(x);

        x -= y;
        System.out.println(x);

        x *= y;
        System.out.println(x);

        x /= y;
        System.out.println(x);
    }

}
~~~
Zauważmy, że w obydwu przypadkach przy każdym działaniu zmieniamy wartość zmiennej "x", dlatego wyniki będą inne, niż gdy korzystaliśmy ze zmiennej "result".

Innymi przydatnymi i często używanymi operatorami są operatory inkrementacji "++" i dekrementacji "--" czyli zwiększenia i zmniejszenia zmiennej o wartość równą jeden. Poniżej krótki przykład użycia tych operatorów:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {

        double x = 10;
        System.out.println(x);

        x++;
        System.out.println(x);

        x--;
        System.out.println(x);
        
    }

}
~~~

Możemy także zapisać je tak:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        double x = 10;
        System.out.println(x);
        System.out.println(x++);
        System.out.println(x--);
        System.out.println(x);
    }

}
~~~
Niestety po uruchomieniu powyższego programu wyniki które zobaczymy w terminalu mogą się różnić od tych, których byśmy się spodziewali. Dzieje się tak dlatego, że nasze zmienne zostaną najpierw przekazane do funkcji *println* i wyświetlone, a dopiero potem zostaną zastosowane operatory "++" i "--". Gdybyśmy chcieli najpierw wykonać operacje inkrementacji i dekrementacji powinniśmy użyć tych operatorów z lewej strony zmiennej, co będzie oznaczało dla kompilatora "najpierw zwiększ, potem wyślij do funkcji println". Poniżej przykład:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        double x = 10;
        System.out.println(x);
        System.out.println(++x);
        System.out.println(--x);
        System.out.println(x);
    }

}
~~~

Ostatnimi podstawowymi i często używanymi operatorami są operator równości "==", operator nierówności "!=" i dwa cztery operatory większości ">", "<", ">=", "<=", pozwalające sprawdzić czy wartości dwóch zmiennych są równe lub nie, oraz która wartość jest większa. Operatory te zwracają prawdę (true) lub fałsz (false) czyli zmienną typu boolean. Poniżej przykład użycia tych operatorów:

~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        double x = 10;
        double y = 5;

        System.out.println(x == y); // czy x jest równe y
        System.out.println(x != y); // czy x jest różne od y

        System.out.println(x > y); // czy x jest większe od y
        System.out.println(x >= y); // czy x jest większe lub równe y

        System.out.println(x < y); // czy x jest mniejsze od y
        System.out.println(x <= y); // czy x jest mniejsze lub równe y
    }

}
~~~
Operatory te są używane przede wszystkim w instrukcjach sterujących (patrz dalej).

Więcej operatorów możemy znaleźć pod adresem: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html. Zamieszczona tam tabela nie tylko pokazuje wszystkie dostępne operatory w języku Java, ale także odzwierciedla kolejność wykonywania operacji gdy w jednej linijce kodu użyjemy kilku operatorów, podobnie jak w szkole podstawowej na lekcjach matematyki słuchaliśmy o kolejności wykonywania działań matematycznych. Tak samo jak na matematyce, w Javie także można się wspomagać nawiasami, gdy domyślna kolejność nam nie odpowiada.

# Instrukcje warunkowe

W Javie, podobnie jak w większości językach programowania, wykonaniem naszego programu sterujemy instrukcjami warunkowymi. Do budowania tych instrukcji korzystamy ze słówka kluczowego **if** oraz **else**.

W najprostszej postaci instrukcja warunkowa wygląda tak:
~~~java
if (warunek) polecenie;
~~~

Jeśli chcemy dla danego warunku wykonać kilka instrukcji musimy użyć nawiasów {}:
~~~java
if (warunek) {
    instrukcja 1;
    instrukcja 2;
}
~~~

Gotowy kod Java może więc wyglądać tak:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        double x = 10;
        double y = 5;

        if(x > y){
            System.out.println("x jest większe od y");
        }
    }

}
~~~
Funkcja *println* zostanie wykonana tylko i wyłącznie wtedy gdy wartość zmiennej x będzie większa od y (co w tym przypadku jest prawdą).

Jeśli chcemy wykonać jakąś akcję gdy warunek nie zostanie spełniony możemy użyć słowa kluczowego **else**:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        double x = 10;
        double y = 5;

        if(x > y){
            System.out.println("x jest większe od y");
        } else {
            System.out.println("x nie jest większe od y");
        }
    }

}
~~~

Instrukcję **if** można łączyć ze sobą robiąc kaskadę warunków:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        double x = 10;
        double y = 5;

        if(x > y){
            System.out.println("x jest większe od y");
        } else if (x < y) {
            System.out.println("x jest mniejsze od y");
        } else {
            System.out.println("x jest równe y");
        }
    }

}
~~~


# Legenda
* https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html
* http://www.oracle.com/technetwork/java/codeconventions-150003.pdf
* https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html
* https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html


TODO:
- pętle
- warunki
- String
- rzutowanie
- %
- komentarze