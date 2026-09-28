# Wildlife Rescue Operations System — PROG6112 PA2

A console-based Java/Maven application for WildLife SA, built against the
PROG6112 Practical Assignment 2 brief.

 How to build and run

```
mvn test              # compiles and runs the JUnit tests
mvn package            # builds an executable jar
java -jar target/WildlifeRescueSystem-1.0.0.jar
```

## Project structure

```
src/main/java/wildlife/
  RescueStatus.java            enum: Reported / Rescue in Progress / Under Observation / Rescue Completed
  RescuePriority.java          enum: LOW / MEDIUM / HIGH / CRITICAL
  RescueOperations.java        interface: startRescue(), completeRescue(), generateRescueSummary()
  RescueCase.java               abstract base class — shared fields, abstract methods
  InjuredAnimalRescue.java      extends RescueCase
  OrphanedAnimalRescue.java     extends RescueCase
  EndangeredSpeciesRescue.java  extends RescueCase
  RescueCaseManager.java        holds the ArrayList<RescueCase>, search/report logic
  Main.java                     console menu + input validation

src/test/java/wildlife/
  RescueCaseTest.java           JUnit 5 tests
```

## How this maps to the rubric

- **Rescue Case Management** — `RescueCaseManager` (create/search/update status/display),
  backed by an `ArrayList<RescueCase>`.
- **Inheritance / Rescue Case Types** — `RescueCase` is abstract; the three
  subclasses inherit shared fields (ID, ranger, days, daily cost, status) and
  each implements its own `calculateTotalCost()` and `determinePriority()`.
- **Rescue Operations (interface + polymorphism)** — `RescueOperations` is
  implemented by `RescueCase`; `Main` and `RescueCaseManager` call
  `startRescue()` / `completeRescue()` / `generateRescueSummary()` on
  `RescueCase` references without knowing the concrete subtype.
- **Reports** — `RescueCaseManager.generateReport()`.
- **Validation** — `Main`'s `readNonBlank`, `readPositiveInt`,
  `readPositiveDouble`, `readYesNo`, `readMenuChoice` loop until valid
  input is given; duplicate IDs are rejected in `addRescueCase`.
- **JUnit Testing** — `RescueCaseTest` covers cost calculations, priority
  calculations, status updates, search, and duplicate ID prevention.

## Cost/priority rules implemented (worth checking against your own judgement)

| Type | Extra cost | Priority logic |
|---|---|---|
| Injured | + vet cost, +R5000 if surgery | CRITICAL if surgery; HIGH if vet cost > R10 000; else MEDIUM |
| Orphaned | + feeding cost, +R2500 if foster care | CRITICAL if ≤2 months old; HIGH if ≤6 months or foster care needed; else MEDIUM |
| Endangered | + security cost, +R8000 if specialist team | CRITICAL if "Critically Endangered"; HIGH if "Endangered" or specialist team needed; else MEDIUM |
