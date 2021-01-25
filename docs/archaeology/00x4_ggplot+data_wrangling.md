Intro
=====

Datenmanagement. Klingt nicht spannend, ist aber essentiell! Denn:
Häufig liegen Daten nicht auf die Art und Weise vor, wie wir sie
benötigen, um bestimmte Abfragen, Plots und Tests zu erstellen.

Ein Beispiel erarbeiten wir uns mithilfe des Bornhom-Datensatzes und
anhand der Erstellung von Liniendiagrammen. Danach geht es um einfaches
zusammenfügen von verschiedenen Datensätzen.

Mein Tip: Arbeitet bitte in dem R-Projekt und legt euch hierfür wieder
ein neues Rmarkdown-Dokument an. Kommentiert und erläutert euch den
Code!

Liniendiagramme!
----------------

Liniendiagramme sind nur sinnvoll, wenn auf der X-Achse eine Abfolge
erstellt werden kann, also mindestens ordinale Daten abgetragen werden
können. Für zeitliche Entwicklungen eignen sie sich super, aber es muss
beachtet werden, dass jedem X-Wert nur ein Y-Wert zurgeordnet werden
darf. Das ist der Grund, warum wir in dem Bsp erst eine neue Tabelle
bauen müssen.

Im Bornholmer Datensatz gibt es eine schöne Abfolge von verschiedenen
Perioden und ich will einfach nur darstellen, wie viele Fundstellen es
pro Periode gibt. Vielleicht benutze ich das für eine
Bevölkerungsdichtenrekonstruktion, wer weiß.

Wenn man allerdings in den Datensatz schaut, sind dort diese Angaben
nicht direkt verzeichnet. Deswegen nutze ich den `table` - Befehl

Der `table`-Befehl zählt, wie häufig eine Variable mit einer anderen
vorkommt. Da ich nur eine Variable angeben, zählt er einfach, wie häufig
diese vorkommt. Da ggplot nicht mit dem Format `table` arbeiten kann,
wandeln wir `bh_table` noch in einen `data frame` um.

Schaut euch die Tabelle `bh_table` einmal mit `View` an, dann versteht
ihr, warum ich im Liniendiagramm die Variablen so gewählt habe, wie sie
da stehen.

``` r
library(archdata)
```

    ## Warning: package 'archdata' was built under R version 4.0.2

``` r
data(Bornholm)


bh_table <- table(Bornholm$Period) # Häufigkeiten zählen 
bh_table <- as.data.frame(bh_table) # als Dataframe überspeichern

library(ggplot2)

ggplot(data = bh_table)+
  geom_point(aes(x = Var1, y = Freq))+
  geom_line(aes(x = Var1, y = Freq, group = 1)) # group = 1 ist für geom_line wichtig, weil es sonst Daten gruppieren soll. Ich habe die Gruppierung aber schon vorher mit dem table-Befehl erledigt.
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/liniendiagramm%201-1.png)

``` r
# ein Punktdiagramm über einem Liniendiagramm "markiert" die Stellen, wo ich Datenpunkte habe

