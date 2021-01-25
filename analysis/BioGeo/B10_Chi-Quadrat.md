Der Chi-Quadrat-Test
====================

Die erste Anwendung für den Chi-Quadrat-Test, die wir benutzen wollen,
ist ähnlich wieder Binomialtest. ABER ich kann sie für deutlich mehr
Werte anwenden als nur zwei. Aus Einfachheitsgründen machen wir das aber
hier nochmal mit zwei Werten und einer einfachen Verteilung. Ich
überprüfe, ob die beobachteten Werte von Wallnussbäumen, der Verteilung
der Fläche auf Ebene und Hang entsprechen:

``` r
# die Walnussbäume
wn_b <- c(10,50)
# die Wahrscheinlichkeit, mit der sie sich eigentlich auf die zwei Bodengruppen verteilen sollten
Vert_fl <- c(0.404,0.596)
# voll simpel der Test
chisq.test(wn_b, p= Vert_fl)
```

Allerdings sind die Ergebnisse, die er uns gibt, nicht ganz so spannend,
wie wenn wir es selber ausrechnen. Ich will zB unbedingt herausfinden,
wie die Erwartungswerte sind. Dann kann ich nämlich auch erst
abschätzen, ob ich den Test überhaupt nehmen sollte (Erinnerung: nicht
mehr als 20% der Erwartunswerte sollten unter 5 fallen).

Deswegen weise ich *den ganzen Test* einer Variablen zu. Und dann kann
ich nachschauen, wie die Erwartungswerte aussehen und was mich noch
alles interessiert.

``` r
# ich nenne die Variable chi
chi <- chisq.test(wn_b, p = Vert_fl)
chi$expected
chi$p.value
```

Machen wir weiter mit etwas komplizierterem. Wie wär es wenn es mal mehr
als nur zwei Ausprägungen der Variable geben dürfte? Ist doch
langweilig, immer nur nach der Verteilung einer Variable zu schauen…

Damit ändert sich auch unsere Fragestellung. Wir untersuchen jetzt, ob
es einen Zusammenhang gibt zwischen zwei nominalen Variablen!

Chi Quadrat mit zwei oder mehr Variablen!
-----------------------------------------

Also wie wär es, wenn wir schauen, ob in unserem Pinguindatensatz
bestimmte Arten auf bestimmten Inseln leben?

Der Chi-Quadrat-Test von R arbeitet am liebsten mit tables oder matrices
(Matrizen, pl. von Matrix).

Tables entstehen, wenn man die table-Funktion benutzt. Sie sind auch
eine Arte Datenformat in R, das im Hintergrund anders aussieht als ein
Dataframe. Muss uns nicht weiter beschäftigen, wir brauchen eh die
Gegenüberstellung von Werten, wie wir sie mit table bekommen:

``` r
# zuerst schauen wir einmal, wie diese Table aussieht:
table(penguins$island, penguins$species)

# und dann bauen wir sie einfach in den Befehl für den Chi-Quadrat-Test ein (chisq.test)
chisq.test(table(penguins$species, penguins$island))
```

Oh oh. Das ist aber ein komisch hoher Chi-Wert. Ich bin ein bisschen
skeptisch. Schauen wir uns noch einmal die Tabelle an, die wir
eingegeben hatten.

Da fällt doch was auf! Ja, einige Felder sind mit 0 belegt. Das geht in
einem Chi-Quadrat-Test so gar nicht. Wir können ihn für diese
Fragestellung nicht anwenden. Wenn ein Datensatz die Voraussetzungen für
einen Test nicht erfüllt, kann man ihn nicht anwenden. Allerdings, wenn
man auf den Datensatz schaut, sind die Zusammenhänge auch so sonnenklar.

Chi Quadrat: Auswahl treffen
----------------------------

Dann gucken wir doch lieber, ob bei den Adelie-Pinguinen die Männchen
oder Weibchen eine Insel bevorzugen:

``` r
adelie <- subset(penguins, species == "Adelie")

chisq.test(table(adelie$island, adelie$sex))
```

