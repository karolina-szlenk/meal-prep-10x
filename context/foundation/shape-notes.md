---
project: "meal prep"
context_type: greenfield
created: 2026-09-20
updated: 2026-09-21
checkpoint:
  current_phase: 8
  phases_completed: [1, 2, 3, 4, 5, 6, 7]
  gray_areas_resolved:
    - topic: "pain category"
      decision: "paraliż decyzyjny + tarcie w procesie + brak różnorodności (nuda)"
    - topic: "primary persona scope"
      decision: "użytkowniczka sama (jedna osoba z planem treningowym); MVP dopasowane do jednego użytkownika"
    - topic: "insight"
      decision: "wartość daje połączenie realnego koszyka z Lidla/Biedronki i zapotrzebowania zmieniającego się z dniem treningowym — żadne z osobna nie wystarcza"
    - topic: "access model"
      decision: "logowanie email + hasło; dwie role: user + admin; konto admina oddzielone od konta zwykłego"
    - topic: "MVP scope-down (v1 cuts)"
      decision: "admin tylko CRUD produktów (podgląd planów, blokowanie i usuwanie kont ręcznie w Supabase); profil bez alergenów i typu diety; bez edycji zapisanego posiłku z przeliczaniem kcal"
    - topic: "timeline"
      decision: "mvp_weeks = 6; zostaje mimo ostrzeżenia (>3 tyg.), świadomie zaakceptowane"
  frs_drafted: 13
  quality_check_status: accepted
---

# Shape notes

## Seed idea (verbatim, from the user)

Chcę zrobić aplikację do przygotowywania posiłków. Baza produktów ma pochodzić głównie z Lidla i Biedronki. Użytkownik ma konkretne zapotrzebowanie energetyczne, które jest różne w zależności od dnia (planu treningowego). Przepisy mają być generowane na podstawie bazy tych produktów i wrzucane w kalendarz.

## Facts already stated by the user

- Baza produktów z Lidla i Biedronki jest już przygotowana, w formacie CSV.
- Dostępny czas: ok. 10 godzin tygodniowo.
- Doświadczenie: 5 lat frontend; backend i baza danych (Supabase); wdrożenia na chmurę — brak ostatniego doświadczenia.
- Cel po stronie użytkownika: wsparcie AI w całym procesie, od PRD do deploymentu.
- Docelowy termin oddania: 4 listopada 2026 (pierwszy termin).

## Open items carried in (not yet resolved)

