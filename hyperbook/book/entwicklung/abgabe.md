---
name: Computerspieler abgabefertig machen
index: 9
---

# :t[Computerspieler]{#player} abgabefertig machen

Damit das :t[Wettkampfsystem]{#contest} mit dem :t[Computerspieler]{#player} arbeiten kann,
muss er als ausführbares Programm in ein ZIP-Archiv gepackt werden.

Je nach Programmiersprache, in der der :t[Computerspieler]{#player} entwickelt wurde,
sind unterschiedliche Schritte notwendig.

Wie man den abgabefertigen :t[Computerspieler]{#player} dann im :t[Wettkampfsystem]{#contest} einsendet,
ist unter :t[Wettkampfsystem->Computerspieler]{#contest.computerspieler} beschrieben.

## Java

Diese Anleitung beschreibt, wie man mit der Java-Spielervorlage umgehen sollte.

Hierzu gibt es zwei Möglichkeiten: 
Die Jar-Datei selbst erstellen oder die Verwendung von Gradle (empfohlen).

### Erste Möglichkeit - JAR erstellen

#### Eclipse

1.  In Eclipse im Menü auf "File" → "Export". Dann unter "Java" →
    "Runnable JAR file" wählen

2.  Im nächsten Fenster wird die "Run Configuration" ausgewählt (dazu
    muss die Spielervorlage mindestens einmal mit Eclipse gestartet worden
    sein). Darunter wird mit "Browse" die Zieldatei (z.B.
    `[…​]/my_player.jar`) ausgewählt. Bei "Library handling" am besten
    die erste Option nehmen. So wird eine einzige JAR Datei erzeugt, in
    der alles nötige drin ist. Mit einem Klick auf Finish wird die
    JAR-Datei erzeugt. Eventuell erhält man einen Hinweis "This
    operation repacks referenced libraries", den man mit "OK" bestätigen
    muss.

Wenn alles geklappt hat, wurde der :t[Computerspieler]{#player} in ein ausführbares
Programm überführt. Damit der Wettkampfserver den :t[Computerspieler]{#player} verarbeiten
kann, muss er noch in ein ZIP-Archiv gepackt werden (auch wenn ein JAR
technisch gesehen bereits ein ZIP-Archiv ist).

#### NetBeans

Nach einem Rechtsklick auf das Projekt in der Projektansicht kann man
"Properties" auswählen. Auf der linken Seite unter "Categories" die
Kategorie "Run" auswählen und dann rechts unter "Main Class" die Klasse
"Starter" eintragen.

Nach einem Rechtsklick auf das Projekt in der Projektansicht kann man
"Clean and Build" auswählen. Danach gibt es den Ordner
"Pfad/zum/Projekt/dist". In diesem befindet sich eine JAR-Datei und eine
Kopie des "lib"-Ordners. Beides zusammen muss jetzt mit einem beliebigen
Archivierungsprogramm in eine ZIP-Datei zusammengepackt werden. Dieses
Archiv kann dann hochgeladen werden.

### Zweite Möglichkeit - Das ANT Script

Man kann auch das Ant-Buildscript benutzen, 
welches der Spielervorlage beiliegt.
Dieses kompiliert die Spielervorlage und erzeugt automatisch eine JAR-Datei sowie ein ZIP-Archiv,
das man direkt im Websystem hochladen kann.

#### Direkt ausführen

Wenn Ant installiert ist, kann man über die Kommandozeile in das
Verzeichnis der Spielervorlage wechseln und mit dem Aufruf "Ant" den
Build ausführen. Am Ende sollte die Meldung "BUILD SUCCESSFUL"
erscheinen. Im Spielervorlage-Ordner findet man dann im Unterordner
"build" die JAR Datei im Ordner "jar" sowie die fertig gepackte
ZIP-Datei im Ordner "zipped", die direkt im :t[Wettkampfsystem]{#contest} hochgeladen
werden kann.

#### Eclipse

Eclipse kann von Haus aus auch mit Ant-Scripten umgehen.

1.  Im Menü "Run" → "External Tools" → "External Tools Configuration"
    wählen

2.  Links in dem neuen Fenster mit einem Rechtsklick auf "Ant Build" →
    "New" erstellt man eine neue Konfiguration und wählt diese aus

3.  Auf der rechten Seite muss man nun das Buildfile auswählen. Das geht
    entweder mit "Browse Workspace" oder "Browse File System". Das
    Buildfile heißt "build.xml" und liegt direkt in dem Spielervorlage-Ordner, 
    den man auf der Software-Challenge Homepage heruntergeladen
    hat. Anschließend mit "Apply" bestätigen und das Fenster schließen

4.  Um den Buildprozess zu starten, muss im Menü "Run" → "External
    Tools" die gerade erstellte Konfiguration ausgewählt werden. Der
    Build wird dann durchgeführt (dauert i.d.R. wenige Sekunden).

Am Ende erhält man die Meldung "BUILD SUCCESSFUL".

Im Spielervorlage-Ordner findet man dann im Unterordner "build" die JAR
Datei im Ordner "jar" sowie die fertig gepackte ZIP-Datei im Ordner
"zipped", die direkt ins :t[Wettkampfsystem]{#contest} hochgeladen werden kann.

## Ruby

Da Ruby eine interpretierte Sprache ist, muss der Ruby-Quellcode direkt
in ein ZIP-Archiv gepackt und auf das :t[Wettkampfsystem]{#contest} hochgeladen
werden. Auf dem :t[Wettkampfsystem]{#contest} ist ein Ruby-Interpreter sowie das
aktuellste `software_challenge_client` Gem installiert. Alle weiteren
Bibliotheken müssen im ZIP-Archiv vorhanden sein. Nach dem Hochladen des ZIP-Archivs muss die auszuführende Hauptdatei in :t[Wettkampfsystem]{#contest}
ausgewählt werden. Diese wird dann zum Start des Computerspielers
gestartet. Damit dies richtig funktioniert, ist es entscheidend, dass
die Hauptdatei mit einer sogenannten "Shebang"-Zeile beginnt:

    #!/usr/bin/env ruby

Weiterhin ist es ratsam, den Magic-Comment zum Encoding direkt unter die
Shebang-Zeile zu schreiben:

    # encoding: UTF-8

Ein vollständiges Beispiel für einen abgabefertigen Ruby-Computerspieler
gibt es im [example Verzeichnis des Computerspieler-Gems bei
Github](https://github.com/software-challenge/client-ruby/tree/main/example).
Packt man die beiden Dateien `client.rb` und `main.rb` in ein
ZIP-Archiv, hat man einen abgabefertigen Computerspieler. Beim Hochladen
wählt man `main.rb` als Hauptdatei.

## C#

Stellt sicher, dass euer Projekt nur Libraries nutzt, die auch in [Mono
5.4](#ausfuehrungsumgebungen) enthalten sind. Kompiliert bzw. erstellt
euer Projekt und fügt zu dem build eine start.sh hinzu, wie
[nachfolgend](#andere-sprache) beschrieben, sofern ihr noch keine habt,
da der Wettkampfserver eure exe sonst nicht ausführen kann. Sie sollte
folgenden Inhalt haben, um eure exe mit mono zu starten:

    #!/bin/sh
    chmod +x csharp_exe_dateiname
    mono csharp_exe_dateiname "$@"

## Andere Programmiersprache

Bei Computerspielern in einer anderen Programmiersprache muss das
ZIP-Archiv ein Shell-Script mit genauen Instruktionen zum Start des
Computerspielers enthalten. Dieses muss nach dem Hochladen im
Wettkampfystem als Hauptdatei ausgewählt werden.

Beachten Sie hierbei, dass diesem Script vom :t[Wettkampfsystem]{#contest} Parameter
übergeben werden, die an Ihr Programm weitergegeben werden müssen. Diese
Parameter sind mindestens Host und Port des Spielservers sowie die
Reservierungsnummer des Spiels, dem der :t[Computerspieler]{#player} beitreten soll.
Ein Aufruf sieht also in etwa wie folgt aus (falls `start.sh` als
Hauptdatei eingestellt ist):

    start.sh -h gameserver -p 13050 -r 590e5e6f-cf93-488e-a12d-5c194ecf95c2

Die Parameter folgen dabei den [GNU Getopt
Konventionen](https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html#Argument-Syntax).
Das heißt, die drei Parameter können in beliebiger Reihenfolge und als
kurze oder lange Version übergeben werden. Folgende Variante muss also
auch von Ihrem Programm korrekt verarbeitet werden können:

    start.sh --reservation 590e5e6f-cf93-488e-a12d-5c194ecf95c2 --host gameserver --port 13050

Auch der Server mit grafischer Oberfläche ruft Ihr Programm mit diesen
Parametern auf. Ihr Programm wird also nur von der grafischen Oberfläche
richtig gestartet, wenn es die Parameter richtig verarbeitet.

Für die meisten Programmiersprachen gibt es Bibliotheken, die die
Kommandozeilenparameter nach diesem Schema verarbeiten können, sie
müssen diese Funktion also nicht unbedingt selbst implementieren.

Weiterhin ist es wichtig, den Interpreter in der ersten Zeile des Script
anzugeben, da das Script nicht von einer Shell aufgerufen wird. Ein
`start.sh` Script sieht also in etwa so aus:

    #!/bin/sh
    chmod +x hauptprogramm_dateiname
    ./hauptprogramm_dateiname "$@"

-   Script soll von `/bin/sh` interpretiert werden, es ist also ein
    einfaches Shell-Script.

-   Die Binärdatei wird ausführbar gemacht (das ist nötig, da in einem
    ZIP-Archiv das Ausführbar-Attribut nicht gespeichert wird).

-   Die Binärdatei wird aufgerufen und alle Parameter, die das Script
    bekommen hat, werden weitergereicht (`"$@"`).

Die `start.sh` muss in UTF-8 und mit UNIX(LF) Zeilenenden kodiert sein.
Andere Kodierungen führen zu Fehlern bei der Ausführung auf dem Server.
In Notepad++ kann die Kodierung einfach in dem Tab `Kodierung` angepasst
werden, die Zeilenenden in `Bearbeiten > Format Zeilenende`.

Bei compilierten Sprachen müssen die :t[Computerspieler]{#player} für 64bit Linux
compiliert werden, bei interpretierten Sprachen muss ein passender
Interpreter auf dem :t[Wettkampfsystem]{#contest} vorhanden sein. Weiterhin müssen
Abhängigkeiten wie z.B. genutzte Bibliotheken vorhanden sein oder
mitgeliefert werden.
