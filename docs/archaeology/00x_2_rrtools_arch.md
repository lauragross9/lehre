RRtools - Tools für Reproduzierbare Wissenschaft
================================================

rrtools ist von Ben Marwick et al entwickelt worden, um es einfach zu
machen ein R-Paket zu erstellen, in dem man alle seine Daten, seine
Analyse und sein “paper” zusammen ablegt. Dieses Paket kann man dann
auch einfach anderen Menschen zur Verfügung stellen, damit diese die
eigenen Analysen nachvollziehen können und es lassen sich
unterschiedliche Versionen leicht über github kontrollieren:
<a href="https://github.com/benmarwick/rrtools" class="uri">https://github.com/benmarwick/rrtools</a>

Das Bereitstellen von Code und Daten (open access) ist wichtig für die
Wissenschaftlichkeit einer Analyse, da sie so reproduzierbar wird (wenn
jemand weder Code noch Daten hergibt, kann niemand jemals überprüfen, ob
er/sie sich nicht doch verrechnet hat), aber auch für die
Replizierbarkeit. Replizieren heißt, ich nehme die Methode der anderen
und wende sie auf meine eigenen Daten an. Viele Artikel sind nicht
sonderlich replizierbar, weil die Methode kaum im Detail beschrieben
wird. Das ist frustrierend, wenn man die Methode und das Ergebnistoll
fand und selber ausprobieren möchte.

Deswegen ist Code teilen toll und wichtig.

#### Installation

Also. Wir installieren rrtools. Weitere Infos dazu gibt es hier:
<a href="https://github.com/benmarwick/rrtools" class="uri">https://github.com/benmarwick/rrtools</a>.
Da das Paket auf github liegt, nutzen wir die Funktion `install_github`
aus dem Paket `devtools`.

Da ich das Paket devtools nur einmal in dieser Sitzung brauche, kann ich
es mit `devtools::` einmalig für die danach genannte Funktion aufrufen.
So kann ich mir einen `library`-Befehl sparen:

``` r
devtools::install_github("benmarwick/rrtools") 
```

``` r
library(rrtools) # wir laden das Paket und alles was zum Thema git kommt, ignorieren wir.

# Wer Lust hat git kennen zu lernen, findet hier ein paar hilfreiche Links zu dem Thema: http://archaeoinformatics.net/git-how-not-to-learn-it/ 

rrtools::use_compendium("C:/Hier/kommt/ein/Pfadname/pkgname") # wir definieren wo unser Paket liegt und wie es heißen wird "pkgname"
```

Jetzt muss sich einfach so ein neues Projekt / Paket in Rstudio geöffnet
haben. Oben rechts seht ihr den Namen eures Pakets. Oder ihr navigiert
zu dem Pfad, in dem ihr eben das neue Projekt angelegt habt und öffnet
es.

Was tut rrtools?
----------------

rrtools erstellt die Struktur eines “normalen” R-Pakets, wie man es von
CRAN herunterladen und bei sich installieren kann. Wenn ihr jetzt rechts
unter “Files” schaut, seht ihr eine DESCRIPTION-Datei. Hier kann man
jetzt ein paar Metadaten zu dem Paket editieren:

Auf jeden Fall sollte man die Autoreninformation ändern und einen Satz
dazu schreiben, was das Paket soll (Title). Sollte man seinen Code
tatsächlich eines Tages veröffentlichen wollen, ist außerdem unbedingt
über eine Lizenz nachzudenken. Lizenzen regeln, wer wie den Code
weiterbenutzen darf und wie ihr als Autoren zitiert werden möchtet.
Neben Encoding: empfehle ich UTF-8 zu schreiben, falls da was anderes
stehen sollte.

Als Beispiel, in meinem Paket, in dem ich die Lehrmaterialien für diesen
Kurs sammle, steht in der DESCRIPTION-Datei das folgende:

