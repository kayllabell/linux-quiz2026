<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Linux midterm 2026</title>

<style>
:root {
  --bg: #000;
  --panel: rgba(0, 20, 0, 0.85);
  --green: #00ff41;
  --green-dim: #00aa2e;
  --text: #d9ffd9;
  --shadow: 0 0 15px rgba(0, 255, 65, 0.6);
}

body {
  background: black;
  color: var(--text);
  font-family: "Courier New", monospace;
  margin: 0;
  overflow-x: hidden;
}

/* ---------- BOOT LOG ---------- */
#bootLog {
  position: absolute;
  top: 20vh;
  left: 50%;
  transform: translateX(-50%);
  width: 70%;
  background: rgba(0, 0, 0, 0.85);
  border: 1px solid var(--green);
  padding: 20px;
  color: var(--green);
  font-size: 1.1rem;
  box-shadow: var(--shadow);
  border-radius: 8px;
  opacity: 1;
  transition: opacity 1.5s ease;
  white-space: pre-line;
  z-index: 10;
}

/* ---------- QUIZ PANEL ---------- */
#quizContainer {
  max-width: 800px;
  margin: 40px auto;
  background: var(--panel);
  padding: 30px;
  border: 1px solid var(--green);
  border-radius: 10px;
  box-shadow: var(--shadow);
  display: none;
}

.question {
  font-size: 1.4rem;
  margin-bottom: 20px;
  color: var(--green);
  text-shadow: var(--shadow);
}

.options label {
  display: block;
  margin: 10px 0;
  padding: 10px;
  background: rgba(0, 255, 65, 0.05);
  border-left: 3px solid var(--green-dim);
  cursor: pointer;
  transition: 0.2s;
}

.options label:hover {
  background: rgba(0, 255, 65, 0.15);
}

button {
  background: var(--green);
  border: none;
  padding: 12px 25px;
  font-size: 1.1rem;
  cursor: pointer;
  color: black;
  font-weight: bold;
  border-radius: 5px;
  margin-top: 15px;
  box-shadow: var(--shadow);
}

button:hover {
  background: var(--green-dim);
  color: white;
}

#explanation {
  margin-top: 20px;
  padding: 15px;
  border-left: 4px solid var(--green);
  background: rgba(0, 255, 65, 0.1);
  display: none;
}

#resultScreen {
  text-align: center;
  padding-top: 10vh;
  display: none;
}

.score {
  font-size: 2rem;
  color: var(--green);
  text-shadow: var(--shadow);
}
</style>
</head>

<body>

<!-- BOOT LOG -->
<div id="bootLog">
[BOOT] Starting system...
[BOOT] Fetching question modules...
[ERROR] Question 17 attempted to escape containment. Reinforcing firewall...
[WARNING] AI-generated content detected.
[WARNING] If something is wrong, please direct all complaints to /dev/null.
[INFO] Attempting to appear confident...
[INFO] Confidence.exe has stopped responding.
[BOOT] Linux Midterm 2026 loading...
</div>

<!-- QUIZ -->
<div id="quizContainer">
  <div id="questionText" class="question"></div>
  <div id="options" class="options"></div>
  <input id="shortAnswer" style="display:none;width:100%;padding:10px;margin-top:10px;background:black;color:var(--green);border:1px solid var(--green);" placeholder="Type your answer…" />
  <button onclick="submitAnswer()">Submit</button>
  <div id="explanation"></div>
</div>

<!-- RESULTS -->
<div id="resultScreen">
  <div class="score" id="finalScore"></div>
  <p>System: Quiz complete. You have survived… for now.</p>
</div>

<script>
/* ============================================================
   QUESTION BANK
   ============================================================ */

