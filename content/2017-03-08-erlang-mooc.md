Title: Functional Programming in Erlang
Date: 2017-03-08 11:20
Category: Programming

Aktuell belege ich den Kurs "Functional Programming in Erlang" auf [futurelearn.com](https://www.futurelearn.com/courses/functional-programming-erlang). Der Kurs ist ein Einstieg sowohl in Erlang, als auch in funktionale Programmierung und fängt folgerichtig bei sehr einfachen Themen an: Was sind Module und Funktionen in Erlang? Wie verwendet man Rekursion? Wie Funktionen höherer Ordnung? Und wie kann man mit *pattern matching* und mehreren Funktionsköpfen auf if-then-else-Blöcke verzichten?

Hier ein Beispiel aus einem von zwei "Assignments" ([Assignment 1](https://gist.github.com/ggb/95c8d01f790c24542c3cb1525bca3755), [Assignment 2](https://gist.github.com/ggb/3999970908a4e78462f2482baf324b2b)) die während des Kurses angefertigt werden mussten und von Peers gereviewt wurden:

    :::erlang
    bits(0, M) ->
        M;
    bits(N, M) ->
        bits(N div 2, M + N rem 2).

    bits(N) ->
        bits(N, 0).

Die bits-Funktion nimmt eine Zahl und berechnet, wie viele 1en in der Bit-Repräsentation vorkommen. Wie zu sehen ist, kommen schon in diesem einfachen Beispiel gleich mehrere der oben genannten Konzepte zum Einsatz.

Das ist alles in allem eine super Wiederholung. Erlang war die erste funktionale Sprache mit der ich mich beschäftigt habe, aber ich bin - wohl vor allem auf Grund der von Prolog beeinflussten Syntax - nie sehr warm mit ihr geworden. Daher habe ich mich seinerzeit dazu entschieden Elixir zu lernen (das war eine gute und erfolgreiche Entscheidung!). 

Nachdem ich nun nach einigen Jahren wieder zu Erlang zurückkehre muss ich feststellen, dass die Syntax gar nicht so schlimm ist: Mein anfängliches Unwohlsein ist wohl vor allem auf das Nicht-vertraut-sein mit funktionaler Programmierung zurückzuführen. Wenn man die Konzepte erst mal kennt, ist die Syntax kaum mehr ein Problem. 

Es gibt einen [Folgekurs](https://www.futurelearn.com/courses/concurrent-programming-erlang/), der im April anfängt und sich mit den *wirklich* interessanten Aspekten von Erlang befasst: Stark nebenläufiger und verteilter Programmierung. Auch diesen MOOC werde ich mir ansehen und ich freue mich schon darauf.