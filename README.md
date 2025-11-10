# 🕋 Quran Journal

A cross-platform desktop application built with **Electron.js** that allows users to explore Surah information from the Al-Quran and record their personal spiritual reflections.  
This project was developed as part of the **CSC2744 – Object-Oriented Programming (Final Assignment)** for **Kolej Profesional MARA Beranang**, Session 2 2025/2026.

---

## 🌟 Application Overview

**Quran Journal** provides a calm, user-friendly space for readers to:
- View details about any Surah in the Al-Quran.
- Write and save personal reflections and ayah progress.
- Manage reflection notes through complete CRUD operations (Create, Read, Update, Delete).

Developed with **HTML, CSS, and JavaScript** within the **Electron.js** framework, the app delivers a responsive, elegant desktop interface designed for both reflection and productivity.

---

## 🌐 API Integration

The application integrates with the **AlQuran Cloud API**, a publicly accessible REST API that provides complete Quranic data.

**API Endpoint Used:**  
http://api.alquran.cloud/v1/surah


**Data Retrieved Includes:**
- Surah name (Arabic and English)
- Surah number
- English translation
- Number of ayahs
- Revelation type (Meccan or Medinan)

All data is fetched dynamically using JavaScript’s `fetch()` method, ensuring the app displays real-time and accurate information.

---

## ⚙️ Features and Functionalities

| Feature | Description |
|----------|--------------|
| 🔍 **Search Surah** | Users can search any Surah by name to view its details dynamically retrieved from the API. |
| 📖 **Surah Details Display** | Displays Surah name, number, translation, number of ayahs, and revelation type. |
| 📝 **Add Reflection (Create)** | Allows users to create new notes about their reflections or thoughts on a specific Surah. |
| 📚 **View Saved Notes (Read)** | Displays a list of saved reflections in a dedicated “My Journal” screen. |
| ✏️ **Edit Reflection (Update)** | Users can update their reflections or modify the number of ayahs completed. |
| 🗑️ **Delete Reflection (Delete)** | Users can delete any saved note from the journal. |
| 💾 **Data Persistence** | All data is stored in Local Storage to maintain entries across sessions. |
| 🎨 **GUI and User Interface** | Clean, minimalist design with meaningful colors and typography for a spiritual reading atmosphere. |

---

## 🧱 Technologies Used

- **Electron.js** – Cross-platform desktop framework  
- **HTML, CSS, JavaScript** – Interface and logic  
- **Local Storage** – For data persistence  
- **AlQuran Cloud API** – For Quran data  

---

## 🗂 Folder Structure

QURANJOURNAL/
├── main.js
├── preload.js
├── package.json
├── index.html
├── home.html
├── journal.html
├── index.js
├── journal.js
├── styles.css
│
├── Code_Text_Files/
│ ├── main.txt
│ ├── preload.txt
│ ├── package.txt
│ ├── index.txt
│ ├── home.txt
│ ├── journal.txt
│ ├── indexjs.txt
│ ├── journaljs.txt
│ ├── styles.txt
│
└── README.md


---

## 👤 Developer

**Name:** Muhammad Rafi'uddin bin Zulkifli
**Course:** CSC2744 – Object-Oriented Programming  
**Lecturer:** Madam Zuliarty  
**Institution:** Kolej Profesional MARA Indera Mahkota
**Session:** 2 2025/2026

---

## 📜 License

This project is created for academic purposes only.  
All Quranic data are provided by the [AlQuran Cloud API](http://api.alquran.cloud/).
