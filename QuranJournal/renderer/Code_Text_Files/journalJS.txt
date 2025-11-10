// Muhammad Rafi'uddin bin Zulkifli (journal.js)
const urlParams = new URLSearchParams(window.location.search);
const selectedNum = urlParams.get("num");
const journalContainer = document.getElementById("journal-container");
const journalSummary = document.getElementById("journal-summary");
const toast = document.getElementById("toast");

function showToast(msg, error = false) {
  toast.textContent = msg;
  toast.className = error ? "toast error" : "toast success";
  setTimeout(() => (toast.className = "toast hidden"), 3000);
}

function getEntries() {
  return JSON.parse(localStorage.getItem("journals") || "[]");
}

function saveEntries(entries) {
  localStorage.setItem("journals", JSON.stringify(entries));
}

function renderSummary(entries) {
  journalContainer.innerHTML = "";
  journalSummary.innerHTML = "";

  if (entries.length === 0) {
    journalSummary.innerHTML = `
      <div class="empty-state">
        <h2>No Reflections Yet</h2>
        <p>Start your Quran journey by exploring surahs and writing your reflections.</p>
        <button onclick="window.location.href='index.html'">Browse Surahs</button>
      </div>
    `;
    return;
  }

  entries.sort((a, b) => a.surahNumber - b.surahNumber);

  journalSummary.innerHTML = `
    <div class="summary-card">
      <h2>Your Quran Journal</h2>
      <p><strong>${entries.length}</strong> surahs with reflections</p>
      <p><strong>${entries.reduce((a, e) => a + e.ayahsCompleted, 0)}</strong> total ayahs completed</p>
    </div>
  `;

  entries.forEach((e) => {
    const card = document.createElement("div");
    card.className = "journal-card";
    const percent = Math.round((e.ayahsCompleted / e.numberOfAyahs) * 100);

    card.innerHTML = `
      <h3>${e.surahNumber}. ${e.englishName} (${e.surahName})</h3>
      <div class="progress-bar">
        <div class="progress-fill" style="width:${percent}%"></div>
      </div>
      <p class="progress-text">${e.ayahsCompleted}/${e.numberOfAyahs} Ayahs • ${percent}% completed</p>
      <p class="reflection-preview">${e.reflection ? e.reflection.slice(0, 120) + '...' : '(No reflection yet)'}</p>
      <div class="journal-actions">
        <button class="btn-view" data-num="${e.surahNumber}">View / Edit</button>
        <button class="btn-delete" data-num="${e.surahNumber}">Delete</button>
      </div>
    `;

    journalContainer.appendChild(card);
  });

  document.querySelectorAll(".btn-view").forEach((btn) =>
    btn.addEventListener("click", (e) => {
      const num = e.currentTarget.dataset.num;
      window.location.href = `journal.html?num=${num}`;
    })
  );

  document.querySelectorAll(".btn-delete").forEach((btn) =>
    btn.addEventListener("click", (e) => {
      const num = e.currentTarget.dataset.num;
      if (!confirm("Delete this reflection?")) return;
      let entries = getEntries();
      entries = entries.filter((ent) => ent.surahNumber != num);
      saveEntries(entries);
      showToast("Deleted successfully!");
      renderSummary(entries);
    })
  );
}


async function renderCRUD(num) {
  const API_URL = `https://api.alquran.cloud/v1/surah/${num}`;
  const res = await fetch(API_URL);
  const data = await res.json();
  const s = data.data;
  let entries = getEntries();
  let entry = entries.find((e) => e.surahNumber == num);

  if (!entry) {
    entry = {
      surahNumber: num,
      surahName: s.name,
      englishName: s.englishName,
      numberOfAyahs: s.numberOfAyahs,
      ayahsCompleted: 0,
      reflection: "",
    };
  }

  journalContainer.innerHTML = `
    <div class="entry-card">
      <h2>${s.englishName} (${s.name})</h2>
      <p><em>${s.revelationType}</em> • ${s.numberOfAyahs} ayahs</p>
      <label>Ayahs Completed</label>
      <input type="number" id="ayahsCompleted" value="${entry.ayahsCompleted}" min="0" max="${s.numberOfAyahs}">
      <label>Your Reflection</label>
      <textarea id="reflection" placeholder="Write your thoughts here...">${entry.reflection}</textarea>
      <div class="form-actions">
        <button id="saveBtn">Save</button>
        <button id="deleteBtn" class="danger">Delete</button>
      </div>
    </div>
  `;

  const ayahsInput = document.getElementById("ayahsCompleted");
  const reflectionInput = document.getElementById("reflection");
  const saveBtn = document.getElementById("saveBtn");
  const deleteBtn = document.getElementById("deleteBtn");

  saveBtn.addEventListener("click", () => {
    const ayahsCompleted = Number(ayahsInput.value);
    const reflection = reflectionInput.value.trim();
    if (ayahsCompleted < 0 || ayahsCompleted > s.numberOfAyahs)
      return showToast(`Ayahs must be between 0 and ${s.numberOfAyahs}`, true);

    const idx = entries.findIndex((e) => e.surahNumber == num);
    const payload = {
      surahNumber: num,
      surahName: s.name,
      englishName: s.englishName,
      numberOfAyahs: s.numberOfAyahs,
      ayahsCompleted,
      reflection,
    };

    if (idx >= 0) entries[idx] = payload;
    else entries.push(payload);
    saveEntries(entries);
    showToast("Saved successfully!");
  });

  deleteBtn.addEventListener("click", () => {
    if (!confirm("Are you sure you want to delete this entry?")) return;
    entries = entries.filter((e) => e.surahNumber != num);
    saveEntries(entries);
    showToast("Deleted successfully!");
    window.location.href = "journal.html";
  });
}

(function init() {
  const entries = getEntries();
  if (selectedNum) renderCRUD(selectedNum);
  else renderSummary(entries);
})();
