# Standard Go Project Layout

Translations:

* [한국어 문서](README_ko.md)
* [简体中文](README_zh.md)
* [正體中文](README_zh-TW.md)
* [简体中文](README_zh-CN.md) - ???
* [Français](README_fr.md)
* [日本語](README_ja.md)
* [Portuguese](README_ptBR.md)
* [Español](README_es.md)
* [Română](README_ro.md)
* [Русский](README_ru.md)
* [Türkçe](README_tr.md)

## Overview

Dies ist ein grundlegendes Layout für Go-Anwendungsprojekte. Es ist kein offizieller Standard, der vom Go-Entwicklungsteam definiert wurde; es handelt sich jedoch um eine Reihe gängiger historischer und neuer Projekt-Layout-Muster im Go-Ökosystem. Einige dieser Muster sind beliebter als andere. Es hat auch eine Reihe von kleinen Verbesserungen zusammen mit mehreren unterstützenden Verzeichnissen, die für jede ausreichend große Anwendung in der realen Welt üblich sind.

**`Wenn Sie versuchen, Go zu lernen oder wenn Sie einen Proof of Concept (PoC) oder ein einfaches Projekt für sich selbst erstellen, ist dieses Projektlayout unverhältnismäßig. Beginnen Sie stattdessen mit etwas wirklich Einfachem (eine einzige main.go und go.mod Datei sind mehr als genug).`** Wenn Ihr Projekt wächst, denken Sie daran, dass es wichtig ist, Ihren Quellcode gut zu strukturieren, da Sie sonst mit einem unordentlichen Quellcode mit vielen versteckten Abhängigkeiten und globalen Zuständen enden werden. Wenn mehr Leute an dem Projekt arbeiten, brauchen Sie noch mehr Struktur. In diesem Fall ist es wichtig, eine gemeinsame Methode zur Verwaltung von Paketen/Bibliotheken einzuführen. Wenn Sie ein Open-Source-Projekt haben oder wenn Sie wissen, dass andere Projekte den Quellcode aus Ihrer Repository importieren, dann ist es wichtig, dass Sie private (auch `internal`) Pakete und Quellcode haben. Klonen Sie das Repository, behalten Sie, was Sie brauchen und löschen Sie den restlichen Quellcode! Nur weil es da ist, heißt das nicht, dass Sie alles verwenden müssen. Keines dieser Muster wird in jedem einzelnen Projekt verwendet. Selbst das `Vendor` Muster ist nicht universell.

