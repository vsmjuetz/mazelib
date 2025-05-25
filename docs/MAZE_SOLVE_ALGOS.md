# Algorithmen zur Lösung von Labyrinthen

##### Zurück zum Haupt [README](.. /README.md)

Da Benutzer Labyrinthe auf so unterschiedliche Weise erstellen und modifizieren dürfen, unterstützt die `mazelib`-Bibliothek nur universelle Algorithmen zur Lösung von Labyrinthen. Das bedeutet, dass `mazelib` keinen Labyrinth-Lösungsalgorithmus implementiert, der beispielsweise unvollkommene Labyrinthe (die mit Schleifen oder mehr als einer Lösung) nicht lösen kann. Andernfalls muss der Benutzer interne Details über die von ihm verwendeten Labyrinth-Generierungs- / Soliving-Algorithmen kennen und ob sie kompatibel sind.

## Kettenalgorithmus

###### Der Algorithmus

1. Zeichnen Sie eine gerade Linie von Anfang bis Ende, ignorieren Sie die Wände.

2. Folgen Sie der Linie von Anfang bis Ende.

1. Wenn du gegen eine Wand stößt, musst du herumgehen.

2. Senden Sie Backtracking-Roboter in die 1 oder 2 offenen Richtungen.

1. Wenn der Roboter Ihren neuen Punkt finden kann, fahren Sie fort.

2. Wenn der Roboter Ihre Linie an einem Punkt kreuzt, der weiter stromabwärts liegt, nehmen Sie den Weg dort auf.

3. Wiederholen Sie Schritt 2, bis Sie am Ende sind.

1. Wenn beide Roboter zu ihrem ursprünglichen Standort und ihrer ursprünglichen Richtung zurückkehren, ist das Labyrinth unlösbar.

###### Optionale Parameter

* *Drehen*: Zeichenfolge ['links', 'rechts']

* Möchten Sie der rechten Wand oder der linken Wand folgen? (Standard 'rechts')

###### Ergebnisse

* 1 Lösung

* Nicht die kürzeste Lösung

* Wirkt gegen unvollkommene Labyrinthe

###### Notizen

Die Idee hier ist, dass Sie das Labyrinth in eine Abfolge kleinerer Labyrinthe aufteilen. Es gibt zweifellos Fälle, in denen dies hilft, und Fälle, in denen dies eine schreckliche Idee ist. Verhindert Emptor.

Dieser Algorithmus verwendet den Wall Follower-Algorithmus, um die Sub-Mainths zu lösen. Daher ist es deutlich komplizierter und speicherintensiver als Ihr Standard-WallFollower.

## Kollisionslöser

###### Der Algorithmus

1. Schritt durch das Labyrinth und überflutet alle Richtungen gleichermaßen

2. Wenn sich zwei Überschwemmungspfade treffen, bilden Sie eine Mauer, an der sie sich treffen

3. Füllen Sie alle Sackgassen auf

4. Wiederholen, bis es keine Kollisionen mehr gibt

###### Ergebnisse

* Findet die kürzesten Lösungen

* Funktioniert nicht immer gegen unvollkommene Labyrinthe

###### Notizen

In einem perfekten Labyrinth unterscheidet sich dies kaum vom Dead End Filler-Algorithmus. Aber in stark geflochtenen und unvollkommenen Labyrinthen durchsucht dieser Algorithmus einfach noch ein paar Mal das gesamte Labyrinth und findet die optimalen Lösungen. Es ist ziemlich elegant.

## Zufällige Maus

###### Der Algorithmus:

Eine Maus wandert einfach zufällig durch das Labyrinth, bis sie den Käse findet.

###### Ergebnisse

* 1 Lösung

* Nicht die kürzeste Lösung

* Wirkt gegen unvollkommene Labyrinthe

###### Notizen

Eine zufällige Maus wird vielleicht nie fertig. Technisch. Es ist zwar zeitlich ineffizient, aber im Gedächtnis sehr effizient.

Ich empfehle dringend, dass dieser Solver im Standardschnittmodus ausgeführt wird, um alle unnötigen Zweige und Backtracks in der Lösung loszuwerden.

## Rekursiver Backtracker

###### Der Algorithmus:

1) Wählen Sie eine zufällige Richtung und folgen Sie ir

