<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Рефлексия: море и горизонты</title>
  <style>
    :root{
      --bg: #0b1020;
      --panel: rgba(12, 18, 40, .85);
      --panel2: rgba(255,255,255,.06);
      --stroke: rgba(255,255,255,.18);
      --text: rgba(255,255,255,.92);
      --muted: rgba(255,255,255,.70);
      --accent: #6ee7ff;
    }
    *{ box-sizing: border-box; }
    body{
      margin:0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      background: radial-gradient(1200px 600px at 20% 0%, #1a2a6c22, transparent 60%),
                  radial-gradient(1000px 500px at 80% 20%, #00c6ff18, transparent 55%),
                  var(--bg);
      color: var(--text);
      height: 100vh;
      overflow: hidden;
    }

    .app{
      height: 100vh;
      display: grid;
      grid-template-rows: 1fr 210px;
      gap: 10px;
      padding: 10px;
    }

    /* Canvas */
    .stageWrap{
      position: relative;
      border: 1px solid var(--stroke);
      border-radius: 16px;
      overflow: hidden;
      background: #000;
      box-shadow: 0 20px 50px rgba(0,0,0,.35);
    }

    .stage{
      position: absolute;
      inset: 0;
      background: #000;
      user-select: none;
      touch-action: none;
    }

    .stage img.bg{
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
      filter: saturate(1.05) contrast(1.03);
      pointer-events: none;
      user-select: none;
    }

    .overlay{
      position: absolute;
      inset: 0;
    }

    /* Sticker instance placed on canvas */
    .sticker{
      position: absolute;
      width: 110px;
      height: 110px;
      cursor: grab;
      transform-origin: center center;
      border-radius: 999px;
      user-select: none;
      touch-action: none;
      box-shadow: 0 12px 25px rgba(0,0,0,.35);
    }
    .sticker:active{ cursor: grabbing; }

    .sticker.selected{
      outline: 3px solid rgba(110,231,255,.85);
      outline-offset: 2px;
    }

    /* Top mini HUD */
    .hud{
      position: absolute;
      left: 12px;
      top: 12px;
      right: 12px;
      display: flex;
      gap: 10px;
      align-items: center;
      justify-content: space-between;
      pointer-events: none;
    }

    .hud .card{
      pointer-events: auto;
      background: rgba(10, 15, 35, .55);
      border: 1px solid rgba(255,255,255,.16);
      backdrop-filter: blur(10px);
      border-radius: 14px;
      padding: 10px 12px;
      display: flex;
      gap: 12px;
      align-items: center;
      box-shadow: 0 10px 22px rgba(0,0,0,.25);
    }

    .btn{
      pointer-events: auto;
      border: 1px solid rgba(255,255,255,.18);
      background: rgba(255,255,255,.08);
      color: var(--text);
      padding: 9px 12px;
      border-radius: 12px;
      cursor: pointer;
      font-weight: 600;
    }
    .btn:hover{ border-color: rgba(110,231,255,.55); }
    .btn.danger:hover{ border-color: rgba(255,90,120,.65); }

    .hint{
      font-size: 12px;
      line-height: 1.25;
      color: var(--muted);
    }
    .hint b{ color: var(--text); }

    /* Bottom tray */
    .tray{
      border: 1px solid var(--stroke);
      border-radius: 16px;
      overflow: hidden;
      background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.03));
      box-shadow: 0 18px 40px rgba(0,0,0,.25);
      display: grid;
      grid-template-rows: auto 1fr;
    }

    .trayHeader{
      padding: 10px 12px;
      display: flex;
      gap: 12px;
      align-items: center;
      justify-content: space-between;
      background: rgba(12, 18, 40, .55);
      border-bottom: 1px solid rgba(255,255,255,.12);
    }
    .legend{
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
      font-size: 13px;
      color: var(--muted);
    }
    .pill{
      display: inline-flex;
      gap: 8px;
      align-items: center;
      padding: 7px 10px;
      border-radius: 999px;
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.10);
    }
    .dot{
      width: 10px;
      height: 10px;
      border-radius: 999px;
      background: var(--accent);
      box-shadow: 0 0 0 3px rgba(110,231,255,.15);
    }

    .trayBody{
      display: grid;
      grid-template-columns: 1fr;
      overflow: hidden;
    }

    .scroller{
      overflow-x: auto;
      overflow-y: hidden;
      padding: 12px;
      display: grid;
      grid-auto-flow: column;
      grid-auto-columns: max-content;
      gap: 14px;
      align-items: start;
      scrollbar-color: rgba(110,231,255,.35) rgba(255,255,255,.06);
    }

    .group{
      min-width: 320px;
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(12, 18, 40, .35);
      border-radius: 14px;
      padding: 10px;
    }
    .groupTitle{
      font-weight: 800;
      letter-spacing: .2px;
      margin: 0 0 8px 0;
      font-size: 14px;
    }
    .groupDesc{
      margin: 0 0 10px 0;
      font-size: 12px;
      color: var(--muted);
      line-height: 1.25;
    }

    .grid{
      display: grid;
      grid-template-columns: repeat(10, 48px);
      gap: 8px;
    }

    .thumb{
      width: 48px;
      height: 48px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,.18);
      background: rgba(255,255,255,.06);
      cursor: grab;
      box-shadow: 0 8px 18px rgba(0,0,0,.25);
      overflow: hidden;
      user-select: none;
      touch-action: none;
    }
    .thumb:active{ cursor: grabbing; }
    .thumb img{
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
      pointer-events: none;
      user-select: none;
    }

    /* Small floating help for selected sticker */
    .floatTools{
      position: absolute;
      right: 12px;
      top: 70px;
      width: min(360px, calc(100% - 24px));
      background: rgba(10, 15, 35, .60);
      border: 1px solid rgba(255,255,255,.16);
      border-radius: 14px;
      padding: 10px 12px;
      backdrop-filter: blur(10px);
      display: none;
      pointer-events: auto;
    }
    .floatTools.show{ display: block; }
    .floatTools .row{
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
    }
    .kbd{
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
      font-size: 12px;
      padding: 2px 6px;
      border-radius: 6px;
      background: rgba(255,255,255,.08);
      border: 1px solid rgba(255,255,255,.12);
      color: var(--text);
    }
    .mini{
      font-size: 12px;
      color: var(--muted);
      line-height: 1.25;
      margin-top: 8px;
    }
  </style>
