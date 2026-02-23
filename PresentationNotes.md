# Notizen

## TODO

* Alle Abbildungen - Nummerieren
* Quellen prüfen (sind alle Angaben gemacht? Sind die Quellen korrekt?)

## Backup Folien

* Drücke Escape 
* Jetzt erscheint eine Übersicht aller Folien für die schnelle Navigation zu den Backup Folien

## Folie - Was ist RISC-V

Softwareentwicklung (Compiler, Bibliotheken und Betriebssysteme) können unabhängig von der Hardware (CPUs) entwickelt werden.
Wenn man Software und Hardware dann schließlich zusammenbringt läuft die Software auf der Hardware.
Damit könnte man mehrere Teams unabhängig voneinander beschäftigen.

## Folie - RISC-V - Zeitliche Einordnung

Die Historie ist viel größer, dies stellt nur einen Ausschnitt dar der zur Einordnung reichen soll.
Intel ist das Urgestein. hat mit 8086, 386, 486 begonnen.
AMD ist Intel erst nachgefolgt und nun mit Ryzen und Threadripper dominant auf dem Bereich der privaten Endkunden.
ARM ist ähnlich zu RISC-V. Ist auch eine ISA aber mit kostenpflichtiger Lizenz.

Im Jahre 2026 stehen alle Teilnehmer vor dem gleichen Problem. Wie unterstützt man LLMs und KI.

RVV Ende 2021 ratifiziert.

## Folie - Inhalt der Arbeit

0. Das vorhandene, open-source NEORV32 Design existiert
1. Verwende den VHDL Code und Erweitere um Matrix-Extension
2. Übersetzen mit der Vivado-Toolchain (IDE, Synthese, Routing, ..., Bitstream upload)
3. Test auf eine FPGA.

Ein FPGA ist in den allermeisten Fällen ein Zwischenschritt für das Prototyping.
Ein FPGA ist ein Chip der durch Software programmiert, seine Struktur ändert.
Die Struktur sind eine große Menge an logischen Gattern.
Es zeigt sich, dass z.B. eine CPU durch logische Gatter beschrieben werden kann.
Auf dem FPGA kann man die CPU ausprobieren, bis sie korrekt arbeitet.
Danach übertrag in fixe Chips.

Danach werden Testprogramme ausgeführt und die Geschwindigkeit wird gemessen.

## Folie - Motivation

Warum braucht jeder Wettbewerber die Matrixmultiplikation?

## Folie - Neuronale Netze (1/2)

N/A

## Folie - Neuronale Netze (2/2)

N/A

## Folie - 3D Transformationen

N/A

## Folie - Vorgehen nach Mathematischer Vorschrift

Könnte mit drei geschaltelten For-Schleifen programmiert werden.
Die Frage ist, ob diese implementierung performant ist.

## Folie - Nachteile und Lösungsansätze

1. Beschreibe den Cache

Problem: Wenn eine Matrix zeilenweiße im Speicher abgelegt ist, dann ist die erste Matrix Cache-lokal
aber die zweite nicht, weil die zweite Matrix über Spalten und nicht über Zeilen zugegriffen wird.

Die Cache-Lokalität wird durch Mathe-Bibliotheken organisiert! Sie packen Matrizen um.
Das ist nicht unsere Aufgabe. Die Matrix-Extension wird lediglich eine konkrete Matrix A mit einer konkreten Matrix B multiplizieren.
Das bedeutet, die Matrix-Extension kommt im "Kernel" zum Einsatz.

Unsere Aufgabe ist es SIMD effizient zu ermöglichen.

Single Instruction Multiple Data (SIMD)

## Folie - SISD vs. SIMD

## Folie - Strip Mining (1/3)

Wie macht es die Vektor-Extension?
Wie wird SIMD herbeigeführt?
Man lädt einen ganzen Bereich in den CPU (Daher gibt es SIMD-LOAD instructions in der Extension)
Dann verarbeitet man den ganzen Bereich (SIMD-OPERATION)
Dann speichert man alles wieder zurück (SIMD-STORE)

## Folie - Strip Mining (2/3)

Der Compiler muss das Generieren können!

## Folie - Strip Mining (3/3)

Hier sind die neuen Anweisungen die durch die Arbeit hinzugefügt werden
Es gibt load, operation und store mit SIMD
Die Zahlen sind da um den Datentyp zu beschreiben (32-bit)

## Folie - RISC-V Ansätze zur Matrix-Erweiterung

https://www.youtube.com/watch?v=2kdhMMQ6eZU&t=223s

Task Forces, Interest Groups, Charter

Vector-Batch Dot Product
IME TG - Integrated Matrix Extensions (TG = Task Group)
VME TG - Vector-Matrix-Extension (TG = Task Group)
AME TG - Attached Matrix Extension (TG = Task Group)

Vector-Batch Dot Product
- No new state
- up to eight dot products in parallel. Each VLEN bits of input.
- A is a vector register containing a row of A
- B is a group of vector registers containing all columns of Matrix B
- Compute up to eight outputs of Matrix C
- A new instruction is added to compute up to eight of these aformentioned results.

IME - Integrated Matrix Extensions 
- Each input and output is a vector register
- Matrix A goes into one register
- Matrix B goes into one register
- Matrix C is computed into one register
- An instruction is added that knows how the matrixes are layed out and computes C.
- Problem: Bits to coordinate C can become very large.

VME - Vector-Matrix-Extension
- new State for the C Accumulator is added.
- Matrix A and Matrix B use vector registers.
- Uses outer-product multiply (dyadisches Produkt)

AME
- adds completely new state for all matrices A, B and C
- No RVV needed

## Folie - VHDL Entities

Oben rechts befindet sich der Instruktionsspeicher aus dem das Frontend Anweisunge lädt und der Execution Engine Präsentiert.

Die Execution Engine / CPU Control wird drei neue Anweisungen verstehen
- Load und Store werden an die LoadStore Unit weitergegeben
- Die Matrixmultiplikationsoperation wird an die Matrix-Engine geschickt. (Sie request/response zustände der StateMachine)

Ein Load lädt 16 Werte aus dem RAM/Cache über den Bus über die BusRequests, die die LSU erstellen muss in die Matrix Register der
Matrix Engine. Die Matrix Register sind auf der linken Seite in der Matrix Engine dargestellt.
Der PROC_RAM prozess führt all diese Übertragungen durch.

Wenn die Matrixmultiplikation ausgeführt werden muss, dann lädt die Matrixengine eine Spalte und eine Zeile in die Arbeitsregister.
Die Arbeitsregister sind auf der rechten Seite in der Matrixengine dargestellt. 

Der Prozess PROC_MATRIX_MUL führt die Multiply Accumulate Operation durch und berechnet die Matrix.
Die Berechnete Matrix wird zurück in die Matrixregister übertragen.

Eine Store-Anweisung lädt die Daten aus den Matrixregistern über den Bus über die Bus-Requests der LSU zurück in den Cache bzw. den RAM.