2) Gehen Sie zurück, wenn und nur wenn Sie eine Sackgasse treffen.

###### Ergebnisse

* 1 Lösung

* Nicht die kürzeste Lösung

* Wirkt gegen unvollkommene Labyrinthe

###### Notizen

Mathematisch gibt es nur sehr wenig Unterschied zwischen diesem Algorithmus und Random Mouse. Der einzige Unterschied besteht darin, dass die zufällige Maus an jedem Punkt dorthin zurückkehren kann, wo sie herkam. Aber Backtracker wird das nur tun, wenn es eine Sackgasse erreicht.

## Kürzester Pfadfinder

###### Der Algorithmus:

1) Erstellen Sie eine mögliche Lösung für jeden Nachbarn der Startposition

2) finden Sie die Nachbarn des letzten Elements in jeder Lösung, Zweige erstellen neue Lösungen

3) Wiederholen Sie Schritt 2, bis Sie das Ende erreichen

4) Die erste Lösung, die das Ende erreicht, gewinnt.

###### Ergebnisse

* Findet alle Lösungen

* Findet die kürzeste(n) Lösung(en)

* Wirkt gegen unvollkommene Labyrinthe

###### Notizen

In CS-Begriffen handelt es sich um einen Broadth-First-Suchalgorithmus, der gekürzt wird, wenn die erste Lösung gefunden wird.

## Kürzeste Pfade Finder

###### Der Algorithmus

1) Erstellen Sie eine mögliche Lösung für jeden Nachbarn der Startposition

2) finden Sie die Nachbarn des letzten Elements in jeder Lösung, Zweige erstellen neue Lösungen

3) Wiederholen Sie Schritt 2, bis alle Lösungen in die Sackgassen kommen oder das Ende erreichen

4) Entfernen Sie alle Sackgassenlösungen

###### Ergebnisse

* Findet alle Lösungen

* Wirkt gegen unvollkommene Labyrinthe

###### Notizen

In CS-Begriffen ist dies ein Broadth-First-Suchalgorithmus. Es findet alle einzigartigen, nicht geschleiften Lösungen für das Labyrinth.

Obwohl diese Version optimiert ist, um die Geschwindigkeit zu verbessern, konnte nichts gegen die Tatsache unternommen werden, dass dieser Algorithmus wesentlich mehr Speicher verbraucht als nur das Labyrinthgitter selbst.

## Der Algorithmus von Trémaux

###### Der Algorithmus

1) Jedes Mal, wenn Sie eine Zelle besuchen, markieren Sie sie einmal.

2) Wenn Sie eine Sackgasse erreichen, drehen Sie sich um und gehen Sie zurück.

3) Wenn Sie eine Kreuzung treffen, die Sie noch nicht besucht haben, wählen Sie zufällig eine neue Passage aus.

4) Wenn Sie einen neuen Durchgang hinuntergehen und auf eine Kreuzung treffen, die Sie besucht haben, behandeln Sie sie wie eine Sackgasse und gehen Sie zurück.

5) Wenn Sie eine Passage hinuntergehen, die Sie zuvor besucht haben (d. h. einmal markiert) und Sie auf eine Kreuzung treffen, nehmen Sie einen neuen Durchgang, der verfügbar ist, ansonsten nehmen Sie einen alten Durchgang (d. h. einmal markiert).

6) Wenn Sie endlich das Ende erreichen, folgen Sie den markierten Zellen einmal genau zurück zum Anfang.

7) Wenn das Labyrinth keine Lösung hat, befinden Sie sich am Anfang mit allen Zellen, die zweimal markiert sind.

###### Ergebnisse

* Findet eine nicht optimale Lösung.

* Arbeitet gegen unvollkommene Labyrinthe.

###### Notizen

Diese Methode zur Lösung des Labyrinths soll von einem Menschen im Labyrinth verwendet werden.

## Wortschatz

1. __cell__ - ein offener Durchgang im Labyrinth

2. __grid__ - das Gitter ist die Kombination aller Durchgänge und Barrieren im Labyrinth

3. __perfect__ - ein Labyrinth ist perfekt, wenn es nur eine Lösung hat

4. __wall__ - eine unpassierbare Barriere im Labyrinth

##### Zurück zum Haupt [README](.. /README.md)