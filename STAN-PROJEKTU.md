# STAN PROJEKTU – VeloApp
Ostatnia aktualizacja: 25 września 2026

Ten plik mówi, **gdzie jesteśmy**. Zasady pracy są w `CLAUDE.md`.
Po każdej większej sesji: poproś Claude o aktualizację tego pliku i podmień go w repozytorium.

---

## 1. Podstawowe informacje
- **Czym jest VeloApp:** marketplace łączący klientów potrzebujących remontu/naprawy z fachowcami. Strona www, później aplikacja mobilna.
- **Rynek:** tylko Hiszpania. Start: Marbella i Costa del Sol, potem cała Hiszpania. Inne kraje (Niemcy, Włochy) najwcześniej za ok. 2 lata, jeśli Hiszpania się sprawdzi.
- **Firma:** działamy przez istniejącą polską spółkę z o.o. (bez zakładania osobnej firmy w Hiszpanii).
- **Przewaga na start:** społeczność ok. 6000 fachowców na Facebooku w Marbelli.
- **Właściciel nie programuje** – całość budujemy z pomocą AI.

## 2. Infrastruktura (działa)
- Domena **veloapp.es** (rejestrator GoDaddy, DNS w Cloudflare, HTTPS działa).
- Repozytorium GitHub: **veloapp137/veloapp-website**.
- Hosting: Cloudflare Workers (pliki statyczne). Każdy commit na GitHubie publikuje stronę.
- Baza **Supabase** – założona i skonfigurowana, jeszcze niepodłączona do strony.
- W repozytorium: `index.html` (prototyp), `CLAUDE.md` (zasady), `STAN-PROJEKTU.md` (ten plik).
- `CLAUDE.md` nie jest publikowany jako strona (sprawdzone: veloapp.es/CLAUDE.md pokazuje stronę główną).

## 3. Prototyp (index.html) – co jest
To **makieta**: wszystko działa wizualnie, ale bez prawdziwego logowania, bazy i płatności.
- 4 języki: polski, angielski, hiszpański, arabski (z układem od prawej do lewej).
- Logowanie z wyborem roli: klient / fachowiec / firma-hotel. Admin osobno.
- Panel klienta: nowe zlecenie w 4 krokach, oferty, czat, profil.
- Panel fachowca: leady, punkty, abonament, profil, portfolio, certyfikaty, opinie.
- Panel admina: weryfikacja fachowców, spory, przychody.
- Oferty pracy dla hoteli/firm (trzeci moduł – **decyzja: odłożyć na później**, najpierw zlecenia remontowe).
- Licznik "X/5 fachowców odpowiedziało" przy leadzie.

## 4. Zmiany zrobione 24–25.09.2026
- Waluta: wszędzie **euro** (wcześniej część w złotówkach).
- Lokalizacje: Wrocław zamieniony na Marbellę i okolice (Nueva Andalucía, Casco Antiguo, Puerto Banús, San Pedro Alcántara, Estepona, Benahavís, Málaga). Mapa domyślnie na Marbelli.
- Prawdziwe nazwy hoteli (Ibis, Radisson itd.) zamienione na wymyślone.
- Certyfikat elektryka: zamiast polskiego "SEP do 1 kV" – hiszpański "Instalador autorizado en Baja Tensión (REBT)".
- Ujednolicony cennik (patrz punkt 5).
- **Filtr danych kontaktowych** (ochrona przed omijaniem płatności) – patrz punkt 6.
- Naprawiony błąd: pierwsza wiadomość po odebraniu leada była wyświetlana jako wiadomość klienta.

## 5. Cennik (decyzje)
- **Abonamenty fachowca:** Starter 7 €, Pro 18 €, Pro Max 35 € / miesiąc.
- **Punkty:** 1 pkt = 1 € dla wszystkich. Pakiety: 20 pkt = 20 €, 55 pkt = 50 € (+10%), 115 pkt = 100 € (+15%). Ważne 180 dni.
- **Lead kosztuje tyle samo punktów w każdym planie** (w prototypie 1–6 pkt, zależnie od zlecenia).
- **Różnice planów:** Pro dostaje +20% punktów gratis przy każdym zakupie, Pro Max +40%. Wyższe plany widzą leady wcześniej (15 / 30 min), mają szybszy zwrot punktów (Starter 48h, Pro 24h, Pro Max 12h), odznakę i wyższą pozycję.
- **Maks. 5 fachowców** na jedno zlecenie.
- **Oferty pracy:** 25 € za ogłoszenie albo 59 € / miesiąc abonament.
- Płatności: karta i Bizum. Operator płatności – **jeszcze niewybrany**.
- Kwoty są propozycją wyjściową – mogą się zmienić.

