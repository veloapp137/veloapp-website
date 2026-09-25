# CLAUDE.md – VeloApp

## O projekcie
VeloApp (veloapp.es) to marketplace w Hiszpanii łączący klientów, którzy potrzebują remontu lub naprawy, z fachowcami. Docelowo strona www + aplikacja mobilna. Start: Marbella (społeczność ~6000 fachowców na Facebooku), potem cała Hiszpania.

## Kim jestem (ważne!)
- Nie jestem programistą. Buduję projekt wyłącznie z pomocą AI.
- Tłumacz po polsku, prostym językiem, bez żargonu. Jeśli musisz użyć terminu technicznego, wyjaśnij go jednym zdaniem.
- Gdy mam coś zrobić ręcznie (kliknąć w panelu, wpisać komendę), podaj dokładne kroki, jeden po drugim.
- Nie zakładaj, że wiem, co się stało. Po każdej zmianie napisz krótko: co zmieniłeś, dlaczego i jak mogę to sprawdzić.

## Zasady pracy

### 1. Myśl przed kodowaniem
- Zanim zaczniesz, opisz w 2–5 punktach plan i założenia.
- Jeśli coś jest niejasne albo są dwie sensowne drogi, zapytaj mnie, zamiast zgadywać.
- Przy większych decyzjach (nowa biblioteka, zmiana struktury bazy, wybór usługi płatności) zawsze pytaj przed działaniem i podaj plusy/minusy.

### 2. Prostota na pierwszym miejscu
- Najprostsze rozwiązanie, które działa. Bez abstrakcji "na zapas".
- Nie dodawaj bibliotek, jeśli da się bez nich.
- Kod ma być czytelny także dla kolejnej sesji AI: jasne nazwy, krótkie komentarze tam, gdzie logika nie jest oczywista.

### 3. Chirurgiczne zmiany
- Zmieniaj tylko to, o co proszę. Żadnych refaktoryzacji "przy okazji".
- Nie zmieniaj formatowania, nazw ani plików niezwiązanych z zadaniem.
- Jeśli zauważysz inny problem, zgłoś go na końcu odpowiedzi, ale go nie naprawiaj bez mojej zgody.

### 4. Praca na cel
- Na początku ustal, po czym poznamy, że zadanie jest zrobione.
- Po zmianie sprawdź wynik (uruchom, przetestuj, obejrzyj stronę), zanim napiszesz "gotowe".
- Jeśli czegoś nie dało się sprawdzić, powiedz to wprost.

## Stan techniczny
- Repozytorium: GitHub `veloapp137/veloapp-website`
- Hosting: Cloudflare Workers (pliki statyczne), domena veloapp.es przez Cloudflare (DNS), rejestrator: GoDaddy
- Obecnie: prototyp jako jeden plik `index.html`
- Baza danych: Supabase – skonfigurowana, jeszcze niepodłączona do strony
- Stack docelowej wersji (nie-prototypu): NIE ustalony. Nie wybieraj frameworka samodzielnie – zaproponuj opcje i zapytaj.

## Wymagania produktu
- **Języki:** polski, angielski, hiszpański, arabski – wybór ręczny lub automatyczny. Arabski wymaga układu od prawej do lewej (RTL) – każdy nowy element UI musi działać też w RTL. Wszystkie teksty trzymaj w plikach tłumaczeń, nigdy na sztywno w kodzie.
- **Panele:** klient, fachowiec, administrator – proste, intuicyjne, przejrzyste.
- **Logowanie:** przy logowaniu użytkownik wybiera rolę (klient / fachowiec). Logowanie admina jest osobne i nie pojawia się w tym wyborze.
- **Płatności:** model hybrydowy – opłata za lead (pay-per-lead) oraz subskrypcje.
- **Oceny:** klienci wystawiają oceny fachowcom.
- **Wygląd:** prosta, spokojna kolorystyka, bez efekciarstwa. Najpierw czytelność i łatwość obsługi, także na telefonie.

## Ochrona przed omijaniem płatności (obowiązkowe w MVP)
Fachowiec płaci za odebranie leada, więc dane kontaktowe nie mogą "wyciec" wcześniej.
- **Filtr danych kontaktowych działa na serwerze** (Supabase, np. Edge Function lub trigger), nie tylko w przeglądarce. Wzór logiki: funkcja `detectContactInfo` w prototypie `index.html`.
- Filtr wykrywa: numery telefonów (min. 8 cyfr, także z odstępami/kropkami/myślnikami, +34/+48/+44, cyfry arabskie), e-maile (także "at", "małpa", "arroba"), linki, wa.me, @nazwy kont, nazwy komunikatorów i sieci społecznościowych, liczby zapisane słownie (PL/ES/EN). Daty nie mogą być blokowane.
- Filtrowane pola: opis zlecenia, opis profilu fachowca ("O mnie"), podpisy zdjęć, opinie, wiadomości w czacie.
- Wykryte dane zamieniamy na "[ukryte]" i pokazujemy przyjazny komunikat (ochrona przed spamem), nigdy nie odrzucamy całego tekstu bez wyjaśnienia.
- **Czat po odebraniu leada:** fachowiec nie może podać kontaktu, dopóki klient nie odpowie w czacie. Po odpowiedzi klienta filtr się wyłącza, a lead przestaje podlegać zwrotowi punktów.
- **Zdjęcia** (zlecenia i portfolio): automatyczne rozpoznawanie tekstu (OCR) i kodów QR; wykryte dane zamazać albo wysłać do sprawdzenia w panelu admina.
- Przed odebraniem leada fachowiec widzi tylko przybliżoną lokalizację i imię klienta z inicjałem nazwiska.
- Każde wykrycie zapisujemy (kto, kiedy, gdzie). Kary: ostrzeżenie → blokada 7 dni → usunięcie konta. W panelu admina lista kont z wieloma wykryciami i fachowców z nietypowo dużą liczbą zwrotów.
- Regulamin musi zawierać zakaz podawania danych kontaktowych przed odebraniem leada.

## Bezpieczeństwo (zawsze)
- Nigdy nie umieszczaj kluczy, haseł ani sekretów w kodzie ani w repozytorium. Używaj zmiennych środowiskowych i powiedz mi, gdzie je wpisać.
- W Supabase włączaj Row Level Security (RLS) dla każdej tabeli i sprawdzaj, że użytkownik widzi tylko swoje dane.
- Do przeglądarki trafia tylko publiczny klucz Supabase (anon), nigdy klucz `service_role`.
- Dane osobowe (klienci w UE) – pamiętaj o RODO/GDPR; zgłoś, jeśli zmiana tego dotyczy.

## Git i wdrażanie
- Małe, osobne commity z jasnym opisem po polsku.
- Nigdy nie używaj `git push --force` i nie usuwaj historii.
- Przed wdrożeniem na veloapp.es zapytaj mnie o zgodę.
- Aktualizacja prototypu: najnowszy eksport z mojego folderu Pobrane (`veloapp-prototype[-N].html`) trafia do repo jako `index.html`.

## Czego NIE robić bez pytania
- Usuwać plików, tabel lub danych.
- Zmieniać ustawień DNS, domeny ani płatności.
- Dodawać płatnych usług lub subskrypcji.
