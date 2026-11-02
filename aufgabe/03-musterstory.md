# Musterstory: Zeiteintrag erfassen

So sieht eine fertige User Story aus. Sie gehört zu einem **anderen** Service der HiTec GmbH,
dem Stundenerfassungs-Service. Ihr setzt sie **nicht** um. Die Stories für die
Projektverwaltung schreibt ihr alle selbst, nach diesem Muster.

**Titel:** Zeiteintrag erfassen

**Story:** Als Mitarbeiterin der HiTec GmbH möchte ich die Zeit erfassen, die ich für eine
interne Tätigkeit gearbeitet habe, damit sie in meiner Stundenabrechnung auftaucht.

**Kundennutzen:** hoch (Muss)

**Akzeptanzkriterien**

1. Ein Zeiteintrag wird mit `POST /time-entries` angelegt. Der Body enthält die Pflichtangaben
   Mitarbeiter-ID, Datum, Dauer in Minuten und Tätigkeit. Ein Kommentar ist freiwillig: Fehlt
   er, wird der Eintrag trotzdem angelegt.
2. Im Erfolgsfall antwortet der Service mit **201 Created**. Der Body enthält den angelegten
   Eintrag mit seiner neuen ID.
3. Fehlt eine Pflichtangabe oder ist die Dauer nicht größer als 0 → **400** mit einer Meldung,
   welches Feld nicht stimmt.
4. Liegt das Datum in der Zukunft → **422** mit einer Meldung, dass das Datum nicht in der
   Zukunft liegen darf. Der Eintrag wird nicht angelegt. (Die Anfrage ist formal richtig,
   aber fachlich nicht zulässig; so legt es die API-Richtlinie fest.)
5. Gibt es die Mitarbeiter-ID im Employee-Service nicht → **404** mit einer Meldung, die die ID
   nennt.
6. Ist der Employee-Service nicht erreichbar → **503**. Der Eintrag wird dann nicht angelegt.
7. Ohne gültiges Token → **401**.

Swagger-Dokumentation, Integrationstest und `SampleRequests.http` stehen hier bewusst nicht.
Sie gelten für jede Story gleich und gehören deshalb in eure Definition of Done
([Auftrag 2.5](04-arbeitsauftraege.md)).

**Tasks**

- DTOs für Anfrage und Antwort anlegen, mit Validierung
- Entität `TimeEntry` und Repository anlegen
- Service-Methode: Mitarbeiter über den `EmployeeClient` prüfen, Datum prüfen, dann speichern
- Controller-Endpunkt `POST /time-entries` mit OpenAPI-Annotationen
- Integrationstest für den Erfolgsfall (EmployeeClient gemockt)
- Request in `SampleRequests.http` ergänzen

## Woran ihr eine gute Story erkennt

- Die Story sagt, **wer** etwas **wozu** will, nicht wie es technisch umgesetzt wird.
- Jedes Akzeptanzkriterium lässt sich mit Ja oder Nein prüfen. Statuscodes stehen drin, und zwar
  nach der API-Richtlinie in der [Anforderungsdefinition](02-anforderungsdefinition.md). Sie gilt
  für alle Services der HiTec GmbH, also auch für euren. Jeder Fehlerfall nennt genau einen
  Statuscode; „der passende Statuscode“ lässt die Entscheidung offen.
- Die Kriterien gelten nur für diese Story. Was für jede Story gilt (Swagger,
  Integrationstest, `SampleRequests.http`), steht einmal in der Definition of Done.
- Die Tasks sind so klein, dass eine Person sie in höchstens einer Doppelstunde schafft.
