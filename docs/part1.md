# Infrastructure as Code

Als ersten Schritt wirst du dich auf der Weboberfläche der *Ansible Automation Platform* bewegen. Hier wirst du klassische *Ops*-Aufgaben (Betriebsaufgaben) durchführen, du wirst im UI einige Ressourcen erstellen, um anschließend einen Automatisierungsjob ausführen zu können.  
Mit Code selbst kommst du erstmal nicht in Berührung, der ist bereits vorbereitet (vom *Dev*-Team), du musst jetzt aber den Entwickler-Code in der Automatisierungs-Plattform für dich und deine *Ops*-Kollegen ausführbar machen.

Du wirst in der Ansible Automation Platform die folgenden Ressourcen erstellen:

* **Project** - Hiermit kann die AAP den Code herunterladen
* **Credential** - Die Automatisierung braucht Nutzernamen und Passwort
* **Job-Template** - Hier wird definiert was genau während der Automatisierung passieren soll

## 1. Projekt erstellen

In der AAP Oberfläche auf der linken Seite, den Abschnitt **Automation Execution (Automation Controller)** erweitern und Punkt **Projects** wählen.  
Klicke oben auf den Button **Create project**, es öffnet sich eine Maske mit Eingabe-Feldern. Du musst lediglich die mit einem roten Sternchen markierten Felder zwingend ausfüllen, alles weitere ist optional.

Gib deinem Projekt einen Namen, z.B. `QEP Repository`, als *Organization* wählst du `Default`.

Wähle aus der Liste **Source Control Credential Type** den Punkt **Git**, es werden dadurch weitere Felder angezeigt.

Im Feld **Source Control URL** trägst du die folgende Adresse ein:

```text
https://github.com/computacenter-com/qep-devops-automation.git
```

!!! tip
    Du kannst die Adresse rechts mit einem kleinen Button (*In Zwischenablage kopieren*) kopieren.

Im Feld **Source Control Branch/Tag/Commit** den Wert `dev` eintragen.  

Bei den *Checkboxen* unter **Options** wählst du **Update Revision on Launch** aus.

Abschließend wählst du unten den Button **Create project**.

??? info "**Wozu das Ganze?**"
    Ein *Project* in der AAP zeigt auf ein *Repository*, quasi einen (versionskontrollierten) Ordner mit allem *Code* für die Automatisierung.  
    In unserem Fall liegt der gesamte *Code* für die Ansible Automatisierung in einem (öffentlich zugänglichen) *Github* Repository, die AAP zieht sich also den Code aus diesem und sorgt (durch die Auswahl der Checkbox) dafür, dass vor jeder Ausführung der Automatisierung der aktuellste Stand geladen wird.

Es wird ein *Job* vom Typ *Source Control Update* gestartet, klicke auf der linken Seite auf **Jobs**. Nach einer kurzen Zeit endet der Job (hoffentlich) mit dem Status **Success**.

## 2. Credential erstellen

Das *Project* (der Code) enthält ein sogenanntes *Playbook* für die automatisierte Konfiguration der Ansible Automation Platform (*Automate the Automation* :smile:).  

In der AAP Oberfläche auf der linken Seite, den Abschnitt **Automation Execution (Automation Controller)** erweitern und jetzt den Abschnitt **Infrastructure** erweitern. Hier den Punkt **Credentials** wählen.  
Klicke oben auf den Button **Create credential**, es öffnet sich eine Maske mit Eingabe-Feldern.

Das neue Credential bekommt den Namen **Controller Access**, als *Credential Type* wählst du **Red Hat Ansible Automation Platform**.

Es werden weitere Felder geöffnet, im Feld **Red Hat Ansible Automation Platform** wählst du die *Console*-URL-Adresse aus der Workshop-Übersichts-Seite (sieht ähnlich aus wie `https://controller.wxyz.sandbox123.opentlc.com`). Auch den Wert für **Benutzername** und **Passwort** findest du dort.

Unter **Options** die Checkbox **Verify SSL** aktivieren!

Abschließend den **Save**-Button verwenden.

??? info "**Wozu das Ganze?**"
    Damit die Automatisierung ausgeführt werden kann, muss das Tool (hier die AAP selbst) sich *authentifizieren* (anmelden), dafür ist ein Nutzer-Name und Passwort notwendig, welches als *Credential* hinterlegt werden muss.

## 3. Job Template erstellen

Nachdem der Code in der AAP verfügbar ist und das passende Credential erstellt wurde, wollen wir die Automatisierung auch ausführen können. Dazu wird ein *Job Template* erstellt.  