- ~~Czy CSV zawiera wartości odżywcze?~~ — rozstrzygnięte 2026-09-21 (wartości na 100 g potwierdzone; uwagi o cukrach, soli i tolerancji przyjęte do wiadomości). Opis z zrzutu ekranu: na podstawie zrzutu ekranu: plik ma kolumny: nazwa produktu, marka, sklep, gramatura, wartość energetyczna [kcal], tłuszcz [g], w tym nasycone [g], węglowodany [g], w tym cukry [g], błonnik [g], białko [g], sól [g]. Do potwierdzenia przez użytkowniczkę: (a) czy wartości są na 100 g (z samych liczb wygląda na to, ale nagłówki tego nie mówią), (b) kolumny „cukry" (0 przy daktylach, bananie i jabłku) i „sól" (0 przy orzeszkach „smażonych i solonych") wyglądają na niewypełnione zerami, (c) świeże produkty (banan, jabłko, cukinia, por) mają w polach marka/sklep/gramatura „N/A".
- Sposób generowania przepisów — nierozstrzygnięte.
- ~~Nazwa projektu~~ — rozstrzygnięte 2026-09-21: "meal prep".

## Vision & Problem Statement

Pain: osoby, które liczą posiłki na każdy dzień według zapotrzebowania kalorycznego i makroskładników, mają czas gotować samodzielnie, ale nie mają pomysłu na komponowanie posiłków tak, żeby jedzenie było różnorodne, a jednocześnie mieściło się w przedziale kalorii i makro. Objawy: paraliż decyzyjny, tarcie w procesie (ręczne liczenie i układanie tygodnia) i nuda (brak różnorodności).

Moment: piątek — wtedy użytkowniczka planuje posiłki na tydzień, bo w sobotę rano robi zakupy (Biedronka/Lidl).

Cost today: liczy wszystko w Excelu; zajmuje to ok. 30–40 minut; jedzenie jest bardzo powtarzalne.

Insight: wartość daje połączenie dwóch rzeczy naraz — realnego koszyka z Lidla i Biedronki oraz zapotrzebowania zmieniającego się z dniem (plan treningowy). Żadne z nich osobno nie wystarcza.

## User & Persona

Primary persona: użytkowniczka sama — osoba z planem treningowym, która robi zakupy w Lidlu/Biedronce, ma czas gotować samodzielnie i planuje posiłki w piątek przed sobotnimi zakupami. MVP jest dopasowane do jednego użytkownika (ją).

Secondary users: kilku znajomych będzie testować aplikację (zwykłe konta użytkowników).

## Access Control

Logowanie: email + hasło. Dwie role:

- **User** — zalogowana osoba tworzy i widzi wyłącznie własne dane (własne zapotrzebowanie, plany, kalendarz).
- **Admin** — zarządza bazą produktów: dodaje, usuwa i modyfikuje produkty, w tym kalorie i makroskładniki (to jedyna funkcja admina w v1). Docelowo (poza v1) widzi plany wszystkich użytkowników oraz blokuje i usuwa konta; na v1 robi to ręcznie w panelu Supabase.

Adminem jest na razie użytkowniczka (właścicielka projektu), ale chce też korzystać z aplikacji jako zwykły user. Konto admina ma być odseparowane od konta zwykłego użytkownika (osobne konta), ponieważ aplikację będzie testować kilku znajomych.

Uwaga do kosztu (do rozważenia w kolejnych fazach, nie decyzja): rola admina obejmuje zarządzanie produktami, podgląd planów wszystkich, blokowanie i usuwanie kont — to kilka ekranów i reguł dostępu do zbudowania i przetestowania w ~65 godzinach.

## MVP flow (v1, after scope-down)

1. Użytkowniczka loguje się (email + hasło) i podaje kcal na każdy dzień tygodnia (np. poniedziałek 2000, wtorek 1800 — dzień nietreningowy), liczbę posiłków oraz podział kcal na posiłki (np. śniadanie 450, obiad 700).
2. W piątek wieczorem otwiera kalendarz tygodnia i wybiera dzień.
3. Dla posiłku wpisuje w polu tekstowym, czego chce (np. „słodkie śniadanie z jajek i kakao").
4. Aplikacja generuje posiłek z bazy produktów i pokazuje jego kalorie (np. omlet kakaowy, 530 kcal).
5. Akceptuje; posiłek trafia do kalendarza i do bazy posiłków.
6. Na inny dzień może wybrać zapisany wcześniej posiłek.

Poza v1 (świadomie przesunięte): alergeny i typ diety w profilu; edycja zapisanego posiłku z przeliczaniem kcal (np. 530 → 590 kcal); podgląd planów wszystkich, blokowanie i usuwanie kont w aplikacji (na v1 ręcznie w panelu Supabase).

Baza produktów (CSV) istnieje z góry; bazy posiłków na starcie nie ma — powstaje z zaakceptowanych posiłków.

## Success Criteria

### Primary
- Zalogowana użytkowniczka w piątek wieczorem układa posiłki na dni tygodnia, a każdy wygenerowany posiłek mieści się w limicie kcal przypisanym do tego posiłku i dnia.

### Secondary
- (nie podano — użytkowniczka nie wskazała dodatkowego kryterium)

### Guardrails
- Aplikacja zawsze jasno pokazuje, gdy wygenerowany posiłek przekracza limit kcal lub makroskładników; użytkowniczka może go mimo to zaakceptować (ostrzeżenie zamiast blokady — decyzja z rundy Socrates, FR-006).

## Timeline budget

- mvp_weeks: 6
- hard_deadline: 2026-11-04 (pierwszy termin oddania; do 23:59)
- Timeline acknowledgment: Acknowledged on 2026-09-20: 6-week MVP requires sustained dedication; user accepted.

## Functional Requirements

### Authentication
- FR-010: User can register an account and log in. Priority: must-have
  > Socrates: Counter-argument considered: "otwarta rejestracja = obcy w bazie". Resolution: kept; ryzyko zaakceptowane (kilku znajomych testerów; konta można ręcznie usunąć w Supabase).

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

(FR numbers keep the order in which the capabilities were stated; FR-014 comes from the user's story "Then". FR-011 was merged into FR-005 during the Socrates round, so numbering has a gap at 011; /10x-prd may renumber.)

## User Stories

### US-01: User plans a week of meals by generating new ones or picking saved ones

- **Given** a logged-in user who has a kcal value for a given day and meal, and a calendar in front of them
- **When** they click a day in the calendar and set the meals for it — generating a new meal or choosing one from the saved meals database
- **Then** at the end they see the whole week in the calendar filled with meals, and it is clearly marked where something is missing ("za mało")

#### Acceptance Criteria
- "Za mało" oznacza: za mało kalorii, za mało makroskładników lub niewypełniony posiłek (rozstrzygnięte w rundzie Socrates, FR-014).

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

## Product framing

- product_type: web-app (działa w przeglądarce na laptopie i telefonie)
- target_scale.users: medium (dziesiątki do stu osób)
- Insight ze skali: przy 100-krotnie większej skali reguła się nie zmienia; wystarcza jedna wspólna baza produktów.
- timeline_budget.mvp_weeks: 6
- timeline_budget.hard_deadline: 2026-11-04
- timeline_budget.after_hours_only: true

## Non-Goals

- Alergeny i typ diety w profilu — świadomie przesunięte poza v1 (faza 3).
- Zmiana kaloryczności zapisanego posiłku z przeliczaniem (np. 530 → 590 kcal) — poza v1 (faza 3).
- Podgląd planów wszystkich użytkowników oraz blokowanie i usuwanie kont w aplikacji — poza v1; na v1 ręcznie w panelu Supabase (faza 3).
- Integracja z Google Calendar i eksport planu — kalendarz jest tylko widokiem w aplikacji.
- Lista zakupów na sobotę — aplikacja nie zbiera składników z planu tygodnia w listę zakupów.
- Aktualizacja bazy produktów z Lidla i Biedronki na żywo — baza pochodzi z gotowego CSV; brak pobierania cen, promocji i nowych produktów ze stron sklepów.
- Natywna aplikacja mobilna — tylko aplikacja w przeglądarce; bez publikacji w sklepach z aplikacjami.

## Forward: tech-stack

(informacyjne — nie jest częścią PRD; do wykorzystania przy wyborze stosu)

- Użytkowniczka ma doświadczenie z Supabase (backend i baza danych) i 5 lat we froncie; wspomniała Supabase jako miejsce, gdzie ręcznie zarządza kontami na v1.
- Baza produktów istnieje w formacie CSV (Lidl, Biedronka).
- Sposób generowania posiłków (algorytmicznie czy z użyciem modelu językowego) nierozstrzygnięty; weryfikacja zgodności z celem kcal/makro ma być zawsze wykonywana.
- Wdrożenie na chmurę: brak ostatniego doświadczenia; do zaplanowania wcześnie.

## Quality cross-check

Status: accepted — wszystkie 5 elementów obecne (Access Control, Business Logic, Project artifacts, Timeline-cost acknowledgment, Non-Goals).

Otwarte sprawy do przeniesienia do Open Questions w PRD:

1. ~~**Nazwa projektu**~~ — rozstrzygnięte 2026-09-21: "meal prep".
2. ~~**Podstawa wartości odżywczych w CSV i jakość danych**~~ — rozstrzygnięte 2026-09-21: użytkowniczka potwierdziła, że wartości są na 100 g; przyjęła do wiadomości, że zera w kolumnach „cukry" i „sól" wyglądają na braki danych (kolumny nie są używane przez regułę kcal/makro), i zaakceptowała, że walidacja z FR-001 dopuszcza różnicę między kcal a sumą 4·białko + 4·węglowodany + 9·tłuszcz. Pierwotny opis: CSV ma kcal, tłuszcz, nasycone, węglowodany, cukry, błonnik, białko i sól (zrzut z 2026-09-21), więc reguła domenowa jest wykonalna. Do potwierdzenia: czy wartości są na 100 g; czy zera w kolumnach „cukry" i „sól" oznaczają brak danych; czy walidacja spójności z FR-001 ma dopuszczać różnicę między kcal a sumą 4·białko + 4·węglowodany + 9·tłuszcz (w próbce widać ok. 3–5%, bo część kalorii pochodzi z błonnika). Owner: użytkowniczka.
3. **Success Criteria / Secondary** — puste; użytkowniczka nie wskazała kryterium drugorzędnego.
4. **Primary vs Guardrails** — Primary mówi, że każdy wygenerowany posiłek mieści się w limicie, a Guardrails dopuszczają akceptację posiłku ponad limit z ostrzeżeniem; do pogodzenia przy PRD.
5. **Numeracja FR** — FR-011 scalono z FR-005; numeracja ma lukę (do ewentualnej zmiany w /10x-prd).
6. **Sposób generowania posiłków** — poza PRD (decyzja stosu), ale wymaganie weryfikacji zgodności z celem kcal/makro jest zapisane w Business Logic.
