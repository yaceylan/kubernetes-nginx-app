## Reflexionsfragen zu Kubernetes Deployments

**Warum ist ein Deployment in Kubernetes nicht einfach nur eine etwas andere Version von `docker run --restart=always`?**

Kubernetes Deployment sorgt dafür, dass immer die richtige Anzahl der Anwendungs-Kopien läuft, führt Updates ohne Ausfallzeiten durch und erlaubt einfaches Zurückgehen zu alten Versionen. `docker run --restart always` startet nur einen einzelnen Container neu, wenn er abstürzt. Kubernetes kümmert sich um die gesamte Anwendung im Cluster.

**Was tut das Deployment, wenn ein Pod plötzlich verschwindet – und warum ist das nicht einfach nur Magie?**

Wenn ein Pod plötzlich verschwindet, startet das Deployment automatisch einen neuen Pod, um die gewünschte Anzahl an laufenden Pods wiederherzustellen. Dies ist keine Magie, sondern das Ergebnis der deklarativen Konfiguration des Deployments, das eine bestimmte Anzahl von Replikaten (`replicas`) definiert. Der Deployment Controller überwacht den Zustand der Pods, die seinem Selektor entsprechen, und gleicht den tatsächlichen Zustand mit dem gewünschten Zustand ab.

**Was konntest du beim Rolling Update mit `kubectl get pods -w` beobachten – und wie wird hier sichergestellt, dass es keinen kompletten Ausfall gibt?**

Mit `kubectl get pods -w` konnten wir beobachten, wie neue Pods mit der aktualisierten Version erstellt werden, während gleichzeitig alte Pods schrittweise beendet werden. Ein kompletter Ausfall wird durch die Rolling-Update-Strategie des Deployments sichergestellt (standardmäßig `RollingUpdate`). Diese Strategie stellt sicher, dass während des Updates immer eine bestimmte Anzahl von Pods der alten und/oder neuen Version verfügbar ist. Die Parameter `maxSurge` und `maxUnavailable` in der Deployment-Spezifikation steuern dieses Verhalten.

**Wie sorgt der Kubernetes-Service dafür, dass dein Browser-Ping (über NodePort) den richtigen Pod trifft – selbst wenn sich gerade ein Update vollzieht?**

Der Service nutzt Label-Selektoren, um Anfragen an bereite Pods mit dem passenden Label weiterzuleiten. Da sowohl die alten als auch die neuen Pods während eines Rolling Updates in der Regel dasselbe Label haben, kann der Service Anfragen an jeden der verfügbaren, bereiten Pods weiterleiten, unabhängig von seiner Version. Kubernetes stellt sicher, dass nur "ready" Pods Traffic erhalten.

**In der Deployment-YAML: Welche Angaben betreffen die Pod-Vorlage, und welche regeln das Verhalten des Deployments (z.B. Skalierung, Strategie)?**

Die **Pod-Vorlage** ist unter `spec.template` definiert. Sie enthält die Spezifikation für die einzelnen Pods, die das Deployment verwaltet (z.B. Container, Images, Ports, Labels der Pods).

Angaben wie `spec.replicas` (für die Skalierung) und `spec.strategy` (für die Update-Strategie) regeln das **Verhalten des Deployments** selbst.

**Was passiert mit den Pods, wenn du das Deployment löschst – und warum ist das Verhalten logisch?**

Wenn du das Deployment löschst, werden auch die von diesem Deployment erstellten und verwalteten Pods gelöscht. Dieses Verhalten ist logisch, da die Pods vom Deployment kontrolliert werden und nicht unabhängig davon existieren. Das Deployment ist die "Steuerungsebene" für diese Pods.
