Lineare Regression
==================

Vorhin hatten wir das Thema Korrelation und dabei den R-Wert von
Pearson-Bravais von der Größe und des Gewichts der Piraten und der
Pinguin-Schnabellänge und -dicke bestimmt.

Jetzt erstellen wir noch eine Linare Regression daraus. Also laden wir
als erstes die Datenpakete:

``` r
library(yarrr)
data(pirates)
```

Lineare Regression
------------------

Die lineare Regression legt eine “best-fit”-Linie zwischen die Punkte.
Sie soll möglichst gut den Punktverlauf abbilden.

Die Berechnung in R erfolgt sehr einfach mit dem Befehl `lm` (= “linear
model”). So ein Modell wird häufig “fit” genannt (“Passung”).

``` r
fit <- lm(weight ~ height, data = pirates) # Berechnung: 1. Variable ist die abhängige Variable, 2. Variable die "vorhersagende" Variable
```

Schaut man sich dieses Modell dann einmal mit `summary` an, erhält man
eine ganze Reihe an Informationen:

``` r
summary(fit)
#> 
#> Call:
#> lm(formula = weight ~ height, data = pirates)
#> 
#> Residuals:
#>      Min       1Q   Median       3Q      Max 
#> -12.3592  -2.6564  -0.0708   2.7275  11.1451 
#> 
#> Coefficients:
#>              Estimate Std. Error t value Pr(>|t|)    
#> (Intercept) -68.87722    1.71250  -40.22   <2e-16 ***
#> height        0.81434    0.01003   81.16   <2e-16 ***
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 3.928 on 998 degrees of freedom
#> Multiple R-squared:  0.8684, Adjusted R-squared:  0.8683 
#> F-statistic:  6587 on 1 and 998 DF,  p-value: < 2.2e-16
```

Erst einmal wird wiederholt, wie das Modell berechnet wurde, dann gibt
es einen Überblick über die Residuen und Werte Wie die Schätzung des
Y-Achsenabschnitts und des Steigungswinkels. Daraus kann man sich selber
schon die Formel für die Regressionslinie ableiten:

`y = -68.877 + 0.814 * x`

Außerdem finden wir den Hinweis auf die Standardabweichung, den t-Wert
und die Irrtumswahrscheinlichkeit für diese beiden Werte. Sowie die
Standardabweichung der Residuen, den R²-Wert und dessen
Wahrscheinlichkeit (mit einem F-Test getestet).

Es ist vor allem jedoch imm hilfreich, diese “best fit”-Linie zu
visualisieren. In R ist das ziemlich einfach, in dem ich einem
Streudiagramm den geom\_smooth-Befehl mit der Methode “lm” (linear
model) hinzufüge. Der Befehl “se = FALSE” sagt aus, dass ich jetzt gern
*kein* Konfidenzintervall (standard error) dazu visualisieren möchte.

``` r
library(ggplot2)

ggplot(data = pirates)+
  geom_point(aes(x = height, y = weight))+
  geom_smooth(aes(x = height, y = weight), method = "lm",
              se = FALSE)
```

![](./figures/lin_reg-1.png)

Einfach oder?

Dieses Diagramm können wir jetzt noch ein bisschen verbessern. Wir
könnten

1.  doch das Konfidenzintervall angeben

2.  dazuschreiben, wie diese Linie mathematisch beschrieben werden kann
    und angeben, wie der R²-Wert der Linie aussieht.

Also, fangen wir an mit 1.:

``` r
ggplot(data = pirates)+
  geom_point(aes(x = height, y = weight))+
  geom_smooth(aes(x = height, y = weight), method = "lm",
              se = TRUE)
```

![](./figures/lin_reg_se-1.png) Denkbar einfach. Das Konfidenzintervall
ist sehr schmal, was für eine gute Anpassung der Linie an die Punkte
spricht.

Schauen wir uns doch einmal an, wie diese Linie mathematisch beschrieben
werden und zusammen mit dem R²-Wert dem plot hinzugefügt werden kann.

Wir brauchen dafür das Paket “ggpmisc”.

Dann fügen wir dem bisherigen Plot (am besten ihr kopiert das bisherige
einfach mit strg+c und strg+v) den Befehl `stat_poly_eq` hinzu.
`stat_poly_eq` kann die Statistik der equation (Formel) einfügen. Dafür
braucht es erst einmal noch die Eingabewerte der Regression (x = und y =
), dann die Information, welcher Text als “label” in den Graphen
hinzugefügt werden soll: `paste` (füge ein) die Formel (`..eq.label..`,)
und den R²-Wert (`..adj.rr.label..`) und separiere die beiden mit vier
Leerzeichen (symbolisiert durch die Tilde). Formula sind eine bestimmte
Art von Objekten in R. An dieser Stelle sagt man mit `formula = y~x`,
dass y die abhängige Variable sein soll. `parse = TRUE` bedeutet “ja
bitte schreib es hin” und “size” gibt die Schriftgröße an.
\`label.y.npc\`\` platziert die Schrift und zwar auf der y-Achse nach
Prozent (also in diesem Fall bei 90% der Y-Achse).

``` r