# wie ihr seht, kann man unterschiedliche Diagrammtypen übereinander plotten!
```

Aufgabe: Das ist zwar ein ordentliches Diagramm, aber die Beschriftung
ist eher hässlich. Bitte kramt den Code der letzten Sitzung raus und
beschriftet die Achsen angemessen.

Viel Erfolg!

### Liniendiagramm mit mehreren Linien und der notwendige Umstellungsspaß mit den Daten

Häufig will ich ja gar nicht nur eine Linie darstellen, sondern mehrere
Verläufe vergleichen. Selbstverständlich geht das auch mit R. Mit R geht
aaaaalles.

Ich muss allerdings erst ein bisschen die Daten in eine Form bringen,
mit der ich arbeiten kann. Ich möchte jetzt gern wissen, wie häufig
welche Fundtypen in welcher Periode auftauchen. Die Fundtypen sind die
ganzen komischen Kürzel im Datensatz Bornholm.

Was ich kreiieren möchte, ist eine Tabelle mit den Spalten: Periode,
Fundtypus, Häufigkeit in der Periode. Site und Number interessieren mich
nicht mehr. Ich wandle ein “breites” Datenformat in ein “langes” um.
Allgemein geht ggplot lieber mit langen Datensätzen um als mit breiten.

Was meine ich damit? Was passiert hier?

Vielleicht erklärt dieser Blogpost mehr:
<a href="http://archaeoinformatics.net/r-seperate-gather-spread/" class="uri">http://archaeoinformatics.net/r-seperate-gather-spread/</a>
Das Vorgehen “gather” brauchen wir hier auch: Es ist etwas komplizierter
als die Tabelle vorhin, deswegen benutzen wir die beiden neuen Pakete
`tidyr` und `dplyr`. Beide gehören zu einer R-Philosophie, die sich
tidyverse nennt. Das ist eine Reihe von Paketen, die gut miteinander
zurecht kommen und ähnliche Syntaxen verwenden. ggplot gehört auch dazu.

Sie benutzen ein neues Zeichen, das in R base keine Rolle spielt: Die
“Pipe” `%>%`. Damit haben wir vorher schon gearbeitet, hier noch einmal
etwas ausführlicher:

Pipes sind auch aus anderen Programmiersprachen bekannt. Sie sagen
eigentlich nur “was ich gerade in dieser Zeile gemacht habe, übertrage
ich auch in die nächste” und man spart sich eine Reihe von
“Zwischensicherungen” in Variablen.

Als Bsp mach ich diese Umformung der Daten auf beide Weisen:

Zuerst oldschool:

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)

Bornh1  <- Bornholm[, -c(1:2)] #mit eckigen Klammern kann ich aus dem Datensatz Bornholm bestimmte Spalten entfernen: - Spalte 1 bis 2

Bornh2 <-  gather(Bornh1, key = "Typ", value = "Haeufigkeit", "N2c":"A2e") # das ist der Code der die Umformung vornimmt.
#Schaut euch Bornh2 einmal an, damit ihr versteht, was passiert. Manchmal gibt es jetzt gleiche Periode und Typ mit unterschiedlichen Häufigkeiten. Das muss noch einmal zusammengefasst werden: aggregate!

Bornh3 <- aggregate(Bornh2$Haeufigkeit, by = list(Typ = Bornh2$Typ, Period = Bornh2$Period), FUN = sum)
# jetzt ist dummerweise Haeufigkeit in x umbenannt worden

ggplot(data = Bornh3)+
  geom_point(aes(x = Period, y = x, color = Typ))+
  geom_line(aes(x = Period, y = x, color = Typ, group = Typ)) # group bestimmt, welche Punkte verbunden werden
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/Liniendiagramm%202-1.png)

Jetzt das gleiche in tidy code. Wie ihr seht, liegt der Unterschied v.a.
darin, dass ich nicht dauernd neue Variablen benenne:

``` r
Bornholm %>%
  select(-Site, -Number) %>%
  gather(key = "Typ", value = "x", "N2c":"A2e") %>%
  group_by(Typ, Period)%>% #ich gruppiere mein Daten, wie bei aggregate
  summarize(Haeufigkeit = sum(x))%>% #ich summiere sie jetzt und geb den Namen Haeufigkeit für die Spalte
  ggplot()+
  geom_point(aes(x = Period, y = Haeufigkeit, color = Typ))+
  geom_line(aes(x = Period, y = Haeufigkeit, color = Typ, group = Typ))
```

    ## `summarise()` regrouping output by 'Typ' (override with `.groups` argument)

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/Liniendiagramm%202%20tidy-1.png)

``` r
# Nach tidyverse-Logik ist das leichter zu lesen. Was denkt ihr?
```

Aber oje oje, weg vom Code, hin zum Plot: Was ist denn da passiert?

Schöne Idee war das ja, mit den Typenhäufigkeiten nach Periode, aber…

Wir erkennen ein Problem: Zu viele Informationen auf einmal sind keine
gute Idee.

Können wir die Daten also vllt ein bisschen gruppieren?

Ich bin dafür, dass wir nur die Großbuchstaben der Typenbezeichnungen
benutzen, weil ich davon ausgehe, dass das irgendwelche Übergruppen
darstellen könnte. Ich arbeite dafür mit Bornh2 weiter.

Drei Schritte braucht es:

1.  Ich brauche eine neue Spalte, in der die neuen
    Gruppentypenbezeichnungen eingetragen werden ,

2.  Ich muss dort die richtigen Typen eintragen – in diesem Fall kann
    ich einfach nur den ersten Buchstaben aus den Typenbezeichnungen
    behalten Wie das geht, hab ich ergoogelt, sowas hab ich vorher nicht
    gebraucht – und

3.  die neuen Gruppen müssen zusammengerechnet werden mit der Funktion
    aggregate.

``` r
#1. neue Spalte
Bornh2$grobeTypen  <- Bornh2$Typ # hiermit erstelle ich eine neue Spalte, die genau den gleichen Inhalt hat, wie die Typ-Spalte

