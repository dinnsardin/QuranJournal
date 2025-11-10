// Muhammad Rafi'uddin bin Zulkifli (preload.js)
const { contextBridge } = require('electron');

contextBridge.exposeInMainWorld('electronAPI', {
});