</head>

<body>
  <div class="app">
    <div class="stageWrap" id="stageWrap">
      <div class="stage" id="stage">
        <img class="bg" src="Рефлексия.png" alt="Фон рефлексии" />
        <div class="overlay" id="overlay"></div>

        <div class="hud">
          <div class="card">
            <div>
              <div style="font-weight:900; font-size:14px;">Рефлексия «Горизонт»</div>
              <div class="hint">
                Перетащи значок на картинку. <b>Колёсико</b> — размер, <b>Shift+колёсико</b> — поворот.
                <b>Del</b> — удалить выбранный.
              </div>
            </div>
          </div>
          <div class="card" style="gap:8px;">
            <button class="btn" id="btnClear" title="Убрать все размещённые стикеры">Очистить поле</button>
            <button class="btn" id="btnExport" title="Сохранить результат как картинку (PNG)">Скачать PNG</button>
          </div>
        </div>

        <div class="floatTools" id="floatTools">
          <div class="row">
            <button class="btn danger" id="btnDeleteOne">Удалить</button>
            <button class="btn" id="btnFront">Вперёд</button>
            <button class="btn" id="btnBack">Назад</button>
          </div>
          <div class="mini">
            Подсказка: чтобы «точно поставить», отпусти мышь/палец. Двойной клик по стикеру — тоже удаляет.
          </div>
        </div>

      </div>
    </div>

    <div class="tray">
      <div class="trayHeader">
        <div style="font-weight:900;">Стикеры (перетаскивай вниз → на фон)</div>
        <div class="legend">
          <span class="pill"><span class="dot"></span> Альпинист — «Я на пике! Вижу все взаимосвязи и могу помочь другим»</span>
          <span class="pill"><span class="dot" style="background:#ffd46e; box-shadow:0 0 0 3px rgba(255,212,110,.18)"></span> Корабль — «Я вижу путь, знаю формулы, могу двигаться дальше»</span>
          <span class="pill"><span class="dot" style="background:#7cffb2; box-shadow:0 0 0 3px rgba(124,255,178,.18)"></span> Подлодка — «Я пока погружаюсь в тему, многое скрыто в глубине»</span>
        </div>
      </div>

      <div class="trayBody">
        <div class="scroller" id="scroller"></div>
      </div>
    </div>
  </div>

  <script>
    // ====== CONFIG (файлы) ======
    const ASSETS = {
      climber: { src: "2.png", label: "Альпинист" },
      ship:    { src: "4.png", label: "Корабль" },
      sub:     { src: "sub.png", label: "Подлодка" }
    };

    // Сколько иконок каждого типа внизу:
    const COUNT_EACH = 20;

    // ====== DOM ======
    const stage = document.getElementById("stage");
    const overlay = document.getElementById("overlay");
    const scroller = document.getElementById("scroller");
    const btnClear = document.getElementById("btnClear");
    const btnExport = document.getElementById("btnExport");
    const floatTools = document.getElementById("floatTools");
    const btnDeleteOne = document.getElementById("btnDeleteOne");
    const btnFront = document.getElementById("btnFront");
    const btnBack = document.getElementById("btnBack");

    // ====== Helpers ======
    const clamp = (v, a, b) => Math.max(a, Math.min(b, v));

    function stageRect(){ return stage.getBoundingClientRect(); }

    let selected = null;
    let zCounter = 10;

    function selectSticker(el){
      if(selected && selected !== el) selected.classList.remove("selected");
      selected = el;
      if(selected){
        selected.classList.add("selected");
        floatTools.classList.add("show");
      }else{
        floatTools.classList.remove("show");
      }
    }

    function removeSticker(el){
      if(!el) return;
      if(el === selected) selectSticker(null);
      el.remove();
    }

    function bringFront(el){
      if(!el) return;
      zCounter += 1;
      el.style.zIndex = zCounter;
    }

    function sendBack(el){
      if(!el) return;
      const z = parseInt(el.style.zIndex || "0", 10);
      el.style.zIndex = Math.max(1, z - 5);
    }

    // ====== Create tray groups ======
    function makeGroup(title, desc, key){
      const wrap = document.createElement("div");
      wrap.className = "group";

      const h = document.createElement("div");
      h.className = "groupTitle";
      h.textContent = title;

      const p = document.createElement("div");
      p.className = "groupDesc";
      p.textContent = desc;

      const grid = document.createElement("div");
      grid.className = "grid";

      for(let i=0;i<COUNT_EACH;i++){
        const t = document.createElement("div");
        t.className = "thumb";
        t.dataset.kind = key;
        t.title = title;

        const img = document.createElement("img");
        img.src = ASSETS[key].src;
        img.alt = title;

        t.appendChild(img);
        grid.appendChild(t);

        // Drag from tray -> create new sticker on stage
        t.addEventListener("pointerdown", (e) => {
          e.preventDefault();
          const rect = stageRect();
          const x = e.clientX - rect.left;
          const y = e.clientY - rect.top;
          const s = spawnSticker(key, x, y);
          startDrag(e, s, true);
        });
      }

      wrap.appendChild(h);
      wrap.appendChild(p);
      wrap.appendChild(grid);
      return wrap;
    }

    scroller.appendChild(
      makeGroup(
        "Альпинист",
        "«Я на пике! Вижу все взаимосвязи и могу помочь другим»",
        "climber"
      )
    );
    scroller.appendChild(
      makeGroup(
        "Корабль",
        "«Я вижу путь, знаю формулы, могу двигаться дальше»",
        "ship"
      )
    );
    scroller.appendChild(
      makeGroup(
        "Подводная лодка",
        "«Я пока погружаюсь в тему, многое скрыто в глубине»",
        "sub"
      )
    );

    // ====== Spawn sticker on stage ======
    function spawnSticker(kind, x, y){
      const el = document.createElement("img");
      el.className = "sticker";
      el.src = ASSETS[kind].src;
      el.alt = ASSETS[kind].label;
      el.dataset.kind = kind;

      el.dataset.scale = "1";
      el.dataset.rot = "0";
      el.style.left = (x - 55) + "px";
      el.style.top  = (y - 55) + "px";
      el.style.zIndex = (++zCounter).toString();

      overlay.appendChild(el);
      selectSticker(el);

      // Select / double click remove
      el.addEventListener("pointerdown", (e) => {
        e.stopPropagation();
        selectSticker(el);
        startDrag(e, el, false);
      });

      el.addEventListener("dblclick", (e) => {
        e.stopPropagation();
        removeSticker(el);
      });

      // Wheel: resize, Shift+wheel: rotate
      el.addEventListener("wheel", (e) => {
        e.preventDefault();
        selectSticker(el);
        const scale = parseFloat(el.dataset.scale || "1");
        const rot = parseFloat(el.dataset.rot || "0");

        if(e.shiftKey){
          const nextRot = rot + (e.deltaY > 0 ? 6 : -6);
          el.dataset.rot = nextRot.toString();
        }else{
          const next = clamp(scale + (e.deltaY > 0 ? -0.06 : 0.06), 0.45, 2.2);
          el.dataset.scale = next.toString();
        }
        applyTransform(el);
      }, { passive:false });

      applyTransform(el);
      return el;
    }

    function applyTransform(el){
      const s = parseFloat(el.dataset.scale || "1");
      const r = parseFloat(el.dataset.rot || "0");
      el.style.transform = `rotate(${r}deg) scale(${s})`;
    }

    // ====== Drag logic (Pointer Events) ======
    function startDrag(e, el, justSpawned){
      selectSticker(el);
      bringFront(el);

      const rect = stageRect();
      const startX = e.clientX - rect.left;
      const startY = e.clientY - rect.top;

      const left0 = parseFloat(el.style.left || "0");
      const top0  = parseFloat(el.style.top  || "0");

      el.setPointerCapture(e.pointerId);

      function onMove(ev){
        const r = stageRect();
        const x = ev.clientX - r.left;
        const y = ev.clientY - r.top;
        const dx = x - startX;
        const dy = y - startY;

        // Keep inside stage bounds a bit
        const w = el.getBoundingClientRect().width;
        const h = el.getBoundingClientRect().height;

        const nx = clamp(left0 + dx, -w*0.25, r.width - w*0.75);
        const ny = clamp(top0  + dy, -h*0.25, r.height - h*0.75);

        el.style.left = nx + "px";
        el.style.top  = ny + "px";
      }

      function onUp(ev){
        el.releasePointerCapture(ev.pointerId);
        el.removeEventListener("pointermove", onMove);
        el.removeEventListener("pointerup", onUp);
        el.removeEventListener("pointercancel", onUp);

        // Если случайно "бросили" стикер внизу вне поля — удалим (чтобы не мусорить)
        const r = stageRect();
        const bb = el.getBoundingClientRect();
        if(bb.bottom < r.top || bb.top > r.bottom || bb.right < r.left || bb.left > r.right){
          removeSticker(el);
        }
      }

      el.addEventListener("pointermove", onMove);
      el.addEventListener("pointerup", onUp);
      el.addEventListener("pointercancel", onUp);
    }

    // Click empty space => deselect
    stage.addEventListener("pointerdown", (e) => {
      if(e.target.classList.contains("sticker")) return;
      selectSticker(null);
    });

    // Keyboard delete
    window.addEventListener("keydown", (e) => {
      if(e.key === "Delete" || e.key === "Backspace"){
        if(selected) removeSticker(selected);
      }
      if(e.key === "Escape"){
        selectSticker(null);
      }
    });

    // Buttons
    btnClear.addEventListener("click", () => {
      overlay.querySelectorAll(".sticker").forEach(s => s.remove());
      selectSticker(null);
    });

    btnDeleteOne.addEventListener("click", () => removeSticker(selected));
    btnFront.addEventListener("click", () => bringFront(selected));
    btnBack.addEventListener("click", () => sendBack(selected));

    // ====== Export to PNG (canvas render) ======
    btnExport.addEventListener("click", async () => {
      // Простая отрисовка на canvas (фон + стикеры с трансформациями)
      const r = stageRect();
      const canvas = document.createElement("canvas");
      canvas.width = Math.round(r.width);
      canvas.height = Math.round(r.height);
      const ctx = canvas.getContext("2d");

      // Load images helper
      const load = (src) => new Promise((res, rej) => {
        const im = new Image();
        im.onload = () => res(im);
        im.onerror = rej;
        im.src = src;
      });

      try{
        const bg = await load("Рефлексия.png");
        ctx.drawImage(bg, 0, 0, canvas.width, canvas.height);

        const stickers = Array.from(overlay.querySelectorAll(".sticker"))
          .sort((a,b)=> (parseInt(a.style.zIndex||"0") - parseInt(b.style.zIndex||"0")));

        for(const s of stickers){
          const im = await load(s.src);
          const left = parseFloat(s.style.left||"0");
          const top  = parseFloat(s.style.top||"0");
          const baseSize = 110;

          const scale = parseFloat(s.dataset.scale||"1");
          const rot = parseFloat(s.dataset.rot||"0") * Math.PI/180;

          const size = baseSize * scale;

          // center
          const cx = left + baseSize/2;
          const cy = top  + baseSize/2;

          ctx.save();
          ctx.translate(cx, cy);
          ctx.rotate(rot);
          ctx.translate(-size/2, -size/2);
          ctx.drawImage(im, 0, 0, size, size);
          ctx.restore();
        }

        const a = document.createElement("a");
        a.download = "reflexia.png";
        a.href = canvas.toDataURL("image/png");
        a.click();

      }catch(err){
        alert("Не смог сохранить PNG. Проверь, что все файлы лежат рядом и имена совпадают.\n\n" + err);
      }
    });
  </script>
</body>
</html>
