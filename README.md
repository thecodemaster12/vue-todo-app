# 📝 Vue 3 Todo App

A simple and clean **Vue 3 Composition API Todo Application** that supports:

- Adding tasks  
- Marking tasks as complete  
- Filtering (All / Active / Completed)  
- Deleting tasks  
- Saving data in **localStorage**  
- Username persistence  
- TailwindCSS-based styling  

---

## 🚀 Features

### ✔ Add Tasks  
Create new tasks using the input field. Tasks are stored with a timestamp-based ID.

### ✔ Task Filters  
Switch between:  
- **All** tasks  
- **Active** (not completed)  
- **Completed** tasks  

### ✔ Local Storage Persistence  
Both the **username** and the **todos list** are automatically synced with localStorage.

### ✔ Clean UI  
Uses TailwindCSS-based classes for a modern layout.

---

## 📂 Project Structure

The main logic lives in a single Vue component using `<script setup>`:

- **Reactive State:**  
  `todos`, `username`, `inputContent`, `filter`

- **Computed Properties:**  
  `filteredData`, `hasTodos`

- **Watchers:**  
  Automatically store updates to `username` and `todos` in localStorage.

- **Lifecycle Hook:**  
  `onMounted()` retrieves localStorage data on load.