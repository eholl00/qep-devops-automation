# Teil 2 - Arbeiten mit Git

In der vorherigen Übung hast du automatisiert ein Job-Template in der Ansible Automation Platform erstellt, welches über ein *Survey* (eine interaktive Abfrage) die Möglichkeit bietet, den Webserver (bzw. die vielen Webserver, es werden tatsächlich drei Webserver-Instanzen erstellt) zu personalisieren.  
Die Abfrage bietet aktuell nur einen einzelnen Namen an, damit auch dein Name dort auftaucht, musst du den Code selbst anpassen.  
Die Erstellung der Ressourcen war eine klassische *Ops*-Aufgabe, jetzt wirst eine *Developer*-Aufgabe übernehmen.  
Die Code-Anpassung erfordert, dass du dich mit dem Versionskontroll-Tool *Git* vertraut machst, die folgenden Schritte bereiten dich und deine Entwicklungsumgebung darauf vor.

## 1. VSCode Entwicklungsumgebung öffnen

Um den *Code* anzupassen, verwendest du VScode (eine IDE = Integrated Developer Environment), dort ist alles installiert was du zur Programmierung brauchst. Keine Sorge, wirklich programmieren musst du heute gar nicht.  

Auf der Übersichts-Seite des Workshops in der Red Hat Demo Umgebung den Link zu **VS Code** wählen, dein Trainer geht mit dir die einzelnen Schritte durch, um VSCode erstmalig einzurichten.

## 2. SSH-Schlüsselpaar erstellen

Um den notwendigen Code (das *Repository*) herunterladen zu können und, noch wichtiger, anpassen und wieder hochladen zu können, benötigst du ein *Schlüsselpaar*.  

Öffne ein Terminal in VSCode (in der Titelzeile **Terminal** und **New Terminal** wählen) und gib das folgende Kommando ein:

```console
ssh-keygen -t ed25519
```

!!! tip
    Kopiere dir das Kommando über den *Copy*-Button rechts im Code-Fenster.

Die Abfragen kannst du einfach mit ++enter++ bestätigen (drei Mal bestätigen), die Ausgabe sieht in etwa so aus:

```{ .console .no-copy }
[student1@ansible-1 qep]$ ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/student1/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/student1/.ssh/id_ed25519.
Your public key has been saved in /home/student1/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:8HXsZw6mD5m6PNl6WHoiYy0B/Il+1zRHVrfcpR+rZpI student1@ansible-1.example.com
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|           .  . o|
|   .  .   . o..o+|
|    o  o . oo .+.|
|     + .S  o+ o.o|
|    . +   +=.= ..|
|   .   o O=o. o  |
|    . *.O.=E +   |
|     o ==B  =    |
+----[SHA256]-----+
```

Du hast ein neues SSH (Secure Shell) Schlüsselpaar erstellt, einen *privaten* Schlüssel und einen *öffentlichen* Schlüssel (mit der Endung `.pub` für *public*), welchen du gefahrlos verbreiten darfst. Wir werden diesen öffentlichen Schlüssel im nächsten Schritt benötigen, du kannst ihn dir bereits einmal auf der Kommandozeile anzeigen lassen, von dort kannst du ihn gleich markieren und kopieren.

```console
cat ~/.ssh/id_ed25519.pub
```

??? example "Beispielausgabe"

    ```{ .console .no-copy }
    [student1@ansible-1 qep]$ cat ~/.ssh/id_ed25519.pub
    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDt+WFoUBWhs77m/784FaT+eqqavHf/Jz/+8DW04l2fP student1@ansible-1.example.com
    ```

## 3. Öffentlichen Schlüssel in Github hinterlegen

Den **öffentlichen** Schlüssel (mit der `.pub`-Endung) musst du in deinem Github-Account hinterlegen.

Im Github-Browser-Fenster, klicke rechts oben auf dein Nutzer-Bild und wähle **Settings**.  

Auf der linken Seite (unter dem Punkt *Access**), klicke auf **SSH and GPG keys**. Noch sind keine Keys hinterlegt, klicke auf den Button **New SSH key**.  

Kopiere den Inhalt des **öffentlichen** Schlüssel, als Titel kannst du *QEP* hinterlegen. Wenn alles eingefügt ist, klicke unten auf den Button **Add SSH key**.

Du solltest von deinem Trainer als Entwickler zum Projekt hinzugefügt worden sein, in der folgenden Aufgabe lädst du dir den Code auf deine Entwicklungsumgebung.

## 4. Repository *forken*

Öffne das [QEP-Repository in deinem Browser (click me)](https://github.com/computacenter-com/qep-devops-automation){ .target=blank}.

Erstelle einen **Fork** des Original-Repositories, klicke dazu auf den entsprechenden Button (neben dem Namen des Repos befindet sich der Button neben dem *Star* Button ganz rechts) oder nutze [diesen Link](https://github.com/computacenter-com/qep-devops-automation/fork).

Ein Fork ist ein neues Repository, das denselben Code und dieselben Sichtbarkeitseinstellungen verwendet wie das ursprüngliche *Upstream-Repository*.

Der *Owner* bist du selbst, der Name des Repositories kann ebenfalls gleich bleiben. Klicke den grünen Button **Create fork**.

## 5. Vom Remote Repository zum lokalen Repository

Das Projekt (deine Kopie) vom *Remote Repository* herunterladen (*klonen*). Klicke dazu auf den grünen Button **<> Code**, wähle im *Local* Tab den **SSH** Tab und kopiere dir die komplette Adresse (am besten über den Button rechts neben dem Feld).

Zunächst in das *Home-Directory* des Nutzers wechseln. Im Terminal das folgende Kommando eingeben:

```console
cd
```

!!! tip

    `cd` heißt *change directory*, ohne weitere Parameter wechselt das Kommando immer in das Home Directory des aktuellen Nutzers.

Auf der Kommandozeile/im Terminal, nutze das `git clone` Kommando und füge die Adresse dahinter ein. Das komplette Kommando sieht in etwa so aus:

```{ .console .no-copy }
git clone git@github.com:TimGrt/qep-devops-automation.git
```

Ein neuer Ordner ist entstanden, öffne ihn indem du im Menü auf **File** und **Open Folder...** klickst und hier den Ordner wählst. In der Zeile sollte also `/home/student1/qep-devops-automation/` stehen, wähle den Button **OK** daneben.  
Das VScode-Fenster wird sich erneut laden, auf der linken Seite siehst du dann den Inhalt des Repositories. Öffne wieder ein Terminal.

Du bist jetzt in einem *git-versionierten* Ordner, prüfe mit dem Kommando `git status`, der Output sollte folgendermaßen aussehen:

``` { .console .no-copy }
[student1@ansible-1 qep-devops-automation]$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean
```

Ok, nach den ganzen Vorbereitungen kannst du im nächsten Schritt dann endlich die Code-Anpassungen vornehmen!