let questions = [
  {q:"Which command schedules a one-time job at a specific time?",type:"multiple-choice",a:["at"],options:["at","cron","sleep","bg"],explanation:"`at` schedules a job at a specific time."},
  {q:"Which command runs a job immediately using the at scheduler?",type:"multiple-choice",a:["at now"],options:["at now","cron now","bg","fg"],explanation:"`at now` executes immediately using `at`."},
  {q:"Which daemon processes at jobs?",type:"multiple-choice",a:["atd"],options:["atd","cron","systemd","rsyslogd"],explanation:"`atd` daemon handles `at` jobs."},
  {q:"Which command removes a scheduled at job?",type:"multiple-choice",a:["atrm"],options:["atrm","atq","rm","kill"],explanation:"`atrm` removes scheduled `at` jobs."},
  {q:"Which command lists pending at jobs?",type:"multiple-choice",a:["atq"],options:["atq","atd","cron","jobs"],explanation:"`atq` lists pending `at` jobs."},
  {q:"Which command is used for pattern scanning and processing?",type:"multiple-choice",a:["awk"],options:["awk","sed","grep","cat"],explanation:"`awk` is for pattern scanning and text processing."},
  {q:"Which command resumes a suspended job in the background?",type:"multiple-choice",a:["bg"],options:["bg","fg","jobs","kill"],explanation:"`bg` resumes a suspended job in the background."},
  {q:"Which command decompresses .bz2 files?",type:"multiple-choice",a:["bunzip2"],options:["bunzip2","bzip2","gzip","gunzip"],explanation:"`bunzip2` decompresses `.bz2` files."},
  {q:"Which command compresses files using the bzip2 algorithm?",type:"multiple-choice",a:["bzip2"],options:["bzip2","bunzip2","gzip","gunzip"],explanation:"`bzip2` compresses files using bzip2."},
  {q:"Which command displays file contents?",type:"multiple-choice",a:["cat"],options:["cat","less","more","head"],explanation:"`cat` outputs file contents."},
  {q:"The command `cd` changes the current directory.",type:"true-false",a:["True"],explanation:"`cd` is used to change directories."},
  {q:"Which command modifies password aging settings?",type:"multiple-choice",a:["chage"],options:["chage","passwd","usermod","chown"],explanation:"`chage` modifies password expiration."},
  {q:"Which command changes a file’s group ownership?",type:"multiple-choice",a:["chgrp"],options:["chgrp","chown","chmod","grpadd"],explanation:"`chgrp` changes the group ownership."},
  {q:"Which command changes file permissions?",type:"multiple-choice",a:["chmod"],options:["chmod","chown","chgrp","umask"],explanation:"`chmod` changes permissions."},
  {q:"Which command changes file ownership?",type:"multiple-choice",a:["chown"],options:["chown","chgrp","chmod","usermod"],explanation:"`chown` changes file ownership."},
  {q:"The daemon crond runs scheduled cron jobs.",type:"true-false",a:["True"],explanation:"`crond` runs cron jobs as scheduled."},
  {q:"Which command edits a user’s cron table?",type:"multiple-choice",a:["crontab"],options:["crontab","cronedit","vim /etc/cron","at"],explanation:"`crontab -e` edits cron jobs for a user."},
  {q:"Which daemon synchronizes system time?",type:"multiple-choice",a:["chronyd"],options:["chronyd","ntpd","systemd","timed"],explanation:"`chronyd` keeps system time synchronized."},
  {q:"Which command displays or sets the system date/time?",type:"multiple-choice",a:["date"],options:["date","time","timedatectl","clock"],explanation:"`date` shows or sets system date/time."},
  {q:"Which command shows disk space usage?",type:"multiple-choice",a:["df"],options:["df","du","lsblk","fdisk"],explanation:"`df` shows disk space usage for mounted filesystems."},
  {q:"Which command queries DNS servers?",type:"multiple-choice",a:["dig"],options:["dig","host","nslookup","ping"],explanation:"`dig` queries DNS records."},
  {q:"The command `sudo` runs commands as root.",type:"true-false",a:["True"],explanation:"`sudo` executes commands with root privileges."},
  {q:"Which command lists directory contents?",type:"multiple-choice",a:["ls"],options:["ls","dir","tree","find"],explanation:"`ls` lists files and directories."},
  {q:"Which command searches text patterns in files?",type:"multiple-choice",a:["grep"],options:["grep","awk","sed","find"],explanation:"`grep` searches text patterns."},
  {q:"The `top` command shows system resource usage in real time.",type:"true-false",a:["True"],explanation:"`top` displays running processes and resource usage."},
  {q:"Which command displays the current working directory?",type:"multiple-choice",a:["pwd"],options:["pwd","cd","ls","whoami"],explanation:"`pwd` prints current working directory."},
  {q:"Which command safely edits sudoers?",type:"multiple-choice",a:["visudo"],options:["visudo","nano /etc/sudoers","sudoedit","vim /etc/sudoers"],explanation:"Always use `visudo` to safely edit sudoers."},
  {q:"The root directory is represented by `/`.",type:"true-false",a:["True"],explanation:"`/` is the root of the filesystem."},
  {q:"Which directory contains user home directories?",type:"multiple-choice",a:["/home"],options:["/home","/root","/usr","/var"],explanation:"User homes are stored in `/home`."},
  {q:"Which directory contains bootloader files?",type:"multiple-choice",a:["/boot"],options:["/boot","/bin","/usr","/etc"],explanation:"`/boot` contains bootloader and kernel files."},
  {q:"Which directory stores temporary mounts?",type:"multiple-choice",a:["/mnt"],options:["/mnt","/media","/tmp","/var"],explanation:"Temporary mounts often go in `/mnt`."},
  {q:"Which command prints text to the terminal?",type:"multiple-choice",a:["echo"],options:["echo","print","cat","write"],explanation:"`echo` prints text to terminal."},
  {q:"Which command decompresses .gz files?",type:"multiple-choice",a:["gunzip"],options:["gunzip","gzip","bunzip2","tar"],explanation:"`gunzip` decompresses `.gz` files."},
  {q:"Which command compresses files using gzip?",type:"multiple-choice",a:["gzip"],options:["gzip","gunzip","bzip2","tar"],explanation:"`gzip` compresses files."},
  {q:"Which command shows who is logged in?",type:"multiple-choice",a:["w"],options:["w","who","users","id"],explanation:"`w` shows logged in users."},
  {q:"Which command creates a new user?",type:"multiple-choice",a:["useradd"],options:["useradd","adduser","usermod","passwd"],explanation:"`useradd` creates a new user account."},
  {q:"The command `rm -rf /` is safe to run on your system.",type:"true-false",a:["False"],explanation:"Never run `rm -rf /` unless you want to destroy everything. 💀"},
  {q:"Which command shows system uptime?",type:"multiple-choice",a:["uptime"],options:["uptime","time","top","date"],explanation:"`uptime` shows how long the system has been running."},
  {q:"Which command shows IP addresses?",type:"multiple-choice",a:["ip addr"],options:["ip addr","ifconfig","ip link","netstat"],explanation:"`ip addr` lists network interfaces and addresses."},
  {q:"Which command connects to a remote system via SSH?",type:"multiple-choice",a:["ssh"],options:["ssh","telnet","rlogin","scp"],explanation:"`ssh` connects to remote systems securely."},
  {q:"Which command transfers files securely?",type:"multiple-choice",a:["sftp"],options:["sftp","scp","ftp","rsync"],explanation:"`sftp` securely transfers files."}
];