In der AAP Oberfläche auf der linken Seite, unter dem Abschnitt **Automation Execution (Automation Controller)**, den Punkt **Templates** wählen.  
Klicke oben auf den Button **Create template** und **Create job template**, es öffnet sich eine Maske mit Eingabe-Feldern.

Die folgende Tabelle zeigt alle zu befüllenden Felder:

<!-- markdownlint-disable MD056 MD060 -->
| Feld            | Inhalt                                                    | Tipps                                                          |
| --------------- | --------------------------------------------------------- | -------------------------------------------------------------- |
| **Name**        | `Controller Automation`                                   |                                                                |
| **Job Type**    | `Run`                                                     |                                                                |
| **Inventory**   | `Workshop Inventory`                                      | Klicke auf die Lupe und<br>wähle das vorhandene Inventory.     |
| **Project**     | `QEP Repository`                                          | Klicke auf die Lupe und<br>wähle das passende Repository.      |
| **Playbook**    | `playbook_controller_automation.yml`                      | Wähle das passende Playbook<br> aus dem Drop-Down-Menü.        |
| **Credentials** | `Controller Access | Red Hat Ansible Automation Platform` | Die *Category Red Hat Ansible<br> Automation Platform* wählen. |
<!-- markdownlint-disable MD056 MD060-->

Speichert euer Job Template mit dem Button **Create job template**.

??? info "**Wozu das Ganze?**"  
    In einem *Job Template* wird ein Automatisierungs-*Run* festgelegt, damit lässt sich die Automatisierung später ausführen (so oft wir wollen!).  
    Dazu müssen wir festlegen, welchen Code wollen wir ausführen wollen. Wir verweisen also auf das *Project* und wählen daraus ein sogenantes *Playbook* (hier drin steht der wirkliche *Code*). Ebenfalls müssen wir festlegen *wo* wir den Code ausführen wollen, das steht im *Inventory*. Das *Credential* legt fest *wie* wir uns mit dem Zielhost (oder den *vielen* Zielhosts) verbinden, wir müssen uns dort einloggen (*authentifizieren*).

## 4. Automatisierung ausführen

Alles ist vorbereitet, jetzt kann die Automatisierung ausgeführt werden!  
Wähle **Templates** aus. Neben eurem Job Template seht ihr ein kleines Raketen-Symbol (:fontawesome-solid-rocket:), klickt drauf und die Automatisierung läuft los!

Ist das *Playbook* (das Job Template) durchgelaufen, sind weitere Objekte in der AAP (Ansible Automation Platform) entstanden!  
Unter den **Templates** findest du jetzt unter anderem das *Workflow Template* `Deploy Webserver`.  

Du kannst das Playbook ausführen mit dem kleinen Raketen-Symbol (:fontawesome-solid-rocket:), du wirst mit einer Abfrage (*Survey*) begrüßt. Hier kannst du den Webserver personalisieren, allerdings nicht mit deinem eigenen Namen, das sollten wir ändern!

## 5. Issue erstellen

Damit das *dev*-Team weiss was zu tun ist, musst du ein *Issue* im Github-Code-Repository erstellen. Ein Issue klingt erst einmal nach einem Fehler, aber ein Issue kann auch ein *Feature*-Request sein, also eine Verbesserung des bestehenden Codes.  

Öffne den folgenden Link zum Github Repository: [https://github.com/TimGrt/qep](https://github.com/TimGrt/qep){ target=_blank }

Klicke auf den **Issues**-Tab und erstelle über den Button **New Issue**.  

!!! warning
    Nur mit einem Github Account kannst du einen Issue erstellen!  
    Die Anmeldung zu Github geht schnell (und für deine Entwicklungsaufgaben wirst du ohnehin einen Github Account brauchen).

Öffne den folgenden Link: [https://github.com/signup](https://github.com/signup){ target=_blank }

![Github Sign-Up Page](assets/images/GithubSignUp.png)

Nutze deine Computacenter E-Mail-Adresse, wähle ein sicheres Passwort und einen Nutzernamen (dein Nutzerkürzel, Spitzname, etc.).

Account ist erstellt und bestätigt? Super, dann kannst du jetzt, wie ursprünglich geplant, den Issue im QEP-Repository erstellen, hier ist nochmal der Link, direkt zur Issue-Seite: [https://github.com/computacenter-com/qep-devops-automation/issues](https://github.com/computacenter-com/qep-devops-automation/issues){ target=_blank }

Vergib einen aussagekräftigen Titel und beschreibe dein gewünschtes *Feature* (dein Name soll im *Survey* erscheinen, damit du den Webserver wie gewünscht personalisieren kannst). Den Issue über den **Create** Button unterhalb der Text-Box erstellen.