## 6. Ochrona przed omijaniem płatności
- Fachowiec płaci za odebranie leada; do tego czasu nie widzi numeru ani dokładnego adresu klienta.
- W prototypie działa filtr: opis zlecenia, opis profilu "O mnie" i czat fachowca (dopóki klient nie odpowie). Wykryte dane zamieniane na "[ukryte]" z przyjaznym komunikatem.
- Filtr łapie: numery (także mieszane cyfry+słowa, np. "537 dwa zero dwa 3 osiem dwa"), liczebniki w 4 językach, e-maile, loginy typu "s.wilar2", linki, komunikatory. Nie blokuje dat, metraży, kodów typu RAL9010, 30x60.
- **Znana luka:** nazwy profili w social media ("szukaj mnie na fejsie jako Wilar Budowlanka") – reguły tego nie złapią.
- **Decyzje do prawdziwej wersji** (szczegóły w CLAUDE.md): filtr na serwerze, moderacja AI, sprawdzanie zdjęć (OCR), a przede wszystkim **podgląd leada bez oryginalnego tekstu klienta** – przed zapłatą fachowiec widzi tylko uporządkowane dane i streszczenie.
- Zasada: kontakt w czacie odblokowuje się dopiero po odpowiedzi klienta; wtedy lead przestaje podlegać zwrotowi punktów.

## 7. Konkurencja (wnioski z analizy)
- **Habitissimo:** subskrypcja wymagana do otrzymywania zleceń + płatność za każdy kontakt (cena zależy od kategorii i miejscowości), bez prowizji.
- **Cronoshare:** płatność za kontakt ("cronos"), max 4 fachowców na zlecenie; skargi fachowców na leady bez efektu.
- **Bark:** kredyty ważne 3 miesiące, lead dla max 5 fachowców, opcjonalny miesięczny status Elite; skargi na fałszywe leady i ukryte koszty.
- **Nasze przewagi:** niska cena leada, punkty ważne 180 dni, automatyczny zwrot, cena w euro jasno pokazana.
- **Pomysły na wyróżnienie (priorytet):**
  1. Czat z automatycznym tłumaczeniem (klienci i fachowcy mówią różnymi językami na Costa del Sol).
  2. "Płacisz dopiero, gdy klient odpisze" (blokada punktów zamiast pobrania i zwrotu).
  3. Program "Członek założyciel" dla społeczności z Facebooka (np. 3 miesiące bez abonamentu, odznaka "Fundador").
  4. Później: obsługa właścicieli drugich domów (raporty zdjęciowe), bezpieczne płatności przez platformę.

## 8. Następne kroki
**Decyzje (przed budową prawdziwej wersji):**
- [ ] Wybór technologii (propozycja: najpierw strona działająca jak aplikacja – PWA).
- [ ] Wybór operatora płatności (karty + subskrypcje + Bizum).
- [ ] Ostateczny cennik.
- [ ] Rozmowa z księgową o VAT (polska spółka sprzedaje hiszpańskim fachowcom).

**MVP:**
- [ ] Prawdziwe konta z rolami (Supabase + zabezpieczenia RLS).
- [ ] Dodawanie zleceń, zakup leadów, automatyczne zwroty.
- [ ] Płatności: pakiety punktów i abonamenty.
- [ ] Filtr danych kontaktowych na serwerze + podgląd leada bez oryginalnego tekstu.
- [ ] Powiadomienia e-mail, weryfikacja fachowców, oceny tylko po zleceniu.
- [ ] Regulamin, RODO, cookies, aviso legal (z zakazem podawania kontaktu przed odebraniem leada).

**Później:** aplikacje w sklepach, oferty pracy dla hoteli, tłumaczenie czatu, sprawdzanie zdjęć (OCR).

## 9. Jak aktualizować stronę
1. Pobierz nowy plik od Claude.
2. GitHub → repozytorium `veloapp-website` → **Add file** → **Upload files** → przeciągnij plik(i) → **Commit changes**.
3. Po 1–2 minutach odśwież veloapp.es (Cmd + Shift + R).