/* ============================================================
   QUIZ LOGIC
   ============================================================ */

let index = 0;
let score = 0;

function startQuiz() {
  document.getElementById("quizContainer").style.display = "block";
  loadQuestion();
}

function loadQuestion() {
  const q = questions[index];
  document.getElementById("questionText").innerText = q.q;
  document.getElementById("explanation").style.display = "none";

  const optionsDiv = document.getElementById("options");
  const shortInput = document.getElementById("shortAnswer");

  optionsDiv.innerHTML = "";
  shortInput.style.display = "none";

  if (q.type === "multiple-choice" || q.type === "true-false") {
    q.options.forEach(opt => {
      optionsDiv.innerHTML += `
        <label>
          <input type="radio" name="answer" value="${opt}"> ${opt}
        </label>`;
    });
  }

  if (q.type === "short-answer") {
    shortInput.style.display = "block";
  }
}

function submitAnswer() {
  const q = questions[index];
  let userAnswer;

  if (q.type === "short-answer") {
    userAnswer = document.getElementById("shortAnswer").value.trim().toLowerCase();
    if (userAnswer === q.a[0].toLowerCase()) score++;
  } else {
    const selected = document.querySelector("input[name='answer']:checked");
    if (!selected) return alert("Select an answer first!");
    userAnswer = selected.value;
    if (q.a.includes(userAnswer)) score++;
  }

  document.getElementById("explanation").innerHTML = q.explanation;
  document.getElementById("explanation").style.display = "block";

  setTimeout(() => {
    index++;
    if (index >= questions.length) return endQuiz();
    loadQuestion();
  }, 1200);
}

function endQuiz() {
  document.getElementById("quizContainer").style.display = "none";
  document.getElementById("resultScreen").style.display = "block";
  document.getElementById("finalScore").innerText =
    `You scored ${score} / ${questions.length}`;
}

/* ============================================================
   BOOT LOG FADE + QUIZ START
   ============================================================ */

setTimeout(() => {
  const boot = document.getElementById("bootLog");
  boot.style.opacity = 0;

  setTimeout(() => {
    boot.style.display = "none";
    startQuiz();
  }, 1500);

}, 4000); // 4 seconds before fade
</script>

</body>
</html>
