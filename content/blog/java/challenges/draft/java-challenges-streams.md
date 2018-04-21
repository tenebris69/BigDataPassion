+++
author = ""
categories = []
description = ""
linktitle = ""
featured = ""
featuredpath = ""
featuredalt = ""
type = "post"

draft=true
+++


Klasa Scanner komunikuje się z systemem operacyjnym by pobrać wpisany przez użytkownika tekst w terminalu, dlatego korzysta ze specjalnego strumienia systemowego i to właśnie ten strumień trzeba zamknąć. Więcej o strumieniach standardowych można poczytać trochę tutaj https://pl.wikipedia.org/wiki/Standardowe_strumienie. Strumieniom w Javie i ich obsłudze poświęcimy oddzielne wyzwanie, bo jest to dość szeroki temat, trochę zbyt zaawansowany by o tym wspominać na tym etapie.

W artykule o świecie Javy http://kodolamacz.pl/blog/wprowadzenie-do-swiata-jezyka-java/ opisałem, że dzięki specjalnemu "odśmiecaczowi" (garbage collector) Java sama zarządza pamięcią operacyjną i tutaj w przeciwieństwie do niektórych języków nie musimy tej pamięci ani alokować ani dealokować, czyli zwalniać na inne potrzeby. Ze strumieniami jest jednak inaczej, bo często dotyczą zasobów typu Input/Output inaczej IO czyli wejścia wyjścia gdzie korzystamy z zasobów systemówych, które trzeba zamykać. Jeśli tego nie zrobimy możemy spowodować problem zwany "Resource leak" czyli wyciek zasobów https://en.wikipedia.org/wiki/Resource_leak. Dlatego wszystkie zasoby inne niż pamięć takie jak uchwyty do plików, otwarte sockety i tym podobne, trzeba samodzielnie zamykać, zwłaszcza, że systemy operacyjne pozwalają zwykle na ograniczoną ilość otwartych zasobów systemowych. Oczywiście, sam system operacyjny, a dokładnie jego kernel czyli jądro systemu, może uznać, że jakiś zasób zostanie zamknięty, nawet jeśli program sam tego nie zrobił, ale zwykle dzieje się to dopiero gdy nasz program zostanie wyłączony, więc w naszym krótkim programie nic takiego wielkiego się nie stanie, jeśli tego nie zrobimy, ale trzeba pamiętać, że w Javie tworzy się często oprogramowanie które działa miesiącami lub nawet latami bez restartu i wtedy prawidłowa obsługa strumieni jest niezwykle ważna.

W drugą stronę, jeśli spróbujemy zamknąć strumień dwukrotnie, lub wykonać jakąś akcję na nim po zamknięciu dostaniemy wyjątek "java.lang.IllegalStateException" lub podobny. O obsłudze wyjątków także powiemy sobie na dalszym etapie wyzwań.


