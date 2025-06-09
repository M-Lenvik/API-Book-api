# 📚 Book API – lösenordsskyddat boksystem med MongoDB och användarhantering

## Innehållsförteckning


1. 📍 [Inlämningar](#inlämningar)  
2. 🔍 [Projektbeskrivning](#projektbeskrivning)  
3. 🗄️ [Databasstruktur – MongoDB](#databasstruktur--mongodb)  
4. 🔌 [API Endpoints](#api-endpoints)  
5. 🖥️ [Klientgränssnitt](#klientgränssnitt)  
6. 🧪 [Tekniker & Verktyg](#tekniker--verktyg)  
7. 👥 [Projektmedlemmar](#projektmedlemmar)  
8. 🛠️ [Installationer](#installationer)  
9. 📌 [Sammanfattning](#sammanfattning) 
10. ⭐ [Slutbetyg](#slutbetyg)


| Inlämningar |
| ----------- |
| [MongoDB](external-assets/MongoDB) |
| [Insomnia](external-assets/Insomnia/Insomnia_2025-05-15.yaml) |
| [API](book-api) |
| [Klient](book-client) |
| [Grupp Dagbok](external-assets/Dagbok/API-Grupp-15-Dagbok.pdf) |

## 🔍 Projektbeskrivning

Detta projekt är ett fullstack-baserat bokhanteringssystem som bygger på **MongoDB** som databas. API:t är lösenordsskyddat och erbjuder funktionalitet för att hantera **användare**, **böcker** och **recensioner**. Projektet har stöd för inloggning, adminpanel samt en besökardel där man kan bläddra bland böcker och lämna recensioner.

Målet är att skapa ett system som är uppdelat i tydliga ansvarsområden med separata API-endpoints för varje datatyp, och där viss funktionalitet kräver autentisering med **JSON Web Tokens (JWT)**.

### Projektets struktur är uppdelad enligt följande
- `book-api` – innehåller all kod och filer relaterade till API-delen
- `book-client` – innehåller frontend-applikationen
- `external-assets` – här finns övrigt material såsom:
  - Projektdagbok
  - Figma-design
  - Insomnia-exporter
  - Lighthouse-analyser
  - MongoDB-länkar och exporter
  - Bilder på den färdiga webbplatsen

<details>
<summary><strong>Skärmdumpar från webbplatsen</strong></summary>

#### Startsida
![Start Page](external-assets/Site%20pictures/start_page.png)

#### Navigationsmeny
![Nav Bar](external-assets/Site%20pictures/nab_bar.png)

#### Boköversikt
![Book Page](external-assets/Site%20pictures/book_page.png)

#### Inloggningssida
![Login Page](external-assets/Site%20pictures/login_page.png)

#### Registreringssida
![Register Page](external-assets/Site%20pictures/register_page.png)

#### Adminpanel – Användare
![Admin Panel Users](external-assets/Site%20pictures/admin_panel_users.png)

#### Adminpanel – Böcker
![Admin Panel Books](external-assets/Site%20pictures/admin_panel_books.png)

#### Lägg till bok
![Add Book Page](external-assets/Site%20pictures/add_book_page.png)

#### Redigera bok
![Edit Book Page](external-assets/Site%20pictures/edit_book_page.png)
</details>

---

## 🗄️ Databasstruktur – MongoDB

Alla data lagras i en MongoDB-databas med tre huvudsakliga samlingar (collections):

### `users`
Lagrar information om registrerade användare:
- `username`: *String*
- `password`: *String*
- `is_admin`: *Boolean*
- `created_at`: *Date*

### `books`
Innehåller information om böcker samt referens till recensioner:
- `title`: *String*
- `description`: *String*
- `author`: *String*
- `genres`: *Array*
- `image`: *String*
- `published_year`: *Number*
- `reviews`: *[{ type: ObjectId, ref: "Reviews" }]*

### `reviews`
Recensioner som hör till specifika böcker:
- `name`: *String*
- `content`: *String*
- `rating`: *Number (1–5)*
- `created_at`: *Date*

---

## 🔌 API Endpoints

### 👤 Användare (`/users`)
- `GET /users` – Hämta alla användare
- `GET /users/:id` – Hämta en användare
- `PATCH /users/:id` – Uppdatera användare *(kräver JWT)*
- `DELETE /users/:id` – Radera användare *(kräver JWT)*

### 🔐 Autentisering (`/auth`)
- `POST /auth/register` – Registrera ny användare
- `POST /auth/login` – Logga in
- `POST /auth/logout` – Logga ut

### 📚 Böcker (`/books`)
- `GET /books` – Hämta alla böcker
- `GET /books/:id` – Hämta specifik bok inkl. recensioner
- `POST /books` – Skapa ny bok *(kräver JWT)*
- `PATCH /books/:id` – Uppdatera bok *(kräver JWT)*
- `DELETE /books/:id` – Radera bok *(kräver JWT)*

### ✍️ Recensioner (`/reviews`)
- `GET /reviews` – Hämta alla recensioner
- `GET /reviews/:id` – Hämta en recension
- `POST /reviews` – Skapa ny recension
- `PATCH /reviews/:id` – Uppdatera recension *(kräver JWT)*
- `DELETE /reviews/:id` – Radera recension *(kräver JWT)*

---

## 🖥️ Klientgränssnitt

### 🔐 Autentisering
- **Registrering**
  - Fält: `username`, `password`
- **Inloggning**
  - Fält: `username`, `password`

### 🛠️ Adminpanel (skyddad)
- **Användarlista**
  - Visar: `username`, `password`, `is_admin`, `created_at`
- **Boklista**
  - Visar: `title`, `author`, `genres`, `created_at`
- **Ny bok**
  - Formulär med alla fält enligt `books`-strukturen

**Användarnamn: super-admin**
**Lösenord: 1234**

### 🌍 Publik vy
- **Boklista**
  - Innehåll: liten bild, `title`, `author`, `genres`, `published_year`
  - Klickbar länk till bokens detaljer
- **Bokdetaljer**
  - Innehåll: större bild, `title`, `author`, `published_year`, `description`, `genres`
  - Formulär för att lämna ny recension
  - Lista över alla recensioner tillhörande boken

---

## 🧪 Tekniker & Verktyg

- **Node.js** + **Express.js** (Backend)
- **MongoDB** + **Mongoose** (Databas)
- **JWT (JSON Web Token)** (Autentisering)
- **Insomnia** (Testa REST API:er)
- **HTML/CSS/JavaScript** (Frontend)

---

## 👥 Projektmedlemmar

| Namn                   | GitHub-länk                              | Ansvarsfördelning 
|------------------------|-------------------------------------------|-------------------------
| Vy Petersson           | [github.com/tgvie](https://github.com/tgvie) | Ansvarsområde 1           |
| Angelica Nylander      | [github.com/angien90](https://github.com/angien90) | Ansvarsområde 2           |
| Marie Lenvik           | [github.com/M-Lenvi](https://github.com/M-Lenvik) | Ansvarsområde 3           |

---

## 🛠️ Installationer
För att komma igång med projektet, följ dessa steg:

1. Kloning av projektet
    - git clone [repo-url]
    - cd [projektmapp]

2. Installera alla beroenden
Kör följande kommando för att installera alla nödvändiga paket enligt package.json:
    - npm install

3. Starta localhost 
Projektet kör nu lokalt på sina respektive portar (t.ex. backend på localhost:3000 och frontend på localhost:5173, beroende på inställningar). Navigera till backend-projektets rotmapp (cd book-api) och kör npm run dev. Öppna en ny terminal och navigera till frontend-projektets rotmapp (cd book-client) och kör npm run dev.

Exempel på specifika beroenden som används:
  - Node.js, Express, TypeScript – Backend och typning
  - dotenv – Hantering av miljövariabler
  - cors – Hantering av CORS-förfrågningar
  - mongoose – Koppling till MongoDB
  - jsonwebtoken – Autentisering med JWT
  - bcrypt – Hashning av lösenord

💡 Tips: Se till att du har Node.js och npm installerat innan du börjar.

---

## 📌 Sammanfattning

Book API är ett webbaserat system som ger både administratörer och vanliga användare möjlighet att hantera ett digitalt bibliotek. Genom integration med MongoDB och ett rollbaserat skyddssystem kan projektet enkelt utökas med ytterligare funktioner såsom filtrering, sök eller rollstyrd tillgång.

Alla filer relaterade till projektet, såsom Figma-design, Insomnia-export, MongoDB-export, dagboksanteckningar med mera, finns samlade i mappen "External assets". 


## ⭐ Slutbetyg
<img width="505" alt="Betyg API verktyg API gruppuppgift Book API" src="https://github.com/user-attachments/assets/d9026aa3-0f74-4e4f-ae30-f6a329ac6469" />
