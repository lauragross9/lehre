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

**Für die Cloud-User: rrtools läuft nicht gut in der Cloud! In dem in der Cloud ein neues Projekt angelegt wird, entsteht eine Kopie eines Basisprojektes, das ich vorbereitet habe und in dem die folgenden Schritte schon durchgeführt wurden. Ihr müsst nichts installieren und keine Funktion ausführen!**

#### für die nicht-Cloud nutzer:

Also. Wir installieren rrtools. Weitere Infos dazu gibt es hier:
<a href="https://github.com/benmarwick/rrtools" class="uri">https://github.com/benmarwick/rrtools</a>.

``` r
devtools::install_github("benmarwick/rrtools") #devtools:: ruft das Paket devtools. ich brauch es nur einmal, deswegen habe ich es nicht mit library ins Boot geholt.
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

#### hier weiter für alle

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
Anfang. Deswegen **nur** für die nicht-Cloud-User:

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
    │   ├── paper.Rmd       # Das ist das Hauptdokumen, in dem der Artikel geschrieben wird
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

Bitte arbeitet ab sofort in diesem RMarkdown-Dokuments. 
Hier könnt ihr euch den Code gleich in
“normal-Text” dokumentieren (Was soll die Berechnung? Welche Vorgaben
gab es? Wie kann das Ergebnis interpretiert werden?).

Die Prüfungsleistung wird mir am Ende als R-Projekt (mit Daten im Ordner
data und den Text im paper.Rmd abgegeben! Bitte übt das also schon
während des Kurses.

Hinweise
---------------

Empfohlen wird für rrtools die Zusammenarbeit mit [zotero](https://www.zotero.org/). Zotero
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

Ich füge den Literaturhinweis im Fließtext so ein: R ist eine tolle
Skriptsprache (Legendre & Legendre, 2012).

In Wirklichkeit habe ich geschrieben: R ist eine tolle Skriptsprache
\[@legendre\_2012\].

Und Pandoc hat das automatisch in die richtige Art und Weise formatiert,
sowohl im Text als auch unter “Bibliographie”.

Als csl habe ich für dieses Beispeil einen Ziterstil namens
“Geoboiology” von
<a href="https://www.zotero.org/styles/geobiology" class="uri">https://www.zotero.org/styles/geobiology</a>
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
schreiben und dann innerhalb meiner Texte immer auf mit (<span
class="citeproc-not-found" data-reference-id="ggplot2">**???**</span>)
das Paket zitieren können. Das ist besonders wichtig bei den Paketen,
die man von anderen kostenlos herunterladen konnte. Wenn man die
Programmierer schon nicht in Geld bezahlt, sollte man sie wenigstens mit
Reputation bezahlen.

Bibliographie
=============

Legendre P, Legendre L (2012) *Numerical ecology*. Elsevier, Amsterdam.