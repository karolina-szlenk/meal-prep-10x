---
project: "meal prep"
version: 1
status: draft
created: 2026-09-21
context_type: greenfield
product_type: web-app
target_scale:
  users: medium
# TODO: target_scale.qps — see Open Questions
  qps: null
# TODO: target_scale.data_volume — see Open Questions
  data_volume: null
timeline_budget:
  mvp_weeks: 6
  hard_deadline: 2026-11-04
  after_hours_only: true
---

## Vision & Problem Statement

Osoby, które liczą posiłki na każdy dzień według zapotrzebowania kalorycznego i makroskładników, mają czas gotować samodzielnie, ale nie mają pomysłu na komponowanie posiłków tak, żeby jedzenie było różnorodne, a jednocześnie mieściło się w przedziale kalorii i makro. Objawy: paraliż decyzyjny, tarcie w procesie (ręczne liczenie i układanie tygodnia) i nuda (brak różnorodności). Moment: piątek — wtedy użytkowniczka planuje posiłki na tydzień, bo w sobotę rano robi zakupy (Biedronka/Lidl). Koszt dziś: liczy wszystko w arkuszu kalkulacyjnym, co zajmuje ok. 30–40 minut, a jedzenie jest bardzo powtarzalne.

Wartość daje połączenie dwóch rzeczy naraz — realnego koszyka z Lidla i Biedronki oraz zapotrzebowania zmieniającego się z dniem (plan treningowy). Żadne z nich osobno nie wystarcza.

## User & Persona

Primary persona: użytkowniczka sama — osoba z planem treningowym, która robi zakupy w Lidlu/Biedronce, ma czas gotować samodzielnie i planuje posiłki w piątek przed sobotnimi zakupami. MVP jest dopasowane do jednego użytkownika (ją).

### Secondary persona

Kilku znajomych będzie testować aplikację (zwykłe konta użytkowników).

## Success Criteria

### Primary
- Zalogowana użytkowniczka w piątek wieczorem układa posiłki na dni tygodnia, a każdy wygenerowany posiłek mieści się w limicie kcal przypisanym do tego posiłku i dnia.

### Secondary
# TODO: Success Criteria / Secondary — see Open Questions

### Guardrails
- Aplikacja zawsze jasno pokazuje, gdy wygenerowany posiłek przekracza limit kcal lub makroskładników; użytkowniczka może go mimo to zaakceptować (ostrzeżenie zamiast blokady — decyzja z rundy Socrates, FR-006).

## User Stories

### US-01: User plans a week of meals by generating new ones or picking saved ones

- **Given** a logged-in user who has a kcal value for a given day and meal, and a calendar in front of them
- **When** they click a day in the calendar and set the meals for it — generating a new meal or choosing one from the saved meals database
- **Then** at the end they see the whole week in the calendar filled with meals, and it is clearly marked where something is missing ("za mało")

#### Acceptance Criteria
- "Za mało" oznacza: za mało kalorii, za mało makroskładników lub niewypełniony posiłek (rozstrzygnięte w rundzie Socrates, FR-014).

## Functional Requirements

### Authentication
- FR-010: User can register an account and log in. Priority: must-have
  > Socrates: Counter-argument considered: "otwarta rejestracja = obcy w bazie". Resolution: kept; ryzyko zaakceptowane (kilku znajomych testerów; konta można ręcznie usunąć poza aplikacją).

### Product management (admin)
- FR-001: Admin can add a product, and the product's calories and macros are validated for consistency when added. Priority: must-have
  > Socrates: Counter-argument considered: "ręcznie dodany produkt może mieć błędne makro, co psuje wyliczanie każdego posiłku z tym produktem". Resolution: kept; dodana walidacja spójności kcal i makro przy dodawaniu.
- FR-002: Admin can deactivate a product so it is no longer used in new meals but remains in already saved meals. Priority: must-have
  > Socrates: Counter-argument considered: "lepsze jest wygaśnięcie niż usuwanie". Resolution: FR changed from "delete" to "deactivate" (the original wording was "Admin can delete a product").