``` r
Package: lehre
Title: In diesem Paket wird Lehrmaterial zu Statistischen Methoden zur Verfügung gestellt.
Version: 0.0.0.9000
Authors@R:
    person(given = "Sophie C.",
           family = "Schmidt",
           role = c("aut", "cre"),
           email = "s.c.schmidt@uni-koeln.de")
Description: Verschiedene Rmd-Dateien mit Hinweisen zu ggplot, statistischen Tests, reproduzierbarer Wissenschaft in Archäologie und Biologie.
License: What license it uses
ByteCompile: true
Encoding: UTF-8
LazyData: true
Imports: bookdown
```

Wenn wir jetzt mit rrtools weiter arbeiten wollen, müssen wir es noch
einmal laden, weil wir in einem neuen Projekt unterwegs sind als ganz am
Anfang. Deswegen für die nicht-Cloud-User:

``` r
library(rrtools)
use_analysis()
```

Dinge passieren! Netterweise erklärt rrtools in der Konsole, was es
gerade getan hat. Wir haben jetzt eine sinnvolle Ordnerstruktur und
schon das erste Rmd-Dokument. Was insgesamt also entstanden ist, ist ein
“ganz normaler” Ordner, der den Namen eures Pakets trägt, in dem weitere
Ordner angelegt wurden:

    analysis/
    |
    ├── paper/
    │   ├── paper.Rmd       # Das ist das Hauptdokument, in dem der Artikel geschrieben wird
    │   └── references.bib  # Literaturdatenbank

    ├── figures/            # Ablageort für Grafiken, die im Rmd erstellt werden
    |
    ├── data/
    │   ├── raw_data/       # Rohdaten (nie überschreiben!)
    │   └── derived_data/   # bearbeitete Daten
    |
    └── templates
        ├── journal-of-archaeological-science.csl
        |                   # "citation style language" definiert Zitationsstil
        ├── template.docx   # Vorlage, wie die paper.Rmd als docx formatiert werden soll
        └── template.Rmd

-   Das `paper.Rmd` kann sofort genutzt werden und mit `bookdown`
    ge-render-t. In ihm steht schon:
    -   ein “YAML header” , der auf die `references.bib` (die Ablage der
        Literatur) und eine `csl` verweist
    -   ein Kolophon, in dem automatisch Details zu dem letzten git
        commit des papers geschrieben werden. Damit wird das ausgegebene
        Dokument (HTML/PDF/Word) immer zu einem bestimmten Stand des
        Codes auf github verlinkt.
-   Das `references.bib` Dokument hat nur einen einzigen Eintrag, um das
    Format zu zeigen. Weitere Literaturangaben können einfach
    hinzugeschrieben werden.
