<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Radio Active Drive</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
      background-color: #f8f9fa;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
      color: #333;
    }
    
    .player {
      background: white;
      padding: 30px 25px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
      text-align: center;
      width: 100%;
      max-width: 360px;
    }
    
    .station-info {
      margin-bottom: 24px;
    }
    
    h1 {
      font-size: 1.5rem;
      font-weight: 600;
      margin-bottom: 8px;
      color: #222;
    }
    
    .station-name {
      font-size: 1rem;
      color: #666;
      margin: 12px 0;
      height: 24px;
    }
    
    select {
      width: 100%;
      padding: 10px 12px;
      border-radius: 6px;
      border: 1px solid #ddd;
      background-color: white;
      font-size: 14px;
      margin: 8px 0 16px;
      appearance: none;
      background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23333' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
      background-repeat: no-repeat;
      background-position: right 12px center;
      background-size: 16px;
    }
    
    .controls {
      display: flex;
      justify-content: center;
      gap: 12px;
      margin: 20px 0;
    }
    
    button {
      background: #f1f3f4;
      color: #333;
      border: none;
      padding: 12px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 16px;
      width: 50px;
      height: 50px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
    }
    
    button:hover {
      background: #e8eaed;
    }
    
    button:active {
      transform: scale(0.96);
    }
    
    button:disabled {
      background: #f8f9fa;
      color: #aaa;
      cursor: not-allowed;
      transform: none;
    }
    
    #playBtn {
      background: #333;
      color: white;
    }
    
    #playBtn:hover {
      background: #555;
    }
    
    #playBtn:disabled {
      background: #e0e0e0;
      color: #aaa;
    }
    
    .volume-control {
      display: flex;
      align-items: center;
      gap: 12px;
      margin: 20px 0;
    }
    
    input[type="range"] {
      flex: 1;
      height: 4px;
      border-radius: 2px;
      background: #e0e0e0;
      outline: none;
      -webkit-appearance: none;
    }
    
    input[type="range"]::-webkit-slider-thumb {
      appearance: none;
      width: 16px;
      height: 16px;
      border-radius: 50%;
      background: #333;
      cursor: pointer;
    }
    
    input[type="range"]::-moz-range-thumb {
      width: 16px;
      height: 16px;
      border-radius: 50%;
      background: #333;
      cursor: pointer;
      border: none;
    }
    
    #volDisplay {
      min-width: 36px;
      font-size: 14px;
      color: #666;
    }
    
    .status {
      margin-top: 16px;
      padding: 8px 12px;
      border-radius: 6px;
      font-size: 14px;
      background-color: #f8f9fa;
      color: #666;
    }
    
    .progress-container {
      width: 100%;
      height: 3px;
      background-color: #e9ecef;
      border-radius: 2px;
      margin: 16px 0 8px;
      overflow: hidden;
    }
    
    .progress-bar {
      height: 100%;
      background-color: #333;
      width: 0%;
      transition: width 0.3s ease;
    }
    
    .frequency-display {
      font-size: 14px;
      color: #888;
      margin-top: 8px;
    }
    
    footer {
      text-align: center;
      padding: 10px;
      background-color: #f8f9fa;
      color: #666;
      font-size: 14px;
      margin-top: 10px;
    }
    
    .heart {
      color: #ff0000; /* Red color for the heart */
    }
  </style>