- FR-003: Admin can modify a product (including calories and macros). Priority: must-have
  > Socrates: Counter-argument considered: "edycja pojedynczych produktów jest rzadka (dane z CSV)". Resolution: kept as must-have.

### Profile and targets
- FR-005: User can fill in a short initial form (kcal per day, number of meals) and correct it later; the kcal split per meal is computed by default and can be changed later. Priority: must-have
  > Socrates: Counter-argument considered: "za dużo pól na start". Resolution: kept, but form shortened (podział kcal na posiłki liczony domyślnie, do zmiany później). Merged with the former FR-011 ("User can correct their kcal targets after the initial form") — "to ten sam formularz". FR-011 removed.

### Calendar and meal planning
- FR-004: User can select a day in the calendar. Priority: must-have
  > Socrates: Counter-argument considered: "to element interfejsu, nie osobna funkcja". Resolution: kept as a separate FR.
- FR-006: User can generate a meal; the app shows when the generated meal exceeds the kcal/macro limit for that day and meal, and the user can still accept it. Priority: must-have
  > Socrates: Counter-argument considered: "wygenerowany posiłek może przekroczyć limit kcal/makro". Resolution: kept; ostrzeżenie zamiast blokady (user może zaakceptować mimo przekroczenia). Guardrails updated to match (user's original wording was "nie może przekraczać limitu kcal i makro").
- FR-007: User can assign a generated meal to a given day and meal type; accepting it also saves it in the meal database. Priority: must-have
  > Socrates: Counter-argument considered: "nie wiadomo, czy zapisuje się w bazie posiłków". Resolution: clarified; akceptacja przypisuje do kalendarza i zapisuje w bazie posiłków.
- FR-012: User can remove a meal assigned to a day and replace it with another. Priority: must-have
  > Socrates: No counter-argument; it stands as written.
- FR-014: User can see in the weekly calendar where a day or meal falls short — of calories, of macros, or where a meal is not filled in. Priority: must-have
  > Socrates: Counter-argument considered: "niejasne, co znaczy 'za mało'". Resolution: clarified; "za mało" = za mało kalorii, za mało makroskładników, niewypełnione posiłki.

### Saved meals
- FR-008: User can browse previously saved meals on a separate page and filter them (e.g., by meal type or kcal). Priority: must-have
  > Socrates: Counter-argument considered: "bez wyszukiwania i filtrowania lista się rozrośnie". Resolution: kept; dodane filtrowanie zapisanych posiłków.
- FR-009: User can select a previously saved meal from the list and assign it to a given day and meal type; the app shows when it exceeds the kcal/macro limit for that day and meal, and the user can still accept it. Priority: must-have
  > Socrates: Counter-argument considered: "zapisany posiłek może nie pasować do limitu kcal tego dnia". Resolution: kept; ostrzeżenie jak przy generowaniu (spójne z FR-006).
- FR-013: User can delete a saved meal from the meal database; if the meal is assigned in the calendar, the app warns the user first and lets them choose what to do. Priority: must-have
  > Socrates: Counter-argument considered: "usunięcie posiłku, który jest w kalendarzu". Resolution: kept; aplikacja pyta przed usunięciem, gdy posiłek jest w kalendarzu.

## Non-Functional Requirements

- Po wpisaniu życzenia użytkowniczka widzi wygenerowany posiłek najpóźniej po 10 sekundach, a w tym czasie widzi, że operacja trwa.
- Zwykły użytkownik nigdy nie widzi cudzych celów kcal, kalendarzy ani zapisanych posiłków.
- Kalorie i makroskładniki posiłku pokazywane użytkowniczce odchylają się od wartości wynikających z danych produktów o nie więcej niż 5%.
- Aplikacja działa w przeglądarce na laptopie i na telefonie.

## Business Logic

Aplikacja dopasowuje do celu kcal i makro danego dnia (treningowego lub nietreningowego) posiłki, zapisane albo nowo wygenerowane z produktów z bazy, i zawsze sprawdza, czy mieszczą się w tym celu.

Dane wejściowe: cel kcal i makroskładników na dzień (inny dla dnia treningowego i nietreningowego) oraz jego podział na posiłki, baza produktów oraz wpisane przez użytkowniczkę życzenie (np. „słodkie śniadanie z jajek i kakao") albo wybór zapisanego posiłku.

Wynik: posiłek z kaloriami i makroskładnikami oraz informacją, czy mieści się w celu; jeśli nie mieści się, użytkowniczka widzi o ile go przekracza i może go mimo to zaakceptować.

Użytkowniczka spotyka regułę po kliknięciu dnia w kalendarzu, przy każdym posiłku, a potem w widoku tygodnia, gdzie widać, gdzie brakuje kalorii, makroskładników lub posiłku.

## Access Control

Logowanie: email + hasło. Dwie role:

- **User** — zalogowana osoba tworzy i widzi wyłącznie własne dane (własne zapotrzebowanie, plany, kalendarz).
- **Admin** — zarządza bazą produktów: dodaje, usuwa i modyfikuje produkty, w tym kalorie i makroskładniki (to jedyna funkcja admina w v1). Docelowo (poza v1) widzi plany wszystkich użytkowników oraz blokuje i usuwa konta; na v1 robi to ręcznie poza aplikacją.

Adminem jest na razie użytkowniczka (właścicielka projektu), ale chce też korzystać z aplikacji jako zwykły user. Konto admina ma być odseparowane od konta zwykłego użytkownika (osobne konta), ponieważ aplikację będzie testować kilku znajomych.

## Non-Goals

- Alergeny i typ diety w profilu — świadomie przesunięte poza v1.
- Zmiana kaloryczności zapisanego posiłku z przeliczaniem (np. 530 → 590 kcal) — poza v1.
- Podgląd planów wszystkich użytkowników oraz blokowanie i usuwanie kont w aplikacji — poza v1; na v1 ręcznie poza aplikacją.
- Integracja z zewnętrznym kalendarzem i eksport planu — kalendarz jest tylko widokiem w aplikacji.
- Lista zakupów na sobotę — aplikacja nie zbiera składników z planu tygodnia w listę zakupów.
- Aktualizacja bazy produktów z Lidla i Biedronki na żywo — baza pochodzi z gotowego CSV; brak pobierania cen, promocji i nowych produktów ze stron sklepów.
- Natywna aplikacja mobilna — tylko aplikacja w przeglądarce; bez publikacji w sklepach z aplikacjami.

## Open Questions

1. **Jakie jest szacowane obciążenie (zapytania na sekundę) i wolumen danych?** — nie podano w notatkach; pola `target_scale.qps` i `target_scale.data_volume` w frontmatter są puste. Owner: użytkowniczka. Block: no.
2. **Jakie jest kryterium drugorzędne (Success Criteria / Secondary)?** — użytkowniczka nie wskazała żadnego. Owner: użytkowniczka. Block: no.
3. **Jak pogodzić Primary z Guardrails?** — Primary mówi, że każdy wygenerowany posiłek mieści się w limicie, a Guardrails dopuszczają akceptację posiłku ponad limit z ostrzeżeniem. Owner: użytkowniczka. Block: no.
4. **Czy przenumerować FR-ów?** — FR-011 scalono z FR-005, więc numeracja ma lukę i nie odpowiada kolejności grup. Owner: użytkowniczka. Block: no.
5. **Jak generować posiłki?** — decyzja o sposobie realizacji jest poza zakresem PRD i należy do wyboru stosu; wymaganie, że zgodność z celem kcal i makro jest zawsze weryfikowana, jest zapisane w Business Logic. Owner: użytkowniczka, na etapie wyboru stosu. Block: no.