-   Das angegbene `csl` - Dokument kann auch einfach ersetzt werden
    (viele weitere unter:
    <a href="https://github.com/citation-style-language/" class="uri">https://github.com/citation-style-language/</a>)
-   Empfohlen wird das [citr addin](https://github.com/crsh/citr) und
    [Zotero](https://www.zotero.org/) für die Literaturverwaltung zu
    nutzen

Mehr zu dem paper.Rmd gleich unten unter dem Kapitel [R-Markdown](#rmd)

Bitte arbeitet ab sofort in eurem neuen R-Projekt und dort innerhalb des
RMarkdown-Dokuments. Hier könnt ihr euch den Code gleich in
“normal-Text” dokumentieren (Was soll die Berechnung? Welche Vorgaben
gab es? Wie kann das Ergebnis interpretiert werden?).

Die Prüfungsleistung wird mir am Ende als R-Projekt (mit Daten im Ordner
data und den Text im paper.Rmd abgegeben! Bitte übt das also schon
während des Kurses.

R Markdown
----------

Wer rrtools installiert hat, muss Rmarkdown nicht extra installiern.
Ansonsten geht das so:

``` r
install.packages("rmarkdown")
```

Ein *Rmarkdown*-Dokument (Rmd) ist eigentlich nur ein txt-Dokument, also
das simpelste an Textdatei was man sich vorstellen kann. In dem ich in
dieser Textdatei einer bestimmten Syntax folge, kann ich die Datei von
einem Programm im Hintergrund (namens pandoc) in hübsch formatierte
Word-, html- oder PDF-Dateien umwandeln. Zum Bsp lassen sich
Überschriften mit dem Rautensymbol markieren, Listen mit einem einfach
Minus-Symbol und Text durch Unterstriche *kursiv* und durch zwei
Sternchensymbole **fett** drucken:

![](https://d33wubrfki0l68.cloudfront.net/e3541891e3115642d605aca52e4556d397e95c6f/4e2ba/images/quicktourexample.png)

(Beispiel von der offiziellen homepage)

### Dokument-Struktur

Fangen wir in einem Dokument von oben an. Ganz oben steht ein
sogenannter “YAML-Header”. Er beginn und endet mit 3 Minuszeichen in
einer eigenen Zeile. In ihm werden einige Meta-Daten zu dem Dokument
abgelegt:

-   Der Titel wird hinter `title:` in Hochkommas geschrieben
-   Hinter `author:` schreibt man seinen eigenen Namen in Hochkommas
-   `date:` ist nicht verpflichtend, aber das kann hilfreich sein
-   und mit `output:` wird spezifiziert, in was für eine Art Dokument
    das Rmarkdown-Dokument verwandelt werden soll. Es gibt
    “html\_document”, “word\_document” oder “pdf\_document” und
    tatsächlich noch einige andere Versionen, die hier nicht so wichtig
    sind.

Wenn das Paket `knitr` installiert ist (wer nicht in der Cloud arbeitet:
bitte installiert es), gibt es oben den **Knit**-Knopf (englisch für
stricken und das Symbol ist ein Wollknäul mit Stricknadeln), der das
Dokument umwandelt. Wer faul ist wie ich, benutzt den Shortcut
Strg+Umschalt+k.

Überschriften werden in einer einfachen Syntax gegliedert. Ein Hashtag
(\#) markiert die oberste Überschrift, zwei Hashtags (\#\#) die zweite
Rangordnung, drei Hashtags (\#\#\#) die nächste. “Dokument-Struktur” ist
eine Überschrift dritten Grades:
`### Dokument-Struktur{#dokument-struktur}`.

Ach, was soll denn dieses `{#dokument-struktur}`? Das setzt einen Anker.
Mit einem Anker kann ich links innerhalb des Dokuments setzen. Also, auf
andere Überschriften verweisen. Wer html kennt, dem ist das Prinzip
bekannt. Wenn ihr [hierauf](#dokument-struktur) klickt, dann springt ihr
wieder zu der Überschrift Dokument-Struktur. Markiert wurde das
folgendermaßen: `[hierauf](#dokument-struktur)`.

Praktisch, nicht wahr? Ein Anker steht in geschweiften Klammern ({}) und
beginnt mit einer Rautensymbol (\#). Er darf keine Leerzeichen
enthalten! Wenn auf den Anker verwiesen werden soll, wird er in runde
Klammern () hinter das Wort in eckigen Klammern \[\], das als Link
formatiert wird, geschrieben.

So viele Rauten… Hashtags… \#\#\# … Hilfe!

Nicht verwirren lassen! Innerhalb eines Code-Chunks und innerhalb eines
“normalen R-Skripts” gilt die Raute als Symbol zum Markieren von
Kommentaren. Im Text-Teil eines Markdown-Dokuments zeichnet es eine
Überschrift aus, wenn davor und danach eine Leerzeile steht. In
geschweiften Klammern ({}) beginnt ein Hasthag einen Anker, in runden
Klammern () wird daraus ein Link, der auf den Anker verweist.

### Text und Code (das R in RMarkdown)

In einem Rmd gibt es “normale” Textbereiche, wie diesen hier und Code
Chunks: Code chunks sind die Bereiche, in denen ich Code unterbringen
kann. Man erstellt am einfachsten einen mit dem Shortcut Strg+Alt+i.

Sie folgen einer bestimmten Logik und werden durch drei Hochkommas
(\`\`\` = die Art, die man auf der deutschen Tastatur neben dem
Back-Pfeil findet) und einer öffnenden geschweiften Klammer mit einem r
darin geöffnet. Daneben kann ich den Code Chunk benennen. Kann voll
praktisch sein, wenn ich später mal darauf referenzieren möchte.
Dahinter kann man noch Parameter für den Code Chunk setzen, dazu gleich
mehr. Nach diesen Parametern folgt jedoch eine schließende geschweifte
Klammer und der Bereich für Code beginnt. Er ist immer etwas dunkler
hinterlegt. In diese erste Zeile sollte nichts weiter hinter die
geschweifte Klammer kommen.

Der Code Chunk muss aber auch beendet werden. Dafür braucht man wieder
die drei Hochkommas alleine (\`\`\`) in einer Zeile.

``` r
# das hier ist ein Code Chunk. Ich sollte also nur Code hinein schreiben
# alles was nicht Code ist, muss auskommentiert werden, sonst will pandoc es gerne als Code behandeln und fängt an zu weinen, weil es sehr fehlerhafter Code wäre
```

Dieser Code wird in der Regel gleich ausgeführt und, wenn er zu einer
Grafik führen soll, wird die Grafik angezeigt:

![](https://d33wubrfki0l68.cloudfront.net/44f781299f23419d5314e5322e7c44393f7190d3/c5915/images/markdownchunk.png)

(Mehr zum Bilder einfügen [hier](#bilder-einfügen))

Die Parameter des Code Chunks können bestimmen, ob der Code mit als Code
in das html-Dokument übertragen werden werden soll (`echo = TRUE`) oder
nicht (`echo = FALSE`) oder falls ein Diagramm erstellt wird, wie groß
es sein soll (fig.height und fig.width). Einen Parameter, den ich immer
benutze, um die Skripte zu erstellen, ist zB “`eval = FALSE`”, was
besagt, dass der Code in diesem Chunk nicht ausgeführt werden soll.
Ansonsten würde ich bei jedem Umwandeln des RMarkdown-Dokuments in die
html alle Pakete neu installieren, deren Installationscode ich
hinschreibe, das wäre Quatsch. Diese Parameter werden mit Komma
voneinander getrennt, weitere findet ihr unter:
<a href="https://rmarkdown.rstudio.com/lesson-3.html" class="uri">https://rmarkdown.rstudio.com/lesson-3.html</a>
und eine super Zusammenstellung hier:
<a href="https://yihui.org/knitr/options/" class="uri">https://yihui.org/knitr/options/</a>.
Man kann sie aber auch händisch einstellen, wenn man auf das kleine
Zahnradsymbol klickt.

Noch ein Beispiel für einen Code Chunk. Ich möchte das Histogramm aus
dem Beispiel oben in dieses Dokument einfügen. Diesmal gebe ich nicht
den Parameter `eval = FALSE` sondern `eval = TRUE` und spezifiziere,
dass die Grafik 5cm groß sein soll. Dadurch dass in meinem obersten Code
Chunk in diesem Dokument im [setup](#header)
`knitr::opts_chunk$set(echo = TRUE)` gesetzt habe, gilt `echo = TRUE`
als default für alle Code Chunks. Damit wird der Code immer mit
ausgeschrieben.

``` r
library(ggplot2)
library("palmerpenguins")
data("penguins")

# Histogramm erstellen, um Verteilung der Größe der Pinguine einzuschätzen
ggplot()+
  geom_histogram(data = penguins, aes(x = body_mass_g), 
                 binwidth = 100, # Klassengröße 100g zeigt klare Verteilungsstrukturen
                 na.rm = TRUE) # zwei NA-Sätze müssen entfernt werden
```

![](00x_2_rrtools_arch_files/figure-markdown_github/unnamed-chunk-3-1.png)

Wenn ich das nicht möchte, muss ich noch einmal in meinen Code Chunk
Options spezifizieren, dass `echo = FALSE` sein soll. Bsp:

![](00x_2_rrtools_arch_files/figure-markdown_github/unnamed-chunk-4-1.png)

Außerhalb eines Code Chunks weiß pandoc jedoch nicht, dass was ich
schreibe, Code sein soll, es geht davon aus, dass das alles Text ist.

install.packages(“knitr”) tut also genau gar nichts.

Was man aber machen kann, ist Variablen, die man erstellt hat, und
kleine Berechnungen im Text einbetten. Man brauch ein Hochkomma \` r
“Variablenname / Befehl” und wieder ein Hochkomma ´ einfach mitten im
Fließtext. Der Median von der Spalte body\_mass\_g ist 4050g.
Geschrieben habe ich nicht die Zahl sondern: \` r
median(penguins$body\_mass\_g, na.rm = TRUE)\` und das Programm hat es
sozusagen “on the fly” im Text für mich ausgerechnet.

Das ist suuuuper praktisch, weil ich mir so keine Zahlen merken muss und
so eine Fehlerquelle reduziere.

### Style guide für innerhalb der Code-Chunks

Damit man seinen eigenen Code später auch noch gut lesen kann und auch
andere Leute ihn gut lesen können, gibt es ein paar Hinweise, wie er
geschrieben wird, sogenannte “style guides”. Ich orientiere mich an dem
Style Guide der “Initiative für Statistische Analysen in Archäologie
Kiel”
(<a href="https://github.com/ISAAKiel/StyleGuide" class="uri">https://github.com/ISAAKiel/StyleGuide</a>)
und gebe euch hier ein paar Hinweise:

1.  Leerzeichen um jeden Operator (`=, +, -, <-`, etc.)) herum, kein
    Leerzeichen vor einem Komma, aber eins dahinter. Kein Leerzeichen
    zwischen einer Funktion und der Klammer (zB `mean()` nicht
    `mean ()`)

2.  Namen von Datensätzen und Variablen sollten keine Sonderzeichen
    enthalten. Leerzeichen sollten mit einem Unterstrich \_ ersetzt
    werden. “Sprechende” Namen, die einen Sinn ergeben, sind nützlich
    (zB `mean_body_mass_g` und nicht einfach nur `m`).

3.  Eine Zeile Code sollte nicht länger als 80 Zeichen sein. Stattdessen
    ist es sinnvoll, den Code hinter jedem Komma “umzubrechen”, denn
    dann hat an einen besseren Überblick und kann ihn viel besser
    kommentieren (siehe 4.). Rstudio “setzt” das auch von alleine
    sinnvoll, in dem die neue Zeile innerhalb einer Klammer dann auf der
    gleichen “Höhe” beginnt, wie der vorhergehende Eintrag.

4.  Kommentiert den Code mit einem Hashtag (\#)-Symbol.

-   VOR dem Code und in eine eigene Zeile kommen längere Hinweise, was
    der folgende Code machen soll: zB “\# Histogramm erstellen, um
    Verteilung der Größe der Pinguine einzuschätzen”
-   IN DIE GLEICHE ZEILE wie Code kommen kurze Hinweise auf die genutzte
    Funktionen und Argumente, zB:

``` r
library(ggplot2)
library("palmerpenguins")
data("penguins")

# Histogramm erstellen, um Verteilung der Größe der Pinguine einzuschätzen
ggplot()+
  geom_histogram(data = penguins, aes(x = body_mass_g), 
                 binwidth = 100, # Klassengröße 100g zeigt klare Verteilungsstrukturen
                 na.rm = TRUE) # zwei NA-Sätze müssen entfernt werden
```

-   Tipp: lieber zu viel als zu wenig kommentieren.

1.  Lasst ein bisschen Platz zwischen verschiedenen Sinneinheiten im
    Code. Eine Leerzeile zwischen `data("penguins")` und dem Kommentar
    im Bsp erleichtert das Lesen ungemein.

### Bilder einfügen

Die Bilder im Text habe ich per Markdown eingefügt. Sie sind online
verfügbar, deshalb war der Code für das erste der folgende:
`![](https://d33wubrfki0l68.cloudfront.net/44f781299f23419d5314e5322e7c44393f7190d3/c5915/images/markdownchunk.png)`.
Um Bilder einzufügen, die irgendwoe auf meinem PC liegen, gilt:
`![](Pfad/zum/Bild/bild.jpg`)

Links lassen sich auch einfügen, und zwar als [Hyperlink im
Text](https://rmarkdown.rstudio.com/authoring_quick_tour.html) (Code:
`[Hyperlink im Text](https://rmarkdown.rstudio.com/authoring_quick_tour.html`)

oder einfach so:
<a href="https://rmarkdown.rstudio.com/authoring_quick_tour.html" class="uri">https://rmarkdown.rstudio.com/authoring_quick_tour.html</a>.

### Kommentare

Auch in Markdown-Text kann man Kommentare abgeben, die nicht mehr
sichtbar sind, wenn das Dokument in eine html/docx oder pdf umgewandelt
wird. Sie werden im “html-style” geschrieben: Mit \<!– hier steht das
Kommentar –\>

Schaut einmal in mein Rmd-Dokument (unter
<a href="https://github.com/SCSchmidt/lehre/blob/R-Kurs-Koblenz/analysis/paper_00x_2_rrtools.Rmd" class="uri">https://github.com/SCSchmidt/lehre/blob/R-Kurs-Koblenz/analysis/paper_00x_2_rrtools.Rmd</a>)
um zu sehen, was ich unter dieser Zeile kommentiert habe.

<!-- Haha. Das hier ist unsichtbar -->

Einfügen von Zitationen
-----------------------

Empfohlen wurde für rrtools [zotero](https://www.zotero.org/). Zotero
ist ein Programm mit dem Literatur verwaltet werden kann, ähnlich wie
Citvai, nur freie und offene Software und allein schon deshalb besser.
Ich empfehle es sehr, sich spätestens für die Abschlussarbeit ein
Bibliographieprogramm zuzulegen und kontinuierlich zu benutzen! Im Laufe
des Studiums sowie später bei der wissenschaftlichen Arbeit gilt es
unzählige Aufsätze, Berichte und Bücher zu finden und zu bearbeiten. Um
den Überblick über bereits Gelesenes nicht zu verlieren, aber auch um
unbearbeitete Literaturhinweise zu ordnen, bieten sich
Bibliographie-Programme an.

Hier kann man die Zitate (über die man die Literatur eindeutig
wiederfinden kann) mit Notizen, Verweisen und Schlagwörtern verknüpfen.
So entsteht eine Datenbank, welche über die Autoren, Schlagwörter, Titel
oder selbst angelegte Gruppierungen leicht durchsuchbar ist. Schon
gelesene Werke können mit Zusammenfassungen versehen werden. Dies
erleichtert das spätere Wiederauffinden der Informationen ungemein und
ist ein Muss für modernes wissenschaftliches Arbeiten. Bei Zotero kann
man mit einem Klick die Website, auf der man sich gerade befindet bzw.
den Artikel oder das PDF, als Snapshot in die Datenbank aufnehmen.

Aus Literaturverwaltungsprogrammen lassen sich die Einträge (also diese
Informationen zu Artikeln, Autor, Jahr, Titel, Verlag etc) in mehreren
Formaten exportieren. Markdown arbeitet gut mit “bib”-Dateien zusammen,
die im Stil “Bibtex” oder “BibLatex” abgelegt werden.

Ein Eintrag für ein Buch sieht dann so aus:

    @book{legendre_2012,
        location = {Amsterdam},
        title = {Numerical Ecology},
        isbn = {978-0-444-53868-0},
        publisher = {Elsevier},
        author = {Legendre, Pierre and Legendre, Louis},
        date = {2012}
    }

`@book` bedeutet: Das hier ist ein Buch.

`legendre_2012` ist die ID dieses Buches, mit der ich es “ansprechen”
kann

`location, title, isbn, publisher, author` und `date` sind die
Informationen, die das Programm dann in einen Literatureintrag umwandeln
kann

Ich füge den Literaturhinweis im Fließtext so ein:

R ist eine tolle Skriptsprache (Legendre/Legendre 2012).

In Wirklichkeit habe ich geschrieben: R ist eine tolle Skriptsprache
\[@legendre\_2012\].

Und Pandoc hat das automatisch in die richtige Art und Weise formatiert,
sowohl im Text als auch unter “Bibliographie”.

Als csl habe ich für dieses Beispeil einen Ziterstil namens
“RGK\_archaeology\_DGUF” von
<a href="https://www.dguf.de/203.html" class="uri">https://www.dguf.de/203.html</a>
heruntergeladen und einfach in dem Ordner abgelegt, in dem dieses Rmd
liegt. Je nachdem, welchen Zitierstil man für die Abgabe des Manuskripts
bei einer Zeitschrift benötigt, kann man von dem [Zotero Style
Repository](https://www.zotero.org/styles/) die passende csl-Datei
herunterladen. Wenn sie schon einmal jemand programmiert hat. Manche
Zeitschriften stellen inzwischen ihre bevorzugten Zitierstile auch
selber auf ihren Websiten als csl zur Verfügung.

Wenn man dieses bib-file und die csl hat, kann man mit `citr` eine
kleine GUI installieren, die einen interaktiv innerhalb des bib-files
nach der Veröffentlichung suchen lässt, die man gerade zitieren möchte.
Macht es einfacher, aber ist nicht unbedingt notwendig. Nach der
Installation von citr muss Rstudio neu gestartet werden und dann findet
man es unter “Addins” oben in der Leiste unterhalb
“File”“,”Edit“,”Code“,”View“,”Plots"… in Rstudio.

Toll ist auch, dass wenn ich R-Pakete zitieren möchte, ich mir die
Informationen, wie sie zitiert werden sollen in einem Bibtex-Stil
ausgeben lassen kann:

``` r
citation("ggplot2")
```

    ## 
    ## To cite ggplot2 in publications, please use:
    ## 
    ##   H. Wickham. ggplot2: Elegant Graphics for Data Analysis.
    ##   Springer-Verlag New York, 2016.
    ## 
    ## A BibTeX entry for LaTeX users is
    ## 
    ##   @Book{,
    ##     author = {Hadley Wickham},
    ##     title = {ggplot2: Elegant Graphics for Data Analysis},
    ##     publisher = {Springer-Verlag New York},
    ##     year = {2016},
    ##     isbn = {978-3-319-24277-4},
    ##     url = {https://ggplot2.tidyverse.org},
    ##   }

Das kann ich dann in meine bib-Datei kopieren und ihm dort eine eigene
ID geben. Seht ihr, dass das was bei dem Beispiel oben “legendre\_2012”
war, hier fehlt? Statt “Book{,” würde ich z. B. “Book{ggplot2,”
schreiben und dann innerhalb meiner Texte immer auf mit @ggplot2 das
Paket zitieren können. Das ist besonders wichtig bei den Paketen, die
man von anderen kostenlos herunterladen konnte. Wenn man die
Programmierer schon nicht in Geld bezahlt, sollte man sie wenigstens mit
Reputation bezahlen.

### Zusammenfassung

Mit rrtools erstellen wir eine Ordnerstruktur, in der Daten, R-Code und
Text für jedes Projekt gemeinsam abgelegt werden können.

RMarkdown verknüpft Markdown, eine Auszeichnungssprache mit
Möglichkeiten R-Code auszuführen. Der große Vorteil eines
RMarkdown-Dokumentes ist es, dass man seinen Code und den Text zur
Interpretation beisammen hat. So lassen sich direkt Zeitschriftenartikel
mit Berechnungen, Analysen, Grafiken etc in einem Dokument ablegen. Man
kann in Markdown mit einem Literaturverwaltungsprogramm zusammenarbeiten
und sich automatisch Literaturverzeichnisse generieren lassen. Dazu
später mehr.

### Links

Wer mehr zu Markdown allgemein lesen will, sei hierauf verwiesen:
<a href="https://vijual.de/2019/03/11/artikel-mit-markdown-und-pandoc-schreiben/" class="uri">https://vijual.de/2019/03/11/artikel-mit-markdown-und-pandoc-schreiben/</a>
(insb. Kap 1). Details zu RMarkdown findet ihr hier:
<a href="http://rmarkdown.rstudio.com" class="uri">http://rmarkdown.rstudio.com</a>
und einen sehr praktischen Schummelzettel hier:
<a href="https://rstudio.com/wp-content/uploads/2015/06/rmarkdown-german.pdf" class="uri">https://rstudio.com/wp-content/uploads/2015/06/rmarkdown-german.pdf</a>
. Hilfreich ist auch dieses Buch:
<a href="https://bookdown.org/yihui/rmarkdown" class="uri">https://bookdown.org/yihui/rmarkdown</a>.
Wer auf interaktive Weise mehr Markdown lernen möchte, findet hier ein
gutes Tutorial:
<a href="https://commonmark.org/help/tutorial/" class="uri">https://commonmark.org/help/tutorial/</a>.

Übungsaufgabe
=============

Erstellst mindestens ein neues Projekt mir rrtools. Öffne es und schau
dir in Ruhe an, was alles erstellt wurde. Ordnerstrukturen, Dateien etc.

Im dem Projekt, das ihr erstellt habt, liegt eine RmD-Datei unter
“/analysis/paper/”. Schaut sie euch an, drückt *knit* und schaut, was
passiert.

Anschließend probiert einmal folgendes aus:

-   im YAML-header steht ein großes Stück Code

<!-- -->

    output: 
       bookdown::word_document2:
         fig_caption: yes
         reference_docx: "../templates/template.docx" # Insert path for the DOCX file
         pandoc_args:
         - --lua-filter=../templates/scholarly-metadata.lua
         - --lua-filter=../templates/author-info-blocks.lua
         - --lua-filter=../templates/pagebreak.lua

Ersetzt es durch

    output:
      html_document

und auf *knit* drücken (oder Strg+shift+k). Was passiert?

An die Stelle könnt ihr auch einmal schreiben:

    output:
      pdf_document

und dann *knit* drücken.

Toll oder?! Mit einem Knopfdruck aus einer “Grund”-Datei drei
unterschiedliche Formate exportieren, ohne Probleme. Mit Code, der
korrekt umgewandelt wird. Ich schreibe inzwischen alle meine Artikel in
Rmarkdown, da ich so am einfachsten den Output generieren kann, der
gewünscht ist.

Die in dem Originaldokument gegebene reference\_docx definiert solche
Sachen wie Überschriften formatiert werden sollen, wenn es als docx
ausgegeben werden soll. Die pandoc-arcs sind für ähnliche Dinge da. Aber
das führt hier zu weit.

Bibliographie
=============

Legendre/Legendre 2012: P. Legendre/L. Legendre, Numerical Ecology
(Amsterdam 2012).