library(ggpmisc)


ggplot(data = pirates)+
  geom_point(aes(x = height, y = weight))+
  geom_smooth(aes(x = height, y = weight), method = "lm",
              se = FALSE)+
  stat_poly_eq(aes(x = height, y = weight, 
                   label =  paste(..eq.label..,     # kopier die Formel (equation)
                   ..adj.rr.label.., # kopier R²adjusted
                   sep = "~~~~")), # Trenn es mit 4 Leerzeichen
             rr.digits = 4, # vier Nachkommastellen für R²
             formula = y~x, # lineare Gleichung nach der Formel y auf x
             parse = TRUE, # zeig es an
             size = 5,     # in Größe 5
             color = "gray50", # Farbe
             label.y.npc = 0.9) # Farbe
```

![](./figures/lin%20reg_formel-1.png)

Cool oder?

Eine Angabe würde uns noch interessieren, und das ist der p-Wert. Bauen
wir den also auch noch ein!

Dafür brauchen wir aus `ggpmisc` noch eine weitere Funktion namens
`stat_fit_glance`. Ihr müssen wir auch wieder all die Angaben über die
Berechnung geben (zum Glück gibt es copy und paste), sagen dann aber

``` r
ggplot(data = pirates)+
  geom_point(aes(x = height, y = weight))+
  geom_smooth(aes(x = height, y = weight), method = "lm",
              se = FALSE)+
  stat_poly_eq(aes(x = height, y = weight, 
                   label =  paste(..eq.label..,     # kopier die Formel (equation)
                                  ..adj.rr.label.., # kopier R²adjusted
                   sep = "~~~~")), # Trenn es mit 4 Leerzeichen
             rr.digits = 4, # vier Nachkommastellen für R²
             formula = y~x, # lineare Gleichung nach der Formel y auf x
             parse = TRUE, # zeig es an
             size = 5,     # in Schriftgröße 5
             color = "gray50", # Farbe
             label.y.npc = 0.8) +  # Platzierung  auf Y-Achse
  stat_fit_glance(method = "lm",  # es wird eine lineare regression berechnet
                   method.args = list(formula = y ~ x), # y abhängig von X
                   geom = "text",       # es handelt sich um eine Geometrie der Art "Text"
                   aes(x = height, y = weight, 
                       label = paste("p-Wert: ", # kopier die Angabe "p-Wert:"
                                      signif(..p.value.., # kopier den p-Wert
                                      digits = 5),  # 5 Nachkommastellen für p
                                      sep = "")),   # diesmal keine "Lücke"
                  label.y = 30,        # Platzierung  auf Y-Achse (in absoluten Zahlen)
                  label.x = 'right',   # Platzierung auf der x-Achse am rechten Rand
                  size = 4,            # Schriftgröße 5
                  color = "gray50")    # Farbe