#2. ersten Buchstaben erhalten (Buchstaben 1 bis 1 erhalten)
# wir brauchen ein neues Paket namens "stringr". Bitte installiert es, bevor ihr es mit library ladet.
library(stringr)

Bornh2$grobeTypen <- str_sub(Bornh2$Typ, 1,1) # wir nehmen die Worte in Bornh2$Typ und benutzen nur Buchstabe 1 bis 1

#3. Daten wieder zusammenfassen: 
# es sind die gleichen Datensaetze mit unterschiedlichen Haeufigkeiten entstanden, die muessen noch mal zusammengefasst werden

Bornh3 <- aggregate(Bornh2$Haeufigkeit, by = list(grobeTypen = Bornh2$grobeTypen, Period = Bornh2$Period), FUN = sum)
```

``` r
# wieso hier ein neuer Code Chunk? Reine Gewohnheit meinerseits, damit ich einen besseren Überblick behalte, trenne ich nach längeren Umwandlungen gern den Code für die Grafik nochmal ab.
# neues Diagramm

ggplot(data = Bornh3)+
  geom_point(aes(x = Period, y =x, color = grobeTypen))+
  geom_line(aes(x = Period, y = x, color = grobeTypen, group = grobeTypen))
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/Liniendiagramm%203-1.png)
Das sieht doch gleich viel besser aus. Auch wenn immer noch etwas viel
vllt.

Ich kann den Datensatz auch noch weiter verkleinern, in dem ich mir zB
nur bestimmte Typen rauspicke. Dafür gibt es unterschiedliche
Möglichkeiten. Die subset-Funktion, mit der ich dann auch wieder neue
Datensätze erstelle oder das Filtern:

``` r
Bornh3%>%
  filter(grobeTypen > "M")%>% # nur die groben Typen, die "größer als M" sind, also nach M im Alphabet kommen
  ggplot()+
  geom_point(aes(x = Period, y =x, color = grobeTypen))+
  geom_line(aes(x = Period, y = x, color = grobeTypen, group = grobeTypen))
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/filter-1.png)

``` r
#oder
Bornh3%>%
  filter(grobeTypen == "A" | grobeTypen== "J" | grobeTypen == "R")%>% #bitte nur die Zeilen, wo grobe Typen A, J oder R ist. Das Zeichen für ODER ist ein senkrechter Strich und findet sich links unten auf der deutschen Tastatur.
  ggplot()+
  geom_point(aes(x = Period, y =x, color = grobeTypen))+
  geom_line(aes(x = Period, y = x, color = grobeTypen, group = grobeTypen))
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/filter-2.png)

Das gleiche mit subset:

``` r
Bornh4 <- subset(Bornh3, Bornh3$grobeTypen > "M") #ich nehme eine Auswahl von Bornh3 und zwar da, wo Bornh3$grobe Typen größer als M ist und weise diesem Datensatz die Variable Bornh4 zu

ggplot(data = Bornh4)+
  geom_point(aes(x = Period, y =x, color = grobeTypen))+
  geom_line(aes(x = Period, y = x, color = grobeTypen, group = grobeTypen))
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/subset-1.png)

``` r
Bornh5 <- subset(Bornh3, Bornh3$grobeTypen == "A" | grobeTypen== "J" | grobeTypen == "R")

ggplot(data = Bornh5)+
  geom_point(aes(x = Period, y =x, color = grobeTypen))+
  geom_line(aes(x = Period, y = x, color = grobeTypen, group = grobeTypen))
