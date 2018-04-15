+++
author = "Radosław Szmit"
categories = ["Java", "Wyzwania"]
date = "2018-04-12"
description = ""
featured = "java-kodolamacz.jpg"
featuredalt = ""
featuredpath = "/img/java"
linktitle = ""
title = "Wyzwanie programistyczne nr 1: Zacznijmy programować w Javie!"
type = "post"

+++

Jak już wiecie, chcemy Was zachęcić i zmotywować do nauki programowania! Do rozpoczęcia tej przygody potrzebny będzie nam komputer, dostęp do internetu i dużo, dużo chęci i wytrwałości :) 

Naszym pierwszym wyzwaniem będzie **napisanie swojego pierwszego programu w Javie!**

# Przygotujmy się

Zanim zaczniemy cokolwiek programować musimy przygotować nasz komputer do pracy. Będziemy potrzebować na początek **wirtualnej maszyny Java** (ang. Java Virtual Machine, w skrócie JVM) oraz środowiska dla programistów, tak zwane **zintegrowane środowisko programistyczne** (ang. integrated development environment, w skrócie IDE).



### Wirtualna Maszyna Java

Java to jeden z najpopularniejszych języków programowania na świecie. Jednak  Java, to nie tylko język programowania, to także cały ekosystem technologii i języków programowania zorientowanych wokół tak zwanej wirtualnej maszyny Java, czyli środowiska uruchomieniowego, które musimy zainstalować w naszym systemie operacyjnym (np. Windows, Linux, macOS) lub innym dowolnym urządzeniu, na którym chcemy wykonywać nasze programy. Taką maszynę wirtualną znajdziemy na większości komputerach na świecie, telefonach z systemem Android czy nawet na odtwarzaczach Blu-ray. Według strony https://go.java/ obecnie już około 15 miliardów urządzeń korzysta z Javy.

Dlatego zanim zaczniemy cokolwiek robić w Javie, potrzebujemy zainstalować sobie wirtualną maszynę Java, która jest udostępniana w dwóch wersjach; Java Runtime Environment (JRE) zawierająca podstawową instalację maszyny wirtualnej wraz z dodatkowymi bibliotekami i komponentami, służącą przede wszystkim do uruchamiania programów oraz Java Development Kit (JDK) zawierająca wszystko co ma JRE plus dodatkowe narzędzia dla programistów. My oczywiście będziemy potrzebować JDK, które możemy ściągnąć ze strony: http://www.oracle.com/technetwork/java/javase/downloads/index.html



Poniżej proces instalacji jdk wersji 10 na systemie Windows:

![](/img/java/jdk-setup.png)
![](/img/java/jdk-setup2.png)
![](/img/java/jdk-setup3.png)
![](/img/java/jdk-setup4.png)
![](/img/java/jdk-setup5.png)
![](/img/java/jdk-setup6.png)



### Zintegrowane środowisko programistyczne

Programowanie w skrócie to tak naprawdę pisanie odpowiednich poleceń w wybranym języku programowania które to polecenia są następnie tłumaczone na instrukcje rozumiane przez komputer, tak zwany kod maszynowy. Tak naprawdę nasz kod, czyli zestaw tych poleceń, możemy pisać w dowolnym narzędziu, począwszy od notatnika. Takie pisanie niestety wymaga od nas dużo większego wysiłku, dlatego z pomocą przychodzą właśnie zintegrowane środowiska programistyczne, które zawierają szereg ułatwień jak podpowiadanie składki, sprawdzanie poprawności naszego programu czy łatwiejsze testowanie.

Najpopularniejszymi IDE dla Javy są:

* IntelliJ IDEA https://www.jetbrains.com/idea/
* Eclipse http://www.eclipse.org
* NetBeans https://netbeans.org

To które środowisko wybierzemy zależy tak naprawdę od nas, czyli naszych predyspozycji. Z naszej strony polecamy wybranie IntelliJ IDEA i z tego narzędzia będziemy korzystali w przykładach. Oczywiście wystarczy nam darmowa wersja Community.



Poniżej proces instalacji IntelliJ IDEA na systemie Windows:

![](/img/java/idea-install.png)
![](/img/java/idea-install2.png)
![](/img/java/idea-install3.png)
![](/img/java/idea-install4.png)
![](/img/java/idea-install5.png)
![](/img/java/idea-install6.png)




# Pierwszy program

