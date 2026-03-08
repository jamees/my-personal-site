# Horario - Testing Notes

## ✅ Implemented Features

### 1. localStorage Persistence
- Changes made via drag-and-drop are automatically saved to browser's localStorage
- Changes persist across page reloads
- Sync status indicator shows when changes are unsaved

### 2. Sync Status Indicator
- Located in top-right corner
- Shows "Cambios sin sincronizar" when there are local changes
- Shows "Cambios guardados localmente" briefly after saving
- Yellow background for unsaved, green for saved

### 3. Restore Original Button
- Located in the sync status indicator
- Clicking "↻ Restaurar" restores the original schedule from JSON
- Asks for confirmation before restoring
- Clears localStorage and reloads the page

### 4. Drag-and-Drop Editing
- Works on both desktop and mobile (touch events)
- Activated via the ✏️ button
- Visual feedback: dragging cell becomes semi-transparent, target shows dashed border
- Cells swap content, classes, and styles
- Changes are automatically saved to localStorage after each swap

## 🧪 How to Test

### Test 1: Basic Drag-and-Drop (Desktop)
1. Open `horario.html` in a browser
2. Click the ✏️ (edit) button - it should turn pink with pulse animation
3. Drag any subject cell to another cell
4. The cells should swap positions
5. Sync status indicator should appear showing "Cambios sin sincronizar"

### Test 2: localStorage Persistence
1. Make some changes using drag-and-drop
2. Refresh the page (F5 or Cmd+R)
3. The changes should persist
4. Sync status should still show "Cambios sin sincronizar"

### Test 3: Restore Original
1. Make some changes using drag-and-drop
2. Click the "↻ Restaurar" button in the sync status indicator
3. Confirm the dialog
4. Page should reload with original schedule from JSON
5. Sync status should disappear

### Test 4: Mobile Drag-and-Drop
1. Open `horario.html` on a mobile device or use browser dev tools mobile emulation
2. Click the ✏️ button to enable edit mode
3. Touch and hold a subject cell
4. Drag it to another cell
5. Release to swap
6. Changes should be saved to localStorage

### Test 5: PDF Export with Changes
1. Make some changes using drag-and-drop
2. Click the 📄 (PDF export) button
3. PDF should export with the modified schedule
4. Background colors should be preserved

### Test 6: Legend Highlighting
1. Click the 📚 button to show the legend
2. Click on any book (subject)
3. That subject should be highlighted in the schedule
4. Other subjects should fade out
5. Click the same book again to deselect

## 📝 Technical Details

### localStorage Key
- Key: `horario-changes`
- Value: JSON array of horarios (schedule data)

### Data Structure Saved
```json
[
  {
    "hora": "08:10",
    "lunes": { "materia": "...", "clase": "...", "style": "..." },
    "martes": { "materia": "...", "clase": "...", "style": "..." },
    ...
  },
  ...
]
```

### Functions Added
- `saveToLocalStorage()` - Saves current schedule to localStorage
- `getCurrentHorarioData()` - Extracts current schedule from DOM
- `getCellData(cell)` - Extracts data from a single cell
- `updateSyncStatus(status)` - Updates sync indicator UI
- `restoreOriginal()` - Clears localStorage and reloads
- `renderHorario(horarios)` - Renders desktop schedule table
- `renderMobileSchedule(horarios)` - Renders mobile schedule cards

### Modified Functions
- `cargarHorario()` - Now checks localStorage on load
- `swapCells()` - Now calls `saveToLocalStorage()` after swap

## 🐛 Known Limitations

1. Changes are only saved locally in the browser
2. To sync changes to the JSON file, user must manually copy the JSON from the "Guardar Cambios" modal
3. Mobile schedule view (cards) doesn't update in real-time when editing - requires page reload
4. No undo/redo functionality (only full restore)

## 🚀 Future Enhancements

1. Add server-side API to save changes permanently
2. Update mobile view in real-time when editing
3. Add undo/redo functionality
4. Add visual indicator on cells that have been modified
5. Add export of changes as JSON file download
