/* Business Hub — All client-side logic (no backend required)
   Features:
   - Thoughts (blog) with categories + images (base64)
   - Work updates
   - Projects / Portfolio
   - Contact form (local storage + optional Formspree)
   - Admin: create/edit/delete; client-side login (local)
   - Export / Import JSON DB
   - Theme toggle (light/dark)
*/

(() => {
  // --------------------------
  // Simple client DB (localStorage)
  // --------------------------
  const DB_KEY = 'bhub_db_v1';
  // default structure
  const DEFAULT_DB = {
    thoughts: [],
    work: [],
    projects: [],
    messages: [],
    admin: null // {email, passHash}
  };

  function loadDB() {
    try {
      const raw = localStorage.getItem(DB_KEY);
      return raw ? JSON.parse(raw) : JSON.parse(JSON.stringify(DEFAULT_DB));
    } catch (e) {
      console.error('DB load error', e);
      return JSON.parse(JSON.stringify(DEFAULT_DB));
    }
  }
  function saveDB(db) {
    localStorage.setItem(DB_KEY, JSON.stringify(db));
    updateStats();
  }
  function resetDB() {
    localStorage.removeItem(DB_KEY);
    showToast('DB cleared');
    initApp();
  }

  // --------------------------
  // Utility helpers
  // --------------------------
  const qs = s => document.querySelector(s);
  const qsa = s => Array.from(document.querySelectorAll(s));
  const uid = () => Math.random().toString(36).slice(2,9);
  function showToast(msg, timeout=2500){
    const t = qs('#toast');
    t.textContent = msg; t.classList.remove('hidden');
    setTimeout(()=> t.classList.add('hidden'), timeout);
  }
  function nowISO(){ return new Date().toISOString(); }

  // --------------------------
  // Image file -> base64 helper
  // --------------------------
  function fileToBase64(file){
    return new Promise((res, rej) => {
      if(!file) return res(null);
      const reader = new FileReader();
      reader.onload = () => res(reader.result);
      reader.onerror = e => rej(e);
      reader.readAsDataURL(file);
    });
  }

  // --------------------------
  // Crypto: hash password (SHA-256)
  // --------------------------
  async function hashText(text){
    const enc = new TextEncoder();
    const data = enc.encode(text);
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    return hashArray.map(b => b.toString(16).padStart(2,'0')).join('');
  }

  // --------------------------
  // DOM elements
  // --------------------------
  const dbEl = loadDB();
  const themeToggle = qs('#themeToggle');
  const yearEl = qs('#year');

  // Sections / UI
  const thoughtGrid = qs('#thoughtGrid');
  const thoughtEditor = qs('#thoughtEditor');
  const thoughtFormEls = {
    title: qs('#thoughtTitle'),
    category: qs('#thoughtCategory'),
    content: qs('#thoughtContent'),
    image: qs('#thoughtImage')
  };
  let editingThoughtId = null;

  const workList = qs('#workList');
  const workEditor = qs('#workEditor');
  const workFormEls = {
    title: qs('#workTitle'),
    tags: qs('#workTags'),
    content: qs('#workContent'),
    image: qs('#workImage')
  };
  let editingWorkId = null;

  const projectGrid = qs('#projectGrid');
  const projectEditor = qs('#projectEditor');
  const projectFormEls = {
    title: qs('#projectTitle'),
    link: qs('#projectLink'),
    desc: qs('#projectDesc'),
    image: qs('#projectImage')
  };
  let editingProjectId = null;

  const contactForm = qs('#contactForm');
  const savedMessagesEl = qs('#savedMessages');

  const adminLoginBox = qs('#loginBox');
  const adminPanel = qs('#adminPanel');
  const adminEmailEl = qs('#adminEmail');
  const adminPassEl = qs('#adminPassword');
  const loginBtn = qs('#loginBtn');
  const setupBtn = qs('#setupBtn');
  const logoutBtn = qs('#logoutBtn');

  // others
  const newThoughtBtn = qs('#newThoughtBtn');
  const cancelThoughtBtn = qs('#cancelThought');
  const saveThoughtBtn = qs('#saveThought');
  const newWorkBtn = qs('#newWorkBtn');
  const cancelWorkBtn = qs('#cancelWork');
  const saveWorkBtn = qs('#saveWork');
  const newProjectBtn = qs('#newProjectBtn');
  const cancelProjectBtn = qs('#cancelProject');
  const saveProjectBtn = qs('#saveProject');
  const exportBtn = qs('#exportBtn');
  const importBtn = qs('#importBtn');
  const importFileInput = qs('#importFile');

  const manageThoughtsBtn = qs('#manageThoughts');
  const manageWorkBtn = qs('#manageWork');
  const manageProjectsBtn = qs('#manageProjects');
  const clearDataBtn = qs('#clearData');
  const downloadDbBtn = qs('#downloadDbBtn');

  const categoryFilter = qs('#categoryFilter');
  const searchThoughts = qs('#searchThoughts');

  // --------------------------
  // Renderers
  // --------------------------
  function updateStats(){
    const db = loadDB();
    qs('#statThoughts').textContent = db.thoughts.length;
    qs('#statProjects').textContent = db.projects.length;
    qs('#statContacts').textContent = db.messages.length;
  }

  function renderCategoryOptions(){
    const db = loadDB();
    const cats = Array.from(new Set(db.thoughts.map(t => t.category || 'Uncategorized')));
    categoryFilter.innerHTML = '<option value="all">All categories</option>' +
      cats.map(c => `<option value="${c}">${c}</option>`).join('');
  }

  function renderThoughts(){
    const db = loadDB();
    const q = searchThoughts.value.trim().toLowerCase();
    const cat = categoryFilter.value;
    let items = db.thoughts.slice().reverse();
    if(cat && cat !== 'all') items = items.filter(it => (it.category||'').toLowerCase()===cat.toLowerCase());
    if(q) items = items.filter(it => (it.title + ' ' + it.content).toLowerCase().includes(q));
    thoughtGrid.innerHTML = items.map(t => `
      <div class="card">
        ${t.image ? `<div style="background-image:url('${t.image}');height:140px;background-size:cover;border-radius:8px;margin-bottom:10px"></div>` : ''}
        <h3>${escapeHtml(t.title)}</h3>
        <small class="muted">${escapeHtml(t.category || 'Uncategorized')} • ${new Date(t.updated||t.created).toLocaleString()}</small>
        <p>${escapeHtml(truncate(t.content, 220))}</p>
        <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:8px">
          <button class="btn small" data-id="${t.id}" data-action="view">View</button>
          <button class="btn small ghost" data-id="${t.id}" data-action="edit">Edit</button>
          <button class="btn small ghost" data-id="${t.id}" data-action="delete">Delete</button>
        </div>
      </div>
    `).join('') || '<div class="card">No thoughts yet — create one!</div>';

    // attach actions
    qsa('[data-action]').forEach(btn => {
      btn.onclick = (e) => {
        const id = btn.dataset.id;
        const action = btn.dataset.action;
        if(action === 'view') viewThought(id);
        if(action === 'edit') openThoughtEditorFor(id);
        if(action === 'delete') { if(confirm('Delete this thought?')) deleteThought(id); }
      };
    });
  }

  function renderWork(){
    const db = loadDB();
    workList.innerHTML = db.work.slice().reverse().map(w => `
      <div class="card">
        <div style="display:flex;gap:12px">
          ${w.image ? `<img src="${w.image}" style="width:140px;height:100px;object-fit:cover;border-radius:6px" />` : ''}
          <div>
            <h3>${escapeHtml(w.title)}</h3>
            <small class="muted">${(w.tags||[]).join(', ')} • ${new Date(w.updated||w.created).toLocaleDateString()}</small>
            <p>${escapeHtml(truncate(w.content,220))}</p>
            <div style="display:flex;gap:8px;justify-content:flex-end">
              <button class="btn small" data-id="${w.id}" data-action="editw">Edit</button>
              <button class="btn small ghost" data-id="${w.id}" data-action="delw">Delete</button>
            </div>
          </div>
        </div>
      </div>
    `).join('') || '<div class="card">No work updates yet.</div>';

    qsa('[data-action="editw"]').forEach(btn => btn.onclick = ()=> openWorkEditorFor(btn.dataset.id));
    qsa('[data-action="delw"]').forEach(btn => btn.onclick = ()=> { if(confirm('Delete update?')) deleteWork(btn.dataset.id); });
  }

  function renderProjects(){
    const db = loadDB();
    projectGrid.innerHTML = db.projects.slice().reverse().map(p => `
      <div class="card project-card">
        <div class="project-thumb" style="background-image:url('${p.image || ''}')"></div>
        <h3>${escapeHtml(p.title)}</h3>
        <p>${escapeHtml(truncate(p.desc,150))}</p>
        <div style="display:flex;gap:8px;justify-content:space-between;align-items:center">
          <div><a href="${p.link||'#'}" target="_blank" class="btn small outline">Open</a></div>
          <div>
            <button class="btn small ghost" data-id="${p.id}" data-action="editp">Edit</button>
            <button class="btn small ghost" data-id="${p.id}" data-action="delp">Delete</button>
          </div>
        </div>
      </div>
    `).join('') || '<div class="card">No projects yet.</div>';

    qsa('[data-action="editp"]').forEach(btn => btn.onclick = ()=> openProjectEditorFor(btn.dataset.id));
    qsa('[data-action="delp"]').forEach(btn => btn.onclick = ()=> { if(confirm('Delete project?')) deleteProject(btn.dataset.id); });
  }

  function renderMessages(){
    const db = loadDB();
    savedMessagesEl.innerHTML = db.messages.slice().reverse().map(m => `<li><strong>${escapeHtml(m.name)}</strong>: ${escapeHtml(truncate(m.message,80))}</li>`).join('') || '<li class="muted">No messages</li>';
  }

  // --------------------------
  // CRUD: Thoughts
  // --------------------------
  async function openThoughtEditorFor(id = null){
    editingThoughtId = id;
    if(id){
      const db = loadDB();
      const it = db.thoughts.find(t => t.id === id);
      if(!it) return showToast('Not found');
      thoughtFormEls.title.value = it.title;
      thoughtFormEls.category.value = it.category || '';
      thoughtFormEls.content.value = it.content;
      thoughtFormEls.image.value = '';
      qs('#thoughtEditorTitle').textContent = 'Edit Thought';
    } else {
      thoughtFormEls.title.value = '';
      thoughtFormEls.category.value = '';
      thoughtFormEls.content.value = '';
      thoughtFormEls.image.value = '';
      qs('#thoughtEditorTitle').textContent = 'New Thought';
    }
    thoughtEditor.classList.remove('hidden');
  }
  async function saveThought(){
    const title = thoughtFormEls.title.value.trim();
    if(!title) return alert('Title required');
    const category = thoughtFormEls.category.value.trim() || 'Uncategorized';
    const content = thoughtFormEls.content.value.trim();
    const file = thoughtFormEls.image.files[0];
    const img = await fileToBase64(file);

    const db = loadDB();
    if(editingThoughtId){
      const t = db.thoughts.find(x => x.id === editingThoughtId);
      if(!t) return showToast('Not found');
      t.title = title; t.category = category; t.content = content; if(img) t.image = img; t.updated = nowISO();
      showToast('Thought updated');
    } else {
      db.thoughts.push({ id: uid(), title, category, content, image: img||null, created: nowISO() });
      showToast('Thought saved');
    }
    saveDB(db);
    thoughtEditor.classList.add('hidden');
    renderCategoryOptions(); renderThoughts();
  }
  function cancelThought(){ thoughtEditor.classList.add('hidden'); }
  function deleteThought(id){
    const db = loadDB();
    db.thoughts = db.thoughts.filter(t => t.id !== id);
    saveDB(db); renderCategoryOptions(); renderThoughts();
    showToast('Thought deleted');
  }
  function viewThought(id){
    const db = loadDB(); const t = db.thoughts.find(x=>x.id===id);
    if(!t) return showToast('Not found');
    // show a quick modal
    const tmp = document.createElement('div');
    tmp.className = 'modal';
    tmp.style.position='fixed'; tmp.style.inset='10px'; tmp.style.background='rgba(0,0,0,0.6)'; tmp.style.display='flex'; tmp.style.alignItems='center'; tmp.style.justifyContent='center';
    tmp.innerHTML = `<div style="max-width:800px;background:var(--card);padding:20px;border-radius:10px;overflow:auto">
      ${t.image?`<img src="${t.image}" style="max-width:100%;border-radius:8px;margin-bottom:8px" />`:''}
      <h2>${escapeHtml(t.title)}</h2>
      <small class="muted">${escapeHtml(t.category)} • ${new Date(t.created).toLocaleString()}</small>
      <div style="margin-top:12px">${nl2br(escapeHtml(t.content))}</div>
      <div style="text-align:right;margin-top:12px"><button id="closeModal" class="btn">Close</button></div>
    </div>`;
    document.body.appendChild(tmp);
    qs('#closeModal').onclick = ()=> tmp.remove();
  }

  // --------------------------
  // CRUD: Work
  // --------------------------
  async function openWorkEditorFor(id = null){
    editingWorkId = id;
    if(id){
      const db = loadDB(); const w = db.work.find(x=>x.id===id);
      workFormEls.title.value = w.title; workFormEls.tags.value = (w.tags||[]).join(',');
      workFormEls.content.value = w.content; workFormEls.image.value = '';
      qs('#workEditorTitle').textContent = 'Edit Work';
    } else {
      workFormEls.title.value = ''; workFormEls.tags.value = ''; workFormEls.content.value = ''; workFormEls.image.value = '';
      qs('#workEditorTitle').textContent = 'New Work';
    }
    workEditor.classList.remove('hidden');
  }
  async function saveWork(){
    const title = workFormEls.title.value.trim(); if(!title) return alert('Title required');
    const tags = workFormEls.tags.value.split(',').map(s=>s.trim()).filter(Boolean);
    const content = workFormEls.content.value.trim();
    const file = workFormEls.image.files[0];
    const img = await fileToBase64(file);

    const db = loadDB();
    if(editingWorkId){
      const w = db.work.find(x=>x.id===editingWorkId);
      w.title = title; w.tags = tags; w.content = content; if(img) w.image = img; w.updated = nowISO();
      showToast('Work updated');
    } else {
      db.work.push({ id: uid(), title, tags, content, image: img||null, created: nowISO() });
      showToast('Work saved');
    }
    saveDB(db); workEditor.classList.add('hidden'); renderWork();
  }
  function cancelWork(){ workEditor.classList.add('hidden'); }
  function deleteWork(id){ const db = loadDB(); db.work = db.work.filter(w=>w.id!==id); saveDB(db); renderWork(); showToast('Work deleted'); }

  // --------------------------
  // CRUD: Projects
  // --------------------------
  async function openProjectEditorFor(id=null){
    editingProjectId = id;
    if(id){
      const db = loadDB(); const p = db.projects.find(x=>x.id===id);
      projectFormEls.title.value = p.title; projectFormEls.link.value = p.link || ''; projectFormEls.desc.value = p.desc || '';
      projectFormEls.image.value = ''; qs('#projectEditorTitle').textContent = 'Edit Project';
    } else {
      projectFormEls.title.value=''; projectFormEls.link.value=''; projectFormEls.desc.value=''; projectFormEls.image.value='';
      qs('#projectEditorTitle').textContent = 'New Project';
    }
    projectEditor.classList.remove('hidden');
  }
  async function saveProject(){
    const title = projectFormEls.title.value.trim(); if(!title) return alert('Title required');
    const link = projectFormEls.link.value.trim();
    const desc = projectFormEls.desc.value.trim();
    const file = projectFormEls.image.files[0];
    const img = await fileToBase64(file);

    const db = loadDB();
    if(editingProjectId){
      const p = db.projects.find(x=>x.id===editingProjectId);
      p.title = title; p.link = link; p.desc = desc; if(img) p.image = img; p.updated = nowISO();
      showToast('Project updated');
    } else {
      db.projects.push({ id: uid(), title, link, desc, image: img||null, created: nowISO() });
      showToast('Project saved');
    }
    saveDB(db); projectEditor.classList.add('hidden'); renderProjects();
  }
  function cancelProject(){ projectEditor.classList.add('hidden'); }
  function deleteProject(id){ const db = loadDB(); db.projects = db.projects.filter(p=>p.id!==id); saveDB(db); renderProjects(); showToast('Project deleted'); }

  // --------------------------
  // Contact handling
  // --------------------------
  contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    const name = qs('#cname').value.trim();
    const email = qs('#cemail').value.trim();
    const subject = qs('#csubject').value.trim();
    const message = qs('#cmessage').value.trim();
    const endpoint = qs('#formspreeEndpoint').value.trim();

    if(!name || !email || !message) return alert('Fill required fields');

    const payload = { name, email, subject, message, created: nowISO() };

    if(endpoint){
      // try post to Formspree (user-provided endpoint). This is optional.
      try {
        const res = await fetch(endpoint, { method: 'POST', headers: {'Content-Type':'application/json'}, body: JSON.stringify(payload) });
        if(res.ok) {
          showToast('Message sent via Formspree');
        } else {
          showToast('Formspree returned error; saved locally');
          saveLocalMessage(payload);
        }
      } catch (err) {
        console.error(err);
        saveLocalMessage(payload);
        showToast('Network error; saved locally');
      }
    } else {
      saveLocalMessage(payload);
      showToast('Message saved locally');
    }

    contactForm.reset();
    renderMessages();
  });

  function saveLocalMessage(payload){
    const db = loadDB();
    db.messages.push({ id: uid(), ...payload });
    saveDB(db);
  }

  // --------------------------
  // Admin (client-side)
  // --------------------------
  async function setupAdmin(){
    const email = adminEmailEl.value.trim();
    const pass = adminPassEl.value;
    if(!email || !pass) return alert('Email + password required');
    const passHash = await hashText(pass);
    const db = loadDB();
    db.admin = { email, passHash };
    saveDB(db);
    showToast('Admin created (local)');
  }

  async function loginAdmin(){
    const email = adminEmailEl.value.trim();
    const pass = adminPassEl.value;
    if(!email || !pass) return alert('Email + password required');
    const db = loadDB();
    if(!db.admin) return alert('No admin set. Use Create Admin.');
    const passHash = await hashText(pass);
    if(db.admin.email === email && db.admin.passHash === passHash){
      // show admin panel
      adminLoginBox.classList.add('hidden');
      adminPanel.classList.remove('hidden');
      logoutBtn.classList.remove('hidden');
      showToast('Logged in');
    } else {
      alert('Invalid credentials');
    }
  }

  function logout(){
    adminLoginBox.classList.remove('hidden');
    adminPanel.classList.add('hidden');
    logoutBtn.classList.add('hidden');
    showToast('Logged out');
  }

  function adminManage(area){
    const adminArea = qs('#adminArea');
    const db = loadDB();
    if(area === 'thoughts'){
      adminArea.innerHTML = `<h4>All Thoughts</h4>` + db.thoughts.map(t => `<div style="padding:8px;border-bottom:1px solid rgba(0,0,0,0.04)"><strong>${escapeHtml(t.title)}</strong> • ${escapeHtml(t.category)} <div style="float:right"><button class="btn small" data-id="${t.id}" data-act="edit">Edit</button> <button class="btn small ghost" data-id="${t.id}" data-act="del">Delete</button></div><div style="clear:both"></div></div>`).join('');
      qsa('#adminArea [data-act]').forEach(b=> b.onclick = ()=>{ if(b.dataset.act==='edit') openThoughtEditorFor(b.dataset.id); else if(confirm('Delete?')) deleteThought(b.dataset.id); });
    } else if(area === 'work'){
      adminArea.innerHTML = `<h4>Work</h4>` + db.work.map(w => `<div style="padding:8px;border-bottom:1px solid rgba(0,0,0,0.04)"><strong>${escapeHtml(w.title)}</strong> <div style="float:right"><button class="btn small" data-id="${w.id}" data-act="editw">Edit</button><button class="btn small ghost" data-id="${w.id}" data-act="delw">Delete</button></div><div style="clear:both"></div></div>`).join('');
      qsa('#adminArea [data-act="editw"]').forEach(b=> b.onclick = ()=> openWorkEditorFor(b.dataset.id));
      qsa('#adminArea [data-act="delw"]').forEach(b=> b.onclick = ()=> { if(confirm('Delete?')) deleteWork(b.dataset.id); });
    } else if(area === 'projects'){
      adminArea.innerHTML = `<h4>Projects</h4>` + db.projects.map(p => `<div style="padding:8px;border-bottom:1px solid rgba(0,0,0,0.04)"><strong>${escapeHtml(p.title)}</strong> <div style="float:right"><button class="btn small" data-id="${p.id}" data-act="editp">Edit</button><button class="btn small ghost" data-id="${p.id}" data-act="delp">Delete</button></div><div style="clear:both"></div></div>`).join('');
      qsa('#adminArea [data-act="editp"]').forEach(b=> b.onclick = ()=> openProjectEditorFor(b.dataset.id));
      qsa('#adminArea [data-act="delp"]').forEach(b=> b.onclick = ()=> { if(confirm('Delete?')) deleteProject(b.dataset.id); });
    }
  }

  // --------------------------
  // Export / Import
  // --------------------------
  exportBtn.addEventListener('click', () => {
    const db = loadDB();
    const blob = new Blob([JSON.stringify(db, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a'); a.href = url; a/* Business Hub — All client-side logic (no backend required)
   Features:
   - Thoughts (blog) with categories + images (base64)
   - Work updates
   - Projects / Portfolio
   - Contact form (local storage + optional Formspree)
   - Admin: create/edit/delete; client-side login (local)
   - Export / Import JSON DB
   - Theme toggle (light/dark)
*/

(() => {
  // --------------------------
  // Simple client DB (localStorage)
  // --------------------------
  const DB_KEY = 'bhub_db_v1';
  // default structure
  const DEFAULT_DB = {
    thoughts: [],
    work: [],
    projects: [],
    messages: [],
    admin: null // {email, passHash}
  };

  function loadDB() {
    try {
      const raw = localStorage.getItem(DB_KEY);
      return raw ? JSON.parse(raw) : JSON.parse(JSON.stringify(DEFAULT_DB));
    } catch (e) {
      console.error('DB load error', e);
      return JSON.parse(JSON.stringify(DEFAULT_DB));
    }
  }
  function saveDB(db) {
    localStorage.setItem(DB_KEY, JSON.stringify(db));
    updateStats();
  }
  function resetDB() {
    localStorage.removeItem(DB_KEY);
    showToast('DB cleared');
    initApp();
  }

  // --------------------------
  // Utility helpers
  // --------------------------
  const qs = s => document.querySelector(s);
  const qsa = s => Array.from(document.querySelectorAll(s));
  const uid = () => Math.random().toString(36).slice(2,9);
  function showToast(msg, timeout=2500){
    const t = qs('#toast');
    t.textContent = msg; t.classList.remove('hidden');
    setTimeout(()=> t.classList.add('hidden'), timeout);
  }
  function nowISO(){ return new Date().toISOString(); }

  // --------------------------
  // Image file -> base64 helper
  // --------------------------
  function fileToBase64(file){
    return new Promise((res, rej) => {
      if(!file) return res(null);
      const reader = new FileReader();
      reader.onload = () => res(reader.result);
      reader.onerror = e => rej(e);
      reader.readAsDataURL(file);
    });
  }

  // --------------------------
  // Crypto: hash password (SHA-256)
  // --------------------------
  async function hashText(text){
    const enc = new TextEncoder();
    const data = enc.encode(text);
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    return hashArray.map(b => b.toString(16).padStart(2,'0')).join('');
  }

  // --------------------------
  // DOM elements
  // --------------------------
  const dbEl = loadDB();
  const themeToggle = qs('#themeToggle');
  const yearEl = qs('#year');

  // Sections / UI
  const thoughtGrid = qs('#thoughtGrid');
  const thoughtEditor = qs('#thoughtEditor');
  const thoughtFormEls = {
    title: qs('#thoughtTitle'),
    category: qs('#thoughtCategory'),
    content: qs('#thoughtContent'),
    image: qs('#thoughtImage')
  };
  let editingThoughtId = null;

  const workList = qs('#workList');
  const workEditor = qs('#workEditor');
  const workFormEls = {
    title: qs('#workTitle'),
    tags: qs('#workTags'),
    content: qs('#workContent'),
    image: qs('#workImage')
  };
  let editingWorkId = null;

  const projectGrid = qs('#projectGrid');
  const projectEditor = qs('#projectEditor');
  const projectFormEls = {
    title: qs('#projectTitle'),
    link: qs('#projectLink'),
    desc: qs('#projectDesc'),
    image: qs('#projectImage')
  };
  let editingProjectId = null;

  const contactForm = qs('#contactForm');
  const savedMessagesEl = qs('#savedMessages');

  const adminLoginBox = qs('#loginBox');
  const adminPanel = qs('#adminPanel');
  const adminEmailEl = qs('#adminEmail');
  const adminPassEl = qs('#adminPassword');
  const loginBtn = qs('#loginBtn');
  const setupBtn = qs('#setupBtn');
  const logoutBtn = qs('#logoutBtn');

  // others
  const newThoughtBtn = qs('#newThoughtBtn');
  const cancelThoughtBtn = qs('#cancelThought');
  const saveThoughtBtn = qs('#saveThought');
  const newWorkBtn = qs('#newWorkBtn');
  const cancelWorkBtn = qs('#cancelWork');
  const saveWorkBtn = qs('#saveWork');
  const newProjectBtn = qs('#newProjectBtn');
  const cancelProjectBtn = qs('#cancelProject');
  const saveProjectBtn = qs('#saveProject');
  const exportBtn = qs('#exportBtn');
  const importBtn = qs('#importBtn');
  const importFileInput = qs('#importFile');

  const manageThoughtsBtn = qs('#manageThoughts');
  const manageWorkBtn = qs('#manageWork');
  const manageProjectsBtn = qs('#manageProjects');
  const clearDataBtn = qs('#clearData');
  const downloadDbBtn = qs('#downloadDbBtn');

  const categoryFilter = qs('#categoryFilter');
  const searchThoughts = qs('#searchThoughts');

  // --------------------------
  // Renderers
  // --------------------------
  function updateStats(){
    const db = loadDB();
    qs('#statThoughts').textContent = db.thoughts.length;
    qs('#statProjects').textContent = db.projects.length;
    qs('#statContacts').textContent = db.messages.length;
  }

  function renderCategoryOptions(){
    const db = loadDB();
    const cats = Array.from(new Set(db.thoughts.map(t => t.category || 'Uncategorized')));
    categoryFilter.innerHTML = '<option value="all">All categories</option>' +
      cats.map(c => `<option value="${c}">${c}</option>`).join('');
  }

  function renderThoughts(){
    const db = loadDB();
    const q = searchThoughts.value.trim().toLowerCase();
    const cat = categoryFilter.value;
    let items = db.thoughts.slice().reverse();
    if(cat && cat !== 'all') items = items.filter(it => (it.category||'').toLowerCase()===cat.toLowerCase());
    if(q) items = items.filter(it => (it.title + ' ' + it.content).toLowerCase().includes(q));
    thoughtGrid.innerHTML = items.map(t => `
      <div class="card">
        ${t.image ? `<div style="background-image:url('${t.image}');height:140px;background-size:cover;border-radius:8px;margin-bottom:10px"></div>` : ''}
        <h3>${escapeHtml(t.title)}</h3>
        <small class="muted">${escapeHtml(t.category || 'Uncategorized')} • ${new Date(t.updated||t.created).toLocaleString()}</small>
        <p>${escapeHtml(truncate(t.content, 220))}</p>
        <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:8px">
          <button class="btn small" data-id="${t.id}" data-action="view">View</button>
          <button class="btn small ghost" data-id="${t.id}" data-action="edit">Edit</button>
          <button class="btn small ghost" data-id="${t.id}" data-action="delete">Delete</button>
        </div>
      </div>
    `).join('') || '<div class="card">No thoughts yet — create one!</div>';

    // attach actions
    qsa('[data-action]').forEach(btn => {
      btn.onclick = (e) => {
        const id = btn.dataset.id;
        const action = btn.dataset.action;
        if(action === 'view') viewThought(id);
        if(action === 'edit') openThoughtEditorFor(id);
        if(action === 'delete') { if(confirm('Delete this thought?')) deleteThought(id); }
      };
    });
  }

  function renderWork(){
    const db = loadDB();
    workList.innerHTML = db.work.slice().reverse().map(w => `
      <div class="card">
        <div style="display:flex;gap:12px">
          ${w.image ? `<img src="${w.image}" style="width:140px;height:100px;object-fit:cover;border-radius:6px" />` : ''}
          <div>
            <h3>${escapeHtml(w.title)}</h3>
            <small class="muted">${(w.tags||[]).join(', ')} • ${new Date(w.updated||w.created).toLocaleDateString()}</small>
            <p>${escapeHtml(truncate(w.content,220))}</p>
            <div style="display:flex;gap:8px;justify-content:flex-end">
              <button class="btn small" data-id="${w.id}" data-action="editw">Edit</button>
              <button class="btn small ghost" data-id="${w.id}" data-action="delw">Delete</button>
            </div>
          </div>
        </div>
      </div>
    `).join('') || '<div class="card">No work updates yet.</div>';

    qsa('[data-action="editw"]').forEach(btn => btn.onclick = ()=> openWorkEditorFor(btn.dataset.id));
    qsa('[data-action="delw"]').forEach(btn => btn.onclick = ()=> { if(confirm('Delete update?')) deleteWork(btn.dataset.id); });
  }

  function renderProjects(){
    const db = loadDB();
    projectGrid.innerHTML = db.projects.slice().reverse().map(p => `
      <div class="card project-card">
        <div class="project-thumb" style="background-image:url('${p.image || ''}')"></div>
        <h3>${escapeHtml(p.title)}</h3>
        <p>${escapeHtml(truncate(p.desc,150))}</p>
        <div style="display:flex;gap:8px;justify-content:space-between;align-items:center">
          <div><a href="${p.link||'#'}" target="_blank" class="btn small outline">Open</a></div>
          <div>
            <button class="btn small ghost" data-id="${p.id}" data-action="editp">Edit</button>
            <button class="btn small ghost" data-id="${p.id}" data-action="delp">Delete</button>
          </div>
        </div>
      </div>
    `).join('') || '<div class="card">No projects yet.</div>';

    qsa('[data-action="editp"]').forEach(btn => btn.onclick = ()=> openProjectEditorFor(btn.dataset.id));
    qsa('[data-action="delp"]').forEach(btn => btn.onclick = ()=> { if(confirm('Delete project?')) deleteProject(btn.dataset.id); });
  }

  function renderMessages(){
    const db = loadDB();
    savedMessagesEl.innerHTML = db.messages.slice().reverse().map(m => `<li><strong>${escapeHtml(m.name)}</strong>: ${escapeHtml(truncate(m.message,80))}</li>`).join('') || '<li class="muted">No messages</li>';
  }

  // --------------------------
  // CRUD: Thoughts
  // --------------------------
  async function openThoughtEditorFor(id = null){
    editingThoughtId = id;
    if(id){
      const db = loadDB();
      const it = db.thoughts.find(t => t.id === id);
      if(!it) return showToast('Not found');
      thoughtFormEls.title.value = it.title;
      thoughtFormEls.category.value = it.category || '';
      thoughtFormEls.content.value = it.content;
      thoughtFormEls.image.value = '';
      qs('#thoughtEditorTitle').textContent = 'Edit Thought';
    } else {
      thoughtFormEls.title.value = '';
      thoughtFormEls.category.value = '';
      thoughtFormEls.content.value = '';
      thoughtFormEls.image.value = '';
      qs('#thoughtEditorTitle').textContent = 'New Thought';
    }
    thoughtEditor.classList.remove('hidden');
  }
  async function saveThought(){
    const title = thoughtFormEls.title.value.trim();
    if(!title) return alert('Title required');
    const category = thoughtFormEls.category.value.trim() || 'Uncategorized';
    const content = thoughtFormEls.content.value.trim();
    const file = thoughtFormEls.image.files[0];
    const img = await fileToBase64(file);

    const db = loadDB();
    if(editingThoughtId){
      const t = db.thoughts.find(x => x.id === editingThoughtId);
      if(!t) return showToast('Not found');
      t.title = title; t.category = category; t.content = content; if(img) t.image = img; t.updated = nowISO();
      showToast('Thought updated');
    } else {
      db.thoughts.push({ id: uid(), title, category, content, image: img||null, created: nowISO() });
      showToast('Thought saved');
    }
    saveDB(db);
    thoughtEditor.classList.add('hidden');
    renderCategoryOptions(); renderThoughts();
  }
  function cancelThought(){ thoughtEditor.classList.add('hidden'); }
  function deleteThought(id){
    const db = loadDB();
    db.thoughts = db.thoughts.filter(t => t.id !== id);
    saveDB(db); renderCategoryOptions(); renderThoughts();
    showToast('Thought deleted');
  }
  function viewThought(id){
    const db = loadDB(); const t = db.thoughts.find(x=>x.id===id);
    if(!t) return showToast('Not found');
    // show a quick modal
    const tmp = document.createElement('div');
    tmp.className = 'modal';
    tmp.style.position='fixed'; tmp.style.inset='10px'; tmp.style.background='rgba(0,0,0,0.6)'; tmp.style.display='flex'; tmp.style.alignItems='center'; tmp.style.justifyContent='center';
    tmp.innerHTML = `<div style="max-width:800px;background:var(--card);padding:20px;border-radius:10px;overflow:auto">
      ${t.image?`<img src="${t.image}" style="max-width:100%;border-radius:8px;margin-bottom:8px" />`:''}
      <h2>${escapeHtml(t.title)}</h2>
      <small class="muted">${escapeHtml(t.category)} • ${new Date(t.created).toLocaleString()}</small>
      <div style="margin-top:12px">${nl2br(escapeHtml(t.content))}</div>
      <div style="text-align:right;margin-top:12px"><button id="closeModal" class="btn">Close</button></div>
    </div>`;
    document.body.appendChild(tmp);
    qs('#closeModal').onclick = ()=> tmp.remove();
  }

  // --------------------------
  // CRUD: Work
  // --------------------------
  async function openWorkEditorFor(id = null){
    editingWorkId = id;
    if(id){
      const db = loadDB(); const w = db.work.find(x=>x.id===id);
      workFormEls.title.value = w.title; workFormEls.tags.value = (w.tags||[]).join(',');
      workFormEls.content.value = w.content; workFormEls.image.value = '';
      qs('#workEditorTitle').textContent = 'Edit Work';
    } else {
      workFormEls.title.value = ''; workFormEls.tags.value = ''; workFormEls.content.value = ''; workFormEls.image.value = '';
      qs('#workEditorTitle').textContent = 'New Work';
    }
    workEditor.classList.remove('hidden');
  }
  async function saveWork(){
    const title = workFormEls.title.value.trim(); if(!title) return alert('Title required');
    const tags = workFormEls.tags.value.split(',').map(s=>s.trim()).filter(Boolean);
    const content = workFormEls.content.value.trim();
    const file = workFormEls.image.files[0];
    const img = await fileToBase64(file);

    const db = loadDB();
    if(editingWorkId){
      const w = db.work.find(x=>x.id===editingWorkId);
      w.title = title; w.tags = tags; w.content = content; if(img) w.image = img; w.updated = nowISO();
      showToast('Work updated');
    } else {
      db.work.push({ id: uid(), title, tags, content, image: img||null, created: nowISO() });
      showToast('Work saved');
    }
    saveDB(db); workEditor.classList.add('hidden'); renderWork();
  }
  function cancelWork(){ workEditor.classList.add('hidden'); }
  function deleteWork(id){ const db = loadDB(); db.work = db.work.filter(w=>w.id!==id); saveDB(db); renderWork(); showToast('Work deleted'); }

  // --------------------------
  // CRUD: Projects
  // --------------------------
  async function openProjectEditorFor(id=null){
    editingProjectId = id;
    if(id){
      const db = loadDB(); const p = db.projects.find(x=>x.id===id);
      projectFormEls.title.value = p.title; projectFormEls.link.value = p.link || ''; projectFormEls.desc.value = p.desc || '';
      projectFormEls.image.value = ''; qs('#projectEditorTitle').textContent = 'Edit Project';
    } else {
      projectFormEls.title.value=''; projectFormEls.link.value=''; projectFormEls.desc.value=''; projectFormEls.image.value='';
      qs('#projectEditorTitle').textContent = 'New Project';
    }
    projectEditor.classList.remove('hidden');
  }
  async function saveProject(){
    const title = projectFormEls.title.value.trim(); if(!title) return alert('Title required');
    const link = projectFormEls.link.value.trim();
    const desc = projectFormEls.desc.value.trim();
    const file = projectFormEls.image.files[0];
    const img = await fileToBase64(file);

    const db = loadDB();
    if(editingProjectId){
      const p = db.projects.find(x=>x.id===editingProjectId);
      p.title = title; p.link = link; p.desc = desc; if(img) p.image = img; p.updated = nowISO();
      showToast('Project updated');
    } else {
      db.projects.push({ id: uid(), title, link, desc, image: img||null, created: nowISO() });
      showToast('Project saved');
    }
    saveDB(db); projectEditor.classList.add('hidden'); renderProjects();
  }
  function cancelProject(){ projectEditor.classList.add('hidden'); }
  function deleteProject(id){ const db = loadDB(); db.projects = db.projects.filter(p=>p.id!==id); saveDB(db); renderProjects(); showToast('Project deleted'); }

  // --------------------------
  // Contact handling
  // --------------------------
  contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    const name = qs('#cname').value.trim();
    const email = qs('#cemail').value.trim();
    const subject = qs('#csubject').value.trim();
    const message = qs('#cmessage').value.trim();
    const endpoint = qs('#formspreeEndpoint').value.trim();

    if(!name || !email || !message) return alert('Fill required fields');

    const payload = { name, email, subject, message, created: nowISO() };

    if(endpoint){
      // try post to Formspree (user-provided endpoint). This is optional.
      try {
        const res = await fetch(endpoint, { method: 'POST', headers: {'Content-Type':'application/json'}, body: JSON.stringify(payload) });
        if(res.ok) {
          showToast('Message sent via Formspree');
        } else {
          showToast('Formspree returned error; saved locally');
          saveLocalMessage(payload);
        }
      } catch (err) {
        console.error(err);
        saveLocalMessage(payload);
        showToast('Network error; saved locally');
      }
    } else {
      saveLocalMessage(payload);
      showToast('Message saved locally');
    }

    contactForm.reset();
    renderMessages();
  });

  function saveLocalMessage(payload){
    const db = loadDB();
    db.messages.push({ id: uid(), ...payload });
    saveDB(db);
  }

  // --------------------------
  // Admin (client-side)
  // --------------------------
  async function setupAdmin(){
    const email = adminEmailEl.value.trim();
    const pass = adminPassEl.value;
    if(!email || !pass) return alert('Email + password required');
    const passHash = await hashText(pass);
    const db = loadDB();
    db.admin = { email, passHash };
    saveDB(db);
    showToast('Admin created (local)');
  }

  async function loginAdmin(){
    const email = adminEmailEl.value.trim();
    const pass = adminPassEl.value;
    if(!email || !pass) return alert('Email + password required');
    const db = loadDB();
    if(!db.admin) return alert('No admin set. Use Create Admin.');
    const passHash = await hashText(pass);
    if(db.admin.email === email && db.admin.passHash === passHash){
      // show admin panel
      adminLoginBox.classList.add('hidden');
      adminPanel.classList.remove('hidden');
      logoutBtn.classList.remove('hidden');
      showToast('Logged in');
    } else {
      alert('Invalid credentials');
    }
  }

  function logout(){
    adminLoginBox.classList.remove('hidden');
    adminPanel.classList.add('hidden');
    logoutBtn.classList.add('hidden');
    showToast('Logged out');
  }

  function adminManage(area){
    const adminArea = qs('#adminArea');
    const db = loadDB();
    if(area === 'thoughts'){
      adminArea.innerHTML = `<h4>All Thoughts</h4>` + db.thoughts.map(t => `<div style="padding:8px;border-bottom:1px solid rgba(0,0,0,0.04)"><strong>${escapeHtml(t.title)}</strong> • ${escapeHtml(t.category)} <div style="float:right"><button class="btn small" data-id="${t.id}" data-act="edit">Edit</button> <button class="btn small ghost" data-id="${t.id}" data-act="del">Delete</button></div><div style="clear:both"></div></div>`).join('');
      qsa('#adminArea [data-act]').forEach(b=> b.onclick = ()=>{ if(b.dataset.act==='edit') openThoughtEditorFor(b.dataset.id); else if(confirm('Delete?')) deleteThought(b.dataset.id); });
    } else if(area === 'work'){
      adminArea.innerHTML = `<h4>Work</h4>` + db.work.map(w => `<div style="padding:8px;border-bottom:1px solid rgba(0,0,0,0.04)"><strong>${escapeHtml(w.title)}</strong> <div style="float:right"><button class="btn small" data-id="${w.id}" data-act="editw">Edit</button><button class="btn small ghost" data-id="${w.id}" data-act="delw">Delete</button></div><div style="clear:both"></div></div>`).join('');
      qsa('#adminArea [data-act="editw"]').forEach(b=> b.onclick = ()=> openWorkEditorFor(b.dataset.id));
      qsa('#adminArea [data-act="delw"]').forEach(b=> b.onclick = ()=> { if(confirm('Delete?')) deleteWork(b.dataset.id); });
    } else if(area === 'projects'){
      adminArea.innerHTML = `<h4>Projects</h4>` + db.projects.map(p => `<div style="padding:8px;border-bottom:1px solid rgba(0,0,0,0.04)"><strong>${escapeHtml(p.title)}</strong> <div style="float:right"><button class="btn small" data-id="${p.id}" data-act="editp">Edit</button><button class="btn small ghost" data-id="${p.id}" data-act="delp">Delete</button></div><div style="clear:both"></div></div>`).join('');
      qsa('#adminArea [data-act="editp"]').forEach(b=> b.onclick = ()=> openProjectEditorFor(b.dataset.id));
      qsa('#adminArea [data-act="delp"]').forEach(b=> b.onclick = ()=> { if(confirm('Delete?')) deleteProject(b.dataset.id); });
    }
  }

  // --------------------------
  // Export / Import
  // --------------------------
  exportBtn.addEventListener('click', () => {
    const db = loadDB();
    const blob = new Blob([JSON.stringify(db, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a'); a.href = url; a
