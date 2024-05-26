# dt207g-projekt-backend
Projektet som jag har skapat till kursen DT207G har tre delar, backend och frontend både till användarsidan och adminssidan. 
I denna del ska jag beskriva backend delen. 
Till denna del har jag server.js, authRoutes.js, install.js, .env och databasen.

Först börjar jag att beskriva install.js:
require("dotenv").config(); först laddas miljövariblerna från .env
och sedan har jag importerat SQLite-databasen const sqlite3 = require("sqlite3").verbose(); 
const db = new sqlite3.Database(process.env.DATABASE2); här har jag en anslutning till databasen.

Sedan använde jag mig av att skriva db.serialize(() ... ) för att kunna exekvera flera SQLite-frågor i rätt ordning.
I denna del skapas flera tabeller:
user: tabell som innehåller username, password till admin.
booking: tabell som innehåller bokningr som användarna har bokat genom webbplatsen, och admin kan läsa det på webbplatsen till admin. 
menu_{category}:
jag har tabeller till förrätter, huvudrätter, efterrätter, vin och alkoholfritt, där alla meny alternativ katigorisieras. 

Genom att köra koden (node install.js) i terminalen så skapas dessa tabeller i databasen (sökvägen till databasen finns i .env).

Sedan har jag server.js:
const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");
const jwt = require("jsonwebtoken");
const sqlite3 = require("sqlite3").verbose();
require("dotenv").config();
I början av koden importerar jag alla nödvändiga moduler för att skapa server och hantera API-anrop. 
Express: För servern
body-parser: tolkning av JSON-begäranden
cors: hantering av Cross-Origin Resource sharing
jwt: autentisering
sqlite3: hantering av SQLite-databasen
dotenv: hantering av miljövariabler

const app = express(); sedan skapar jag express-applikationen
const port = process.env.PORT ; här definieras den port som servern kommer att lyssna på

Sedan har jag middleware-funktioner för att konfigurera Express-applikationer.
cors(): aktiverar Cross-Origin resource sharing
body-parser.json(): tolkar JSON-begäranden 
express.static(): servering av statiska filer som HTML, CSS och JavaScript

function authenticateUser(req, res, next) {...} här autentiseras användarna genom att verifiera JWT-token, detta görs genom att extrahera JWT-token från begärande huvudet och varifiera det med hjälp av en hemlig nyckel.

const authRoutes = require("./routes/authRoutes");
app.use("/api", authRoutes); här inkluderas routes för att hanera autentisering genom att använda authRoutes.js

app.get("/api/protected", authenticateUser, (req, res) => {
    res.json({ message: "Skyddad route" });
});
här skyddas en specifik API-rutt genom att tillämpa autentiseringen via authenticateUser-funktionen.

function getMenu(menuTable){...} denna funktion används för att hämta alla menyalternativ från tabllerna.



