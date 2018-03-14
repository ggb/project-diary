Title: Irgendwann landet man immer bei der Philosophie
Date: 2017-09-25 11:20
Category: Programming

Ein Kollege hat einen Abrisskalender mit interessanten und/oder lustigen Fakten. Wenn er der Meinung ist, dass einer der Sprüche des Kalenders für einen von uns interessant ist, hinterlässt er uns das entsprechende Kalenderblatt auf dem Schreibtisch. Auf diese Weise hat das Folgende den Weg zu mir gefunden:

![Steile These.](images/kalenderspruch.jpg)

Ist das wirklich so? 

Wie das folgende [Jupyter Notebook](https://gist.github.com/ggb/d8a8a2f7cb8ceb185e496aaff8cc67b3) zeigt: Ja, in der Tat. Die 95% werden nicht immer getroffen, aber die Zahl scheint schon sehr adäquat zu sein. 

Kurze Erläuterung, wie ich vorgegangen bin: Wikipedia hat eine [spezielle Seite](https://en.wikipedia.org/wiki/Special:Random), bei deren Aufruf man einen zufälligen Eintrag aus der Online-Enzyklopädie erhält. Diese Funktion habe ich mir zu Nutze gemacht, in dem ich die Seite 500 mal aufgerufen habe. Jeder Aufruf hat dann den ersten Link (bereinigt, weil es ansonsten zu Problemen kommt) weiter verfolgt und von der dann folgenden Seite den ersten Link und von der und so weiter. Nach 100 Verlinkungen wurde die Suche als erfolglos abgebrochen. In den meisten Fällen ist man aber tatsächlich bei der Philosophie gelandet.