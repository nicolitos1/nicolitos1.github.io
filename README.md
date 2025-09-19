# nicolitos1.github.io
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculador: Calorias → Gramas</title>
  <style>
    body{font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;margin:24px;background:#f6f8fb;color:#111}
    .card{max-width:520px;margin:0 auto;padding:20px;background:#fff;border-radius:10px;box-shadow:0 6px 18px rgba(2,6,23,0.08)}
    h1{font-size:1.2rem;margin:0 0 12px}
    label{display:block;margin-top:10px;font-weight:600;font-size:0.9rem}
    input[type="number"], select{width:100%;padding:10px;border:1px solid #e3e7ee;border-radius:8px;margin-top:6px;font-size:1rem}
    .row{display:flex;gap:8px}
    .row > *{flex:1}
    button{margin-top:12px;padding:10px 12px;border:none;border-radius:8px;background:#1f7aeb;color:#fff;font-weight:700;cursor:pointer}
    .result{margin-top:14px;padding:12px;border-radius:8px;background:#f1f7ff;border:1px solid #dceeff}
    .small{font-size:0.85rem;color:#555;margin-top:8px}
    .muted{color:#666;font-size:0.88rem}
  </style>
</head>
<body>
  <main class="card" role="main">
    <h1>Calculador — Calorias → Gramas de gordura</h1>

    <label for="calorias">Calorias queimadas (kcal)</label>
    <input id="calorias" type="number" min="0" step="any" placeholder="Ex: 20" value="20" />

    <label for="metodo">Base de conversão</label>
    <select id="metodo">
      <option value="7.7">Gordura corporal (1 g ≈ 7,7 kcal) — padrão</option>
      <option value="9">Gordura pura (1 g = 9 kcal)</option>
      <option value="4">Carboidrato/Proteína (1 g = 4 kcal)</option>
    </select>

    <div class="row">
      <button id="calcular">Calcular</button>
      <button id="limpar" type="button" style="background:#e2e6ef;color:#0a2b6b;font-weight:700">Limpar</button>
    </div>

    <div id="resultado" class="result" aria-live="polite">
      Insira um valor e clique em <strong>Calcular</strong>.
    </div>

    <div class="small muted">
      Observação: valores são estimativas. 1 g de gordura corporal costuma ser aproximado por 7,7 kcal devido ao conteúdo não-lipídico (água, proteínas).
    </div>
  </main>

  <script>
    const elCal = document.getElementById('calorias');
    const elMet = document.getElementById('metodo');
    const btn = document.getElementById('calcular');
    const btnLimpar = document.getElementById('limpar');
    const out = document.getElementById('resultado');

    function formatNumber(n){
      if (Math.abs(n) >= 1000) return n.toLocaleString(undefined, {maximumFractionDigits:2});
      return Number(n.toFixed(2));
    }

    function calcular(){
      const kcal = parseFloat(elCal.value);
      const perGram = parseFloat(elMet.value);

      if (Number.isNaN(kcal) || kcal < 0){
        out.innerHTML = '<span style="color:#b00020">Por favor insira um número de calorias válido (>= 0).</span>';
        return;
      }

      const gramas = kcal / perGram;
      const kg = gramas / 1000;

      out.innerHTML = `
        <div><strong>${formatNumber(gramas)} g</strong> — equivalente aproximado.</div>
        <div style="margin-top:6px"><strong>${formatNumber(kg)} kg</strong></div>
        <div class="small muted" style="margin-top:8px">
          Cálculo: <code>${kcal} ÷ ${perGram} = ${formatNumber(gramas)}</code>
        </div>
      `;
    }

    btn.addEventListener('click', calcular);
    // Enter key triggers calculate
    elCal.addEventListener('keydown', (e)=>{ if(e.key === 'Enter') calcular(); });

    btnLimpar.addEventListener('click', ()=>{
      elCal.value = '';
      elMet.value = '7.7';
      out.innerHTML = 'Insira um valor e clique em <strong>Calcular</strong>.';
      elCal.focus();
    });

    // Calcula automaticamente com valor inicial
    window.addEventListener('load', calcular);
  </script>
</body>
</html>
