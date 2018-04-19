+++
author = "Radosław Szmit"
categories = ["Java", "Wyzwania"]
date = "2018-04-20"
description = ""
featured = "java-kodolamacz.jpg"
featuredalt = ""
featuredpath = "/img/java"
linktitle = ""
title = "Wyzwanie programistyczne nr 1: Zacznijmy programować w Javie! - Rozwiązanie"
type = "post"

+++

W poprzednim wyzwaniu poprosiliśmy Was o napisanie programu, który wyświetla prośbę o podanie imienia i następnie nazwiska, pobiera te dwa napisy od użytkownika, a następnie wyświetla obydwa jak w przykładzie poniżej:
~~~shell
Proszę podaj swoje imię:
Jan
Proszę podaj swoje nazwisko:
Kowalski
Witaj Jan Kowalski
~~~

W poście na FB z rozwiązaniami zamieściliście już kilkadziesiąt rozwiązań. Wierzymy, że osób które przystąpiły do wyzwania jest jeszcze więcej. Tych którzy jeszcze nie zmierzyli się z naszym zadaniem zachęcamy by to jeszcze zrobili w ten weekend!

Wszystkie rozwiązania były prawidłowe, choć kod od siebie się różnił, co pokazuje pewną wspaniałą rzecz w programowaniu, często ten sam problem możemy rozwiązać na wiele różnych sposobów i wszystkie będą prawidłowe!

Poniżej przykładowe i zarazem najpopularniejsze rozwiązanie wyzwania:
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

Kolejne wyzwanie w poniedziałek 23 kwietnia o 10:00. Podwyższamy poprzeczkę i wprowadzimy sporo nowych rzeczy związanych z językiem Java, w tym nowości z JDK 10! Zaplanujcie sobie trochę wolnego czasu na przyszły tydzień na nasze wyzwanie :)