```

![](./figures/lin_reg_p-1.png)

Wenn man diese Grafik noch mit Titel und schöner Achsenbeschriftung
versieht, hat man echt eine publikationswürdige Grafik.

Achtet bei Regressionen immer darauf, was die abhängige Variable ist.
Zwischen dem Gewicht und der Größe der Piraten kann es nur einen
logischen Zusammenhang geben: Größe bestimmt Gewicht! Nicht das Gewicht
die Größe. In anderen Fällen kann das anders sein, also unklar, in
welcher der beiden Faktoren den anderen beeinflusst. Dann ist es
sinnvoll zwei unterschiedliche Regressionen zu berechnen und die
Abhängigkeit der Variablen umzudrehen.

Residualanalyse
---------------

Wie in der Vorlesung erwähnt, muss die Verteilung der Residuen bei einer
linearen Regression bestimmte Voraussetzungen erfüllen. Dies testen wir
jetzt in einem zweiten Schritt.

Mit dem Befehl von oben `summary(fit)` konnten wir auch schon ein paar
Informationen zu den Residuen abfragen. Hier gab es den Minimalwert
(Min), das 1. Quartil (1Q), den Median, das 3. Quartil (3Q) und den
Maximalwert (Max), die einen Hinweis darauf geben, ob die Residuen
normalverteilt sind.

Außerdem gibt es ncoh ein paar andere Möglichkeiten, die Residuen zu
analysieren.

Den Q-Q-plot (genau wie bei dem Test auf Normalverteilung), dann einen
Plot, der die Residuen zu den vorhergesagten Werten vergleicht, um deren
Verteilung zu zeigen, ein Plot, anhand dessen man die Homoskedastizität
überprüfen kann und ein letzter Plot, der für die best-fit-Linie
besonders einflussreiche Punkte identifiziert.

All diese Plots werden mit der base - Funktion `plot` aufgerufen.

### 1. Normalverteilung der Residuen

Der Q-Q-plot: Wenn die plot-Funktion auf eine lineare Regression
aufgerufen wird, kann man mit `which =` bestimmen, welche Teile jetzt
genau dargestellt werden sollen. `which = 2` erstellt ein Q-Q-plot. Mit
`col = "red"` färben wir die Punkte rot ein.

``` r
plot(fit, which=2, col=c("red"))  # Q-Q Plot
```

![](./figures/qq_fit-1.png)

Natürlich kann man das auch so testen, wie wir es bei der
Normalverteilung gelernt haben:

``` r
library(ggpubr)
ggqqplot(fit$residuals)
```

![](./figures/qq_fitres-1.png)

Das gibt vielleicht ein bisschen publizierbareres Plot. Der Vorteil der
`plot`-Funktion von `base` ist, dass die Achsen schon beschriftet sind
und es schneller geht, wenn man nur intern gucken möchte, wie das
Ergebnis aussieht.

### Residuen und vorhergesagte Werte (residuals vs fitted)

Wenn wir die Residuen mit den vorhergesagten Werten vergleichen, können
wir sehen, ob die Residuen einem bestimmten Muster folgen. Das Ziel ist
hier, dass sie das nicht tun, also möglichst gleich verteilt sind.

Schauen wir uns das an:

``` r
plot(fit, which=1, col=c("blue")) # Residuals vs Fitted Plot
```

![](./figures/fitted_vs_residuals-1.png)

Die rote Linie zeigt eine Regression zwischen den Residuen und den
vorhergesagten Werten. Das “Ziel” für eine gute lineare Regression ist,
dass sie auf der 0-Achse liegt. Auch wenn es hier eine leichte Kurve
gibt, liegt sie aber doch ziemlich nahe dran und ergibt kein
distinktives Muster (zB eine Sinuskurve). Das zeigt, dass die Residuen
halbwegs gleichmäßig verteilt sind und damit eine Voraussetzung für die
lineare Regression erfüllt ist.

### “Scale-Location”

Hier schauen wir, ob die Homoskedastizität gegeben ist, d. h. die
Varianz der Residuen entlang der Regressionslinie etwa gleich bleibt.

Der Plot wird mit `which = 3` aufgerufen:

``` r
plot(fit, which=3, col=c("blue"))  # Scale-Location Plot
```

![](./figures/homoskedas-1.png)

Hier sollten die Punkte gleichmäßig in beide Richtungen der roten Linie
streuen.

Ein gutes und ein schlechtes Beispiel wären diese:

![](./figures/homoskedastizitaet_R.jpg) Das gute links, halbwegs
gleichmäßig verteilte Residuen und das schlechte rechts, wo die Residuen
in der linken Ecke deutliich näher an der roten Linie liegen als weiter
rechts, wo sie mehr streuen (Bildquelle und empfehlenswerte Anleitung:
<a href="https://rpubs.com/iabrady/residual-analysis" class="uri">https://rpubs.com/iabrady/residual-analysis</a>
).

### Residuen gegen Ausreißer

Mit diesem Plot kann man die einflussreichen Punkte des Datensatzes
bestimmen. Ein einflussreicher Punkt ist einer, der wenn er entfernt /
hinzugetan wird, die Regressionslinie signifikant verändert. Dafür wird
die “Cook’sche Distanz” berechnet, ein Maß für den Einfluss eines
Punktes und die Grenzen für eine “Unbedenklichkeit” im plot mit roten
gestrichelten Linien angezeigt. Liegen Punkte außerhalb dieser “Grenze”
(der Markierung), sollte betrachtet werden, welche Rolle sie im
Datensatz spielen.

``` r
plot(fit, which=5, col=c("blue")) 
```

![](./figures/res_ausreisser-1.png)

In unserem Bsp wird die Cooksche Distanz nicht einmal mehr angezeigt,
die Schwellwerte liegen außerhalb unseres Bildbereichs. Insofern gibt es
hier keine Probleme.

Wir können also festhalten: Eine Lineare Regression ist für die
Beschreibung der Abhängigkeit des Gewichts der Piraten von ihrer Größe
ist zulässig. Die Regression kann recht sicher vorherhsagen, wie schwer
ein Pirat sein wird, wenn wir seine Größe kennen.

**Aufgabe:** Mit dem Pearson-Bravais-Test haben wir ja festgestellt,
dass der Zusammenhang zwischen Schnabeldicke und -länge nur sehr klein
zu sein scheint. Jetzt können wir an dieser Stelle einmal testen, ob die
Werte besser werden, wenn wir die Pinguine getrennt nach Art
untersuchen.

**Aufgabe:** Erstellt zwei lineare Regressionen für die
Chinstrap-Pinguine für ihre Schnabeldicke und -länge und prüft,

1. ob die lineare Regression hier angebracht ist und

2.  welcher der beiden Parameter eher von dem anderen abzuhängen
    scheint!