Mit Go 1.14 sind [`Go-Module`](https://github.com/golang/go/wiki/Modules) endlich produktionsreif. Verwenden Sie [`Go-Module`](https://blog.golang.org/using-go-modules) es sei denn, Sie haben einen bestimmten Grund, sie nicht zu verwenden, und wenn Sie es tun, dann müssen Sie sich nicht um $GOPATH und wo Sie Ihr Projekt speichern kümmern. Die grundlegende `go.mod` Datei im Projektverzeichnis geht davon aus, dass Ihr Projekt auf GitHub gehostet wird, aber das ist keine Voraussetzung. Der Modulpfad kann beliebig sein, aber die erste Modulpfadkomponente sollte einen Punkt im Namen haben (die aktuelle Version von Go erzwingt dies nicht mehr, aber wenn Sie etwas ältere Versionen verwenden, seien Sie nicht überrascht, wenn Ihre Kompilierungen ohne diesen Punkt fehlschlagen). Siehe Problem `37554`(https://github.com/golang/go/issues/37554) und `32819`(https://github.com/golang/go/issues/32819), wenn Sie mehr darüber wissen wollen.

Dieses Projektlayout ist absichtlich allgemein gehalten und versucht nicht, eine bestimmte Go-Paketstruktur vorzuschreiben.

Dies ist ein Gemeinschaftsprojekt. Eröffnen Sie ein Problem, wenn Sie ein neues Muster sehen oder wenn Sie denken, dass eines der bestehenden Muster aktualisiert werden muss.

Wenn Sie Hilfe bei der Benennung, Formatierung und dem Stil benötigen, führen Sie [`gofmt`](https://golang.org/cmd/gofmt/) und [`golint`](https://github.com/golang/lint) aus. Lesen Sie auch diese Go-Code-Stilrichtlinien und Empfehlungen:

* https://talks.golang.org/2014/names.slide
* https://golang.org/doc/effective_go.html#names
* https://blog.golang.org/package-names
* https://github.com/golang/go/wiki/CodeReviewComments
* [Style guideline for Go packages](https://rakyll.org/style-packages) (rakyll/JBD)

siehe [`Go Project Layout`](https://medium.com/golang-learn/go-project-layout-e5213cdcfaa2) für weitere Hintergrundinformationen.

Mehr über die Benennung und Organisation von Paketen sowie andere Empfehlungen zur Codestruktur:

* [GopherCon EU 2018: Peter Bourgon - Best Practices for Industrial Programming](https://www.youtube.com/watch?v=PTE4VJIdHPg)
* [GopherCon Russia 2018: Ashley McNamara + Brian Ketelsen - Go best practices.](https://www.youtube.com/watch?v=MzTcsI6tn-0)
* [GopherCon 2017: Edward Muller - Go Anti-Patterns](https://www.youtube.com/watch?v=ltqV6pDKZD8)
* [GopherCon 2018: Kat Zien - How Do You Structure Your Go Apps](https://www.youtube.com/watch?v=oL6JBUk6tj0)

A Chinese Post about Package-Oriented-Design guidelines and Architecture layer
* [面向包的设计和架构分层](https://github.com/danceyoung/paper-code/blob/master/package-oriented-design/packageorienteddesign.md)

## Go Verzeichnisse 

### `/cmd`
Hauptanwendungen für dieses Projekt.

Der Name des Verzeichnisses für jede Anwendung sollte dem Namen der ausführbaren Datei entsprechen, die Sie haben möchten (z. B. `/cmd/myapp`).

Legen Sie nicht zu viel Code in das Anwendungsverzeichnis. Wenn Sie glauben, dass der Code importiert und in anderen Projekten verwendet werden kann, sollte er im Verzeichnis `/pkg` liegen. Wenn der Code nicht wiederverwendbar ist oder wenn Sie nicht wollen, dass andere ihn wiederverwenden, legen Sie den Code in das Verzeichnis `/internal`. Sie werden überrascht sein, was andere tun werden, also seien Sie explizit über Ihre Absichten!

Es ist üblich, eine kleine `main` funktion zu haben, die den Quellcode aus den Verzeichnissen `/internal` und `/pkg` importiert und aufruft und sonst nichts.

Beispiele finden Sie im Verzeichnis [`/cmd`](cmd/README.md).

### `/internal`

Privater Anwendungs- und Bibliothekscode. Dies ist der Quellcode, von dem Sie nicht möchten, dass andere ihn in ihre Anwendungen oder Bibliotheken importieren. Beachten Sie, dass dieses Layout-Muster durch den Go-Compiler selbst erzwungen wird. Siehe die Go 1.4 [`release notes`](https://golang.org/doc/go1.4#internalpackages) für weitere Details. Beachten Sie, dass Sie nicht auf das interne Verzeichnis der obersten Ebene beschränkt sind. Sie können mehr als ein `internal` Verzeichnis auf jeder Ebene Ihres Projektbaums haben.

Optional können Sie Ihren internen Paketen ein wenig zusätzliche Struktur hinzufügen, um Ihren gemeinsam genutzten und nicht gemeinsam genutzten internen Quellcode zu trennen. Das ist nicht erforderlich (vor allem bei kleineren Projekten), aber es ist schön, visuelle Hinweise auf die beabsichtigte Verwendung des Pakets zu haben. Ihr eigentlicher Anwendungscode kann in das Verzeichnis `/internal/app` (z.B. `/internal/app/myapp`) und der von diesen Anwendungen gemeinsam genutzte Code in das Verzeichnis `/internal/pkg` (z.B. `/internal/pkg/myprivlib`).

### `/pkg`

Bibliothekscode, der von externen Anwendungen verwendet werden darf (z. B. `/pkg/mypubliclib`). Andere Projekte werden diese Bibliotheken in der Erwartung importieren, dass sie funktionieren, also denken Sie zweimal nach, bevor Sie etwas hier ablegen :-) Beachten Sie, dass das `internal` Verzeichnis ein besserer Weg ist, um sicherzustellen, dass Ihre privaten Pakete nicht importierbar sind, da es von Go erzwungen wird. Das `/pkg` Verzeichnis ist immer noch ein guter Weg, um explizit mitzuteilen, dass der Code in diesem Verzeichnis für die Verwendung durch andere sicher ist. Der Blogbeitrag [`I'll take pkg over internal`](https://travisjeffery.com/b/2019/11/i-ll-take-pkg-over-internal/) von Travis Jeffery gibt einen guten Überblick über die Verzeichnisse pkg und internal und wann es sinnvoll ist, sie zu verwenden.

Es ist auch eine Möglichkeit, Go-Code an einem Ort zu gruppieren, wenn das Stammverzeichnis viele Nicht-Go-Komponenten und -Verzeichnisse enthält, was es einfacher macht, verschiedene Go-Tools auszuführen (wie in diesen Vorträgen erwähnt: [`Best Practices for Industrial Programming`](https://www.youtube.com/watch?v=PTE4VJIdHPg) von GopherCon EU 2018, [GopherCon 2018: Kat Zien - How Do You Structure Your Go Apps](https://www.youtube.com/watch?v=oL6JBUk6tj0) und [GoLab 2018 - Massimiliano Pippi - Project layout patterns in Go](https://www.youtube.com/watch?v=3gQa1LWwuzk)).

Sehen Sie sich das [`/pkg`](pkg/README.md) Verzeichnis an, wenn Sie sehen wollen, welche populären Go-Repos dieses Projekt-Layout-Muster verwenden. Dies ist ein gängiges Layout-Muster, aber es ist nicht allgemein akzeptiert und einige in der Go-Community empfehlen es nicht.

Es ist in Ordnung, es nicht zu verwenden, wenn Ihr App-Projekt wirklich klein ist und eine zusätzliche Verschachtelungsebene keinen großen Mehrwert bringt (es sei denn, Sie wollen es wirklich :-)). Denken Sie darüber nach, wenn Ihr Projekt groß genug ist und Ihr Stammverzeichnis ziemlich voll wird (vor allem, wenn Sie viele Nicht-Go-App-Komponenten haben).

Die Ursprünge des [`pkg`] Verzeichnisses: Der alte Go-Quellcode benutzte pkg für seine Pakete und dann begannen verschiedene Go-Projekte in der Community, das Muster zu kopieren (siehe [`diesen`](https://twitter.com/bradfitz/status/1039512487538970624) Tweet von Brad Fitzpatrick für mehr Kontext).

### `/vendor`


Anwendungsabhängigkeiten (manuell oder mit dem von Ihnen bevorzugten Tool zur Verwaltung von Abhängigkeiten, z. B. mit der neuen integrierten Funktion [`Go Modules`](https://github.com/golang/go/wiki/Modules)). Mit dem Befehl `go mod vendor` wird das Verzeichnis `/vendor` für Sie erstellt. Beachten Sie, dass Sie möglicherweise das Flag `-mod=vendor` zu Ihrem `go build` Befehl hinzufügen müssen, wenn Sie nicht Go 1.14 verwenden, wo es standardmäßig aktiviert ist.

Übertragen Sie Ihre Anwendungsabhängigkeiten nicht, wenn Sie eine Bibliothek bauen.

Beachten Sie, dass Go seit Version [`1.13`](https://golang.org/doc/go1.13#modules) auch die Modul-Proxy-Funktion aktiviert hat (und standardmäßig [`https://proxy.golang.org`](https://proxy.golang.org) als Modul-Proxy-Server verwendet). Lesen Sie [`hier`](https://blog.golang.org/module-mirror-launch) mehr darüber, um herauszufinden, ob es Ihren Anforderungen und Einschränkungen entspricht. Wenn dies der Fall ist, benötigen Sie das `Vendor` Verzeichnis überhaupt nicht mehr.

## Service Application Directories

### `/api`

OpenAPI/Swagger-Spezifikationen, JSON-Schemadateien, Protokolldefinitionsdateien.

Beispiele finden Sie im Verzeichnis [`/api`](api/README.md).

## Web Application Directories

### `/web`

Webanwendungsspezifische Komponenten: statische Webinhalte, serverseitige Vorlagen und SPAs.

## Common Application Directories

### `/configs`

Vorlagen für Konfigurationsdateien oder Standardkonfigurationen.

Legen Sie hier Ihre `confd` oder `consul-template` Vorlagendateien ab.

### `/init`

Systeminitiierung (systemd, upstart, sysv) und Prozessmanager/-überwacher (runit, supervisord) konfigurieren.

### `/scripts`

Skripte zur Durchführung verschiedener Build-, Installations-, Analyse- usw. Operationen.

Diese Skripte halten die Makefile der Stammebene klein und einfach (z.B. [`https://github.com/hashicorp/terraform/blob/master/Makefile`](https://github.com/hashicorp/terraform/blob/master/Makefile)).

Siehe das Verzeichnis [`/scripts`](scrips/README.md) für Beispiele.

### `/build`

Paketierung und kontinuierliche Integration.

Legen Sie Ihre Cloud- (AMI), Container- (Docker) und Betriebssystem- (deb, rpm, pkg) Paketkonfigurationen und Skripte in das Verzeichnis `/build/package`.

Legen Sie Ihre CI-Konfigurationen und -Skripte (travis, circle, drone) in das Verzeichnis `/build/ci`. Beachten Sie, dass einige der CI-Tools (z.B. Travis CI) sehr wählerisch sind, was den Speicherort ihrer Konfigurationsdateien angeht. Versuchen Sie, die Konfigurationsdateien in das Verzeichnis `/build/ci` zu legen und sie mit dem Ort zu verknüpfen, an dem die CI-Tools sie erwarten (wenn möglich).

### `/deployments`

IaaS, PaaS, System- und Container-Orchestrierungskonfigurationen und -vorlagen (docker-compose, kubernetes/helm, mesos, terraform, bosh). Beachten Sie, dass in einigen Repos (insbesondere bei Anwendungen, die mit kubernetes bereitgestellt werden) dieses Verzeichnis `/deploy` heißt.

### `/test`
Zusätzliche externe Testanwendungen und Testdaten. Sie können das Verzeichnis `/test` nach Belieben strukturieren. Für größere Projekte ist es sinnvoll, ein Unterverzeichnis für Daten zu haben. Sie können z.B. `/test/data` oder `/test/testdata` haben, wenn Sie wollen, dass Go ignoriert, was sich in diesem Verzeichnis befindet. Beachten Sie, dass Go auch Verzeichnisse oder Dateien ignoriert, die mit "." oder " _ " beginnen, so dass Sie mehr Flexibilität haben, wie Sie Ihr Testdatenverzeichnis benennen.

Beispiele finden Sie im Verzeichnis [`/test`](test/README.md).

## Andere Verzeichnisse 

### `/docs`

Entwurfs- und Benutzerdokumente (zusätzlich zu Ihrer mit godoc erstellten Dokumentation).

Beispiele finden Sie im Verzeichnis [`/docs`](docs/README.md).

### `/tools`

Unterstützende Werkzeuge für dieses Projekt. Beachten Sie, dass diese Werkzeuge Quellcode aus den Verzeichnissen `/pkg` und `/internal` importieren können.

Siehe das Verzeichnis [`/tools`](tools/README.md) für Beispiele.

### `/examples`

Beispiele für Ihre Anwendungen und/oder öffentlichen Bibliotheken.

Beispiele finden Sie im Verzeichnis [`/examples`](examples/README.md).

### `/third_party`

Externe Hilfswerkzeuge, forked Quellcode und andere Dienstprogramme von Drittanbietern (z. B. Swagger UI).

### `/githooks`

Git hooks.

### `/assets`

Andere Elemente, die zu Ihrem Repository gehören (Bilder, Logos usw.).

### `/website`

Dies ist der Ort, an dem Sie die Website-Daten Ihres Projekts ablegen, wenn Sie keine GitHub-Seiten verwenden.

Siehe das Verzeichnis [`/website`](website/README.md) für Beispiele.

## Verzeichnisse, die Sie nicht haben sollten

### `/src`

Einige Go-Projekte haben einen `src` Ordner, aber das passiert in der Regel, wenn die Entwickler aus der Java-Welt kommen, wo dies ein gängiges Muster ist. Wenn Sie sich selbst helfen können, versuchen Sie, dieses Java-Muster nicht zu übernehmen. Sie wollen wirklich nicht, dass Ihr Go-Code oder Go-Projekte wie Java aussehen :-)

Verwechseln Sie das `/src` Verzeichnis auf Projektebene nicht mit dem `/src` Verzeichnis, das Go für seine Workspaces verwendet, wie in [`How to Write Go Code`](https://golang.org/doc/code.html). Die Umgebungsvariable `$GOPATH` zeigt auf Ihren (aktuellen) Arbeitsbereich (standardmäßig zeigt sie auf `$HOME/go` auf Nicht-Windows-Systemen). Dieser Arbeitsbereich umfasst die obersten Verzeichnisse `/pkg`, `/bin` und `/src`. Ihr eigentliches Projekt ist ein Unterverzeichnis unter `/src`. Wenn Sie also das Verzeichnis `/src` in Ihrem Projekt haben, sieht der Projektpfad wie folgt aus: /irgendein/pfad/zum/arbeitsbereich/src/ihres_projekts/src/quellcode.go. Beachten Sie, dass es mit Go 1.11 möglich ist, Ihr Projekt außerhalb Ihres `GOPATH` zu haben, aber das bedeutet immer noch nicht, dass es eine gute Idee ist, dieses Layout-Muster zu verwenden.

## Badges

* [Go Report Card](https://goreportcard.com/) - Es wird Ihren Quellcode mit `gofmt`, `go vet`, `gocyclo`, `golint`, `ineffassign`, `license` und `misspell` scannen. Ersetzen Sie `github.com/golang-standards/project-layout` durch Ihre Projectreferenz.

    [![Go Report Card](https://goreportcard.com/badge/github.com/golang-standards/project-layout?style=flat-square)](https://goreportcard.com/report/github.com/golang-standards/project-layout)

* ~~[GoDoc](http://godoc.org) - Sie bietet eine Online-Version der von GoDoc erstellten Dokumentation. Ändern Sie den Link, um auf Ihr Projekt zu verweisen~~

    [![Go Doc](https://img.shields.io/badge/godoc-reference-blue.svg?style=flat-square)](http://godoc.org/github.com/golang-standards/project-layout)

* [Pkg.go.dev](https://pkg.go.dev) -Pkg.go.dev ist ein neues Ziel für Go-Entdeckungen und -Dokumente. Sie können ein Abzeichen mit dem Werkzeug zur Erstellung von Abzeichen erstellen.[Abzeichen erstellungs tool](https://pkg.go.dev/badge).

    [![PkgGoDev](https://pkg.go.dev/badge/github.com/golang-standards/project-layout)](https://pkg.go.dev/github.com/golang-standards/project-layout)

* Release - Es wird die letzte Versionsnummer für Ihr Projekt angezeigt. Ändern Sie den Github-Link so, dass er auf Ihr Projekt verweist.

    [![Release](https://img.shields.io/github/release/golang-standards/project-layout.svg?style=flat-square)](https://github.com/golang-standards/project-layout/releases/latest)

## Notizen 

Eine Projektvorlage mit Beispielen für wiederverwendbare Konfigurationen, Skripte und Code ein unvollendetes Projekt (WIP).