```

![](00x4_ggplot+data_wrangling_files/figure-markdown_github/subset-2.png)
Ob ich subset nehme oder filter, liegt ähnlich wie bei der Entscheidung
oben zwischen tidy verse und old school R v.a. daran, ob ich mit dem
reduzierten Datensatz noch häufiger arbeiten werde. Wenn ja, dann weise
ich ihm lieber eine Variable zu, weil die Chance dann auch nicht ganz
schlecht steht, dass ich noch weiß, was das für eine Variable ist
(Bornh4 ist ein ganz schlechter Name. Sinnvoll wäre Bornh\_M oder so
gewesen). Wenn ich das aber nur ein-zweimal für eine Visualisierung
mache, dann nutze ich filter.

Welche Auswahlen jedoch archäologisch relevant ist, entscheidet ihr als
Expert\*innen im Feld!

Daten zusammenfügen
-------------------

Recht häufig arbeite ich anfangs z.B. mit zwei unterschiedlichen
Tabellen und muss diese dann zusammenfügen.

Dabei gibt es unterschiedliche Dinge, ich kann entweder Spalten
zusammenfügen oder Zeilen zusammenfügen.

Wichtig ist das folgende: Wenn ich Spalten zu einer neuen Tabelle
zusammenfüge, dann müssen diese die gleiche Länge haben!

Wenn ich Zeilen zu einer Tabelle zusammenfüge, dann müssen in allen
Zeilen die gleichen Spaltennamen vorhanden sein!

Wenn ich mit einem dataframe arbeite, gibt es dafür unterschiedliche
Möglichkeiten.

### Spalten zusammenfügen:

Als erstes laden wir uns aber einen Übungsdatensatz ein, er liegt in dem
ILIAS-Ordner “Daten/Daten für den 14.12.2020”. Es handelt sich um
mehrere BACups-Derivate als csv-Dateien. Ladet sie herunter und
speichert sie in dem Ordner data\_raw in eurem R-Projekt

Jetzt lassen Sie sich mit einem *relativen Pfad* von dem Rmd-Dokument
aus ansteuern:

``` r
BACups_c1 <- read.csv2("../data/raw_data/BAcups_c1.csv", sep = ",", row.names = "X")
BACups_c2 <- read.csv2("../data/raw_data/BAcups_c2.csv", sep = ",", row.names = "X")
```

Klärt in eurer Gruppe, ob alle den Unterschied zwischen einem relativen
und einem absoluten Pfad noch kennen!

Zwei zusätzliche Argumente habe ich hier angegeben: `sep = ","` sagt,
dass das Trennzeichen zwischen den Datensätzen ein Komma sein soll und
`row.names = "X"` sagt, dass in der Spalte mit dem Namen X die
Zeilennamen verborgen sind. Schaut euch die Datensätze einmal mit einem
einfachen Texteditor oder einem Tabellenkalkulationsprogramm wie Excel
oder LibreOffice an, dann werdet ihr diese Spalte dort sehen. Sie ist
beim Export aus dem R-Datensatz in eine CSV “mitgeschrieben” worden.

Diese beiden Datensätze beinhalten nur ein Teil der Spalten aus dem
ursprünglichen Datensatz.

Wenn wir den Originaldatensatz wieder herstellen wollen, müssen wir sie
zusammenfügen.

Wenn wir uns SICHER sind, dass die Reihenfolge der Daten nicht verändert
wurde. Das also der 1. Datensatz der einen Tabelle auch der 1. Datensatz
der anderen Tabelle ist; der zweite in der einen ist auch der zweite in
der anderen, usw. usf., DANN können wir das über ein ganz einfaches
`cbind` lösen:

``` r
BACups_complete <- cbind(BACups_c1, BACups_c2)
```

Wenn ihr euch das anseht: Fast wie der alte Datensatz! Nur die
Reihenfolge der Spalten ist etwas vertauscht.

Bin ich mir jetzt nicht so sicher, dass die Reihenfolge der Datensätze
genau die gleiche geblieben ist, dann muss ich ein anderes Verfahren
nutzen. Eine Möglichkeit ist `base R`s `merge`. Hierfür benötige ich
eindeutige IDs für jeden Datensatz. Ihr erinnert euch, die haben wir in
der X-Spalte in der csv-Datei. Wir laden sie also noch einmal neu und
lassen die X-Spalte diesmal stehen:

``` r
BACups_c1n <- read.csv2("../data/raw_data/BAcups_c1.csv", sep = ",")
BACups_c2n <- read.csv2("../data/raw_data/BAcups_c2.csv", sep = ",")
```

Jetzt können wir `merge` nutzen und dabei folgende Argumente setzen:
`by.x` bezeichnet die Spalte im erstgenannten Datensatz, in dem die
gleichen IDs stehen wie in der Spalte, die für `by.y` genannt wird.
Sollte in einer der beiden Tabellen weniger Einträge sein als in der
anderen, kann über `all.x` und `all.y` festgehalten werden, welche der
beiden Tabellen komplett eingefügt werden soll, sozusagen die
“Grundtabelle bildet” und welche der beiden Tabellen Datensätze
verliert, sollten sie keinen “Gegenpart” in der anderen haben. Das ist
hier für das Beispiel nicht relevant, wir wissen ja, dass die beiden
Tabellen genau aufeinander passen, aber hier sage ich: Sollte y (also
BACups\_c2n) mehr Einträge haben als x (also BACups\_c1n), dann lass
diese einfach fallen:

``` r
BACups_complete2 <- merge(BACups_c1n, BACups_c2n, 
                          by.x = "X",
                          by.y = "X", 
                          all.x = TRUE, 
                          all.y = FALSE)