</head>
<body>
  <div class="player">
    <div class="station-info">
      <h1>RadioActive Drive</h1>
      <div class="station-name" id="stationName">Select a station</div>
      <label for="stationSelect">Station</label>
      <select id="stationSelect">
        <option value="https://eu8.fastcast4u.com/proxy/clyedupq?mp=%2F1" data-name="Classic Rock FM" data-frequency="98.5">Classic Rock FM (98.5)</option>
        <option value="https://stream-uk1.radioparadise.com/aac-320" data-name="Radio Paradise" data-frequency="101.3">Radio Paradise (101.3)</option>
        <option value="https://us1.internet-radio.com/proxy/abcd?mp=/stream" data-name="Chillout Lounge" data-frequency="89.7">Chillout Lounge (89.7)</option>
        <option value="https://icecast.rtl.fr/rtl-1-44-128" data-name="RTL Radio" data-frequency="102.5">RTL Radio (102.5)</option>
      </select>
    </div>

    <div class="progress-container" id="progressContainer">
      <div class="progress-bar" id="progressBar"></div>
    </div>

    <div class="controls">
      <button id="stopBtn" disabled title="Stop">■</button>
      <button id="playBtn" title="Play">▶</button>
      <button id="pauseBtn" disabled title="Pause">❚❚</button>
    </div>

    <div class="volume-control">
      <span style="font-size: 14px; color: #888;">Vol</span>
      <input type="range" id="volume" min="0" max="1" step="0.1" value="0.5">
      <span id="volDisplay">50%</span>
    </div>

    <div class="frequency-display" id="frequencyDisplay">--.-- FM</div>

    <div class="status" id="status">Ready to play</div>
  </div>

  <footer>
    <p>Made with <span class="heart">❤</span> By Armeen</p>
  </footer>

  <script>
    const audio = new Audio();
    const playBtn = document.getElementById('playBtn');
    const pauseBtn = document.getElementById('pauseBtn');
    const stopBtn = document.getElementById('stopBtn');
    const volumeSlider = document.getElementById('volume');
    const status = document.getElementById('status');
    const volDisplay = document.getElementById('volDisplay');
    const stationSelect = document.getElementById('stationSelect');
    const stationName = document.getElementById('stationName');
    const progressContainer = document.getElementById('progressContainer');
    const progressBar = document.getElementById('progressBar');
    const frequencyDisplay = document.getElementById('frequencyDisplay');

    // Set initial volume
    audio.volume = 0.5;

    // Update station name and frequency when selection changes
    function updateStationInfo() {
      const selectedOption = stationSelect.options[stationSelect.selectedIndex];
      stationName.textContent = selectedOption.getAttribute('data-name');
      frequencyDisplay.textContent = selectedOption.getAttribute('data-frequency') + ' FM';
    }

    // Set initial station info
    updateStationInfo();

    // Volume control
    volumeSlider.addEventListener('input', (e) => {
      audio.volume = e.target.value;
      volDisplay.textContent = Math.round(audio.volume * 100) + '%';
    });

    // Play button
    playBtn.addEventListener('click', () => {
      // If no source is set, use the selected station
      if (!audio.src) {
        audio.src = stationSelect.value;
      }
      
      audio.play().then(() => {
        playBtn.disabled = true;
        pauseBtn.disabled = false;
        stopBtn.disabled = false;
        status.textContent = 'Playing';
      }).catch((error) => {
        status.textContent = 'Error: Could not play. Check stream URL.';
        console.error('Playback error:', error);
      });
    });

    // Pause button
    pauseBtn.addEventListener('click', () => {
      audio.pause();
      playBtn.disabled = false;
      pauseBtn.disabled = true;
      status.textContent = 'Paused';
    });

    // Stop button
    stopBtn.addEventListener('click', () => {
      audio.pause();
      audio.currentTime = 0;
      playBtn.disabled = false;
      pauseBtn.disabled = true;
      stopBtn.disabled = true;
      status.textContent = 'Stopped';
      progressBar.style.width = '0%';
    });

    // Station change
    stationSelect.addEventListener('change', (e) => {
      audio.src = e.target.value;
      updateStationInfo();
      
      // If already playing, automatically switch to new station
      if (!pauseBtn.disabled) {
        audio.play().then(() => {
          status.textContent = 'Playing new station...';
        }).catch((error) => {
          status.textContent = 'Error: Could not play station.';
          console.error('Station error:', error);
        });
      } else {
        status.textContent = 'Station changed';
      }
    });

    // Update status and progress
    audio.addEventListener('loadstart', () => {
      status.textContent = 'Loading...';
      progressBar.style.width = '0%';
    });
    
    audio.addEventListener('progress', () => {
      if (audio.buffered.length > 0) {
        const bufferedPercent = (audio.buffered.end(0) / audio.duration) * 100;
        progressBar.style.width = bufferedPercent + '%';
      }
    });
    
    audio.addEventListener('canplay', () => {
      status.textContent = 'Ready to play';
    });
    
    audio.addEventListener('waiting', () => {
      status.textContent = 'Buffering...';
    });
    
    audio.addEventListener('playing', () => {
      status.textContent = 'Playing';
    });
    
    audio.addEventListener('error', (e) => {
      status.textContent = 'Stream error. Try another station.';
      console.error('Audio error:', e);
    });
    
    audio.addEventListener('ended', () => {
      status.textContent = 'Playback ended';
      playBtn.disabled = false;
      pauseBtn.disabled = true;
      stopBtn.disabled = true;
    });
  </script>
</body>
</html># RadioActive_Drive
