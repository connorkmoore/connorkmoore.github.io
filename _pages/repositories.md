---
layout: page
permalink: /bulletin/
title: bulletin
description: Leave a sticky note about Connor!
nav: true
nav_order: 4
---

<style>
  :root {
    --note-yellow: #f8e9a1;
    --note-blue-accent: #2591a3;
    --note-purple-accent: #c715b0;
    --note-blue-pale: #dbeaf6;
    --note-purple-pale: #f3d1ee;
    --card-bg-light: rgba(0,0,0,0.02);
    --card-bg-dark: rgba(255,255,255,0.06);
    --card-border-light: rgba(0,0,0,0.08);
    --card-border-dark: rgba(255,255,255,0.15);
    --card-bg: var(--card-bg-light);
    --board-bg: radial-gradient(circle at 10% 20%, rgba(255,255,255,0.08) 0, rgba(255,255,255,0) 25%),
                 radial-gradient(circle at 90% 10%, rgba(255,255,255,0.07) 0, rgba(255,255,255,0) 22%),
                 #f7f2e9;
  }

  .bulletin-board {
    position: relative;
    background: var(--board-bg);
    border: 1px solid rgba(0,0,0,0.05);
    padding: 2.5rem;
    border-radius: 14px;
    box-shadow: 0 16px 40px rgba(0,0,0,0.12);
    min-height: 540px;
    cursor: pointer;
    overflow: hidden;
  }
  
  .add-note-btn {
    position: fixed;
    bottom: 24px;
    right: 24px;
    width: 56px;
    height: 56px;
    border-radius: 50%;
    background: linear-gradient(135deg, #ffb347 0%, #ffcc33 100%);
    border: none;
    font-size: 28px;
    cursor: pointer;
    color: #432;
    box-shadow: 0 10px 24px rgba(0,0,0,0.25);
    z-index: 1000;
    transition: transform 0.16s ease, box-shadow 0.16s ease;
  }
  
  .add-note-btn:hover {
    transform: translateY(-2px) scale(1.03);
    box-shadow: 0 14px 28px rgba(0,0,0,0.28);
  }
  
  .sticky-notes {
    position: relative;
    width: 100%;
    height: 100%;
    min-height: 500px;
  }
  
  .sticky-note {
    background: var(--note-yellow);
    padding: 14px 16px;
    border-radius: 8px 8px 10px 10px;
    box-shadow: 0 10px 26px rgba(0,0,0,0.18);
    min-height: 110px;
    position: absolute;
    color: #111;
    font-family: inherit;
    transform: rotate(var(--rotation)) scale(var(--scale, 1));
    transition: transform 0.18s ease, box-shadow 0.18s ease, padding 0.18s ease;
    cursor: pointer;
    overflow-wrap: break-word;
    width: 160px;
    left: var(--pos-x);
    top: var(--pos-y);
    padding: calc(14px * var(--scale, 1)) calc(16px * var(--scale, 1));
  }

  .sticky-note::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 8px;
    border-radius: 8px 8px 0 0;
    background: rgba(0,0,0,0.08);
  }
  
  .sticky-note:hover {
    transform: rotate(0deg) scale(calc(1.04 * var(--scale, 1)));
    box-shadow: 0 16px 32px rgba(0,0,0,0.25);
    z-index: 10;
  }
  
  .sticky-note.blue { background: var(--note-blue-pale); }
  .sticky-note.purple { background: var(--note-purple-pale); }
  .sticky-note.blue::before { background: var(--note-blue-accent); }
  .sticky-note.purple::before { background: var(--note-purple-accent); }
  
  .sticky-note.upvoted {
    padding: calc(18px * var(--scale, 1)) calc(16px * var(--scale, 1)) calc(14px * var(--scale, 1)) calc(16px * var(--scale, 1));
  }
  
  .sticky-note.downvoted {
    padding: calc(10px * var(--scale, 1)) calc(16px * var(--scale, 1)) calc(14px * var(--scale, 1)) calc(16px * var(--scale, 1));
  }
  
  .note-content {
    font-size: calc(15px * var(--scale, 1));
    line-height: 1.5;
    color: #111;
  }
  
  .note-author {
    font-size: calc(12px * var(--scale, 1));
    font-style: italic;
    margin-top: 8px;
    margin-bottom: 8px;
    text-align: right;
    opacity: 0.75;
    color: #222;
  }
  
  .note-votes {
    display: flex;
    gap: calc(6px * var(--scale, 1));
    justify-content: center;
    align-items: center;
    margin-top: 8px;
  }
  
  .vote-btn {
    background: rgba(0,0,0,0.08);
    border: none;
    border-radius: 4px;
    width: calc(28px * var(--scale, 1));
    height: calc(24px * var(--scale, 1));
    cursor: pointer;
    font-size: calc(14px * var(--scale, 1));
    transition: background 0.16s ease;
    color: #111;
  }
  
  .vote-btn:hover {
    background: rgba(0,0,0,0.16);
  }
  
  .vote-count {
    font-size: calc(12px * var(--scale, 1));
    font-weight: 600;
    color: #222;
  }
  
  .delete-note {
    position: absolute;
    top: 8px;
    right: 8px;
    background: rgba(0,0,0,0.14);
    color: #222;
    border: none;
    border-radius: 50%;
    width: 22px;
    height: 22px;
    font-size: 12px;
    cursor: pointer;
    opacity: 0;
    transition: opacity 0.18s ease;
  }
  
  .sticky-note:hover .delete-note {
    opacity: 1;
  }
  
  .modal {
    display: none;
    position: fixed;
    z-index: 2000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.55);
  }
  
  .modal-content {
    background: var(--card-bg);
    margin: 8% auto;
    padding: 24px;
    border-radius: 12px;
    width: 92%;
    max-width: 520px;
    box-shadow: 0 14px 36px rgba(0,0,0,0.18);
    border: 1px solid var(--card-border-light);
    color: inherit;
  }
  
  .modal-content h2 {
    margin-top: 0;
  }
  
  .form-group {
    margin-bottom: 18px;
  }
  
  .form-group label {
    display: block;
    margin-bottom: 6px;
    font-weight: 600;
  }
  
  .form-group textarea,
  .form-group input {
    width: 100%;
    padding: 12px;
    border: 1px solid rgba(0,0,0,0.12);
    border-radius: 8px;
    font-size: 15px;
    font-family: inherit;
    background: #fffdf8;
  }
  
  .form-group textarea {
    min-height: 110px;
    resize: vertical;
  }
  
  .color-picker {
    display: flex;
    gap: 10px;
    margin-top: 10px;
  }
  
  .color-option {
    width: 40px;
    height: 40px;
    border-radius: 8px;
    cursor: pointer;
    border: 3px solid transparent;
    transition: border 0.16s ease;
  }
  
  .color-option:hover,
  .color-option.selected {
    border-color: #222;
  }
  
  .btn-group {
    display: flex;
    gap: 10px;
    margin-top: 16px;
  }
  
  .btn {
    flex: 1;
    padding: 12px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.18s ease, transform 0.12s ease;
  }
  
  .btn-primary {
    background-color: #2f855a;
    color: white;
  }
  
  .btn-primary:hover {
    background-color: #276749;
    transform: translateY(-1px);
  }
  
  .btn-secondary {
    background-color: #718096;
    color: white;
  }
  
  .btn-secondary:hover {
    background-color: #4a5568;
    transform: translateY(-1px);
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --board-bg: radial-gradient(circle at 10% 20%, rgba(255,255,255,0.04) 0, rgba(255,255,255,0) 25%),
                   radial-gradient(circle at 90% 10%, rgba(255,255,255,0.03) 0, rgba(255,255,255,0) 22%),
                   #1d1b1a;
      --card-bg: var(--card-bg-dark);
    }
    .bulletin-board { box-shadow: 0 16px 40px rgba(0,0,0,0.5); }
    .sticky-note { box-shadow: 0 12px 28px rgba(0,0,0,0.4); }
    .sticky-note, .sticky-note .note-content, .sticky-note .note-author { color: #111 !important; }
    .modal-content { border-color: var(--card-border-dark); }
    .form-group textarea,
    .form-group input { background: #1f1f1f; color: #f1f1f1; border-color: rgba(255,255,255,0.08); }
    .delete-note { background: rgba(0,0,0,0.35); color: #eee; }
  }
</style>

<div class="bulletin-board">
  <div class="sticky-notes" id="notesContainer"></div>
</div>

<!-- Modal -->
<div id="noteModal" class="modal">
  <div class="modal-content">
    <h2>Add a Sticky Note</h2>
    <div class="form-group">
      <label for="noteText">Your message about Connor:</label>
      <textarea id="noteText" placeholder="Leave your thoughts here..." maxlength="200"></textarea>
    </div>
    <div class="form-group">
      <label for="noteAuthor">Your name (optional):</label>
      <input type="text" id="noteAuthor" placeholder="Anonymous" maxlength="30">
    </div>
    <div class="form-group">
      <label>Choose a color:</label>
      <div class="color-picker">
        <div class="color-option selected" style="background: var(--note-yellow);" data-color="yellow" title="Yellow"></div>
        <div class="color-option" style="background: var(--note-blue-accent);" data-color="blue" title="Blue"></div>
        <div class="color-option" style="background: var(--note-purple-accent);" data-color="purple" title="Purple"></div>
      </div>
    </div>
    <div class="btn-group">
      <button class="btn btn-secondary" onclick="closeModal()">Cancel</button>
      <button class="btn btn-primary" onclick="addNote()">Add Note</button>
    </div>
  </div>
</div>

<!-- Firebase -->
<script type="module">
  import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js';
  import { getFirestore, collection, addDoc, onSnapshot, deleteDoc, doc, orderBy, query, updateDoc } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js';

  // Firebase configuration
  const firebaseConfig = {
    apiKey: "AIzaSyBrnIose4SrawPtAlgsVZVChBLjs0oygbg",
    authDomain: "connorcontent.firebaseapp.com",
    projectId: "connorcontent",
    storageBucket: "connorcontent.firebasestorage.app",
    messagingSenderId: "788320571256",
    appId: "1:788320571256:web:045d2a5e811fb9856e028d"
  };

  // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);
  const notesCollection = collection(db, 'notes');

  // Make functions globally available
  window.firebaseLoaded = true;
  window.db = db;
  window.notesCollection = notesCollection;
  window.addDoc = addDoc;
  window.deleteDoc = deleteDoc;
  window.doc = doc;
  window.updateDoc = updateDoc;
  
  // Load notes in real-time
  const positionCache = {}; // Cache generated positions for stable rendering
  const q = query(notesCollection, orderBy('timestamp', 'desc'));
  onSnapshot(q, (snapshot) => {
    const container = document.getElementById('notesContainer');
    container.innerHTML = '';
    
    snapshot.forEach((docSnapshot) => {
      const note = docSnapshot.data();
      const noteDiv = document.createElement('div');
      noteDiv.className = `sticky-note ${note.color}`;
      noteDiv.id = `note-${docSnapshot.id}`;
      
      // Get vote state
      const netVotes = Object.values(note.votes || {}).reduce((a, b) => a + b, 0);
      
      // Calculate scale: min 0.7, max 1.5 based on votes
      // 0 votes = 1.0 scale, +/- votes adjust proportionally
      const scale = Math.max(0.7, Math.min(1.5, 1.0 + (netVotes * 0.04)));
      
      // Calculate position (use stored or cache, generate only once)
      let posX = note.posX;
      let posY = note.posY;
      if (posX === undefined || posX === null) {
        if (!positionCache[docSnapshot.id]) {
          positionCache[docSnapshot.id] = {
            x: Math.random() * 70,
            y: Math.random() * 70
          };
        }
        posX = positionCache[docSnapshot.id].x;
        posY = positionCache[docSnapshot.id].y;
      }
      
      noteDiv.style.setProperty('--rotation', `${(Math.random() - 0.5) * 7}deg`);
      noteDiv.style.setProperty('--pos-x', `${posX}%`);
      noteDiv.style.setProperty('--pos-y', `${posY}%`);
      noteDiv.style.setProperty('--scale', scale);
      
      if (netVotes > 0) noteDiv.classList.add('upvoted');
      if (netVotes < 0) noteDiv.classList.add('downvoted');
      
      const extraHeight = Math.min(120, Math.max(0, (note.text || '').length * 0.35));
      noteDiv.style.minHeight = `${110 + extraHeight}px`;
      
      // Get user ID from localStorage (generate if needed)
      const userId = getUserId();
      const userVote = note.votes && note.votes[userId] ? note.votes[userId] : 0;
      
      noteDiv.innerHTML = `
        <button class="delete-note" onclick="deleteNote('${docSnapshot.id}')">×</button>
        <div class="note-content">${escapeHtml(note.text)}</div>
        <div class="note-author">- ${escapeHtml(note.author || 'Anonymous')}</div>
        <div class="note-votes">
          <button class="vote-btn ${userVote === 1 ? 'voted-up' : ''}" onclick="event.stopPropagation(); voteNote('${docSnapshot.id}', 1)">👍</button>
          <span class="vote-count">${netVotes > 0 ? '+' : ''}${netVotes}</span>
          <button class="vote-btn ${userVote === -1 ? 'voted-down' : ''}" onclick="event.stopPropagation(); voteNote('${docSnapshot.id}', -1)">👎</button>
        </div>
      `;
      container.appendChild(noteDiv);
    });
  });
  
  window.escapeHtml = function(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  };
</script>

<script>
  let selectedColor = 'yellow';
  
  // Color picker
  document.querySelectorAll('.color-option').forEach(option => {
    option.addEventListener('click', function() {
      document.querySelectorAll('.color-option').forEach(o => o.classList.remove('selected'));
      this.classList.add('selected');
      selectedColor = this.dataset.color;
    });
  });
  
  // Modal functions
  function openModal() {
    document.getElementById('noteModal').style.display = 'block';
  }
  
  function closeModal() {
    document.getElementById('noteModal').style.display = 'none';
    document.getElementById('noteText').value = '';
    document.getElementById('noteAuthor').value = '';
  }
  
  // Close modal when clicking outside
  window.onclick = function(event) {
    const modal = document.getElementById('noteModal');
    if (event.target == modal) {
      closeModal();
    }
  }
  
  // Track click position
  let clickX = 0, clickY = 0;
  document.querySelector('.bulletin-board').addEventListener('click', function(e) {
    if (!e.target.closest('.sticky-note') && !e.target.closest('.delete-note')) {
      const rect = this.getBoundingClientRect();
      clickX = ((e.clientX - rect.left) / rect.width) * 100;
      clickY = ((e.clientY - rect.top) / rect.height) * 100;
      // Clamp to board bounds
      clickX = Math.max(0, Math.min(80, clickX));
      clickY = Math.max(0, Math.min(80, clickY));
      openModal();
    }
  });

  // Add new note to Firebase
  async function addNote() {
    const text = document.getElementById('noteText').value.trim();
    if (!text) {
      alert('Please write a message!');
      return;
    }
    
    const author = document.getElementById('noteAuthor').value.trim();
    
    try {
      await window.addDoc(window.notesCollection, {
        text: text,
        author: author || 'Anonymous',
        color: selectedColor,
        timestamp: new Date(),
        posX: clickX,
        posY: clickY,
        votes: {}
      });
      closeModal();
      clickX = 0;
      clickY = 0;
    } catch (error) {
      console.error('Error adding note:', error);
      alert('Error adding note. Please try again.');
    }
  }
  
  // Delete note from Firebase
  async function deleteNote(noteId) {
    if (confirm('Delete this note?')) {
      try {
        await window.deleteDoc(window.doc(window.db, 'notes', noteId));
      } catch (error) {
        console.error('Error deleting note:', error);
        alert('Error deleting note. Please try again.');
      }
    }
  }
  
  // Get or create user ID (persisted in localStorage)
  function getUserId() {
    let userId = localStorage.getItem('bulletinUserId');
    if (!userId) {
      userId = 'user-' + Math.random().toString(36).substr(2, 9);
      localStorage.setItem('bulletinUserId', userId);
    }
    return userId;
  }
  
  // Vote on a note (limit 3 votes per note per user)
  async function voteNote(noteId, direction) {
    const userId = getUserId();
    const voteKey = `vote-${noteId}-${userId}`;
    const voteHistory = JSON.parse(localStorage.getItem(voteKey) || '0');
    
    // Check vote limit (max 3 votes per note per user)
    const voteCount = Math.abs(voteHistory) || 0;
    if (voteCount >= 3 && voteHistory === direction) {
      alert('You\'ve reached the voting limit for this note!');
      return;
    }
    
    // Toggle or change vote
    let newVote = direction;
    if (voteHistory === direction) {
      newVote = 0; // Unvote
    }
    
    try {
      const noteRef = window.doc(window.db, 'notes', noteId);
      const votes = JSON.parse(localStorage.getItem(`votes-${noteId}`) || '{}');
      
      // Update vote
      if (newVote === 0) {
        delete votes[userId];
      } else {
        votes[userId] = newVote;
      }
      
      await window.updateDoc(noteRef, { votes });
      
      // Track in localStorage
      const newCount = voteCount + (newVote === 0 ? -1 : 1);
      localStorage.setItem(voteKey, String(newCount) * (newVote > 0 ? 1 : -1));
    } catch (error) {
      console.error('Error voting:', error);
      alert('Error voting. Please try again.');
    }
  }
  
  // Escape HTML to prevent XSS
  function escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  }
</script>
