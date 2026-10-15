# Melody-Mapper-Chromebook

<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Song Structure & Section Marker - Chromebook Edition</title>

  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#f0f9ff',
              100: '#e0f2fe',
              500: '#0284c7',
              600: '#0284c7',
              700: '#0369a1',
            }
          }
        }
      }
    };
  </script>

  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

  <!-- Google Fonts: Inter & Outfit -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Outfit:wght@500;600;700;800&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Inter', sans-serif;
      user-select: none;
    }
    h1, h2, h3, h4, .font-heading {
      font-family: 'Outfit', sans-serif;
    }
    
    /* Custom Scrollbar */
    ::-webkit-scrollbar {
      width: 8px;
      height: 8px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.6);
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(100, 116, 139, 0.5);
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(148, 163, 184, 0.8);
    }

    /* Print Styles for Printable Summary Report */
    @media print {
      body * {
        visibility: hidden;
      }
      #printableReportModal, #printableReportModal * {
        visibility: visible;
      }
      #printableReportModal {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        background: white !important;
        color: black !important;
      }
      .no-print {
        display: none !important;
      }
    }

    .playhead-glow {
      box-shadow: 0 0 12px #38bdf8;
    }

    /* Key Badge Styling */
    .kbd-badge {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      padding: 0.15rem 0.4rem;
      font-family: monospace;
      font-size: 0.7rem;
      font-weight: 700;
      line-height: 1;
      color: #cbd5e1;
      background-color: #1e293b;
      border: 1px solid #475569;
      border-radius: 0.375rem;
      box-shadow: 0 1px 2px rgba(0,0,0,0.4);
    }

    /* Drag over drop zone styling */
    .drag-active {
      border-color: #38bdf8 !important;
      background-color: rgba(14, 165, 233, 0.15) !important;
    }
  </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col transition-colors duration-200">

  <!-- Top Navigation Header -->
  <header class="bg-slate-800/90 backdrop-blur border-b border-slate-700/60 sticky top-0 z-30 px-3 lg:px-6 py-2.5">
    <div class="max-w-7xl mx-auto flex flex-wrap items-center justify-between gap-3">
      
      <!-- Brand Logo & Chromebook Badge -->
      <div class="flex items-center gap-2.5">
        <div class="w-9 h-9 rounded-xl bg-gradient-to-tr from-cyan-500 via-indigo-500 to-fuchsia-500 flex items-center justify-center text-white shadow-lg shadow-indigo-500/20">
          <i class="fa-solid fa-compact-disc text-lg"></i>
        </div>
        <div>
          <div class="flex items-center gap-2">
            <h1 class="text-lg font-bold bg-gradient-to-r from-cyan-400 via-indigo-300 to-fuchsia-400 bg-clip-text text-transparent leading-tight">
              MelodyMapper
            </h1>
            <span class="px-2 py-0.5 rounded-full bg-cyan-500/10 border border-cyan-500/30 text-cyan-400 font-bold text-[10px] tracking-wide flex items-center gap-1">
              <i class="fa-solid fa-laptop text-[9px]"></i> Chromebook Edition
            </span>
          </div>
          <p class="text-[11px] text-slate-400 font-medium">Song Structure & Section Marker for Music Class</p>
        </div>
      </div>

      <!-- Student & Project Meta Inputs -->
      <div class="flex flex-wrap items-center gap-2.5 bg-slate-900/80 border border-slate-700/80 rounded-xl px-3 py-1.5 text-xs">
        <div class="flex items-center gap-1.5 border-r border-slate-700 pr-2.5">
          <i class="fa-solid fa-user-graduate text-cyan-400 text-xs"></i>
          <input type="text" id="studentNameInput" placeholder="Student Name" value="Alex Rivera" oninput="autoSaveToLocalStorage()" class="bg-transparent text-slate-200 placeholder-slate-500 focus:outline-none w-24 sm:w-32 text-xs font-semibold" />
        </div>
        <div class="flex items-center gap-1.5">
          <i class="fa-solid fa-music text-fuchsia-400 text-xs"></i>
          <input type="text" id="songTitleInput" placeholder="Song Title" value="Neon Pop Adventure" oninput="autoSaveToLocalStorage()" class="bg-transparent text-slate-200 placeholder-slate-500 focus:outline-none w-28 sm:w-40 text-xs font-semibold" />
        </div>
        <span id="autosaveBadge" class="text-[10px] text-emerald-400 font-bold bg-emerald-500/10 px-2 py-0.5 rounded border border-emerald-500/30 opacity-60 transition">Auto-Saved</span>
      </div>

      <!-- Top Action Toolbar -->
      <div class="flex items-center gap-2 flex-wrap">
        
        <!-- Keyboard Shortcuts Trigger -->
        <button onclick="openShortcutsModal()" title="Chromebook Keyboard Shortcuts (?)" class="px-2.5 py-1.5 rounded-lg bg-slate-700/80 hover:bg-slate-700 text-slate-200 text-xs font-medium transition flex items-center gap-1.5 border border-slate-600/60">
          <i class="fa-solid fa-keyboard text-cyan-400"></i>
          <span class="hidden sm:inline">Shortcuts</span>
          <span class="kbd-badge">?</span>
        </button>

        <!-- Glossary Button -->
        <button onclick="openGlossaryModal()" class="px-2.5 py-1.5 rounded-lg bg-slate-700/80 hover:bg-slate-700 text-slate-200 text-xs font-medium transition flex items-center gap-1.5 border border-slate-600/60">
          <i class="fa-solid fa-book-open text-amber-400"></i>
          <span class="hidden sm:inline">Form Guide</span>
        </button>

        <!-- Google Docs / Classroom Copy Button -->
        <button onclick="copyFormattedSummaryForClassroom()" title="Copy clean formatted summary to paste into Google Docs or Classroom" class="px-3 py-1.5 rounded-lg bg-emerald-600/90 hover:bg-emerald-600 text-white text-xs font-semibold transition flex items-center gap-1.5 shadow-md shadow-emerald-600/20">
          <i class="fa-solid fa-copy"></i>
          <span>Copy for Google Docs</span>
        </button>

        <!-- Print Summary Report -->
        <button onclick="openPrintReportModal()" class="px-2.5 py-1.5 rounded-lg bg-indigo-600/80 hover:bg-indigo-600 text-white text-xs font-medium transition flex items-center gap-1.5 shadow-md shadow-indigo-600/20">
          <i class="fa-solid fa-print"></i>
          <span class="hidden sm:inline">Print / PDF</span>
        </button>

        <!-- Export Dropdown -->
        <div class="relative inline-block text-left">
          <button onclick="toggleExportMenu()" class="px-2.5 py-1.5 rounded-lg bg-slate-700/80 hover:bg-slate-700 text-slate-200 text-xs font-medium transition flex items-center gap-1.5 border border-slate-600/60">
            <i class="fa-solid fa-floppy-disk text-cyan-400"></i>
            <span class="hidden sm:inline">File</span>
            <i class="fa-solid fa-chevron-down text-[10px]"></i>
          </button>
          <div id="exportDropdown" class="hidden absolute right-0 mt-2 w-48 bg-slate-800 border border-slate-700 rounded-xl shadow-xl z-50 py-1 text-xs">
            <button onclick="exportProjectJSON()" class="w-full text-left px-4 py-2 hover:bg-slate-700 flex items-center gap-2 text-slate-200">
              <i class="fa-solid fa-download text-cyan-400"></i> Save Project JSON
            </button>
            <label class="w-full text-left px-4 py-2 hover:bg-slate-700 flex items-center gap-2 text-slate-200 cursor-pointer">
              <i class="fa-solid fa-upload text-emerald-400"></i> Open Project JSON
              <input type="file" id="importJsonInput" accept=".json" class="hidden" onchange="importProjectJSON(event)" />
            </label>
            <hr class="border-slate-700 my-1" />
            <button onclick="loadDemoAnalysis()" class="w-full text-left px-4 py-2 hover:bg-slate-700 flex items-center gap-2 text-amber-300">
              <i class="fa-solid fa-wand-magic-sparkles"></i> Load Demo Markings
            </button>
            <button onclick="openConfirmClearModal()" class="w-full text-left px-4 py-2 hover:bg-slate-700 flex items-center gap-2 text-rose-400">
              <i class="fa-solid fa-trash-can"></i> Reset All Work
            </button>
          </div>
        </div>

        <!-- Theme Toggle -->
        <button onclick="toggleTheme()" title="Toggle Light/Dark Theme" class="p-2 rounded-lg bg-slate-700/80 hover:bg-slate-700 text-slate-200 text-xs transition border border-slate-600/60">
          <i class="fa-solid fa-sun text-amber-400" id="themeIcon"></i>
        </button>
      </div>

    </div>
  </header>

  <!-- Main Container -->
  <main class="max-w-7xl mx-auto w-full px-3 lg:px-6 py-4 flex-1 space-y-5">

    <!-- AUDIO SOURCE & CHROMBOOK RECORDER DECK -->
    <section class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-4 sm:p-5 shadow-xl backdrop-blur relative overflow-hidden" id="dropZoneContainer">
      
      <!-- Top Audio Source Controls Bar -->
      <div class="flex flex-col md:flex-row items-start md:items-center justify-between gap-3 mb-4 pb-3 border-b border-slate-700/70">
        
        <!-- Track Selection Dropdown & Input Options -->
        <div class="flex flex-wrap items-center gap-2.5 w-full md:w-auto">
          <div class="flex items-center gap-2">
            <span class="text-xs font-semibold uppercase tracking-wider text-slate-400">Audio Source:</span>
            <select id="sampleTrackSelect" onchange="handleTrackSelectChange(this.value)" class="bg-slate-900 border border-slate-700 text-cyan-300 text-xs font-semibold rounded-xl px-3 py-1.5 focus:outline-none focus:border-cyan-500 transition">
              <option value="pop">🎵 Demo 1: Neon Pop Adventure (Synth Pop - 60s)</option>
              <option value="lofi">🎸 Demo 2: Sunset Lo-Fi Beat (Acoustic Keys - 50s)</option>
              <option value="classical">🎻 Demo 3: Classical Minuet Form (Binary A-B-A - 55s)</option>
              <option value="funk">🎷 Demo 4: Funk & Groove Jam (Upbeat Bass - 55s)</option>
              <option value="custom">📁 Custom Audio File (MP3 / WAV / WebM)</option>
              <option value="mic">🎙️ Chromebook Microphone Live Recorder</option>
            </select>
          </div>

          <!-- Hidden File Upload Button -->
          <div id="customFileContainer" class="hidden">
            <label class="px-3 py-1.5 bg-cyan-600 hover:bg-cyan-500 text-white text-xs font-semibold rounded-xl cursor-pointer transition flex items-center gap-1.5 shadow">
              <i class="fa-solid fa-folder-open"></i> Upload File
              <input type="file" id="audioFileInput" accept="audio/*" class="hidden" onchange="handleCustomAudioUpload(event)" />
            </label>
          </div>

          <!-- Chromebook Microphone Record Trigger Button -->
          <button id="micRecordBtn" onclick="toggleMicrophoneRecorder()" class="px-3 py-1.5 rounded-xl bg-rose-600 hover:bg-rose-500 text-white text-xs font-semibold transition flex items-center gap-1.5 shadow-md shadow-rose-600/20">
            <i class="fa-solid fa-microphone" id="micBtnIcon"></i>
            <span id="micBtnText">Record Mic</span>
            <span id="micRecordTimer" class="hidden font-mono text-[10px] bg-rose-950 px-1.5 py-0.5 rounded">00:00</span>
          </button>
        </div>

        <!-- Audio Active Status Badge & Duration Callout -->
        <div class="flex items-center gap-3">
          <!-- Looping Badge -->
          <div id="loopStatusBadge" class="hidden px-3 py-1 bg-amber-500/20 border border-amber-500/50 rounded-full text-amber-300 text-xs font-medium items-center gap-2 animate-pulse">
            <i class="fa-solid fa-rotate-right"></i>
            <span>Looping: <strong id="loopingSectionName">Section</strong></span>
            <button onclick="stopLoopingSection()" class="hover:text-white ml-1"><i class="fa-solid fa-xmark"></i></button>
          </div>

          <div class="text-right">
            <span id="currentTimeDisplay" class="font-mono text-2xl font-bold text-cyan-400">00:00.0</span>
            <span class="text-slate-500 font-mono text-xs">/</span>
            <span id="totalDurationDisplay" class="font-mono text-xs font-semibold text-slate-400">00:00.0</span>
          </div>
        </div>

      </div>

      <!-- WAVEFORM CANVAS & TIMELINE (TOUCH & ZOOM ENABLED) -->
      <div class="relative mb-3 group">
        
        <!-- Zoom & Viewport Controls Overlay -->
        <div class="flex items-center justify-between text-xs mb-1.5 px-1">
          <div class="flex items-center gap-1.5">
            <span class="text-[10px] font-bold uppercase tracking-wider text-slate-400">Timeline Zoom:</span>
            <button onclick="changeZoom(0.5)" title="Zoom In (Z)" class="px-2 py-0.5 rounded bg-slate-700 hover:bg-slate-600 text-slate-200 text-xs font-mono font-bold">
              + <span class="kbd-badge ml-0.5">Z</span>
            </button>
            <button onclick="changeZoom(-0.5)" title="Zoom Out (X)" class="px-2 py-0.5 rounded bg-slate-700 hover:bg-slate-600 text-slate-200 text-xs font-mono font-bold">
              - <span class="kbd-badge ml-0.5">X</span>
            </button>
            <button onclick="resetZoom()" title="Reset Zoom (R)" class="px-2 py-0.5 rounded bg-slate-700 hover:bg-slate-600 text-slate-300 text-[10px] font-semibold">
              Fit Screen <span class="kbd-badge ml-0.5">R</span>
            </button>
            <span id="zoomLevelDisplay" class="text-[10px] text-cyan-400 font-mono font-bold ml-1">1.0x</span>
          </div>

          <span class="text-[10px] text-slate-400 italic">
            <i class="fa-solid fa-hand-pointer mr-1"></i> Drag / Touch pan supported
          </span>
        </div>

        <!-- Waveform & Canvas Layer -->
        <div class="relative w-full h-28 bg-slate-950 rounded-xl overflow-hidden border border-slate-700/80 cursor-pointer shadow-inner" id="timelineContainer">
          
          <!-- Rendered Waveform Canvas -->
          <canvas id="waveformCanvas" class="w-full h-full block opacity-70"></canvas>

          <!-- Overlay Layer for Marked Colored Sections -->
          <div id="timelineSectionsOverlay" class="absolute inset-0 pointer-events-none"></div>

          <!-- Interactive Playhead Line -->
          <div id="playheadLine" class="absolute top-0 bottom-0 w-1 bg-cyan-400 playhead-glow z-20 pointer-events-none transition-all duration-75" style="left: 0%;">
            <div class="w-3.5 h-3.5 bg-cyan-300 rounded-full -translate-x-[5px] -translate-y-1 shadow"></div>
          </div>

          <!-- Live Section Mark Draft Bounds Overlay -->
          <div id="draftSectionMarker" class="absolute top-0 bottom-0 bg-cyan-500/30 border-x-2 border-cyan-400 hidden z-10 pointer-events-none"></div>

          <!-- Drag and Drop Hint Overlay -->
          <div id="dragDropOverlay" class="hidden absolute inset-0 bg-cyan-900/80 border-2 border-dashed border-cyan-400 rounded-xl flex items-center justify-center text-cyan-200 font-bold text-sm pointer-events-none z-30">
            <i class="fa-solid fa-cloud-arrow-up text-2xl mr-2"></i> Drop Audio File Here (from Chromebook Drive or Files)
          </div>

          <!-- Hover Time Indicator Tooltip -->
          <div id="timelineHoverTooltip" class="hidden absolute top-2 bg-slate-900 border border-cyan-500/50 text-cyan-300 text-[10px] font-mono px-2 py-0.5 rounded shadow z-30 pointer-events-none -translate-x-1/2">
            00:00.0
          </div>
        </div>

      </div>

      <!-- VISUAL SECTION COLOR RIBBON STRIP (0% to 100%) -->
      <div class="space-y-1 mb-4">
        <div class="flex items-center justify-between text-[10px] text-slate-400 font-semibold px-1">
          <span>00:00</span>
          <span class="text-cyan-400 font-bold uppercase tracking-wider text-[10px]">Song Structure Color Ribbon</span>
          <span id="ribbonDurationEnd">01:00</span>
        </div>
        <div id="sectionRibbonStrip" class="w-full h-4 bg-slate-950 border border-slate-700 rounded-lg flex overflow-hidden p-0.5 gap-0.5 shadow-inner">
          <div class="w-full h-full bg-slate-800/80 rounded text-[9px] text-slate-500 flex items-center justify-center font-medium">No sections marked yet</div>
        </div>
      </div>

      <!-- PLAYBACK CONTROLS TOOLBAR WITH CHROMEBOOK SHORTCUT BADGES -->
      <div class="flex flex-wrap items-center justify-between gap-3 bg-slate-900/90 border border-slate-700/60 rounded-xl p-2.5">
        
        <!-- Main Play/Pause & Skip Buttons -->
        <div class="flex items-center gap-2">
          <button onclick="skipTime(-2)" title="Rewind 2s (Left Arrow)" class="p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 transition border border-slate-700 flex items-center gap-1">
            <i class="fa-solid fa-rotate-left text-xs"></i>
            <span class="kbd-badge text-[9px]">←</span>
          </button>

          <button id="playPauseBtn" onclick="togglePlayPause()" title="Play / Pause (Spacebar)" class="w-11 h-11 rounded-xl bg-gradient-to-r from-cyan-500 to-indigo-600 hover:from-cyan-400 hover:to-indigo-500 text-white flex items-center justify-center text-base shadow-lg shadow-cyan-500/25 transition active:scale-95">
            <i class="fa-solid fa-play ml-0.5" id="playIcon"></i>
          </button>

          <button onclick="skipTime(2)" title="Forward 2s (Right Arrow)" class="p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 transition border border-slate-700 flex items-center gap-1">
            <span class="kbd-badge text-[9px]">→</span>
            <i class="fa-solid fa-rotate-right text-xs"></i>
          </button>
        </div>

        <!-- Speed Controls -->
        <div class="flex items-center gap-1 bg-slate-800/80 border border-slate-700 p-1 rounded-xl">
          <span class="text-[10px] font-bold uppercase tracking-wider text-slate-400 px-1.5">Speed:</span>
          <button onclick="setPlaybackSpeed(0.5)" class="speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition" data-speed="0.5">0.5x</button>
          <button onclick="setPlaybackSpeed(0.75)" class="speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition" data-speed="0.75">0.75x</button>
          <button onclick="setPlaybackSpeed(1.0)" class="speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg bg-cyan-500 text-white transition" data-speed="1">1.0x</button>
          <button onclick="setPlaybackSpeed(1.25)" class="speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition" data-speed="1.25">1.25x</button>
          <button onclick="setPlaybackSpeed(1.5)" class="speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition" data-speed="1.5">1.5x</button>
        </div>

        <!-- Volume Slider & Mute Toggle -->
        <div class="flex items-center gap-2">
          <button onclick="toggleMute()" id="muteBtn" class="text-slate-400 hover:text-white text-xs p-1">
            <i class="fa-solid fa-volume-high" id="volumeIcon"></i>
          </button>
          <input type="range" id="volumeSlider" min="0" max="1" step="0.05" value="0.8" oninput="changeVolume(this.value)" class="w-20 sm:w-24 accent-cyan-400 cursor-pointer" />
        </div>

      </div>

    </section>

    <!-- LIVE SECTION MARKER CONTROLS & PALETTE PANEL -->
    <section class="grid grid-cols-1 lg:grid-cols-3 gap-5">
      
      <!-- Live Marking Interactive Box -->
      <div class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-4 sm:p-5 shadow-xl backdrop-blur flex flex-col justify-between">
        <div>
          <div class="flex items-center justify-between mb-2">
            <h2 class="text-base font-bold text-white flex items-center gap-2">
              <i class="fa-solid fa-stopwatch text-cyan-400"></i>
              <span>Live Section Marker</span>
            </h2>
            <span class="text-[10px] px-2 py-0.5 rounded-full bg-cyan-500/10 text-cyan-400 font-bold border border-cyan-500/30">Step 1</span>
          </div>
          <p class="text-xs text-slate-400 mb-3 leading-relaxed">
            Press <span class="kbd-badge">I</span> for <strong>IN</strong> point, then <span class="kbd-badge">O</span> for <strong>OUT</strong> point as you listen.
          </p>

          <!-- Active Time Display Callout -->
          <div class="bg-slate-900 border border-slate-700/80 rounded-xl p-2.5 text-center mb-3">
            <span class="text-[10px] text-slate-400 uppercase tracking-wider block font-semibold">Active Audio Position</span>
            <span id="liveTimeTracker" class="font-mono text-2xl font-extrabold text-white">00:00.0</span>
          </div>

          <!-- Marking Status Draft Box -->
          <div id="liveMarkStatus" class="bg-slate-900/60 border border-dashed border-slate-700 rounded-xl p-2.5 text-center text-xs text-slate-400 mb-3 min-h-[44px] flex items-center justify-center">
            Click <span class="text-emerald-400 font-semibold mx-1">"Start IN"</span> or press <span class="kbd-badge mx-1">I</span>
          </div>
        </div>

        <!-- Live Capture Action Buttons -->
        <div class="grid grid-cols-2 gap-2.5">
          <button id="startMarkBtn" onclick="markStartHere()" title="Set Section Start (Key: I)" class="py-2.5 px-3 rounded-xl bg-emerald-600 hover:bg-emerald-500 text-white font-semibold text-xs transition shadow-lg shadow-emerald-600/20 flex items-center justify-center gap-1.5 active:scale-95">
            <i class="fa-solid fa-flag"></i>
            <span>Start IN</span>
            <span class="kbd-badge bg-emerald-800 text-emerald-200 border-emerald-600">I</span>
          </button>

          <button id="endMarkBtn" onclick="markEndHere()" disabled title="Set Section End (Key: O)" class="py-2.5 px-3 rounded-xl bg-slate-700 text-slate-500 font-semibold text-xs transition flex items-center justify-center gap-1.5 cursor-not-allowed">
            <i class="fa-solid fa-flag-checkered"></i>
            <span>End OUT</span>
            <span class="kbd-badge">O</span>
          </button>
        </div>

        <!-- Manual Add Trigger -->
        <button onclick="openManualSectionModal()" class="w-full mt-2.5 py-1.5 bg-slate-900/80 hover:bg-slate-900 border border-slate-700 rounded-xl text-slate-300 text-xs font-medium transition flex items-center justify-center gap-1.5">
          <i class="fa-solid fa-pen-to-square text-cyan-400"></i>
          <span>Set Exact Times Manually</span>
        </button>
      </div>

      <!-- Quick Section Tag & Color Creator -->
      <div class="lg:col-span-2 bg-slate-800/80 border border-slate-700/80 rounded-2xl p-4 sm:p-5 shadow-xl backdrop-blur flex flex-col justify-between">
        <div>
          <div class="flex items-center justify-between mb-2">
            <h2 class="text-base font-bold text-white flex items-center gap-2">
              <i class="fa-solid fa-palette text-fuchsia-400"></i>
              <span>Section Identity & Preset Palette</span>
            </h2>
            <span class="text-[10px] px-2 py-0.5 rounded-full bg-fuchsia-500/10 text-fuchsia-400 font-bold border border-fuchsia-500/30">Step 2</span>
          </div>

          <!-- Form Tag Selection Grid -->
          <div class="mb-3">
            <label class="block text-xs font-semibold text-slate-300 mb-1.5">Select Section Tag / Form Label:</label>
            <div class="flex flex-wrap gap-1.5" id="presetTagChips">
              <!-- Dynamically rendered chips -->
            </div>
          </div>

          <!-- Color Palette Picker -->
          <div class="mb-3">
            <div class="flex items-center justify-between mb-1.5">
              <label class="text-xs font-semibold text-slate-300">Choose Vivid Color:</label>
              <div class="flex items-center gap-2">
                <span class="text-[10px] text-slate-400">Custom:</span>
                <input type="color" id="customColorPicker" value="#8b5cf6" onchange="selectCustomColor(this.value)" class="w-6 h-6 rounded bg-transparent cursor-pointer border border-slate-600" />
              </div>
            </div>
            
            <div class="flex flex-wrap gap-2" id="colorPaletteChips">
              <!-- Color swatches generated via JavaScript -->
            </div>
          </div>
        </div>

        <!-- Active Selected Tag Preview Strip -->
        <div class="bg-slate-900/90 border border-slate-700 rounded-xl p-2.5 flex flex-wrap items-center justify-between gap-2">
          <div class="flex items-center gap-2.5">
            <div id="activeColorBadge" class="w-5 h-5 rounded-lg shadow bg-purple-500"></div>
            <div>
              <span class="text-[9px] uppercase font-bold text-slate-400 block">Selected Tag:</span>
              <span id="activeTagName" class="text-xs font-extrabold text-white">Verse 1</span>
            </div>
          </div>

          <div class="text-xs text-slate-400">
            Timing: <span id="activeTimingPreview" class="font-mono text-cyan-300 font-semibold">00:00.0 - 00:00.0</span>
          </div>

          <button id="confirmAddSectionBtn" onclick="saveCapturedSection()" disabled title="Save Section (Key: M or Enter)" class="px-4 py-2 rounded-xl bg-gradient-to-r from-cyan-500 to-indigo-600 hover:from-cyan-400 hover:to-indigo-500 text-white font-bold text-xs transition shadow-lg shadow-cyan-500/20 disabled:opacity-50 disabled:cursor-not-allowed flex items-center gap-1.5">
            <i class="fa-solid fa-plus-circle"></i>
            <span>Save Marker</span>
            <span class="kbd-badge bg-cyan-900 text-cyan-200 border-cyan-600 ml-0.5">M</span>
          </button>
        </div>

      </div>

    </section>

    <!-- SONG FORM MAP BANNER -->
    <section class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-4 shadow-xl backdrop-blur">
      <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-2 mb-2">
        <div>
          <h2 class="text-xs font-bold text-white flex items-center gap-2">
            <i class="fa-solid fa-diagram-project text-amber-400"></i>
            <span>Song Form Flow Map</span>
          </h2>
          <p class="text-[11px] text-slate-400">Chronological arrangement of marked song sections.</p>
        </div>
        
        <div class="flex items-center gap-2 text-xs">
          <span class="text-slate-400">Coverage:</span>
          <span id="coveragePercentage" class="font-mono font-bold text-emerald-400 bg-emerald-500/10 px-2 py-0.5 rounded-full border border-emerald-500/30 text-[11px]">0%</span>
        </div>
      </div>

      <!-- Form Flow Sequence Badges -->
      <div id="songFormChain" class="flex items-center gap-2 overflow-x-auto py-2 px-1 min-h-[44px] scrollbar-thin">
        <div class="text-xs text-slate-500 italic">No sections marked yet. Mark live or add sections above to build the song form!</div>
      </div>
    </section>

    <!-- STUDENT ANALYSIS & WORKSHEET CARDS WORKSPACE -->
    <section class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-4 sm:p-5 shadow-xl backdrop-blur">
      
      <!-- Section Header Controls -->
      <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3 mb-5 pb-3 border-b border-slate-700/70">
        <div>
          <h2 class="text-base font-bold text-white flex items-center gap-2">
            <i class="fa-solid fa-list-check text-cyan-400"></i>
            <span>Student Music Analysis Worksheet</span>
          </h2>
          <p class="text-xs text-slate-400">Analyze instruments, dynamics, texture, and notes for each marked section.</p>
        </div>

        <div class="flex items-center gap-2">
          <span class="text-xs font-semibold text-slate-400">Sort By:</span>
          <select id="sectionSortSelect" onchange="renderSectionsList()" class="bg-slate-900 border border-slate-700 text-slate-200 text-xs rounded-xl px-3 py-1 focus:outline-none">
            <option value="time">Start Time (Chronological)</option>
            <option value="name">Section Name</option>
            <option value="duration">Duration (Longest First)</option>
          </select>
        </div>
      </div>

      <!-- Cards Container -->
      <div id="sectionsListContainer" class="space-y-4">
        <!-- Rendered Section Cards -->
      </div>

    </section>

  </main>

  <!-- Footer -->
  <footer class="bg-slate-950 border-t border-slate-800 text-slate-500 py-4 text-center text-xs px-4 mt-6">
    <div class="max-w-7xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-2">
      <p>© 2026 MelodyMapper — Chromebook Music Education & Song Analysis Tool</p>
      <p class="flex items-center gap-2">
        <span class="text-cyan-400"><i class="fa-solid fa-graduation-cap"></i> Designed for Middle & High School Music Classes</span>
      </p>
    </div>
  </footer>


  <!-- MODALS SECTION -->

  <!-- KEYBOARD SHORTCUTS GUIDE MODAL FOR CHROMEBOOKS -->
  <div id="shortcutsModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-slate-800 border border-slate-700 w-full max-w-lg rounded-2xl shadow-2xl p-5">
      <div class="flex items-center justify-between mb-3 border-b border-slate-700 pb-2">
        <h3 class="text-base font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-keyboard text-cyan-400"></i>
          <span>Chromebook Keyboard Shortcuts</span>
        </h3>
        <button onclick="closeShortcutsModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 gap-2 text-xs text-slate-300 my-3">
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Play / Pause</span>
          <span class="kbd-badge">Spacebar</span>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Seek -2s / +2s</span>
          <span class="kbd-badge">←</span> <span class="kbd-badge">→</span>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Set Start (IN)</span>
          <span class="kbd-badge">I</span>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Set End (OUT)</span>
          <span class="kbd-badge">O</span>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Save Marker</span>
          <div><span class="kbd-badge">M</span> or <span class="kbd-badge">Enter</span></div>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Toggle Loop</span>
          <span class="kbd-badge">L</span>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Zoom In / Out</span>
          <div><span class="kbd-badge">Z</span> / <span class="kbd-badge">X</span></div>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700 flex items-center justify-between">
          <span>Fit Screen</span>
          <span class="kbd-badge">R</span>
        </div>
      </div>

      <p class="text-[11px] text-slate-400 italic text-center mt-2">
        Tip: Touchscreen Chromebooks can also tap and swipe directly on the waveform timeline!
      </p>

      <div class="mt-4 text-right">
        <button onclick="closeShortcutsModal()" class="px-4 py-2 bg-cyan-600 hover:bg-cyan-500 text-white font-bold rounded-xl text-xs">Got it!</button>
      </div>
    </div>
  </div>


  <!-- PRINTABLE STUDENT SUMMARY REPORT MODAL -->
  <div id="printReportModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4 overflow-y-auto">
    <div class="bg-slate-800 border border-slate-700 w-full max-w-4xl rounded-2xl shadow-2xl overflow-hidden my-6 max-h-[90vh] flex flex-col">
      
      <!-- Navigation Bar (No-Print) -->
      <div class="no-print bg-slate-900 px-5 py-3 border-b border-slate-700 flex items-center justify-between">
        <h3 class="text-sm font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-file-invoice text-indigo-400"></i>
          <span>Student Song Structure Analysis Report</span>
        </h3>
        <div class="flex items-center gap-2">
          <button onclick="triggerWindowPrint()" class="px-3.5 py-1.5 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-xs rounded-xl transition flex items-center gap-1.5 shadow">
            <i class="fa-solid fa-print"></i> Print / Save PDF
          </button>
          <button onclick="closePrintReportModal()" class="p-1.5 text-slate-400 hover:text-white transition">
            <i class="fa-solid fa-xmark text-lg"></i>
          </button>
        </div>
      </div>

      <!-- Printable Body -->
      <div id="printableReportModal" class="p-6 bg-slate-900 text-slate-100 overflow-y-auto space-y-5 flex-1 text-xs">
        
        <!-- Header Info Card -->
        <div class="border-b-2 border-indigo-500/50 pb-4 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
          <div>
            <span class="text-[10px] uppercase font-bold text-indigo-400 tracking-widest block">Music Class Analysis Worksheet</span>
            <h2 id="reportSongTitle" class="text-xl font-extrabold text-white mt-0.5">Neon Pop Adventure</h2>
            <p id="reportStudentName" class="text-xs font-semibold text-slate-300 mt-0.5">Student: Alex Rivera</p>
          </div>
          <div class="text-right text-[11px] text-slate-400 space-y-0.5">
            <p><strong>Date:</strong> <span id="reportDate">Oct 2026</span></p>
            <p><strong>Total Sections:</strong> <span id="reportSectionCount" class="text-cyan-400 font-bold">0</span></p>
            <p><strong>Track Duration:</strong> <span id="reportTotalDuration" class="font-mono">01:00.0</span></p>
          </div>
        </div>

        <!-- Song Structure Chain Display -->
        <div>
          <h4 class="text-[10px] font-bold uppercase tracking-wider text-slate-400 mb-1.5">Song Form Map Sequence</h4>
          <div id="reportFormChain" class="flex flex-wrap items-center gap-2 p-2.5 bg-slate-950 border border-slate-800 rounded-xl">
            <!-- Rendered badges -->
          </div>
        </div>

        <!-- Color Ribbon Timeline Visual -->
        <div>
          <h4 class="text-[10px] font-bold uppercase tracking-wider text-slate-400 mb-1.5">Visual Section Color Ribbon</h4>
          <div id="reportRibbonStrip" class="w-full h-5 bg-slate-950 border border-slate-800 rounded-lg flex overflow-hidden p-0.5 gap-0.5">
            <!-- Rendered ribbon strip -->
          </div>
        </div>

        <!-- Complete Section Breakdown Table -->
        <div>
          <h4 class="text-[10px] font-bold uppercase tracking-wider text-slate-400 mb-2">Detailed Musical Section Breakdown</h4>
          <div class="overflow-x-auto border border-slate-800 rounded-xl">
            <table class="w-full text-left border-collapse">
              <thead class="bg-slate-950 text-slate-300 font-semibold border-b border-slate-800 text-[11px]">
                <tr>
                  <th class="p-2.5">Section</th>
                  <th class="p-2.5">Time Range</th>
                  <th class="p-2.5">Duration</th>
                  <th class="p-2.5">Form</th>
                  <th class="p-2.5">Instruments</th>
                  <th class="p-2.5">Dynamics & Texture</th>
                  <th class="p-2.5">Observations / Notes</th>
                </tr>
              </thead>
              <tbody id="reportTableBody" class="divide-y divide-slate-800 text-slate-300 text-[11px]">
                <!-- Table rows rendered dynamically -->
              </tbody>
            </table>
          </div>
        </div>

        <!-- Teacher Notes Signoff Box -->
        <div class="border-t border-slate-800 pt-4 mt-6 flex flex-col sm:flex-row justify-between items-end gap-3 text-[10px]">
          <div class="text-slate-500">
            Generated with MelodyMapper Interactive Analysis (Chromebook Edition)
          </div>
          <div class="w-44 text-center border-t border-slate-700 pt-1 text-slate-400">
            Teacher Grade / Feedback
          </div>
        </div>

      </div>

    </div>
  </div>


  <!-- MUSICAL FORM GUIDE & GLOSSARY MODAL -->
  <div id="glossaryModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-slate-800 border border-slate-700 w-full max-w-2xl rounded-2xl shadow-2xl overflow-hidden max-h-[85vh] flex flex-col">
      
      <div class="bg-slate-900 px-5 py-3 border-b border-slate-700 flex items-center justify-between">
        <h3 class="text-sm font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-book-open text-amber-400"></i>
          <span>Music Theory Form Guide & Glossary</span>
        </h3>
        <button onclick="closeGlossaryModal()" class="p-1.5 text-slate-400 hover:text-white transition">
          <i class="fa-solid fa-xmark text-lg"></i>
        </button>
      </div>

      <div class="p-5 overflow-y-auto space-y-5 text-xs leading-relaxed text-slate-300">
        
        <div>
          <h4 class="text-xs font-bold text-cyan-400 mb-2 border-b border-slate-700 pb-1 uppercase tracking-wider">Common Song Forms</h4>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-2.5">
            <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700">
              <strong class="text-white block mb-0.5">Verse-Chorus (ABABCB)</strong>
              Alternates Verses (telling the story) and Choruses (main repeating hook), usually with a Bridge after the second chorus.
            </div>
            <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700">
              <strong class="text-white block mb-0.5">AABA Form</strong>
              32-bar form popular in Jazz and classic Pop. A = main melody, B = contrasting section, A = return to main melody.
            </div>
            <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700">
              <strong class="text-white block mb-0.5">Strophic Form (AAA)</strong>
              Repeats the exact same melody for all sections while lyrics change (common in Folk and Ballads).
            </div>
            <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-700">
              <strong class="text-white block mb-0.5">Binary (AB) & Ternary (ABA)</strong>
              Binary consists of two contrasting sections (A-B). Ternary returns to the main section (A-B-A).
            </div>
          </div>
        </div>

        <div>
          <h4 class="text-xs font-bold text-fuchsia-400 mb-2 border-b border-slate-700 pb-1 uppercase tracking-wider">Musical Elements Definitions</h4>
          <div class="space-y-1.5">
            <p><strong class="text-slate-100">Dynamics:</strong> The loudness or softness of the music (e.g., <em>pianissimo</em> [pp], <em>forte</em> [f], <em>crescendo</em> [getting louder]).</p>
            <p><strong class="text-slate-100">Texture:</strong> Layering of sounds (<em>Monophonic</em> = single line; <em>Homophonic</em> = melody + chords; <em>Polyphonic</em> = multi-melodies).</p>
            <p><strong class="text-slate-100">Bridge:</strong> A contrasting section that creates a musical break before the final chorus.</p>
          </div>
        </div>

      </div>

    </div>
  </div>


  <!-- MANUAL EDIT SECTION MODAL -->
  <div id="manualSectionModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-slate-800 border border-slate-700 w-full max-w-sm rounded-2xl shadow-2xl p-5">
      
      <div class="flex items-center justify-between mb-3">
        <h3 id="modalSectionTitle" class="text-sm font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-pen-to-square text-cyan-400"></i>
          <span>Add / Edit Section</span>
        </h3>
        <button onclick="closeManualSectionModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
      </div>

      <div class="space-y-3 text-xs">
        <div>
          <label class="block font-semibold text-slate-300 mb-1">Section Tag / Name:</label>
          <input type="text" id="manualNameInput" placeholder="e.g. Verse 1" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-slate-100 focus:outline-none focus:border-cyan-500" />
        </div>

        <div class="grid grid-cols-2 gap-2.5">
          <div>
            <label class="block font-semibold text-slate-300 mb-1">Start Time (sec):</label>
            <input type="number" id="manualStartInput" step="0.1" min="0" placeholder="0.0" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-slate-100 focus:outline-none focus:border-cyan-500 font-mono" />
          </div>

          <div>
            <label class="block font-semibold text-slate-300 mb-1">End Time (sec):</label>
            <input type="number" id="manualEndInput" step="0.1" min="0" placeholder="10.0" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-slate-100 focus:outline-none focus:border-cyan-500 font-mono" />
          </div>
        </div>

        <div>
          <label class="block font-semibold text-slate-300 mb-1">Form Label (e.g., A, B, Chorus):</label>
          <input type="text" id="manualFormLabelInput" placeholder="e.g. Section A" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-slate-100 focus:outline-none focus:border-cyan-500" />
        </div>

        <div>
          <label class="block font-semibold text-slate-300 mb-1">Color:</label>
          <input type="color" id="manualColorInput" value="#3b82f6" class="w-full h-8 bg-slate-900 border border-slate-700 rounded-xl p-0.5 cursor-pointer" />
        </div>
      </div>

      <div class="flex justify-end gap-2 mt-5">
        <button onclick="closeManualSectionModal()" class="px-3.5 py-1.5 bg-slate-700 hover:bg-slate-600 text-slate-200 font-semibold rounded-xl text-xs">Cancel</button>
        <button onclick="saveManualSectionModal()" class="px-4 py-1.5 bg-cyan-600 hover:bg-cyan-500 text-white font-bold rounded-xl text-xs shadow">Save Section</button>
      </div>

    </div>
  </div>


  <!-- RESET ALL CONFIRMATION MODAL -->
  <div id="confirmClearModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-slate-800 border border-slate-700 w-full max-w-sm rounded-2xl shadow-2xl p-5 text-center">
      <i class="fa-solid fa-triangle-exclamation text-3xl text-rose-400 mb-2"></i>
      <h3 class="text-base font-bold text-white mb-1">Reset All Work?</h3>
      <p class="text-xs text-slate-400 mb-4">This will delete all marked sections and clear your worksheet notes. This action cannot be undone.</p>
      <div class="flex justify-center gap-3">
        <button onclick="closeConfirmClearModal()" class="px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-200 text-xs font-semibold rounded-xl">Cancel</button>
        <button onclick="executeResetAllWork()" class="px-4 py-2 bg-rose-600 hover:bg-rose-500 text-white text-xs font-bold rounded-xl shadow">Yes, Clear All</button>
      </div>
    </div>
  </div>


  <!-- Toast Notification Container -->
  <div id="toastContainer" class="fixed bottom-4 right-4 z-50 flex flex-col gap-2 pointer-events-none"></div>


  <!-- JAVASCRIPT LOGIC -->
  <script>
    
    // --- APP STATE VARIABLES ---
    let audioContext = null;
    let audioElement = new Audio();
    audioElement.crossOrigin = "anonymous";

    let currentAudioBuffer = null;
    let isPlaying = false;
    let currentSpeed = 1.0;
    let isMuted = false;
    
    // Zoom & Scroll Viewport State
    let zoomLevel = 1.0; // 1.0 to 8.0
    let scrollOffset = 0.0; // 0.0 to 1.0

    // Microphone Recording State
    let mediaRecorder = null;
    let recordedChunks = [];
    let isRecording = false;
    let recordStartTime = 0;
    let recordInterval = null;

    // Active Loop State
    let activeLoopSectionId = null;

    // Live Marking State
    let draftStartTime = null;
    let draftEndTime = null;

    // Active choices for new section
    let activeSelectedTag = "Verse 1";
    let activeSelectedColor = "#8b5cf6"; // Violet default

    // Sections Array
    let sectionsArray = [];
    let editingSectionId = null;

    // Preset definitions
    const PRESET_TAGS = [
      "Intro", "Verse 1", "Verse 2", "Pre-Chorus", "Chorus 1", "Chorus 2", 
      "Bridge", "Solo", "Drop", "Hook", "Outro", "Interlude", "Section A", "Section B"
    ];

    const PRESET_COLORS = [
      { name: "Crimson Verse", hex: "#ef4444" },
      { name: "Golden Chorus", hex: "#f59e0b" },
      { name: "Teal Bridge", hex: "#10b981" },
      { name: "Violet Outro", hex: "#8b5cf6" },
      { name: "Lime Intro", hex: "#84cc16" },
      { name: "Amber Solo", hex: "#d97706" },
      { name: "Cyan Drop", hex: "#06b6d4" },
      { name: "Rose Hook", hex: "#f43f5e" },
      { name: "Indigo Motif", hex: "#6366f1" }
    ];

    const INSTRUMENT_OPTIONS = [
      "Drums/Beat", "Bass", "Acoustic Guitar", "Electric Guitar", "Piano/Keys", 
      "Synthesizer", "Lead Vocals", "Backing Vocals", "Strings", "Brass", "Percussion"
    ];

    const DYNAMICS_OPTIONS = [
      "pp (Very Soft)", "p (Soft)", "mp (Medium Soft)", "mf (Medium Loud)", "f (Loud)", "ff (Very Loud)", "Crescendo ↗", "Decrescendo ↘"
    ];

    const TEXTURE_OPTIONS = [
      "Monophonic (1 Line)", "Homophonic (Melody + Chords)", "Polyphonic (Multi-melodies)", "Sparse / Thin", "Dense / Full Layers"
    ];

    const RHYTHM_OPTIONS = [
      "Fast / Energetic", "Slow / Calm", "Driving Beat", "Syncopated", "Half-Time", "Rubato / Free"
    ];


    // --- CHROMEBOOK KEYBOARD SHORTCUTS ENGINE ---
    window.addEventListener('keydown', (e) => {
      // If user is typing in input or textarea, don't trigger global shortcuts
      const tag = document.activeElement ? document.activeElement.tagName.toLowerCase() : '';
      if (tag === 'input' || tag === 'textarea' || tag === 'select') {
        if (e.key === 'Escape') document.activeElement.blur();
        return;
      }

      if (e.code === 'Space') {
        e.preventDefault();
        togglePlayPause();
      } else if (e.code === 'ArrowLeft') {
        e.preventDefault();
        skipTime(e.shiftKey ? -5 : -2);
      } else if (e.code === 'ArrowRight') {
        e.preventDefault();
        skipTime(e.shiftKey ? 5 : 2);
      } else if (e.code === 'KeyI') {
        markStartHere();
      } else if (e.code === 'KeyO') {
        markEndHere();
      } else if (e.code === 'KeyM' || e.code === 'Enter') {
        if (draftStartTime !== null && draftEndTime !== null) {
          saveCapturedSection();
        }
      } else if (e.code === 'KeyL') {
        if (sectionsArray.length > 0) {
          toggleLoopSection(sectionsArray[0].id);
        }
      } else if (e.code === 'KeyZ') {
        changeZoom(0.5);
      } else if (e.code === 'KeyX') {
        changeZoom(-0.5);
      } else if (e.code === 'KeyR') {
        resetZoom();
      } else if (e.key === '?' || (e.shiftKey && e.code === 'Slash')) {
        openShortcutsModal();
      }
    });


    // --- SYNTHESIZED DEMO AUDIO TRACK GENERATOR ---
    async function generateAudioTrack(type) {
      const sampleRate = 44100;
      let duration = 60;
      if (type === 'pop') duration = 60;
      if (type === 'lofi') duration = 50;
      if (type === 'classical') duration = 55;
      if (type === 'funk') duration = 55;

      const offlineCtx = new OfflineAudioContext(2, sampleRate * duration, sampleRate);
      const bpm = type === 'pop' ? 120 : (type === 'lofi' ? 85 : (type === 'classical' ? 100 : 115));
      const secondsPerBeat = 60 / bpm;

      const masterGain = offlineCtx.createGain();
      masterGain.gain.setValueAtTime(0.7, 0);
      masterGain.connect(offlineCtx.destination);

      // Drum pattern synthesizer
      for (let t = 0; t < duration; t += secondsPerBeat) {
        let beatNum = Math.floor(t / secondsPerBeat) % 4;

        if (beatNum === 0 || beatNum === 2) {
          const osc = offlineCtx.createOscillator();
          const gain = offlineCtx.createGain();
          osc.frequency.setValueAtTime(130, t);
          osc.frequency.exponentialRampToValueAtTime(30, t + 0.12);
          gain.gain.setValueAtTime(0.8, t);
          gain.gain.exponentialRampToValueAtTime(0.01, t + 0.15);
          osc.connect(gain);
          gain.connect(masterGain);
          osc.start(t);
          osc.stop(t + 0.15);
        }

        if (beatNum === 1 || beatNum === 3) {
          const osc = offlineCtx.createOscillator();
          const gain = offlineCtx.createGain();
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(220, t);
          osc.frequency.exponentialRampToValueAtTime(80, t + 0.1);
          gain.gain.setValueAtTime(0.4, t);
          gain.gain.exponentialRampToValueAtTime(0.01, t + 0.12);
          osc.connect(gain);
          gain.connect(masterGain);
          osc.start(t);
          osc.stop(t + 0.12);
        }
      }

      // Chord Progression
      const chords = [
        [261.63, 329.63, 392.00], // C
        [220.00, 261.63, 329.63], // Am
        [174.61, 220.00, 261.63], // F
        [196.00, 246.94, 293.66]  // G
      ];

      for (let t = 0; t < duration; t += secondsPerBeat * 4) {
        let chordIndex = Math.floor(t / (secondsPerBeat * 4)) % 4;
        let chordFreqs = chords[chordIndex];

        chordFreqs.forEach(freq => {
          const osc = offlineCtx.createOscillator();
          const gain = offlineCtx.createGain();
          osc.type = type === 'lofi' ? 'sine' : (type === 'classical' ? 'triangle' : 'sawtooth');
          osc.frequency.setValueAtTime(freq, t);
          
          const filter = offlineCtx.createBiquadFilter();
          filter.type = 'lowpass';
          filter.frequency.setValueAtTime(type === 'lofi' ? 500 : 1200, t);

          gain.gain.setValueAtTime(0.12, t);
          gain.gain.exponentialRampToValueAtTime(0.01, t + (secondsPerBeat * 3.8));

          osc.connect(filter);
          filter.connect(gain);
          gain.connect(masterGain);

          osc.start(t);
          osc.stop(t + (secondsPerBeat * 3.9));
        });
      }

      return await offlineCtx.startRendering();
    }

    function audioBufferToWav(buffer) {
      let numChannels = buffer.numberOfChannels;
      let sampleRate = buffer.sampleRate;
      let format = 1;
      let bitDepth = 16;
      let bytesPerSample = bitDepth / 8;
      let blockAlign = numChannels * bytesPerSample;
      let count = buffer.length;
      let dataSize = count * blockAlign;
      let bufferSize = 44 + dataSize;
      let arrayBuffer = new ArrayBuffer(bufferSize);
      let view = new DataView(arrayBuffer);

      function writeString(view, offset, string) {
        for (let i = 0; i < string.length; i++) view.setUint8(offset + i, string.charCodeAt(i));
      }

      writeString(view, 0, 'RIFF');
      view.setUint32(4, 36 + dataSize, true);
      writeString(view, 8, 'WAVE');
      writeString(view, 12, 'fmt ');
      view.setUint32(16, 16, true);
      view.setUint16(20, format, true);
      view.setUint16(22, numChannels, true);
      view.setUint32(24, sampleRate, true);
      view.setUint32(28, sampleRate * blockAlign, true);
      view.setUint16(32, blockAlign, true);
      view.setUint16(34, bitDepth, true);
      writeString(view, 36, 'data');
      view.setUint32(40, dataSize, true);

      let offset = 44;
      let channels = [];
      for (let i = 0; i < numChannels; i++) channels.push(buffer.getChannelData(i));

      for (let i = 0; i < count; i++) {
        for (let ch = 0; ch < numChannels; ch++) {
          let sample = Math.max(-1, Math.min(1, channels[ch][i]));
          sample = sample < 0 ? sample * 0x8000 : sample * 0x7FFF;
          view.setInt16(offset, sample, true);
          offset += 2;
        }
      }

      return new Blob([arrayBuffer], { type: 'audio/wav' });
    }


    // --- WAVEFORM CANVAS ZOOM & TOUCH PAN ENGINE ---
    function drawWaveform() {
      const canvas = document.getElementById('waveformCanvas');
      const ctx = canvas.getContext('2d');
      const width = canvas.width = canvas.parentElement.clientWidth;
      const height = canvas.height = canvas.parentElement.clientHeight;

      ctx.clearRect(0, 0, width, height);

      if (!currentAudioBuffer) return;

      const data = currentAudioBuffer.getChannelData(0);
      const totalSamples = data.length;
      
      const visibleSamples = Math.floor(totalSamples / zoomLevel);
      const startSample = Math.floor(scrollOffset * (totalSamples - visibleSamples));
      const endSample = startSample + visibleSamples;

      const step = Math.ceil(visibleSamples / width);
      const amp = height / 2;

      ctx.fillStyle = '#38bdf8';

      for (let i = 0; i < width; i++) {
        let min = 1.0;
        let max = -1.0;
        const sampleIdx = startSample + (i * step);
        
        for (let j = 0; j < step; j++) {
          const datum = data[sampleIdx + j];
          if (datum !== undefined) {
            if (datum < min) min = datum;
            if (datum > max) max = datum;
          }
        }

        if (min === 1.0) min = 0;
        if (max === -1.0) max = 0;

        const barHeight = Math.max(2, (max - min) * amp * 0.85);
        const y = (height - barHeight) / 2;

        ctx.fillRect(i, y, 1.5, barHeight);
      }
    }

    function changeZoom(delta) {
      zoomLevel = Math.max(1.0, Math.min(8.0, zoomLevel + delta));
      document.getElementById('zoomLevelDisplay').innerText = `${zoomLevel.toFixed(1)}x`;
      drawWaveform();
      renderTimelineOverlays();
    }

    function resetZoom() {
      zoomLevel = 1.0;
      scrollOffset = 0.0;
      document.getElementById('zoomLevelDisplay').innerText = `1.0x`;
      drawWaveform();
      renderTimelineOverlays();
    }


    // --- AUDIO LOADERS & INPUT HANDLERS ---
    async function loadSampleTrack(type) {
      showToast("Generating audio track...", "info");
      
      try {
        currentAudioBuffer = await generateAudioTrack(type);
        const wavBlob = audioBufferToWav(currentAudioBuffer);
        const blobUrl = URL.createObjectURL(wavBlob);

        audioElement.src = blobUrl;
        audioElement.load();

        audioElement.onloadedmetadata = () => {
          updateDurationDisplay();
          resetZoom();
          renderTimelineOverlays();
          renderRibbonStrip();
          showToast("Track loaded successfully!", "success");
        };

      } catch (err) {
        console.error("Error generating track:", err);
        showToast("Error synthesizing track", "error");
      }
    }

    function handleTrackSelectChange(value) {
      const customContainer = document.getElementById('customFileContainer');
      if (value === 'custom') {
        customContainer.classList.remove('hidden');
      } else if (value === 'mic') {
        customContainer.classList.add('hidden');
        toggleMicrophoneRecorder();
      } else {
        customContainer.classList.add('hidden');
        if (value === 'pop') document.getElementById('songTitleInput').value = "Neon Pop Adventure";
        if (value === 'lofi') document.getElementById('songTitleInput').value = "Sunset Lo-Fi Beat";
        if (value === 'classical') document.getElementById('songTitleInput').value = "Classical Minuet Form";
        if (value === 'funk') document.getElementById('songTitleInput').value = "Funk & Groove Jam";
        loadSampleTrack(value);
      }
      autoSaveToLocalStorage();
    }

    function handleCustomAudioUpload(event) {
      const file = event.target.files[0];
      if (!file) return;
      processAudioFile(file);
    }

    function processAudioFile(file) {
      document.getElementById('songTitleInput').value = file.name.replace(/\.[^/.]+$/, "");
      const objectUrl = URL.createObjectURL(file);
      audioElement.src = objectUrl;
      audioElement.load();

      const reader = new FileReader();
      reader.onload = function(e) {
        const arrayBuffer = e.target.result;
        if (!audioContext) audioContext = new (window.AudioContext || window.webkitAudioContext)();
        audioContext.decodeAudioData(arrayBuffer, (decodedBuffer) => {
          currentAudioBuffer = decodedBuffer;
          resetZoom();
        });
      };
      reader.readAsArrayBuffer(file);

      audioElement.onloadedmetadata = () => {
        updateDurationDisplay();
        renderTimelineOverlays();
        renderRibbonStrip();
        showToast("Audio file loaded!", "success");
        autoSaveToLocalStorage();
      };
    }


    // --- CHROMEBOOK MICROPHONE RECORDER ENGINE ---
    async function toggleMicrophoneRecorder() {
      if (isRecording) {
        stopRecording();
      } else {
        startRecording();
      }
    }

    async function startRecording() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        mediaRecorder = new MediaRecorder(stream);
        recordedChunks = [];

        mediaRecorder.ondataavailable = (e) => {
          if (e.data.size > 0) recordedChunks.push(e.data);
        };

        mediaRecorder.onstop = async () => {
          const blob = new Blob(recordedChunks, { type: 'audio/webm' });
          const objectUrl = URL.createObjectURL(blob);
          audioElement.src = objectUrl;
          audioElement.load();

          document.getElementById('songTitleInput').value = `Mic Recording (${new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})})`;
          document.getElementById('sampleTrackSelect').value = 'custom';

          const arrayBuffer = await blob.arrayBuffer();
          if (!audioContext) audioContext = new (window.AudioContext || window.webkitAudioContext)();
          audioContext.decodeAudioData(arrayBuffer, (decodedBuffer) => {
            currentAudioBuffer = decodedBuffer;
            resetZoom();
            renderTimelineOverlays();
            renderRibbonStrip();
          });

          showToast("Live microphone recording loaded!", "success");
          autoSaveToLocalStorage();
        };

        mediaRecorder.start();
        isRecording = true;
        recordStartTime = Date.now();
        
        document.getElementById('micBtnIcon').className = "fa-solid fa-square text-white";
        document.getElementById('micBtnText').innerText = "Stop Rec";
        document.getElementById('micRecordTimer').classList.remove('hidden');

        recordInterval = setInterval(() => {
          const elapsed = Math.floor((Date.now() - recordStartTime) / 1000);
          document.getElementById('micRecordTimer').innerText = formatTime(elapsed);
        }, 1000);

        showToast("Recording live Chromebook microphone...", "info");

      } catch (err) {
        console.error("Mic access error:", err);
        showToast("Microphone access denied or unavailable.", "error");
      }
    }

    function stopRecording() {
      if (mediaRecorder && isRecording) {
        mediaRecorder.stop();
        mediaRecorder.stream.getTracks().forEach(track => track.stop());
        isRecording = false;
        clearInterval(recordInterval);
        
        document.getElementById('micBtnIcon').className = "fa-solid fa-microphone";
        document.getElementById('micBtnText').innerText = "Record Mic";
        document.getElementById('micRecordTimer').classList.add('hidden');
      }
    }


    // --- PLAYBACK CONTROLS ---
    function togglePlayPause() {
      if (audioElement.paused) {
        audioElement.play();
        isPlaying = true;
        document.getElementById('playIcon').className = 'fa-solid fa-pause';
      } else {
        audioElement.pause();
        isPlaying = false;
        document.getElementById('playIcon').className = 'fa-solid fa-play ml-0.5';
      }
    }

    function skipTime(seconds) {
      audioElement.currentTime = Math.max(0, Math.min(audioElement.duration || 0, audioElement.currentTime + seconds));
    }

    function setPlaybackSpeed(speed) {
      currentSpeed = speed;
      audioElement.playbackRate = speed;
      document.querySelectorAll('.speed-btn').forEach(btn => {
        if (parseFloat(btn.dataset.speed) === speed) {
          btn.className = 'speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg bg-cyan-500 text-white transition';
        } else {
          btn.className = 'speed-btn px-2 py-0.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition';
        }
      });
    }

    function changeVolume(val) {
      audioElement.volume = parseFloat(val);
      const icon = document.getElementById('volumeIcon');
      if (audioElement.volume === 0) icon.className = 'fa-solid fa-volume-xmark';
      else if (audioElement.volume < 0.5) icon.className = 'fa-solid fa-volume-low';
      else icon.className = 'fa-solid fa-volume-high';
    }

    function toggleMute() {
      isMuted = !isMuted;
      audioElement.muted = isMuted;
      const icon = document.getElementById('volumeIcon');
      if (isMuted) icon.className = 'fa-solid fa-volume-xmark text-rose-400';
      else changeVolume(audioElement.volume);
    }

    audioElement.ontimeupdate = () => {
      const cur = audioElement.currentTime || 0;
      const dur = audioElement.duration || 1;

      document.getElementById('currentTimeDisplay').innerText = formatTime(cur);
      document.getElementById('liveTimeTracker').innerText = formatTime(cur);

      // Playhead Position mapped through Zoom & Scroll
      const percentTotal = cur / dur;
      const visibleFraction = 1.0 / zoomLevel;
      const startFraction = scrollOffset * (1.0 - visibleFraction);
      const relPercent = (percentTotal - startFraction) / visibleFraction;

      const playhead = document.getElementById('playheadLine');
      if (relPercent >= 0 && relPercent <= 1) {
        playhead.style.left = `${relPercent * 100}%`;
        playhead.classList.remove('hidden');
      } else {
        playhead.classList.add('hidden');
      }

      // Active Section Loop Handler
      if (activeLoopSectionId) {
        const loopSec = sectionsArray.find(s => s.id === activeLoopSectionId);
        if (loopSec) {
          if (cur >= loopSec.endTime || cur < loopSec.startTime) {
            audioElement.currentTime = loopSec.startTime;
          }
        }
      }

      // Live Section Marking Draft Overlay
      if (draftStartTime !== null && draftEndTime === null) {
        const draftEl = document.getElementById('draftSectionMarker');
        const startRel = (draftStartTime / dur - startFraction) / visibleFraction;
        const widthRel = ((cur - draftStartTime) / dur) / visibleFraction;

        draftEl.style.left = `${startRel * 100}%`;
        draftEl.style.width = `${widthRel * 100}%`;
        draftEl.classList.remove('hidden');

        document.getElementById('activeTimingPreview').innerText = `${formatTime(draftStartTime)} - ${formatTime(cur)}`;
      }
    };

    function updateDurationDisplay() {
      const dur = audioElement.duration || 0;
      document.getElementById('totalDurationDisplay').innerText = formatTime(dur);
      document.getElementById('ribbonDurationEnd').innerText = formatTime(dur);
    }


    // --- TIMELINE TOUCH & CLICK INTERACTION ENGINE ---
    const timelineContainer = document.getElementById('timelineContainer');
    
    timelineContainer.addEventListener('click', (e) => {
      const rect = timelineContainer.getBoundingClientRect();
      const clickX = e.clientX - rect.left;
      const clickRatio = clickX / rect.width;

      const dur = audioElement.duration || 0;
      const visibleFraction = 1.0 / zoomLevel;
      const startFraction = scrollOffset * (1.0 - visibleFraction);
      
      const targetTime = (startFraction + (clickRatio * visibleFraction)) * dur;
      audioElement.currentTime = Math.max(0, Math.min(dur, targetTime));
    });

    timelineContainer.addEventListener('mousemove', (e) => {
      const rect = timelineContainer.getBoundingClientRect();
      const mouseX = e.clientX - rect.left;
      const mouseRatio = mouseX / rect.width;

      const dur = audioElement.duration || 0;
      const visibleFraction = 1.0 / zoomLevel;
      const startFraction = scrollOffset * (1.0 - visibleFraction);
      const hoverTime = (startFraction + (mouseRatio * visibleFraction)) * dur;

      const tooltip = document.getElementById('timelineHoverTooltip');
      tooltip.innerText = formatTime(hoverTime);
      tooltip.style.left = `${mouseX}px`;
      tooltip.classList.remove('hidden');
    });

    timelineContainer.addEventListener('mouseleave', () => {
      document.getElementById('timelineHoverTooltip').classList.add('hidden');
    });

    // Touch & Drag Panning for Touchscreen Chromebooks
    let isPanning = false;
    let startPanX = 0;

    timelineContainer.addEventListener('touchstart', (e) => {
      if (zoomLevel > 1.0 && e.touches.length === 1) {
        isPanning = true;
        startPanX = e.touches[0].clientX;
      }
    });

    timelineContainer.addEventListener('touchmove', (e) => {
      if (isPanning && zoomLevel > 1.0) {
        const deltaX = startPanX - e.touches[0].clientX;
        startPanX = e.touches[0].clientX;

        const panRatio = deltaX / timelineContainer.clientWidth;
        scrollOffset = Math.max(0, Math.min(1.0, scrollOffset + panRatio));
        drawWaveform();
        renderTimelineOverlays();
      }
    });

    timelineContainer.addEventListener('touchend', () => { isPanning = false; });


    // --- DRAG & DROP FILE UPLOAD HANDLER ---
    const dropZone = document.getElementById('dropZoneContainer');
    const dropOverlay = document.getElementById('dragDropOverlay');

    ['dragenter', 'dragover'].forEach(eventName => {
      dropZone.addEventListener(eventName, (e) => {
        e.preventDefault();
        dropOverlay.classList.remove('hidden');
      }, false);
    });

    ['dragleave', 'drop'].forEach(eventName => {
      dropZone.addEventListener(eventName, (e) => {
        e.preventDefault();
        dropOverlay.classList.add('hidden');
      }, false);
    });

    dropZone.addEventListener('drop', (e) => {
      const files = e.dataTransfer.files;
      if (files.length > 0 && files[0].type.startsWith('audio/')) {
        processAudioFile(files[0]);
      }
    });


    // --- LIVE SECTION MARKING ENGINE ---
    function markStartHere() {
      draftStartTime = audioElement.currentTime;
      draftEndTime = null;

      document.getElementById('endMarkBtn').disabled = false;
      document.getElementById('endMarkBtn').className = 'py-2.5 px-3 rounded-xl bg-amber-500 hover:bg-amber-400 text-white font-semibold text-xs transition shadow-lg shadow-amber-500/20 flex items-center justify-center gap-1.5 active:scale-95 animate-bounce';

      document.getElementById('liveMarkStatus').innerHTML = `
        <div class="text-amber-300 font-medium">
          <i class="fa-solid fa-spinner fa-spin mr-1"></i> Section started at <strong class="font-mono text-white">${formatTime(draftStartTime)}</strong>. Play to end, press <span class="kbd-badge mx-1">O</span>
        </div>
      `;

      showToast(`IN point set at ${formatTime(draftStartTime)}`, "info");
    }

    function markEndHere() {
      if (draftStartTime === null) return;
      draftEndTime = audioElement.currentTime;

      if (draftEndTime <= draftStartTime) {
        showToast("End time must be after start time!", "error");
        return;
      }

      document.getElementById('draftSectionMarker').classList.add('hidden');
      document.getElementById('confirmAddSectionBtn').disabled = false;

      document.getElementById('liveMarkStatus').innerHTML = `
        <div class="text-emerald-400 font-medium">
          <i class="fa-solid fa-check-circle mr-1"></i> Captured: <strong class="font-mono text-white">${formatTime(draftStartTime)} - ${formatTime(draftEndTime)}</strong>. Press <span class="kbd-badge mx-1">M</span> to Save!
        </div>
      `;

      document.getElementById('activeTimingPreview').innerText = `${formatTime(draftStartTime)} - ${formatTime(draftEndTime)}`;
      showToast("Captured! Press M to Save.", "success");
    }

    function saveCapturedSection() {
      if (draftStartTime === null || draftEndTime === null) {
        showToast("Please mark start (IN) and end (OUT) first!", "error");
        return;
      }

      const newSec = {
        id: 'sec_' + Date.now(),
        name: activeSelectedTag,
        formLabel: activeSelectedTag,
        startTime: parseFloat(draftStartTime.toFixed(2)),
        endTime: parseFloat(draftEndTime.toFixed(2)),
        color: activeSelectedColor,
        instruments: ["Synthesizer", "Drums/Beat"],
        dynamics: "mf (Medium Loud)",
        texture: "Homophonic (Melody + Chords)",
        rhythm: "Driving Beat",
        notes: ""
      };

      sectionsArray.push(newSec);
      sortSections('time');

      draftStartTime = null;
      draftEndTime = null;
      document.getElementById('endMarkBtn').disabled = true;
      document.getElementById('endMarkBtn').className = 'py-2.5 px-3 rounded-xl bg-slate-700 text-slate-500 font-semibold text-xs transition flex items-center justify-center gap-1.5 cursor-not-allowed';
      document.getElementById('confirmAddSectionBtn').disabled = true;
      document.getElementById('liveMarkStatus').innerHTML = `Click <span class="text-emerald-400 font-semibold mx-1">"Start IN"</span> or press <span class="kbd-badge mx-1">I</span>`;

      autoAdvancePresetTag();
      renderAllViews();
      autoSaveToLocalStorage();
      showToast(`Section "${newSec.name}" saved!`, "success");
    }

    function autoAdvancePresetTag() {
      if (activeSelectedTag.startsWith("Verse")) activeSelectedTag = "Chorus 1";
      else if (activeSelectedTag.startsWith("Chorus")) activeSelectedTag = "Bridge";
      else if (activeSelectedTag === "Intro") activeSelectedTag = "Verse 1";
      else if (activeSelectedTag === "Bridge") activeSelectedTag = "Chorus 2";
      else activeSelectedTag = "Outro";

      document.getElementById('activeTagName').innerText = activeSelectedTag;
      renderPresetChips();
    }


    // --- UI PRESET & PALETTE GENERATORS ---
    function renderPresetChips() {
      const container = document.getElementById('presetTagChips');
      container.innerHTML = PRESET_TAGS.map(tag => {
        const isSelected = tag === activeSelectedTag;
        return `
          <button onclick="selectPresetTag('${tag}')" class="px-2.5 py-1 rounded-xl text-xs font-semibold transition border ${isSelected ? 'bg-fuchsia-600 border-fuchsia-400 text-white shadow' : 'bg-slate-900 border-slate-700 text-slate-300 hover:bg-slate-800'}">
            ${tag}
          </button>
        `;
      }).join('');
    }

    function renderColorPaletteChips() {
      const container = document.getElementById('colorPaletteChips');
      container.innerHTML = PRESET_COLORS.map(c => {
        const isSelected = c.hex.toLowerCase() === activeSelectedColor.toLowerCase();
        return `
          <button onclick="selectPresetColor('${c.hex}')" title="${c.name}" class="w-7 h-7 rounded-xl transition transform hover:scale-110 flex items-center justify-center border-2 ${isSelected ? 'border-white scale-110 shadow' : 'border-transparent'}" style="background-color: ${c.hex}">
            ${isSelected ? '<i class="fa-solid fa-check text-white text-[10px]"></i>' : ''}
          </button>
        `;
      }).join('');
    }

    function selectPresetTag(tag) {
      activeSelectedTag = tag;
      document.getElementById('activeTagName').innerText = tag;
      renderPresetChips();
    }

    function selectPresetColor(hex) {
      activeSelectedColor = hex;
      document.getElementById('activeColorBadge').style.backgroundColor = hex;
      renderColorPaletteChips();
    }

    function selectCustomColor(hex) {
      activeSelectedColor = hex;
      document.getElementById('activeColorBadge').style.backgroundColor = hex;
      renderColorPaletteChips();
    }

    function sortSections(method) {
      if (method === 'time') sectionsArray.sort((a, b) => a.startTime - b.startTime);
      if (method === 'name') sectionsArray.sort((a, b) => a.name.localeCompare(b.name));
      if (method === 'duration') sectionsArray.sort((a, b) => (b.endTime - b.startTime) - (a.endTime - a.startTime));
    }


    // --- STUDENT WORKSHEET CARDS RENDERER ---
    function renderSectionsList() {
      const sortVal = document.getElementById('sectionSortSelect').value;
      sortSections(sortVal);

      const container = document.getElementById('sectionsListContainer');

      if (sectionsArray.length === 0) {
        container.innerHTML = `
          <div class="text-center py-10 border-2 border-dashed border-slate-700/80 rounded-2xl p-6 bg-slate-900/40">
            <i class="fa-solid fa-layer-group text-3xl text-slate-600 mb-2 block"></i>
            <h3 class="text-sm font-bold text-slate-300">No Song Sections Marked Yet</h3>
            <p class="text-xs text-slate-400 mt-1 max-w-md mx-auto">
              Press <span class="kbd-badge">I</span> to set start point and <span class="kbd-badge">O</span> for end point, or click <strong>Load Demo Markings</strong> in the File menu.
            </p>
          </div>
        `;
        return;
      }

      container.innerHTML = sectionsArray.map(sec => {
        const duration = (sec.endTime - sec.startTime).toFixed(1);
        const isLoopingThis = activeLoopSectionId === sec.id;

        return `
          <div class="bg-slate-900/90 border border-slate-700/90 rounded-2xl p-4 shadow-lg transition hover:border-slate-600">
            
            <div class="flex flex-wrap items-center justify-between gap-2.5 pb-3 border-b border-slate-800">
              
              <div class="flex items-center gap-2.5">
                <div class="w-4 h-4 rounded-lg shadow" style="background-color: ${sec.color}"></div>
                <h3 class="text-sm font-bold text-white">${sec.name}</h3>
                <span class="font-mono text-[11px] text-cyan-300 bg-cyan-950/60 border border-cyan-800/60 px-2 py-0.5 rounded-full font-semibold">
                  ${formatTime(sec.startTime)} - ${formatTime(sec.endTime)} (${duration}s)
                </span>
              </div>

              <div class="flex items-center gap-1.5">
                <button onclick="toggleLoopSection('${sec.id}')" class="px-2.5 py-1 rounded-xl text-xs font-semibold transition flex items-center gap-1 border ${isLoopingThis ? 'bg-amber-500 border-amber-400 text-slate-950 shadow' : 'bg-slate-800 border-slate-700 text-slate-300 hover:bg-slate-700'}">
                  <i class="fa-solid fa-rotate-right"></i>
                  <span>${isLoopingThis ? 'Looping' : 'Loop'}</span>
                </button>

                <button onclick="jumpToSection(${sec.startTime})" class="px-2.5 py-1 rounded-xl bg-slate-800 border border-slate-700 hover:bg-slate-700 text-slate-300 text-xs font-semibold transition flex items-center gap-1">
                  <i class="fa-solid fa-play text-[9px]"></i> Play
                </button>

                <button onclick="openEditSectionModal('${sec.id}')" class="p-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 transition">
                  <i class="fa-solid fa-pen-to-square"></i>
                </button>

                <button onclick="deleteSection('${sec.id}')" class="p-1 rounded-lg bg-slate-800 hover:bg-rose-900/50 text-rose-400 transition">
                  <i class="fa-solid fa-trash-can"></i>
                </button>
              </div>

            </div>

            <!-- Worksheet Analysis Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-3 mt-3 text-xs">
              
              <!-- Instruments Checklist -->
              <div class="bg-slate-950/60 border border-slate-800 p-2.5 rounded-xl">
                <span class="font-bold text-slate-300 block mb-1.5 text-[10px] uppercase tracking-wider">
                  <i class="fa-solid fa-drum text-cyan-400 mr-1"></i> Instruments:
                </span>
                <div class="flex flex-wrap gap-1">
                  ${INSTRUMENT_OPTIONS.map(inst => {
                    const checked = sec.instruments.includes(inst);
                    return `
                      <button onclick="toggleInstrumentTag('${sec.id}', '${inst}')" class="px-1.5 py-0.5 rounded-lg text-[9px] font-medium transition ${checked ? 'bg-cyan-500/20 border border-cyan-400 text-cyan-300' : 'bg-slate-900 border border-slate-800 text-slate-500 hover:text-slate-300'}">
                        ${inst}
                      </button>
                    `;
                  }).join('')}
                </div>
              </div>

              <!-- Dynamics Dropdown -->
              <div class="bg-slate-950/60 border border-slate-800 p-2.5 rounded-xl">
                <span class="font-bold text-slate-300 block mb-1.5 text-[10px] uppercase tracking-wider">
                  <i class="fa-solid fa-volume-high text-fuchsia-400 mr-1"></i> Dynamics:
                </span>
                <select onchange="updateSectionField('${sec.id}', 'dynamics', this.value)" class="w-full bg-slate-900 border border-slate-700 text-slate-200 text-xs rounded-lg p-1.5 focus:outline-none">
                  <option value="">Select Dynamics...</option>
                  ${DYNAMICS_OPTIONS.map(d => `<option value="${d}" ${sec.dynamics === d ? 'selected' : ''}>${d}</option>`).join('')}
                </select>
              </div>

              <!-- Texture & Rhythm Selection -->
              <div class="bg-slate-950/60 border border-slate-800 p-2.5 rounded-xl">
                <span class="font-bold text-slate-300 block mb-1.5 text-[10px] uppercase tracking-wider">
                  <i class="fa-solid fa-layer-group text-amber-400 mr-1"></i> Texture & Rhythm:
                </span>
                <div class="space-y-1.5">
                  <select onchange="updateSectionField('${sec.id}', 'texture', this.value)" class="w-full bg-slate-900 border border-slate-700 text-slate-200 text-xs rounded-lg p-1 focus:outline-none">
                    <option value="">Select Texture...</option>
                    ${TEXTURE_OPTIONS.map(t => `<option value="${t}" ${sec.texture === t ? 'selected' : ''}>${t}</option>`).join('')}
                  </select>
                  <select onchange="updateSectionField('${sec.id}', 'rhythm', this.value)" class="w-full bg-slate-900 border border-slate-700 text-slate-200 text-xs rounded-lg p-1 focus:outline-none">
                    <option value="">Select Rhythm...</option>
                    ${RHYTHM_OPTIONS.map(r => `<option value="${r}" ${sec.rhythm === r ? 'selected' : ''}>${r}</option>`).join('')}
                  </select>
                </div>
              </div>

              <!-- Student Notes Textbox -->
              <div class="bg-slate-950/60 border border-slate-800 p-2.5 rounded-xl flex flex-col">
                <span class="font-bold text-slate-300 block mb-1.5 text-[10px] uppercase tracking-wider">
                  <i class="fa-solid fa-comment-dots text-emerald-400 mr-1"></i> Student Notes:
                </span>
                <textarea oninput="updateSectionField('${sec.id}', 'notes', this.value)" placeholder="Write musical observations..." class="w-full flex-1 bg-slate-900 border border-slate-700 rounded-lg p-1.5 text-slate-200 text-xs placeholder-slate-600 focus:outline-none resize-none">${sec.notes || ''}</textarea>
              </div>

            </div>

          </div>
        `;
      }).join('');
    }

    function toggleInstrumentTag(secId, inst) {
      const sec = sectionsArray.find(s => s.id === secId);
      if (!sec) return;
      if (sec.instruments.includes(inst)) {
        sec.instruments = sec.instruments.filter(i => i !== inst);
      } else {
        sec.instruments.push(inst);
      }
      renderSectionsList();
      autoSaveToLocalStorage();
    }

    function updateSectionField(secId, field, val) {
      const sec = sectionsArray.find(s => s.id === secId);
      if (sec) {
        sec[field] = val;
        autoSaveToLocalStorage();
      }
    }

    function toggleLoopSection(secId) {
      if (activeLoopSectionId === secId) {
        stopLoopingSection();
      } else {
        activeLoopSectionId = secId;
        const sec = sectionsArray.find(s => s.id === secId);
        if (sec) {
          audioElement.currentTime = sec.startTime;
          if (audioElement.paused) togglePlayPause();
          document.getElementById('loopStatusBadge').classList.remove('hidden');
          document.getElementById('loopStatusBadge').classList.add('flex');
          document.getElementById('loopingSectionName').innerText = sec.name;
          showToast(`Looping section "${sec.name}"`, "info");
        }
      }
      renderSectionsList();
    }

    function stopLoopingSection() {
      activeLoopSectionId = null;
      document.getElementById('loopStatusBadge').classList.add('hidden');
      document.getElementById('loopStatusBadge').classList.remove('flex');
      renderSectionsList();
    }

    function jumpToSection(startTime) {
      audioElement.currentTime = startTime;
      if (audioElement.paused) togglePlayPause();
    }

    function deleteSection(secId) {
      sectionsArray = sectionsArray.filter(s => s.id !== secId);
      if (activeLoopSectionId === secId) stopLoopingSection();
      renderAllViews();
      autoSaveToLocalStorage();
      showToast("Section deleted", "info");
    }


    // --- TIMELINE OVERLAYS & RIBBON RENDERING ---
    function renderTimelineOverlays() {
      const container = document.getElementById('timelineSectionsOverlay');
      const dur = audioElement.duration || 1;

      const visibleFraction = 1.0 / zoomLevel;
      const startFraction = scrollOffset * (1.0 - visibleFraction);

      container.innerHTML = sectionsArray.map(sec => {
        const startRel = (sec.startTime / dur - startFraction) / visibleFraction;
        const widthRel = ((sec.endTime - sec.startTime) / dur) / visibleFraction;

        if (startRel + widthRel < 0 || startRel > 1) return ''; // Offscreen

        return `
          <div class="absolute top-0 bottom-0 opacity-40 hover:opacity-75 transition border-x border-white/30 flex items-center justify-center cursor-pointer" 
               style="left: ${startRel * 100}%; width: ${widthRel * 100}%; background-color: ${sec.color}" 
               title="${sec.name} (${formatTime(sec.startTime)} - ${formatTime(sec.endTime)})"
               onclick="event.stopPropagation(); jumpToSection(${sec.startTime})">
            <span class="text-[9px] font-bold text-white shadow truncate px-1">${sec.name}</span>
          </div>
        `;
      }).join('');
    }

    function renderRibbonStrip() {
      const container = document.getElementById('sectionRibbonStrip');
      const dur = audioElement.duration || 1;

      if (sectionsArray.length === 0) {
        container.innerHTML = `<div class="w-full h-full bg-slate-800/80 rounded text-[9px] text-slate-500 flex items-center justify-center font-medium">No sections marked yet</div>`;
        return;
      }

      const sorted = [...sectionsArray].sort((a, b) => a.startTime - b.startTime);
      let html = '';
      let currentPos = 0;

      sorted.forEach(sec => {
        if (sec.startTime > currentPos) {
          const gapWidth = ((sec.startTime - currentPos) / dur) * 100;
          if (gapWidth > 0.5) {
            html += `<div class="h-full bg-slate-800/80 rounded-sm" style="width: ${gapWidth}%" title="Unassigned Gap"></div>`;
          }
        }
        const width = ((sec.endTime - sec.startTime) / dur) * 100;
        html += `<div class="h-full rounded-sm flex items-center justify-center text-[8px] font-bold text-white shadow-sm truncate px-0.5 transition hover:brightness-125" style="width: ${width}%; background-color: ${sec.color}" title="${sec.name}">${width > 5 ? sec.name : ''}</div>`;
        currentPos = sec.endTime;
      });

      if (currentPos < dur) {
        const remainingWidth = ((dur - currentPos) / dur) * 100;
        if (remainingWidth > 0.5) {
          html += `<div class="h-full bg-slate-800/80 rounded-sm" style="width: ${remainingWidth}%" title="Unassigned Gap"></div>`;
        }
      }

      container.innerHTML = html;
    }

    function renderSongFormChain() {
      const container = document.getElementById('songFormChain');
      const dur = audioElement.duration || 1;

      if (sectionsArray.length === 0) {
        container.innerHTML = `<div class="text-xs text-slate-500 italic">No sections marked yet. Mark live or add sections above to build the song form!</div>`;
        document.getElementById('coveragePercentage').innerText = `0%`;
        return;
      }

      const sorted = [...sectionsArray].sort((a, b) => a.startTime - b.startTime);

      let totalMarkedSecs = 0;
      sorted.forEach(s => totalMarkedSecs += (s.endTime - s.startTime));
      const coverage = Math.min(100, Math.round((totalMarkedSecs / dur) * 100));
      document.getElementById('coveragePercentage').innerText = `${coverage}%`;

      container.innerHTML = sorted.map((sec, idx) => {
        return `
          <div class="flex items-center gap-1.5 flex-shrink-0">
            <div onclick="jumpToSection(${sec.startTime})" class="px-2.5 py-1 rounded-xl font-extrabold text-xs text-white shadow cursor-pointer transition transform hover:scale-105 flex items-center gap-1" style="background-color: ${sec.color}">
              <i class="fa-solid fa-music text-[9px]"></i>
              <span>${sec.name}</span>
            </div>
            ${idx < sorted.length - 1 ? '<i class="fa-solid fa-arrow-right text-slate-600 text-[10px]"></i>' : ''}
          </div>
        `;
      }).join('');
    }

    function renderAllViews() {
      renderSectionsList();
      renderTimelineOverlays();
      renderRibbonStrip();
      renderSongFormChain();
    }


    // --- GOOGLE CLASSROOM & DOCS INTEGRATION HELPERS ---
    function copyFormattedSummaryForClassroom() {
      const student = document.getElementById('studentNameInput').value || 'Student';
      const song = document.getElementById('songTitleInput').value || 'Song Title';
      const duration = formatTime(audioElement.duration || 0);
      const sorted = [...sectionsArray].sort((a, b) => a.startTime - b.startTime);

      let text = `========================================\n`;
      text += `MUSIC CLASS SONG STRUCTURE ANALYSIS REPORT\n`;
      text += `========================================\n`;
      text += `Student Name: ${student}\n`;
      text += `Song Title: ${song}\n`;
      text += `Track Duration: ${duration}\n`;
      text += `Date: ${new Date().toLocaleDateString()}\n\n`;

      text += `--- SONG FORM SEQUENCE ---\n`;
      if (sorted.length === 0) {
        text += `(No sections marked yet)\n\n`;
      } else {
        text += sorted.map(s => `[ ${s.name} ]`).join(' -> ') + `\n\n`;
      }

      text += `--- DETAILED SECTION BREAKDOWN ---\n`;
      sorted.forEach((sec, idx) => {
        const secDur = (sec.endTime - sec.startTime).toFixed(1);
        text += `${idx + 1}. ${sec.name.toUpperCase()} (${formatTime(sec.startTime)} - ${formatTime(sec.endTime)} | ${secDur}s)\n`;
        text += `   • Form Label: ${sec.formLabel || sec.name}\n`;
        text += `   • Instruments: ${sec.instruments.join(', ') || 'None specified'}\n`;
        text += `   • Dynamics: ${sec.dynamics || 'Not specified'}\n`;
        text += `   • Texture: ${sec.texture || 'Not specified'}\n`;
        text += `   • Rhythm / Tempo: ${sec.rhythm || 'Not specified'}\n`;
        text += `   • Student Notes: ${sec.notes || 'None'}\n\n`;
      });

      text += `========================================\n`;
      text += `Generated with MelodyMapper (Chromebook Edition)\n`;

      // Copy using execCommand for iFrame compatibility
      const tempTextArea = document.createElement('textarea');
      tempTextArea.value = text;
      tempTextArea.style.position = 'fixed';
      tempTextArea.style.left = '-9999px';
      document.body.appendChild(tempTextArea);
      tempTextArea.select();
      
      let successful = false;
      try {
        successful = document.execCommand('copy');
      } catch (err) {
        successful = false;
      }
      document.body.removeChild(tempTextArea);

      if (successful) {
        showToast("Copied to Clipboard! Ready to paste (Ctrl+V) into Google Docs or Classroom!", "success");
      } else {
        showToast("Could not copy automatically. Check clipboard permissions.", "error");
      }
    }


    // --- LOCALSTORAGE AUTO-SAVE ENGINE ---
    function autoSaveToLocalStorage() {
      const data = {
        studentName: document.getElementById('studentNameInput').value,
        songTitle: document.getElementById('songTitleInput').value,
        sections: sectionsArray,
        timestamp: Date.now()
      };
      localStorage.setItem('melody_mapper_chromebook_autosave', JSON.stringify(data));
      
      const badge = document.getElementById('autosaveBadge');
      if (badge) {
        badge.classList.remove('opacity-60');
        setTimeout(() => badge.classList.add('opacity-60'), 1000);
      }
    }

    function restoreFromLocalStorage() {
      const saved = localStorage.getItem('melody_mapper_chromebook_autosave');
      if (saved) {
        try {
          const data = JSON.parse(saved);
          if (data.studentName) document.getElementById('studentNameInput').value = data.studentName;
          if (data.songTitle) document.getElementById('songTitleInput').value = data.songTitle;
          if (data.sections && Array.isArray(data.sections) && data.sections.length > 0) {
            sectionsArray = data.sections;
            renderAllViews();
            showToast("Restored your previous auto-saved work!", "info");
          }
        } catch(e) {
          console.error("Autosave load error", e);
        }
      }
    }


    // --- PROJECT JSON EXPORT & IMPORT ---
    function exportProjectJSON() {
      const data = {
        studentName: document.getElementById('studentNameInput').value,
        songTitle: document.getElementById('songTitleInput').value,
        sections: sectionsArray,
        exportDate: new Date().toISOString()
      };

      const jsonStr = JSON.stringify(data, null, 2);
      const blob = new Blob([jsonStr], { type: 'application/json' });
      const url = URL.createObjectURL(blob);

      const a = document.createElement('a');
      a.href = url;
      a.download = `${data.songTitle.replace(/\s+/g, '_')}_Analysis.json`;
      a.click();
      URL.revokeObjectURL(url);

      showToast("Project JSON exported!", "success");
    }

    function importProjectJSON(e) {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(evt) {
        try {
          const data = JSON.parse(evt.target.result);
          if (data.studentName) document.getElementById('studentNameInput').value = data.studentName;
          if (data.songTitle) document.getElementById('songTitleInput').value = data.songTitle;
          if (data.sections && Array.isArray(data.sections)) {
            sectionsArray = data.sections;
            renderAllViews();
            autoSaveToLocalStorage();
            showToast("Project JSON loaded!", "success");
          }
        } catch (err) {
          showToast("Failed to parse JSON file", "error");
        }
      };
      reader.readAsText(file);
    }

    function loadDemoAnalysis() {
      sectionsArray = [
        {
          id: 'sec_1',
          name: 'Intro',
          formLabel: 'Section A',
          startTime: 0,
          endTime: 8,
          color: '#84cc16',
          instruments: ['Synthesizer'],
          dynamics: 'p (Soft)',
          texture: 'Monophonic (1 Line)',
          rhythm: 'Slow / Calm',
          notes: 'Bright synthesizer arpeggio motif starts the track.'
        },
        {
          id: 'sec_2',
          name: 'Verse 1',
          formLabel: 'Section A',
          startTime: 8,
          endTime: 20,
          color: '#ef4444',
          instruments: ['Drums/Beat', 'Bass', 'Synthesizer'],
          dynamics: 'mf (Medium Loud)',
          texture: 'Homophonic (Melody + Chords)',
          rhythm: 'Driving Beat',
          notes: 'Kick drum and bassline enter, steady 4/4 rhythm.'
        },
        {
          id: 'sec_3',
          name: 'Pre-Chorus',
          formLabel: 'Section B',
          startTime: 20,
          endTime: 30,
          color: '#f59e0b',
          instruments: ['Drums/Beat', 'Synthesizer'],
          dynamics: 'Crescendo ↗',
          texture: 'Dense / Full Layers',
          rhythm: 'Fast / Energetic',
          notes: 'Rising filter sweep with fast snare roll building tension.'
        },
        {
          id: 'sec_4',
          name: 'Chorus 1',
          formLabel: 'Section C',
          startTime: 30,
          endTime: 45,
          color: '#06b6d4',
          instruments: ['Drums/Beat', 'Bass', 'Synthesizer', 'Lead Vocals'],
          dynamics: 'f (Loud)',
          texture: 'Homophonic (Melody + Chords)',
          rhythm: 'Driving Beat',
          notes: 'Full energy peak, catchy synth lead melody.'
        },
        {
          id: 'sec_5',
          name: 'Bridge',
          formLabel: 'Section D',
          startTime: 45,
          endTime: 55,
          color: '#8b5cf6',
          instruments: ['Piano/Keys', 'Synthesizer'],
          dynamics: 'mp (Medium Soft)',
          texture: 'Sparse / Thin',
          rhythm: 'Half-Time',
          notes: 'Drums drop out for a soft atmospheric chord breakdown.'
        },
        {
          id: 'sec_6',
          name: 'Outro',
          formLabel: 'Section A',
          startTime: 55,
          endTime: 60,
          color: '#f43f5e',
          instruments: ['Synthesizer'],
          dynamics: 'Decrescendo ↘',
          texture: 'Monophonic (1 Line)',
          rhythm: 'Rubato / Free',
          notes: 'Soft synth ring out fading to silence.'
        }
      ];

      renderAllViews();
      autoSaveToLocalStorage();
      showToast("Demo section markings loaded!", "success");
    }

    function openConfirmClearModal() {
      document.getElementById('confirmClearModal').classList.remove('hidden');
    }

    function closeConfirmClearModal() {
      document.getElementById('confirmClearModal').classList.add('hidden');
    }

    function executeResetAllWork() {
      sectionsArray = [];
      stopLoopingSection();
      renderAllViews();
      localStorage.removeItem('melody_mapper_chromebook_autosave');
      closeConfirmClearModal();
      showToast("All sections and work cleared", "info");
    }


    // --- MODAL CONTROLS ---
    function openShortcutsModal() { document.getElementById('shortcutsModal').classList.remove('hidden'); }
    function closeShortcutsModal() { document.getElementById('shortcutsModal').classList.add('hidden'); }

    function openPrintReportModal() {
      document.getElementById('reportSongTitle').innerText = document.getElementById('songTitleInput').value || 'Song Analysis';
      document.getElementById('reportStudentName').innerText = `Student: ${document.getElementById('studentNameInput').value || 'Alex Rivera'}`;
      document.getElementById('reportDate').innerText = new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' });
      document.getElementById('reportSectionCount').innerText = sectionsArray.length;
      document.getElementById('reportTotalDuration').innerText = formatTime(audioElement.duration || 0);

      const sorted = [...sectionsArray].sort((a, b) => a.startTime - b.startTime);
      document.getElementById('reportFormChain').innerHTML = sorted.map(sec => `
        <span class="px-2 py-0.5 rounded text-white font-bold text-xs shadow" style="background-color: ${sec.color}">${sec.name}</span>
      `).join(' <i class="fa-solid fa-chevron-right text-slate-600 text-[9px]"></i> ');

      document.getElementById('reportRibbonStrip').innerHTML = document.getElementById('sectionRibbonStrip').innerHTML;

      const tbody = document.getElementById('reportTableBody');
      tbody.innerHTML = sorted.map(sec => `
        <tr>
          <td class="p-2 font-bold flex items-center gap-1.5">
            <span class="w-2.5 h-2.5 rounded-full inline-block" style="background-color: ${sec.color}"></span>
            ${sec.name}
          </td>
          <td class="p-2 font-mono">${formatTime(sec.startTime)} - ${formatTime(sec.endTime)}</td>
          <td class="p-2 font-mono">${(sec.endTime - sec.startTime).toFixed(1)}s</td>
          <td class="p-2">${sec.formLabel || sec.name}</td>
          <td class="p-2">${sec.instruments.join(', ') || 'None listed'}</td>
          <td class="p-2">
            <div>${sec.dynamics || '-'}</div>
            <div class="text-[9px] text-slate-400">${sec.texture || ''}</div>
          </td>
          <td class="p-2 italic text-slate-300">${sec.notes || 'No notes added.'}</td>
        </tr>
      `).join('');

      document.getElementById('printReportModal').classList.remove('hidden');
    }

    function closePrintReportModal() { document.getElementById('printReportModal').classList.add('hidden'); }
    function triggerWindowPrint() { window.print(); }

    function openGlossaryModal() { document.getElementById('glossaryModal').classList.remove('hidden'); }
    function closeGlossaryModal() { document.getElementById('glossaryModal').classList.add('hidden'); }

    function openManualSectionModal() {
      editingSectionId = null;
      document.getElementById('modalSectionTitle').innerText = 'Add Manual Section';
      document.getElementById('manualNameInput').value = activeSelectedTag;
      document.getElementById('manualStartInput').value = audioElement.currentTime.toFixed(1);
      document.getElementById('manualEndInput').value = Math.min(audioElement.duration || 60, (audioElement.currentTime + 10)).toFixed(1);
      document.getElementById('manualFormLabelInput').value = activeSelectedTag;
      document.getElementById('manualColorInput').value = activeSelectedColor;
      document.getElementById('manualSectionModal').classList.remove('hidden');
    }

    function openEditSectionModal(secId) {
      editingSectionId = secId;
      const sec = sectionsArray.find(s => s.id === secId);
      if (!sec) return;

      document.getElementById('modalSectionTitle').innerText = 'Edit Section';
      document.getElementById('manualNameInput').value = sec.name;
      document.getElementById('manualStartInput').value = sec.startTime;
      document.getElementById('manualEndInput').value = sec.endTime;
      document.getElementById('manualFormLabelInput').value = sec.formLabel || sec.name;
      document.getElementById('manualColorInput').value = sec.color;
      document.getElementById('manualSectionModal').classList.remove('hidden');
    }

    function closeManualSectionModal() { document.getElementById('manualSectionModal').classList.add('hidden'); }

    function saveManualSectionModal() {
      const name = document.getElementById('manualNameInput').value.trim() || 'Section';
      const start = parseFloat(document.getElementById('manualStartInput').value);
      const end = parseFloat(document.getElementById('manualEndInput').value);
      const formLabel = document.getElementById('manualFormLabelInput').value.trim() || name;
      const color = document.getElementById('manualColorInput').value;

      if (isNaN(start) || isNaN(end) || end <= start) {
        showToast("Invalid timing bounds!", "error");
        return;
      }

      if (editingSectionId) {
        const sec = sectionsArray.find(s => s.id === editingSectionId);
        if (sec) {
          sec.name = name;
          sec.startTime = start;
          sec.endTime = end;
          sec.formLabel = formLabel;
          sec.color = color;
        }
      } else {
        sectionsArray.push({
          id: 'sec_' + Date.now(),
          name: name,
          formLabel: formLabel,
          startTime: start,
          endTime: end,
          color: color,
          instruments: ["Synthesizer"],
          dynamics: "mf (Medium Loud)",
          texture: "Homophonic (Melody + Chords)",
          rhythm: "Driving Beat",
          notes: ""
        });
      }

      closeManualSectionModal();
      renderAllViews();
      autoSaveToLocalStorage();
      showToast("Section saved!", "success");
    }

    function toggleExportMenu() {
      document.getElementById('exportDropdown').classList.toggle('hidden');
    }

    document.addEventListener('click', (e) => {
      const menu = document.getElementById('exportDropdown');
      const btn = e.target.closest('button[onclick="toggleExportMenu()"]');
      if (!btn && menu && !menu.contains(e.target)) {
        menu.classList.add('hidden');
      }
    });

    function toggleTheme() {
      const html = document.documentElement;
      const icon = document.getElementById('themeIcon');
      if (html.classList.contains('dark')) {
        html.classList.remove('dark');
        icon.className = 'fa-solid fa-moon text-indigo-600';
      } else {
        html.classList.add('dark');
        icon.className = 'fa-solid fa-sun text-amber-400';
      }
    }

    function showToast(message, type = 'info') {
      const container = document.getElementById('toastContainer');
      const toast = document.createElement('div');

      const bgMap = {
        info: 'bg-slate-800 border-cyan-500 text-cyan-300',
        success: 'bg-slate-800 border-emerald-500 text-emerald-300',
        error: 'bg-slate-800 border-rose-500 text-rose-300'
      };

      const iconMap = {
        info: 'fa-info-circle',
        success: 'fa-check-circle',
        error: 'fa-exclamation-circle'
      };

      toast.className = `pointer-events-auto border px-3.5 py-2 rounded-xl shadow-xl text-xs font-semibold flex items-center gap-2 transform transition duration-300 translate-y-2 opacity-0 ${bgMap[type] || bgMap.info}`;
      toast.innerHTML = `<i class="fa-solid ${iconMap[type] || iconMap.info}"></i> <span>${message}</span>`;

      container.appendChild(toast);

      requestAnimationFrame(() => {
        toast.classList.remove('translate-y-2', 'opacity-0');
      });

      setTimeout(() => {
        toast.classList.add('opacity-0', 'translate-y-2');
        setTimeout(() => toast.remove(), 300);
      }, 3000);
    }

    function formatTime(seconds) {
      if (isNaN(seconds)) return '00:00.0';
      const mins = Math.floor(seconds / 60);
      const secs = (seconds % 60).toFixed(1);
      const padMins = String(mins).padStart(2, '0');
      const padSecs = String(secs).padStart(4, '0');
      return `${padMins}:${padSecs}`;
    }

    window.onload = () => {
      renderPresetChips();
      renderColorPaletteChips();
      loadSampleTrack('pop');
      restoreFromLocalStorage();
    };
  </script>
</body>
</html>
