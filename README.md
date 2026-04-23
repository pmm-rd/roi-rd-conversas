<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Calculadora de ROI — RD Station Conversas</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif; background: #0D1B2A; color: #F4F7FA; min-height: 100vh; }

  header { background: #00A896; padding: 20px 32px; display: flex; align-items: center; justify-content: space-between; }
  header h1 { font-size: 18px; font-weight: 600; color: #fff; }
  header span { font-size: 13px; color: rgba(255,255,255,0.8); }

  .container { max-width: 960px; margin: 0 auto; padding: 32px 24px; }

  .intro { background: #1A2E45; border-radius: 12px; padding: 24px; margin-bottom: 28px; border-left: 4px solid #00A896; }
  .intro h2 { font-size: 16px; font-weight: 600; color: #00A896; margin-bottom: 8px; }
  .intro p { font-size: 14px; color: #94A3B8; line-height: 1.6; }

  .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 28px; }
  @media (max-width: 640px) { .grid { grid-template-columns: 1fr; } }

  .card { background: #1A2E45; border-radius: 12px; padding: 24px; }
  .card h3 { font-size: 14px; font-weight: 600; color: #94A3B8; text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 20px; }

  .field { margin-bottom: 18px; }
  .field label { display: block; font-size: 13px; color: #94A3B8; margin-bottom: 6px; }
  .field-row { display: flex; align-items: center; gap: 12px; }
  .field input[type="range"] { flex: 1; -webkit-appearance: none; height: 4px; border-radius: 2px; background: #0D1B2A; outline: none; }
  .field input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; width: 18px; height: 18px; border-radius: 50%; background: #00A896; cursor: pointer; }
  .field-val { font-size: 14px; font-weight: 600; color: #F4F7FA; min-width: 72px; text-align: right; }

  .results { background: #1A2E45; border-radius: 12px; padding: 24px; margin-bottom: 28px; }
  .results h3 { font-size: 14px; font-weight: 600; color: #94A3B8; text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 20px; }
  .result-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-bottom: 20px; }
  @media (max-width: 540px) { .result-grid { grid-template-columns: 1fr 1fr; } }
  .stat { background: #0D1B2A; border-radius: 8px; padding: 16px; text-align: center; }
  .stat-val { font-size: 22px; font-weight: 700; color: #00A896; margin-bottom: 4px; }
  .stat-lbl { font-size: 11px; color: #64748B; }
  .roi-box { background: #00A896; border-radius: 10px; padding: 20px; display: flex; align-items: center; justify-content: space-between; }
  .roi-box-left h4 { font-size: 14px; font-weight: 600; color: rgba(255,255,255,0.85); }
  .roi-box-left p { font-size: 12px; color: rgba(255,255,255,0.65); margin-top: 4px; }
  .roi-number { font-size: 48px; font-weight: 800; color: #fff; }

  .benchmark { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  @media (max-width: 640px) { .benchmark { grid-template-columns: 1fr; } }

  .bench-card { background: #1A2E45; border-radius: 12px; padding: 20px; }
  .bench-card h3 { font-size: 13px; font-weight: 600; color: #64748B; text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 14px; }

  .compare-row { display: flex; align-items: center; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #0D1B2A; font-size: 13px; }
  .compare-row:last-child { border-bottom: none; }
  .compare-dim { color: #94A3B8; }
  .badge { display: inline-block; padding: 2px 10px; border-radius: 12px; font-size: 11px; font-weight: 600; }
  .badge-g { background: rgba(16,185,129,0.15); color: #10B981; }
  .badge-r { background: rgba(239,68,68,0.15); color: #EF4444; }
  .badge-y { background: rgba(245,158,11,0.15); color: #F59E0B; }

  .story { background: #1A2E45; border-radius: 12px; padding: 20px; margin-bottom: 28px; }
  .story h3 { font-size: 14px; font-weight: 600; color: #94A3B8; text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 16px; }
  .story-item { border-left: 3px solid #00A896; padding: 12px 16px; margin-bottom: 12px; background: #0D1B2A; border-radius: 0 8px 8px 0; }
  .story-item:last-child { margin-bottom: 0; }
  .story-hook { font-size: 14px; font-weight: 600; color: #00A896; margin-bottom: 6px; font-style: italic; }
  .story-body { font-size: 13px; color: #94A3B8; line-height: 1.6; }

  footer { text-align: center; padding: 24px; font-size: 12px; color: #334155; }
</style>
</head>
<body>

<header>
  <h1>ROI Framework — RD Station Conversas</h1>
  <span>Ferramenta para vendedores</span>
</header>

<div class="container">

  <div class="intro">
    <h2>Como usar esta ferramenta</h2>
    <p>Ajuste os sliders com os dados do seu prospect durante a reunião de descoberta. Mostre o número de ROI <strong>antes de revelar o preço</strong>. Use os argumentos abaixo para escolher a dimensão que mais ressoa com o perfil do prospect.</p>
  </div>

  <!-- CALCULADORA -->
  <div class="grid">
    <div class="card">
      <h3>Dados do prospect</h3>

      <div class="field">
        <label>Atendimentos por mês</label>
        <div class="field-row">
          <input type="range" min="200" max="10000" step="100" value="1000" id="s1" oninput="calc()">
          <span class="field-val" id="v1">1.000</span>
        </div>
      </div>

      <div class="field">
        <label>Custo atual por atendimento (R$)</label>
        <div class="field-row">
          <input type="range" min="5" max="60" step="1" value="25" id="s2" oninput="calc()">
          <span class="field-val" id="v2">R$25</span>
        </div>
      </div>

      <div class="field">
        <label>% de tickets repetitivos (automatizáveis)</label>
        <div class="field-row">
          <input type="range" min="20" max="80" step="5" value="50" id="s3" oninput="calc()">
          <span class="field-val" id="v3">50%</span>
        </div>
      </div>
    </div>

    <div class="card">
      <h3>Dados de receita</h3>

      <div class="field">
        <label>Leads por mês via WhatsApp</label>
        <div class="field-row">
          <input type="range" min="0" max="2000" step="50" value="300" id="s4" oninput="calc()">
          <span class="field-val" id="v4">300</span>
        </div>
      </div>

      <div class="field">
        <label>Ticket médio (R$)</label>
        <div class="field-row">
          <input type="range" min="100" max="5000" step="50" value="500" id="s5" oninput="calc()">
          <span class="field-val" id="v5">R$500</span>
        </div>
      </div>

      <div class="field">
        <label>Taxa de conversão atual estimada (%)</label>
        <div class="field-row">
          <input type="range" min="1" max="20" step="1" value="5" id="s6" oninput="calc()">
          <span class="field-val" id="v6">5%</span>
        </div>
      </div>
    </div>
  </div>

  <!-- RESULTADOS -->
  <div class="results">
    <h3>Resultado estimado para este prospect</h3>
    <div class="result-grid">
      <div class="stat">
        <div class="stat-val" id="r1">R$0</div>
        <div class="stat-lbl">Economia mensal<br>(custo operacional)</div>
      </div>
      <div class="stat">
        <div class="stat-val" id="r2">R$0</div>
        <div class="stat-lbl">Receita incremental<br>(melhora conversão +5%)</div>
      </div>
      <div class="stat">
        <div class="stat-val" id="r3">R$0</div>
        <div class="stat-lbl">Ganho total<br>em 12 meses</div>
      </div>
    </div>
    <div class="roi-box">
      <div class="roi-box-left">
        <h4>ROI estimado (12 meses)</h4>
        <p>Conservador. Não inclui ganhos de retenção, NPS e dados operacionais.</p>
      </div>
      <div class="roi-number" id="r4">0x</div>
    </div>
  </div>

  <!-- COMPARATIVO -->
  <div class="benchmark" style="margin-bottom: 28px;">
    <div class="bench-card">
      <h3>Chatbot simples vs IA conversacional</h3>
      <div class="compare-row"><span class="compare-dim">Custo/interação</span><div><span class="badge badge-y">R$1,50</span> → <span class="badge badge-g">R$0,50</span></div></div>
      <div class="compare-row"><span class="compare-dim">Taxa de resolução</span><div><span class="badge badge-y">30–50%</span> → <span class="badge badge-g">60–86%</span></div></div>
      <div class="compare-row"><span class="compare-dim">Entende contexto</span><div><span class="badge badge-r">Não</span> → <span class="badge badge-g">Sim</span></div></div>
      <div class="compare-row"><span class="compare-dim">Personalização</span><div><span class="badge badge-r">Nenhuma</span> → <span class="badge badge-g">Alta</span></div></div>
      <div class="compare-row"><span class="compare-dim">ROI médio (12–18m)</span><div><span class="badge badge-y">50–80%</span> → <span class="badge badge-g">148–210%</span></div></div>
    </div>

    <div class="bench-card">
      <h3>Humano puro vs IA + humano híbrido</h3>
      <div class="compare-row"><span class="compare-dim">Custo/interação</span><div><span class="badge badge-r">~R$30</span> → <span class="badge badge-g">~R$8–12</span></div></div>
      <div class="compare-row"><span class="compare-dim">Disponibilidade</span><div><span class="badge badge-r">Comercial</span> → <span class="badge badge-g">24/7</span></div></div>
      <div class="compare-row"><span class="compare-dim">1º resposta</span><div><span class="badge badge-r">> 6h</span> → <span class="badge badge-g">< 4min</span></div></div>
      <div class="compare-row"><span class="compare-dim">Escalar volume</span><div><span class="badge badge-r">Contratar</span> → <span class="badge badge-g">Configurar</span></div></div>
      <div class="compare-row"><span class="compare-dim">Redução de custo</span><div><span class="badge badge-r">0%</span> → <span class="badge badge-g">30–55%</span></div></div>
    </div>
  </div>

  <!-- HISTÓRIAS -->
  <div class="story">
    <h3>Argumentos de venda — escolha 1 por perfil do prospect</h3>

    <div class="story-item">
      <div class="story-hook">Para o perfil Operações/Financeiro: "Você sabe qual o custo real de cada atendimento hoje?"</div>
      <div class="story-body">Com X atendimentos/mês e custo atual, a automação de 50% dos tickets repetitivos gera economia imediata sem reduzir qualidade. Use a calculadora acima ao vivo. Mostre o número antes de falar o preço.</div>
    </div>

    <div class="story-item">
      <div class="story-hook">Para o perfil Vendas/Growth: "Qual canal vocês usam para recuperar leads que não responderam?"</div>
      <div class="story-body">WhatsApp entrega 98% de abertura vs 20% de e-mail, e 45–60% de conversão vs 2–5%. Empresas relatam 70% de melhora na recuperação de carrinho. O canal já converte — a plataforma amplifica.</div>
    </div>

    <div class="story-item">
      <div class="story-hook">Para o perfil Gestor/CS: "Você sabe qual atendente fecha mais vendas no WhatsApp?"</div>
      <div class="story-body">Sem histórico centralizado essa pergunta não tem resposta. Com RD Station Conversas, o gestor vê scripts que convertem, filas em tempo real e padrões de churn antes do cancelamento. Decisão com dado, não intuição.</div>
    </div>

    <div class="story-item">
      <div class="story-hook">Para o perfil que "já tem um bot": "Seu bot atual resolve tickets ou só desvia para o humano?"</div>
      <div class="story-body">A diferença entre chatbot e IA conversacional não é tecnologia — é taxa de resolução. Chatbot: 30–50%. IA: 60–86%. O que não foi resolvido virou frustração, e frustração vira churn invisível.</div>
    </div>
  </div>

</div>

<footer>RD Station Conversas · Framework de ROI para vendedores · Fontes: Forrester, Gartner, ResearchGate, Freshworks CX 2025, Nielsen Norman Group</footer>

<script>
function fmt(n) { return 'R$' + Math.round(n).toLocaleString('pt-BR'); }
function calc() {
  const atend = +document.getElementById('s1').value;
  const custo = +document.getElementById('s2').value;
  const auto  = +document.getElementById('s3').value / 100;
  const leads = +document.getElementById('s4').value;
  const ticket= +document.getElementById('s5').value;
  const conv  = +document.getElementById('s6').value / 100;

  document.getElementById('v1').textContent = atend.toLocaleString('pt-BR');
  document.getElementById('v2').textContent = 'R$' + custo;
  document.getElementById('v3').textContent = Math.round(auto * 100) + '%';
  document.getElementById('v4').textContent = leads.toLocaleString('pt-BR');
  document.getElementById('v5').textContent = 'R$' + ticket.toLocaleString('pt-BR');
  document.getElementById('v6').textContent = Math.round(conv * 100) + '%';

  const economiaMes = atend * auto * (custo - 2.5);
  const receitaIncr = leads * 0.05 * ticket; // +5pp conversão
  const totalAnual  = (economiaMes + receitaIncr) * 12;
  const investAnual = 12000;
  const roi = totalAnual / investAnual;

  document.getElementById('r1').textContent = fmt(economiaMes) + '/mês';
  document.getElementById('r2').textContent = fmt(receitaIncr) + '/mês';
  document.getElementById('r3').textContent = fmt(totalAnual);
  document.getElementById('r4').textContent = roi.toFixed(1) + 'x';
}
calc();
</script>
</body>
</html>
