# qms-web

Q Mailbox - Web interface for file-based messenger

## Phase 6 Step 7 - Повний інтерфейс

**URL:** https://cattus-martius.github.io/qms-web/qms.html

### Функціонал

**Chat:**
- Відправка текстових повідомлень з телеметрією
- Відображення типу контенту (текст, 📸 Camera Frame, 🎤 Audio, 📋 Clipboard, 🎬 Motion)
- Кнопка Details для перегляду повного JSON
- Кнопка "Новий сеанс" (очищає mailbox)

**Media:**
- Camera: Send Frame, Send Motion, Switch Camera, Zoom slider
- Microphone: Record (toggle)
- Speaker: Test Voice (локально)
- Clipboard: Refresh Clipboard (превʼю), Send Clipboard

**Settings:**
- 8 датчиків телеметрії (Light, Accelerometer, Gyroscope, Compass, Battery, Pressure, Proximity, GPS)

### Тестування (16-17.02.2026)

**Протестовано на S23:**
- Camera Frame: працює
- Clipboard: працює (показує превʼю та статус)
- Audio: працює (транскрибується через Whisper)
- GPS: працює (координати Мілана)
- Телеметрія: 4 датчики працюють (accelerometer, gyroscope, compass, battery)
- Синхронізація: ~15 сек через Google Drive

**q_agent media processing:**
- Декодує base64 → зберігає в `/Volumes/BADEMO-512G/Personal/AI/Q_SME/dreams/media/`
- Q бачить зображення через fs_read Image mode

### Технічні деталі

**Google Drive OAuth:**
- Client ID: 737474602371-6ip4hr60vrv69e84h05b386bde7ag5va.apps.googleusercontent.com
- Scope: `https://www.googleapis.com/auth/drive`
- Mailbox: `mailbox/q_vadym_chat/messages.txt`

**Компактний layout для мобільних:**
- Кнопки: 120px, 75px
- Превʼю: 160px
- Zoom slider на всю ширину

**Коміти:**
- `e232a91` - Show clipboard types in UI for debugging
- `90c3784` - Fix clipboard: auto-update on load and window focus
- `2aec7a6` - Make camera controls compact for mobile
- `9df45c2` - Add zoom slider for camera
- `b28ec28` - Add Switch Camera button