Das Ergebnis überrascht eigentlich nicht. Was bedeutet es?

Der Chi-Quadrat-Wert ist sehr klein, d.h. die Abweichungen zwischen
Erwartungswert und beobachteten Wert sind sehr klein. Dazu kommt ein
echt großer p-Wert.

Ja genau, es gibt keine Bevorzugung bestimmter Inseln von den
unterschiedlichen Geschlechtern. Sie sind auf den drei Inseln
gleichverteilt.

#### Noch mal ein anderes Beispiel..

Gehen wir zu dem Piraten-Datensatz, da hier mehr Nominalmerkmale kodiert
wurden. Schauen wir doch einmal, ob es einen Zusammenhang gibt zwischen
dem College, in dem die Piraten waren und ihrem Lieblingspiraten! Kann
ja gut sein, dass es in bestimmten Schulen bestimmte Trends gab.

``` r
library(yarrr)
data("pirates")

chisq.test(table(pirates$college, pirates$favorite.pirate))
```

Yaay!

Aber was ist das für ein komischer p-Wert ? “5.054e-10” ?

Das ist die wissenschaftliche Notation von 5,054 mal e hoch -10.

-   e ist die eulersche Zahl, rund 2,718

-   hoch minus etwas ist ein Bruch: 2⁻² = 2/2² = 2/4 = 1/2

-   2,2 \* 2,718⁻¹⁶ = 2,2/2.781¹⁶ = winzig klein

p ist also winzig klein! Yeah!

**Aufgabe: Diskutieren Sie in der Gruppe: Wie interpretiere ich das
Ergebnis?**

Stärke eines Zusammenhangs
--------------------------

Um die **Stärke** eines Zusammenhangs zu messen, kann ich **nicht** den
p-Wert des Chi-Quadrat-Tests nehmen.

Dafür nehme ich am besten Cramér’s V… oder Cramérs Index (CI) oder K…
manchmal bennenen Statistiker Sachen unterschiedlich und das ist voll
nervig. Aber egal. Alles das gleiche.

Es gibt mehrere Pakete, die man installieren kann, die diese
Berechnungen anstellen, in base r ist das leider nicht umgesetzt. Wer
nicht die Cloud-Lösung nutzt, installiere “lsr”, das Paket for *Learning
Statitics with R*, alle anderen laden es bitte nur.

``` r
install.packages("lsr", dependencies = TRUE)
library(lsr)
```

Im Paket lsr gibt es die Funktion “cramersV”. Lasst uns die mal
ausprobieren. Wir geben ihr einfach die gleiche Matrix wie der
Chi-Quadrat-Funktion:

``` r
cramersV(table(pirates$college, pirates$favorite.pirate))
```

Und was sagt uns das Ergebnis nun?

Cramérs V kann Werte zwischen 0 und 1 annehmen. Ist V = 0, dann besteht
kein Zusammenhang, ist V = 1, besteht ein vollständiger Zusammenhang.
Der Wert von 0,23 ist relativ gering. Es ist ein schwacher Zusammenhang
feststellbar.

Herzlichen Glückwunsch!
-----------------------

Wir haben den Chi-Quadrat-Test genutzt, um die Abhängigkeit zweier
Variablen zueinander zu untersuchen, nested und crossed designs
kennengelernt und die Stärke eines Zusammenhangs berechnet.

Super!

PS: Es lohnt nicht, die Stärke eines Zusammenhangs zu berechnen, wenn
der Chi-Quadrat-Test sagt, dass es keinen Zusammenhang gibt… nur so als
Hinweis. Deshalb rechnet man immer zuerst den Chi-Quadrat-Test

**Aufgabe: Testen Sie noch den Zusammenhang zwischen dem
Lieblingspixar-Film der Piraten und dem Schwerttyp. Warum kommt die
Warnung “Chi-Quadrat-Approximation kann inkorrekt sein”?**

**Testen Sie außerdem noch die Stärke des Zusammenhangs zwischen dem
College, das die Piraten besucht haben und ihrem Geschlecht. Gibt es
einen und wie stark ist er?**