```

Tada! Schaut euch das Ergebnis an.

Als zweites schauen wir uns das Zeilen zusammenfügen an:

### Zeilen an eine Tabelle anhängen

Wieder benötigen wir einen Bsp-Datensatz:

``` r
BACups_pt1 <- read.csv2("../data/raw_data/BAcups_pt1.csv", sep = ",", row.names = "X")
BACups_pt2 <- read.csv2("../data/raw_data/BAcups_pt2.csv", sep = ",", row.names = "X")
```

Diese beiden Datensätze sind zwei Teile des BACups-Datensatzes. Einfach
in der Mitte getrennt.

Wollen wir diese beiden zusammenfügen, müssen wir als erstes überprüfen,
ob die Spaltennamen in beiden Datensätzen in der gleichen Reihenfolge
vorliegen.

Schaut sie euch an, ist sogar so.

Das zusammenfügen geht mit einem einfachen `rbind`:

``` r
BACups_complete_rows <- rbind(BACups_pt1, BACups_pt2)
```

Und der Datensatz sieht aus wie neu. :-D

Sollte der eine Datensatz Variablen / Spalten beinhalten, die der andere
nicht hat, dann müssen diese entweder gelöscht oder in dem zweiten
Datensatz hinzugefügt werden (und dann mit NAs gefüllt).

Jetzt stellen wir uns vor, wir haben diese Daten erfolgreich
manipuliert. Das wollen wir jetzt nicht jedes mal wieder machen, wenn
wir mit ihnen weiterarbeiten, deshalb speichern wir sie ab.

Sie werden in dem Ordner derived\_data gespeichert!

Mehrere Möglichkeiten gibt es: Wenn wir sie nur in R weiter benutzen
möchten, ist es am sinnvollsten, wir speichern sie als RData - Datei ab.
Nutzen wir sie noch woanders, sind csv-Dateien zu empfehlen.

``` r
save(BACups_complete2, file = "../data/derived_data/BAC_compl2.RData")
```

In einer Rdata-Daten-Datei können auch mehrere Dateien gespeichert
werden:

``` r
save(BACups_complete_rows, BACups_complete2, file = "../data/derived_data/BAC_complete_all.RData")
```

Sie können dann wieder in die R-Sitzung eingeladen werden, in dem man

``` r
load("../data/derived_data/BAC_complete_all.RData")
```

aufruft.

Um den Effekt zu sehen, schlage ich vor, dass ihr einmal alle Objekte in
eurer “Environment” (oben rechts) löscht. Das geht, in dem ihr auf das
Besen-Symbol klickt.

Wenn ihr jetzt noch einmal den `load`-Befehl nutzt, seht ihr, dass die
Datensätze wieder erscheinen. Beide auf einmal. Praktisch, nicht wahr?