Mając już JDK i wybrane IDE możemy przystąpić do napisania swojego pierwszego programu w języku Java. 

Po pierwszym uruchomieniu IDEI powinniśmy zobaczyć okno z wyborem kolorystyki. Wybór według własnych preferencji.

![](/img/java/idea-start2.png)

W następnym okienku możemy wybrać różne ustawienia i dodatki - zostawmy te domyślne.

![](/img/java/idea-start3.png)

Tutaj również zostawiamy ustawienia domyślne i klikamy "Start using IntelliJ IDEA".

![](/img/java/idea-start4.png)

Pojawia nam się okno startowe:

![](/img/java/idea-start.png)

Wybieramy opcję "Create New Project".

W nowo otwartym oknie musimy wskazać IDEI lokalizację naszego SDK, klikamy "New" i wybieramy miejsce instalacji SDK w naszym systemie (w przypadku systemu Windows będzie to C:\Program Files\Java\jdk-10). Następnie klikamy dwukrotnie "Next".

![](/img/java/idea-new-project.png)

W kolejnym oknie musimy wpisać nazwę naszego projektu, podajemy "kodolamacz-first-challenge" i klikamy "Finish".

![](/img/java/idea-project-name.png)

Otworzyło nam się główne okno IDEI z naszym projektem.

![](/img/java/idea-new-project-created.png)

To jednak tylko pusty projekt. Teraz musimy napisać swój pierwszy kawałek kodu. W tym celu rozwijamy drzewo z lewej strony z nazwą naszego projektu i na katalogu "src" klikamy prawym przyciskiem myszy i wybieramy opcję "New" a następnie "Java Class". W nowo otwartym oknie jako nazwę wpisujemy "pl.kodolamacz.MyFirstJavaApplication" i klikamy "Ok"

![](/img/java/idea-new-class.png)

W rezultacie dostajemy poniższy kod:

~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {
}
~~~

Czym są klasy Java dowiemy się później, na razie potraktujmy je jako sposób organizacji kodu, czyli naszych poleceń, w świecie Java. Klasy będą się znajdować w plikach z rozszerzeniem *.java*.

W aktualnej wersji nasz kod jeszcze nic nie robi, to tylko szablon. Najprostrszą operacją jaką możemy kazać wykonać komputerowi jest wyświetlenie napisu. By tego dokonać należy najpierw dodać tak zwaną metodę **main** którą można uruchomić, czyli coś w rodzaju punktu startowego naszego programu. Nasz kod powinien wyglądać tak:

~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        
    }
    
}
~~~

Teraz możemy skorzystać z funkcji **println**:
~~~java
package pl.kodolamacz;

public class MyFirstJavaApplication {

    public static void main(String[] args) {
        System.out.println("Witaj świecie!");
    }

}
~~~

Funkcja **println** wyświetla dowolny ciąg znaków na tak zwanym standardowym wyjściu. Jest to jeden z możliwych sposobów komunikacji z naszym programem. Więcej o strumieniach możemy przeczytać na Wikipedii: https://pl.wikipedia.org/wiki/Standardowe_strumienie. Żeby skorzystać ze standardowego wyjścia musimy skorzystać z klasy **System** służącą między innymi do obsługi standardowych strumieni w Javie. W tej klasie możemy skorzystać z pola **out** gdzie mamy dostępną funkcję **println**. Temat pól w klasach (ang. field) także zostanie szczegółowo omówiony w kolejnych naszych wydarzeniach.

Teraz czas najwyższy by uruchomić nasz program! Klikamy w dowolnym miejscu naszego kodu prawym przyciskiem myszki i wybieramy opcję "Run 'MyFirstJavaApplication....main()'" lub naciskamy kombinację klawiszy Ctrl+Shift+F10. Na dole ekranu powinniśmy zobaczyć wynik wykonania naszego programu, czyli oczekiwany napis:
~~~shell
Witaj świecie!

Process finished with exit code 0
~~~

Jeśli napis się nie wyświetlił, trzeba sprawdzić czy wszystkie nawiasy i średniki są na swoim miejscu.

Gratulacje! Udało Ci się napisać i uruchomić swój pierwszy program w języku Java. Jest to tak zwany "Hello world" (https://pl.wikipedia.org/wiki/Hello_world) czyli program wyświetlający napis "Witaj świecie" i najczęściej właśnie od takiego programu zaczynamy naukę nowego lub nawet pierwszego języka programowania lub biblioteki.