# Publikacja strony na edu.jszwab.site – krok po kroku

Całość zajmuje ok. 15 minut. Potrzebne: konto GitHub i konto Cloudflare (to, na którym jest domena jszwab.site).

Pliki strony: `index.html`, `style.css`, `404.html`, `README.md` (ten plik z instrukcją możesz też wrzucić, nie przeszkadza).

---

## Krok 1. Utwórz repozytorium na GitHubie

1. Wejdź na https://github.com/new
2. **Repository name:** `edu-informatyka`
3. **Visibility:** może być *Private* (Cloudflare Pages działa też z prywatnym repo) albo *Public*.
4. Nie zaznaczaj „Add a README” (README już mamy).
5. Kliknij **Create repository**.

## Krok 2. Wrzuć pliki

**Najprościej, przez przeglądarkę:**

1. Na stronie nowego repo kliknij link **uploading an existing file**.
2. Przeciągnij pliki `index.html`, `style.css`, `404.html`, `README.md`.
3. Na dole wpisz opis, np. „Pierwsza wersja strony”, i kliknij **Commit changes**.

**Albo z terminala** (jeśli masz git):

```bash
cd edu-informatyka          # folder z plikami
git init -b main
git add .
git commit -m "Pierwsza wersja strony"
git remote add origin https://github.com/TWOJ_LOGIN/edu-informatyka.git
git push -u origin main
```

## Krok 3. Podłącz repo do Cloudflare Pages

1. Zaloguj się na https://dash.cloudflare.com
2. W menu po lewej: **Workers & Pages** → **Create** (Utwórz).
3. Wybierz zakładkę **Pages** → **Import an existing Git repository** (Połącz z Git).
   - Jeśli Cloudflare domyślnie proponuje „Worker”, poszukaj na dole linku w stylu *„Looking to deploy Pages? Get started”*.
4. Kliknij **Connect GitHub**, zaloguj się i zezwól aplikacji Cloudflare na dostęp (wystarczy tylko do repo `edu-informatyka`).
5. Wybierz repo `edu-informatyka` → **Begin setup**.
6. Ustawienia budowania:
   - **Project name:** `edu-informatyka` (to da adres `edu-informatyka.pages.dev`)
   - **Production branch:** `main`
   - **Framework preset:** `None`
   - **Build command:** zostaw puste
   - **Build output directory:** `/`
7. **Save and Deploy**. Po ok. minucie strona działa pod `https://edu-informatyka.pages.dev`. Sprawdź ją.

## Krok 4. Ustaw domenę edu.jszwab.site

1. W projekcie `edu-informatyka` (Workers & Pages) otwórz zakładkę **Custom domains**.
2. **Set up a custom domain** → wpisz `edu.jszwab.site` → **Continue**.
3. Ponieważ DNS domeny jest w Cloudflare, rekord CNAME doda się sam. Kliknij **Activate domain**.
4. Po kilku minutach (czasem do kilkunastu) status zmieni się na **Active**, a certyfikat HTTPS włączy się automatycznie.

Ważne: **nie dodawaj ręcznie rekordu CNAME w DNS przed tym krokiem**. Sam rekord bez podpięcia w Pages kończy się błędem 522. Jeśli taki rekord dla `edu` już istnieje, usuń go i dodaj domenę przez Custom domains.

## Krok 5. Gotowe. Jak wprowadzać zmiany?

- Każda zmiana wrzucona na gałąź `main` (przez przeglądarkę: otwórz plik na GitHubie → ikona ołówka → **Commit changes**) publikuje się sama w ciągu ok. minuty.
- Historię publikacji widać w Cloudflare: projekt → **Deployments**. Tam też można jednym kliknięciem wrócić do poprzedniej wersji.

---

## Gdy coś nie działa

| Objaw | Co zrobić |
|---|---|
| Nie widać repo przy łączeniu z GitHub | GitHub → Settings → Applications → Cloudflare Pages → Configure → dodaj repo `edu-informatyka`. |
| Błąd 522 na edu.jszwab.site | Usuń ręczny rekord CNAME `edu` w DNS i dodaj domenę jeszcze raz przez **Custom domains**. |
| Domena długo „Pending” | Odczekaj do 30 min; sprawdź, czy w DNS jest CNAME `edu` → `edu-informatyka.pages.dev` (pomarańczowa chmurka). |
| Strona bez stylów | Sprawdź, czy `style.css` leży w głównym folderze repo, obok `index.html`. |
