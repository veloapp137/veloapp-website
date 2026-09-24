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
