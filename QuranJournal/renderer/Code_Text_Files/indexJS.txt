// Muhammad Rafi'uddin bin Zulkifli (index.js)
const API_URL = 'https://api.alquran.cloud/v1/surah';
const surahListEl = document.getElementById('surah-list');
const toast = document.getElementById('toast');
const searchInput = document.getElementById('searchInput');
let surahData = [];

function showToast(msg, isError=false){
  toast.textContent = msg;
  toast.className = isError ? 'toast error' : 'toast success';
  setTimeout(()=> toast.className='toast hidden', 3000);
}

function getEntries(){
  try {
    return JSON.parse(localStorage.getItem("journals")) || [];
  } catch {
    return [];
  }
}

async function loadSurahs(){
  try{
    surahListEl.innerHTML = '<p>Loading surahs...</p>';
    const res = await fetch(API_URL);
    if(!res.ok) throw new Error('API returned ' + res.status);
    const data = await res.json();
    surahData = data.data;
    renderSurahs(surahData);
  }catch(err){
    console.error(err);
    showToast('Failed to load surah list', true);
    surahListEl.innerHTML = '<p class="muted">Unable to fetch surahs.</p>';
  }
}


function renderSurahs(list){
  const savedEntries = getEntries();
  surahListEl.innerHTML = '';

  if(list.length === 0){
    surahListEl.innerHTML = '<p class="muted">No surah found.</p>';
    return;
  }

  list.forEach(s => {
    const saved = savedEntries.find(e => e.surahNumber == s.number);
    const card = document.createElement('div');
    card.className = 'card';

    card.innerHTML = `
      <h3>${s.number}. ${s.englishName} — ${s.name}</h3>
      <p>${s.englishNameTranslation} • ${s.numberOfAyahs} ayahs • ${s.revelationType}</p>
    `;

    if(saved){
      const percent = Math.round((saved.ayahsCompleted / saved.numberOfAyahs) * 100);
      const preview = document.createElement('div');
      preview.className = 'mini-journal';
      preview.innerHTML = `
        <p><strong>Reflection saved:</strong> ${saved.reflection ? saved.reflection.slice(0, 80) + '...' : '(no text)'} </p>
        <p><strong>Progress:</strong> ${saved.ayahsCompleted}/${saved.numberOfAyahs} ayahs (${percent}%)</p>
      `;
      card.appendChild(preview);
    }

    const actions = document.createElement('div');
    actions.className = 'card-actions';
    actions.innerHTML = `<button class="btn-open" data-num="${s.number}" aria-label="Open Journal for ${s.englishName}">Open Journal</button>`;
    card.appendChild(actions);

    surahListEl.appendChild(card);
  });


  document.querySelectorAll('.btn-open').forEach(btn => {
    btn.addEventListener('click', e => {
      const num = e.currentTarget.dataset.num;
      window.location.href = `journal.html?num=${num}`;
    });
  });
}

searchInput?.addEventListener('input', e => {
  const query = e.target.value.toLowerCase().trim();
  const filtered = surahData.filter(s =>
    s.englishName.toLowerCase().includes(query) ||
    s.name.toLowerCase().includes(query)
  );
  renderSurahs(filtered);
});

loadSurahs();
