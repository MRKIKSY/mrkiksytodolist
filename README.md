Explanation of the Javascript Source Codes 
In summary, this code below  implements a simple note-taking application using HTML, CSS, and JavaScript, 
with the ability to add, delete, and update notes, and it persists the notes in local storage.

const btnEl = document.getElementById("btn");
This line selects an HTML element with the ID "btn" and assigns it to the constant variable btnEl.


const appEl = document.getElementById("app");
This line selects an HTML element with the ID "app" and assigns it to the constant variable appEl.

getNotes().forEach((note) => {
  const noteEl = createNoteEl(note.id, note.content);
  appEl.insertBefore(noteEl, btnEl);
});

This line calls the getNotes() function to retrieve an array of notes from local storage and then iterates over each note using forEach. 
Inside the loop, it creates a new textarea element for each note using the createNoteEl function and inserts it before the button (btnEl) in the app element (appEl).

function createNoteEl(id, content) {
  const element = document.createElement("textarea");
  element.classList.add("note");
  element.placeholder = "Empty Note";
  element.value = content;


  element.addEventListener("dblclick", () => {
    const warning = confirm("Do you want to delete this note?");
    if (warning) {
      deleteNote(id, element);
    }
  });

  element.addEventListener("input", () => {
    updateNote(id, element.value);
  });

  The createNoteEl function is defined as follows:

const element = document.createElement("textarea"); creates a new textarea element.
element.classList.add("note"); adds the "note" class to the created textarea element.
element.placeholder = "Empty Note"; sets the placeholder text for the textarea.
element.value = content; sets the content of the textarea based on the provided note's content.
Two event listeners are added to the textarea:
dblclick: If the user double-clicks on the textarea, it prompts a confirmation dialog to delete the note using the deleteNote function.
input: Whenever the content of the textarea changes, it updates the note using the updateNote function.

  return element;
}

function deleteNote(id, element) {
    const notes = getNotes().filter((note)=>note.id != id)
    saveNote(notes)
    appEl.removeChild(element)
}


function deleteNote(id, element) {...}

This function deletes a note by filtering out the note with the specified id from the array of notes obtained from getNotes().
It then saves the updated notes using saveNote and removes the corresponding HTML element from the app element.

function updateNote(id, content) {
  const notes = getNotes();
  const target = notes.filter((note) => note.id == id)[0];
  target.content = content;
  saveNote(notes);
}
function updateNote(id, content) {...}

This function updates the content of a note with the specified id in the array of notes obtained from getNotes(). It then saves the updated notes using saveNote.

function addNote() {
  const notes = getNotes();
  const noteObj = {
    id: Math.floor(Math.random() * 100000),
    content: "",
  };
  const noteEl = createNoteEl(noteObj.id, noteObj.content);
  appEl.insertBefore(noteEl, btnEl);

  notes.push(noteObj);

  saveNote(notes);
}

function addNote() {...}

This function adds a new note with a randomly generated ID and empty content. It creates a new note element using createNoteEl, 
inserts it before the button (btnEl) in the app element (appEl), adds the new note object to the array of notes, and saves the updated notes using saveNote.

function saveNote(notes) {
  localStorage.setItem("note-app", JSON.stringify(notes));
}
function saveNote(notes) {...}

This function saves the array of notes to local storage, converting it to a JSON string.

function getNotes() {
  return JSON.parse(localStorage.getItem("note-app") || "[]");
}
function getNotes() {...}

This function retrieves the array of notes from local storage, parsing it from the stored JSON string. If there are no notes, it returns an empty array.

btnEl.addEventListener("click", addNote);
btnEl.addEventListener("click", addNote);

This line adds an event listener to the button (btnEl). When the button is clicked, it calls the addNote function to add a new note.
