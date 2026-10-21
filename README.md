# student-performance-analyzer
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>EduPulse | Student Academic Performance & Risk Predictor</title>
  <!-- Tailwind CSS CDN for modern responsive design -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Chart.js CDN for interactive visualizations -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Plus Jakarta Sans', sans-serif; }
  </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen antialiased p-3 sm:p-6 lg:p-8">

  <div class="max-w-6xl mx-auto space-y-6">

    <!-- Header -->
    <header class="bg-slate-900/80 border border-slate-800 rounded-2xl p-5 sm:p-6 backdrop-blur flex flex-col md:flex-row md:items-center justify-between gap-4">
      <div>
        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-semibold bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 mb-2">
          <span>●</span> AI Academic Analytics System
        </div>
        <h1 class="text-2xl sm:text-3xl font-extrabold tracking-tight text-white">Student Performance & Risk Detector</h1>
        <p class="text-sm text-slate-400 mt-1">Multi-factor academic forecasting, early attrition warnings & intervention strategies</p>
      </div>

      <!-- Quick Preset Buttons -->
      <div class="flex flex-wrap items-center gap-2">
        <span class="text-xs text-slate-400 font-medium">Quick Presets:</span>
        <button onclick="loadPreset('topper')" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-700 transition">🌟 Topper</button>
        <button onclick="loadPreset('average')" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-700 transition">⚖️ Average</button>
        <button onclick="loadPreset('distracted')" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-700 transition">📱 Distracted</button>
        <button onclick="loadPreset('atrisk')" class="text-xs bg-rose-950/60 hover:bg-rose-900/60 text-rose-300 px-3 py-1.5 rounded-lg border border-rose-800/50 transition">⚠️ At Risk</button>
      </div>
    </header>

    <!-- Main Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">

      <!-- Left Column: Input Form (5 cols) -->
      <div class="lg:col-span-5 bg-slate-900/80 border border-slate-800 rounded-2xl p-5 sm:p-6 space-y-5">
        <div class="flex items-center justify-between border-b border-slate-800 pb-3">
          <h2 class="text-lg font-bold text-white flex items-center gap-2">
            <span>⚙️</span> Student Metrics Input
          </h2>
          <span class="text-xs text-indigo-400 font-medium">Dynamic Simulator</span>
        </div>

        <form id="metricsForm" class="space-y-4" onsubmit="event.preventDefault(); recalculate();">
          
          <!-- Previous Score -->
          <div>
            <div class="flex justify-between text-xs font-semibold mb-1">
              <label for="priorScore" class="text-slate-300">Previous Exam Score (%)</label>
              <span id="priorScoreVal" class="text-indigo-400">75%</span>
            </div>
            <input type="range" id="priorScore" min="20" max="100" value="75" step="1" 
                   class="w-full accent-indigo-500 cursor-pointer" oninput="updateValues(); recalculate();">
          </div>

          <!-- Attendance -->
          <div>
            <div class="flex justify-between text-xs font-semibold mb-1">
              <label for="attendance" class="text-slate-300">School / Class Attendance (%)</label>
              <span id="attendanceVal" class="text-indigo-400">82%</span>
            </div>
            <input type="range" id="attendance" min="30" max="100" value="82" step="1" 
                   class="w-full accent-indigo-500 cursor-pointer" oninput="updateValues(); recalculate();">
            <p class="text-[11px] text-slate-500 mt-0.5">⚠️ Below 75% incurs critical attendance risk penalties</p>
          </div>

          <!-- Study Hours -->
          <div>
            <div class="flex justify-between text-xs font-semibold mb-1">
              <label for="studyHours" class="text-slate-300">Daily Self-Study (Hours/Day)</label>
              <span id="studyHoursVal" class="text-indigo-400">3.5 hrs</span>
            </div>
            <input type="range" id="studyHours" min="0" max="10" value="3.5" step="0.5" 
                   class="w-full accent-indigo-500 cursor-pointer" oninput="updateValues(); recalculate();">
          </div>

          <!-- Assignment Completion -->
          <div>
            <div class="flex justify-between text-xs font-semibold mb-1">
              <label for="assignmentRate" class="text-slate-300">Assignment Completion Rate (%)</label>
              <span id="assignmentRateVal" class="text-indigo-400">85%</span>
            </div>
            <input type="range" id="assignmentRate" min="20" max="100" value="85" step="1" 
                   class="w-full accent-indigo-500 cursor-pointer" oninput="updateValues(); recalculate();">
          </div>

          <!-- Sleep Hours -->
          <div>
            <div class="flex justify-between text-xs font-semibold mb-1">
              <label for="sleepHours" class="text-slate-300">Average Sleep (Hours/Night)</label>
              <span id="sleepHoursVal" class="text-indigo-400">7.0 hrs</span>
            </div>
            <input type="range" id="sleepHours" min="3" max="11" value="7" step="0.5" 
                   class="w-full accent-indigo-500 cursor-pointer" oninput="updateValues(); recalculate();">
          </div>

          <!-- Non-Academic Screen Time -->
          <div>
            <div class="flex justify-between text-xs font-semibold mb-1">
              <label for="screenTime" class="text-slate-300">Recreational Screen Time (Hours/Day)</label>
              <span id="screenTimeVal" class="text-indigo-400">2.5 hrs</span>
            </div>
            <input type="range" id="screenTime" min="0" max="10" value="2.5" step="0.5" 
                   class="w-full accent-indigo-500 cursor-pointer" oninput="updateValues(); recalculate();">
          </div>

          <button type="button" onclick="recalculate()" class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl transition shadow-lg shadow-indigo-600/30">
            ⚡ Run Diagnostic Prediction
          </button>
        </form>
      </div>

      <!-- Right Column: Predictive Analytics & Outputs (7 cols) -->
      <div class="lg:col-span-7 space-y-6">

        <!-- Result Cards -->
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
          
          <!-- Predicted Score -->
          <div class="bg-slate-900/80 border border-slate-800 rounded-2xl p-5 flex flex-col justify-between">
            <span class="text-xs font-semibold text-slate-400">Predicted Score</span>
            <div class="mt-2">
              <span id="predScoreDisplay" class="text-4xl font-extrabold text-white">78.4%</span>
              <p id="gradeBadge" class="text-xs font-bold text-emerald-400 mt-1">Grade: A (Very Good)</p>
            </div>
          </div>

          <!-- Risk Category -->
          <div class="bg-slate-900/80 border border-slate-800 rounded-2xl p-5 flex flex-col justify-between">
            <span class="text-xs font-semibold text-slate-400">Academic Risk Tier</span>
            <div class="mt-2">
              <div id="riskBadge" class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-extrabold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                <span class="w-2 h-2 rounded-full bg-emerald-400"></span> LOW RISK
              </div>
              <p id="riskDesc" class="text-xs text-slate-400 mt-2">Stable academic trajectory</p>
            </div>
          </div>

          <!-- Intervention Urgency -->
          <div class="bg-slate-900/80 border border-slate-800 rounded-2xl p-5 flex flex-col justify-between">
            <span class="text-xs font-semibold text-slate-400">Intervention Urgency</span>
            <div class="mt-2">
              <span id="urgencyDisplay" class="text-2xl font-bold text-slate-200">Routine Check</span>
              <p class="text-xs text-slate-400 mt-1">Mentor support level</p>
            </div>
          </div>
        </div>

        <!-- Chart Section -->
        <div class="bg-slate-900/80 border border-slate-800 rounded-2xl p-5">
          <div class="flex items-center justify-between mb-4">
            <h3 class="text-sm font-bold text-slate-200">📊 Multi-Factor Academic Profile Radar</h3>
            <span class="text-xs text-slate-500">Benchmark vs. Student</span>
          </div>
          <div class="w-full h-64">
            <canvas id="performanceChart"></canvas>
          </div>
        </div>

        <!-- Prescriptive Action Plan -->
        <div class="bg-slate-900/80 border border-slate-800 rounded-2xl p-5 space-y-3">
          <h3 class="text-sm font-bold text-slate-200 flex items-center gap-2">
            <span>🎯</span> Recommended Personalized Interventions
          </h3>
          <ul id="recommendationsList" class="space-y-2 text-xs text-slate-300">
            <!-- Populated dynamically -->
          </ul>
        </div>

      </div>

    </div>

    <!-- Footer -->
    <footer class="text-center text-xs text-slate-600 border-t border-slate-900 pt-4">
      EduPulse Academic Performance & Risk Diagnostic System • Capstone Demonstration
    </footer>

  </div>

  <script>
    // Update live value displays on slider move
    function updateValues() {
      document.getElementById('priorScoreVal').innerText = document.getElementById('priorScore').value + '%';
      document.getElementById('attendanceVal').innerText = document.getElementById('attendance').value + '%';
      document.getElementById('studyHoursVal').innerText = document.getElementById('studyHours').value + ' hrs';
      document.getElementById('assignmentRateVal').innerText = document.getElementById('assignmentRate').value + '%';
      document.getElementById('sleepHoursVal').innerText = document.getElementById('sleepHours').value + ' hrs';
      document.getElementById('screenTimeVal').innerText = document.getElementById('screenTime').value + ' hrs';
    }

    // Chart.js Radar Instance
    let chartInstance = null;

    function initOrUpdateChart(studentMetrics) {
      const ctx = document.getElementById('performanceChart').getContext('2d');
      const benchmarkData = [75, 80, 70, 75, 85, 80]; // standard target

      if (chartInstance) {
        chartInstance.data.datasets[0].data = studentMetrics;
        chartInstance.update();
        return;
      }

      chartInstance = new Chart(ctx, {
        type: 'radar',
        data: {
          labels: ['Prior Scores', 'Attendance', 'Daily Study', 'Homework Done', 'Sleep Health', 'Focus Control'],
          datasets: [
            {
              label: 'Student Profile',
              data: studentMetrics,
              backgroundColor: 'rgba(99, 102, 241, 0.25)',
              borderColor: '#818cf8',
              borderWidth: 2,
              pointBackgroundColor: '#818cf8'
            },
            {
              label: 'Healthy Benchmark',
              data: benchmarkData,
              backgroundColor: 'rgba(52, 211, 153, 0.1)',
              borderColor: 'rgba(52, 211, 153, 0.4)',
              borderWidth: 1.5,
              borderDash: [4, 4]
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            r: {
              angleLines: { color: 'rgba(255, 255, 255, 0.08)' },
              grid: { color: 'rgba(255, 255, 255, 0.08)' },
              pointLabels: { color: '#94a3b8', font: { size: 11 } },
              suggestedMin: 20,
              suggestedMax: 100,
              ticks: { display: false }
            }
          },
          plugins: {
            legend: {
              labels: { color: '#cbd5e1', font: { size: 11 } }
            }
          }
        }
      });
    }

    // Mathematical Performance Prediction Algorithm
    function recalculate() {
      const prior = parseFloat(document.getElementById('priorScore').value);
      const att = parseFloat(document.getElementById('attendance').value);
      const study = parseFloat(document.getElementById('studyHours').value);
      const assign = parseFloat(document.getElementById('assignmentRate').value);
      const sleep = parseFloat(document.getElementById('sleepHours').value);
      const screen = parseFloat(document.getElementById('screenTime').value);

      // Core ML Simulation Weights
      let predicted = (prior * 0.35) + (att * 0.25) + (assign * 0.20);

      // Study hours curve (diminishing return after 5 hrs)
      let studyBonus = Math.min(study * 3.5, 18);
      predicted += studyBonus;

      // Penalties & Dampeners
      if (att < 75) {
        predicted -= (75 - att) * 0.5; // Steep attendance penalty
      }
      if (sleep < 6) {
        predicted -= (6 - sleep) * 2.5; // Fatigue factor
      }
      if (screen > 3.5) {
        predicted -= (screen - 3.5) * 2.0; // Distraction drag
      }

      // Clamp between 10% and 99.5%
      predicted = Math.min(Math.max(predicted, 12), 99.4);
      predicted = Math.round(predicted * 10) / 10;

      // Display score
      document.getElementById('predScoreDisplay').innerText = predicted.toFixed(1) + '%';

      // Grade classification
      let grade = 'A+ (Outstanding)';
      let gradeClass = 'text-emerald-400';
      if (predicted < 50) { grade = 'F (Remedial Required)'; gradeClass = 'text-rose-400'; }
      else if (predicted < 60) { grade = 'D (Passing)'; gradeClass = 'text-amber-400'; }
      else if (predicted < 70) { grade = 'C (Satisfactory)'; gradeClass = 'text-amber-300'; }
      else if (predicted < 80) { grade = 'B (Good)'; gradeClass = 'text-blue-400'; }
      else if (predicted < 90) { grade = 'A (Very Good)'; gradeClass = 'text-emerald-400'; }
      
      const gradeBadge = document.getElementById('gradeBadge');
      gradeBadge.innerText = 'Grade: ' + grade;
      gradeBadge.className = 'text-xs font-bold mt-1 ' + gradeClass;

      // Risk level classification
      const riskBadge = document.getElementById('riskBadge');
      const riskDesc = document.getElementById('riskDesc');
      const urgencyDisplay = document.getElementById('urgencyDisplay');

      if (predicted < 55 || att < 65) {
        riskBadge.innerHTML = '<span class="w-2 h-2 rounded-full bg-rose-400"></span> CRITICAL RISK';
        riskBadge.className = 'inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-extrabold bg-rose-500/10 text-rose-400 border border-rose-500/20';
        riskDesc.innerText = 'High probability of course failure / detention';
        urgencyDisplay.innerText = 'Immediate Action';
        urgencyDisplay.className = 'text-2xl font-bold text-rose-400';
      } else if (predicted < 72 || att < 75) {
        riskBadge.innerHTML = '<span class="w-2 h-2 rounded-full bg-amber-400"></span> MODERATE RISK';
        riskBadge.className = 'inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-extrabold bg-amber-500/10 text-amber-400 border border-amber-500/20';
        riskDesc.innerText = 'Underperformance warning flag';
        urgencyDisplay.innerText = 'Targeted Guidance';
        urgencyDisplay.className = 'text-2xl font-bold text-amber-400';
      } else {
        riskBadge.innerHTML = '<span class="w-2 h-2 rounded-full bg-emerald-400"></span> LOW RISK';
        riskBadge.className = 'inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-extrabold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20';
        riskDesc.innerText = 'Consistent on-track academic pace';
        urgencyDisplay.innerText = 'Routine Check';
        urgencyDisplay.className = 'text-2xl font-bold text-emerald-400';
      }

      // Actionable Recommendations
      const recs = [];
      if (att < 75) {
        recs.push('⚠️ Attendance is below 75%: Prioritize attending all regular class hours to eliminate disqualification risk.');
      }
      if (study < 2.5) {
        recs.push('📚 Study deficit: Introduce two dedicated 45-minute distraction-free evening self-study blocks.');
      }
      if (sleep < 6.5) {
        recs.push('🛌 Cognitive fatigue warning: Restore sleep to 7-8 hours to improve memory consolidation.');
      }
      if (screen > 3.5) {
        recs.push('📵 High screen distraction: Cap leisure phone/screen usage under 2 hours during exam cycles.');
      }
      if (assign < 75) {
        recs.push('📝 Assignment backlog: Complete remaining practicals and homework sheets to safeguard internal marks.');
      }
      if (recs.length === 0) {
        recs.push('✨ Student is executing a high-efficiency academic routine. Maintain current habits and focus on revision test series.');
      }

      const recsList = document.getElementById('recommendationsList');
      recsList.innerHTML = recs.map(r => `<li class="p-2 rounded bg-slate-800/60 border border-slate-700/50 flex items-start gap-2"><span>•</span><span>${r}</span></li>`).join('');

      // Normalize metrics for Radar (0-100 scale)
      const studyNorm = Math.min((study / 6) * 100, 100);
      const sleepNorm = Math.min((sleep / 8) * 100, 100);
      const screenNorm = Math.max(100 - (screen * 12), 10);
      
      initOrUpdateChart([prior, att, studyNorm, assign, sleepNorm, screenNorm]);
    }

    // Preset configurations
    function loadPreset(type) {
      if (type === 'topper') {
        setInputs(88, 92, 5.0, 95, 7.5, 1.5);
      } else if (type === 'average') {
        setInputs(72, 80, 3.0, 78, 7.0, 3.0);
      } else if (type === 'distracted') {
        setInputs(68, 76, 1.5, 60, 5.5, 6.0);
      } else if (type === 'atrisk') {
        setInputs(44, 58, 1.0, 45, 5.0, 5.5);
      }
      updateValues();
      recalculate();
    }

    function setInputs(prior, att, study, assign, sleep, screen) {
      document.getElementById('priorScore').value = prior;
      document.getElementById('attendance').value = att;
      document.getElementById('studyHours').value = study;
      document.getElementById('assignmentRate').value = assign;
      document.getElementById('sleepHours').value = sleep;
      document.getElementById('screenTime').value = screen;
    }

    // Run automatically on load
    window.addEventListener('DOMContentLoaded', () => {
      updateValues();
      recalculate();
    });
  </script>
</body>
</html>
