<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="theme-color" content="#0b1422" />
  <title>Finleigh Friday Bingo</title>
  <style>
    :root{
      --bg:#08111d;
      --panel:#f2efe7;
      --ink:#0e1b2d;
      --gold:#c99a32;
      --line:#17263a;
      --green:#18864a;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      min-height:100vh;
      font-family:Arial, Helvetica, sans-serif;
      background:
        radial-gradient(circle at top, #13243b 0%, var(--bg) 48%, #050b12 100%);
      color:white;
      padding:20px;
    }
    .wrap{
      width:min(1100px,100%);
      margin:0 auto;
    }
    .hero{
      text-align:center;
      margin-bottom:18px;
    }
    h1{
      margin:0;
      font-size:clamp(2.2rem,7vw,5rem);
      letter-spacing:.04em;
      color:#f7f3e8;
      text-shadow:0 2px 0 var(--gold);
    }
    .subtitle{
      margin:.35rem 0 0;
      color:#d2b266;
      letter-spacing:.28em;
      font-weight:700;
      font-size:clamp(1rem,3vw,1.6rem);
    }
    .controls{
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:12px;
      flex-wrap:wrap;
      margin:14px 0;
    }
    #status{color:#d8dde5;font-size:.95rem}
    button.reset{
      border:1px solid #53637a;
      background:#14243a;
      color:white;
      border-radius:10px;
      padding:10px 14px;
      font-weight:700;
      cursor:pointer;
    }
    .letters{
      display:grid;
      grid-template-columns:repeat(5,1fr);
      gap:6px;
      margin-bottom:6px;
      text-align:center;
      color:var(--gold);
      font-size:clamp(1.3rem,4vw,2.2rem);
      font-weight:900;
      letter-spacing:.18em;
    }
    .board{
      display:grid;
      grid-template-columns:repeat(5,1fr);
      gap:6px;
    }
    .square{
      min-height:145px;
      border:2px solid var(--line);
      border-radius:10px;
      background:var(--panel);
      color:var(--ink);
      padding:12px 9px;
      font-size:clamp(.7rem,1.7vw,1rem);
      line-height:1.15;
      font-weight:800;
      cursor:pointer;
      display:flex;
      flex-direction:column;
      justify-content:center;
      align-items:center;
      text-align:center;
      transition:.15s ease;
      user-select:none;
      -webkit-tap-highlight-color:transparent;
    }
    .square:active{transform:scale(.98)}
    .square.checked{
      background:var(--green);
      color:white;
      border-color:#52c77d;
    }
    .check{
      display:none;
      font-size:2.5rem;
      line-height:1;
      margin-bottom:7px;
    }
    .checked .check{display:block}
    .footer{
      text-align:center;
      color:#d2b266;
      margin-top:18px;
      font-weight:700;
      letter-spacing:.12em;
      font-size:.8rem;
    }
    @media (max-width:700px){
      body{padding:10px}
      .board,.letters{gap:4px}
      .square{min-height:112px;padding:7px 4px;border-radius:7px}
    }
    @media (max-width:420px){
      .square{min-height:100px;font-size:.59rem}
      .check{font-size:1.8rem}
    }
  </style>
</head>
<body>
  <main class="wrap">
    <header class="hero">
      <h1>FINLEIGH FRIDAY</h1>
      <div class="subtitle">BINGO</div>
    </header>

    <div class="controls">
      <div id="status" aria-live="polite">0 of 25 squares checked.</div>
      <button class="reset" id="reset" type="button">Reset Board</button>
    </div>

    <div class="letters" aria-hidden="true">
      <div>B</div><div>I</div><div>N</div><div>G</div><div>O</div>
    </div>

    <section class="board" id="board" aria-label="Finleigh Friday bingo board"></section>

    <div class="footer">ONE NIGHT. ONE BROTHERHOOD. ENDLESS STORIES.</div>
  </main>

  <script>
    const squares = [
      "Propeller Kid",
      "Speech Impediment",
      "Star Wars Lightsaber",
      "Someone Random Has a FaceTime Convo with Micah",
      "That One Fat Kid",
      "Jacob’s Man Looking Ex",
      "Someone Over 40",
      "Make a Fake Finleigh Story That Lasts 5+ Minutes",
      "Hear Someone Say “Finleigh”",
      "Convince Someone Finleigh Is Here",
      "Someone Asks “Where’s Finleigh?”",
      "“No Hands” Gets Played",
      "Make a Joke No One Laughs At to 5+ People",
      "Andy Has to Deal with a Drunk",
      "Dance Floor Has Fewer Than 3 Girls",
      "Someone Wearing Cargo Shorts",
      "Someone Brings Up Elon Musk",
      "Someone Quotes Star Wars",
      "Chase Reference",
      "Someone Mentions Their GPA",
      "Hear the N Word",
      "Someone Brings Up Abdul",
      "Someone Talks About Robotics",
      "Make a Fake Story with Quigley That Lasts 5 Minutes",
      "5+ Drinks Before 12"
    ];

    const board = document.getElementById("board");
    const status = document.getElementById("status");
    const reset = document.getElementById("reset");

    function updateStatus(){
      const checked = document.querySelectorAll(".square.checked").length;
      status.textContent = `${checked} of ${squares.length} squares checked.`;
    }

    squares.forEach((text) => {
      const btn = document.createElement("button");
      btn.type = "button";
      btn.className = "square";
      btn.setAttribute("aria-pressed","false");

      const check = document.createElement("div");
      check.className = "check";
      check.textContent = "✓";

      const label = document.createElement("div");
      label.textContent = text;

      btn.append(check,label);

      btn.addEventListener("click", () => {
        const active = btn.classList.toggle("checked");
        btn.setAttribute("aria-pressed", active ? "true" : "false");
        updateStatus();
      });

      board.appendChild(btn);
    });

    reset.addEventListener("click", () => {
      document.querySelectorAll(".square").forEach((btn) => {
        btn.classList.remove("checked");
        btn.setAttribute("aria-pressed","false");
      });
      updateStatus();
    });
  </script>
</body>
</html>
