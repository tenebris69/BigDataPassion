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

# Zmienne i ich typy

W poprzednim programie wczytywaliśmy od użytkownika jego imię i nazwisko a następnie wyświetlaliśmy te informacje. Nasz program wyglądał mniej więcej tak:
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

Żeby taki program mógł zadziałać, musieliśmy w jakiś sposób zapamiętać sobie naszą wartość, czyli imię i nazwisko użytkownika. Potrzebujemy do tego czegoś w rodzaju "pudełka" na dane, gdzie będziemy mogli w każdej chwili coś włożyć a potem pobrać i w razie potrzeby włożyć coś innego. Takie "pudełka" na dane w językach programowania nazywamy **zmiennymi**.

W naszym kodzie z wyzwania stworzyliśmy dwie zmienne, *forename* i *surname*:
~~~java
String forename = scanner.nextLine();
String surname = scanner.nextLine();
~~~

Nazwa zmiennej musi się zaczynać literą oraz składać się z liter i cyfr. Za "literę" w Javie traktujemy dowolny znak Unicode (https://pl.wikipedia.org/wiki/Unikod), w tym także polskie znaki potocznie nazywane literami z ogonkami jak np. "ą". Przyjęto jednak zwyczajowo, że nasze **programy piszemy w języku angielskim**, ułatwia to współpracę z programistami z całego świata oraz unikamy problemów z kodowaniem.

W nazwach zmiennych możemy używać zarówno dużych i małych liter, trzeba jednak pamiętać, że wielkość liter ma znaczenie i zmienna *forename*, *Forename* czy *foreName* to zupełnie inne zmienne. Przyjęto, że w Javie stosujemy notację **camelCase** (https://pl.wikipedia.org/wiki/CamelCase), czyli nazwy zmiennych zaczynamy zawsze małą literą, zaś w przypadku nazw będących złożeniem kilku słów, każde kolejne słowo zaczyna się dużą literą, np. *myDogName*.

Dodatkowo nie można stosować w nazwach symboli taki jak "+", spacji oraz słów zarezerwowanych (https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html).


