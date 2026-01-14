---
layout: page
permalink: /bulletin/
title: bulletin
description: Leave a sticky note about Connor!
nav: true
nav_order: 4
---

<style>
  .bulletin-board {
    position: relative;
    background: linear-gradient(135deg, #8B4513 0%, #654321 100%);
    padding: 3rem;
    border-radius: 10px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.3);
    min-height: 600px;
  }
  
  .add-note-btn {
    position: fixed;
    bottom: 30px;
    right: 30px;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background: #FFD700;
    border: none;
    font-size: 30px;
    cursor: pointer;
    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    z-index: 1000;
    transition: transform 0.2s;
  }
  
  .add-note-btn:hover {
    transform: scale(1.1);
  }
  
  .sticky-notes {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
    margin-top: 20px;
  }
  
  .sticky-note {
    background: #FFEB3B;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    min-height: 150px;
    position: relative;
    transform: rotate(var(--rotation));
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: pointer;
  }
  
  .sticky-note:hover {
    transform: rotate(0deg) scale(1.05);
    box-shadow: 0 10px 25px rgba(0,0,0,0.3);
    z-index: 10;
  }
  
  .sticky-note.pink { background: #FFB6C1; }
  .sticky-note.blue { background: #ADD8E6; }
  .sticky-note.green { background: #90EE90; }
  .sticky-note.orange { background: #FFB347; }
  
  .note-content {
    font-family: 'Comic Sans MS', cursive;
    font-size: 14px;
    line-height: 1.5;
    word-wrap: break-word;
  }
  
  .note-author {
    font-size: 11px;
    font-style: italic;
    margin-top: 10px;
    text-align: right;
    opacity: 0.7;
  }
  
  .delete-note {
    position: absolute;
    top: 5px;
    right: 5px;
    background: rgba(255,0,0,0.6);
    color: white;
    border: none;
    border-radius: 50%;
    width: 20px;
    height: 20px;
    font-size: 12px;
    cursor: pointer;
    opacity: 0;
    transition: opacity 0.2s;
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
    background-color: rgba(0,0,0,0.6);
  }
  
  .modal-content {
    background-color: #fefefe;
    margin: 10% auto;
    padding: 30px;
    border-radius: 10px;
    width: 90%;
    max-width: 500px;
    box-shadow: 0 10px 40px rgba(0,0,0,0.3);
  }
  
  .modal-content h2 {
    margin-top: 0;
  }
  
  .form-group {
    margin-bottom: 20px;
  }
  
  .form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
  }
  
  .form-group textarea,
  .form-group input {
    width: 100%;
    padding: 10px;
    border: 2px solid #ddd;
    border-radius: 5px;
    font-size: 14px;
  }
  
  .form-group textarea {
    min-height: 100px;
    resize: vertical;
    font-family: 'Comic Sans MS', cursive;
  }
  
  .color-picker {
    display: flex;
    gap: 10px;
    margin-top: 10px;
  }
  
  .color-option {
    width: 40px;
    height: 40px;
    border-radius: 5px;
    cursor: pointer;
    border: 3px solid transparent;
    transition: border 0.2s;
  }
  
  .color-option:hover,
  .color-option.selected {
    border-color: #333;
  }
  
  .btn-group {
    display: flex;
    gap: 10px;
    margin-top: 20px;
  }
  
  .btn {
    flex: 1;
    padding: 12px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.2s;
  }
  
  .btn-primary {
    background-color: #4CAF50;
    color: white;
  }
  
  .btn-primary:hover {
    background-color: #45a049;
  }
  
  .btn-secondary {
    background-color: #999;
    color: white;
  }
  
  .btn-secondary:hover {
    background-color: #777;
  }
</style>

<div class="bulletin-board">
  <h2 style="color: white; text-align: center; margin-bottom: 30px;">📌 Bulletin Board</h2>
  <p style="color: white; text-align: center; font-size: 14px; margin-top: -20px;">Everyone can see all notes!</p>
  <div class="sticky-notes" id="notesContainer"></div>
</div>

<button class="add-note-btn" onclick="openModal()">+</button>

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
        <div class="color-option selected" style="background: #FFEB3B;" data-color="yellow"></div>
        <div class="color-option" style="background: #FFB6C1;" data-color="pink"></div>
        <div class="color-option" style="background: #ADD8E6;" data-color="blue"></div>
        <div class="color-option" style="background: #90EE90;" data-color="green"></div>
        <div class="color-option" style="background: #FFB347;" data-color="orange"></div>
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
  import { getFirestore, collection, addDoc, onSnapshot, deleteDoc, doc, orderBy, query } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js';

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
  
  // Load notes in real-time
  const q = query(notesCollection, orderBy('timestamp', 'desc'));
  onSnapshot(q, (snapshot) => {
    const container = document.getElementById('notesContainer');
    container.innerHTML = '';
    
    snapshot.forEach((docSnapshot) => {
      const note = docSnapshot.data();
      const noteDiv = document.createElement('div');
      noteDiv.className = `sticky-note ${note.color}`;
      noteDiv.style.setProperty('--rotation', `${(Math.random() - 0.5) * 6}deg`);
      noteDiv.innerHTML = `
        <button class="delete-note" onclick="deleteNote('${docSnapshot.id}')">×</button>
        <div class="note-content">${escapeHtml(note.text)}</div>
        <div class="note-author">- ${escapeHtml(note.author || 'Anonymous')}</div>
      `;
      container.appendChild(noteDiv);
    });
  });
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
        timestamp: new Date()
      });
      closeModal();
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
  
  // Escape HTML to prevent XSS
  function escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  }
  
  // Load notes on page load
  loadNotes();
</script>
