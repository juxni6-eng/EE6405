<template>
  <main class="hero">
    <section class="center">
      <h1 class="title">EE6405 Final Project</h1>

      <p class="subtitle">
        We present a comparative study of classic and modern deep learning models for NLP, trained and evaluated on three widely used datasets: <strong>IMDb</strong> (binary sentiment), <strong>SST‑2</strong> (binary sentiment), and <strong>AG News</strong> (4‑class topic). Models covered include <strong>CNN</strong>, <strong>RNN</strong>, <strong>LSTM</strong>, <strong>Bi‑LSTM</strong>, <strong>BERT</strong>, and <strong>RoBERTa</strong>. Our goal is to deliver a clean, reproducible benchmark and practical takeaways on accuracy, generalization, and efficiency.
      </p>

      <!-- Key highlights cards -->
      <div class="cards">
        <article class="card">
          <h3>Overview</h3>
          <p>
            We unify preprocessing, training, and evaluation under a single codebase with consistent seeds, splits, and metrics. Each model follows the same training protocol and logging format, which makes apples‑to‑apples comparison straightforward.
          </p>
          <ul>
            <li>Tasks, IMDb and SST‑2 [binary], AG News [multiclass]</li>
            <li>Metrics, Accuracy, Precision, Recall, F1, AUC [binary], Macro‑averages [multiclass]</li>
            <li>Artifacts, Confusion matrices, ROC and PR curves, training logs</li>
          </ul>
        </article>

        <article class="card">
          <h3>Model Zoo</h3>
          <ul>
            <li><strong>CNN</strong> with text convolution blocks</li>
            <li><strong>RNN</strong>, vanilla recurrent baseline</li>
            <li><strong>LSTM</strong> and <strong>Bi‑LSTM</strong> for longer context</li>
            <li><strong>BERT</strong> base, uncased</li>
            <li><strong>RoBERTa</strong> base</li>
          </ul>
          <p style="margin-top:6px;">
            We report both effectiveness and rough efficiency signals [epochs to converge, tokens per second] to balance quality and cost.
          </p>
        </article>

        <article class="card">
          <h3>Datasets</h3>
          <ul>
            <li><strong>IMDb</strong>, 50k movie reviews, binary sentiment</li>
            <li><strong>SST‑2</strong>, Stanford Sentiment Treebank, binary sentiment</li>
            <li><strong>AG News</strong>, 4 categories, news headlines</li>
          </ul>
          <p style="margin-top:6px;">
            All datasets are split consistently, and we document any deviations from the original recipes.
          </p>
        </article>
      </div>

      <!-- Team section -->
      <div class="cards team">
        <article class="card">
          <h3>Team</h3>
          <ul class="team-list">
            <li><strong>Li Yiqian</strong>, Transformer baselines, dashboard UI</li>
            <li><strong>Member 2</strong>, CNN, RNN, LSTM implementations, training scripts</li>
            <li><strong>Member 3</strong>, data preprocessing, evaluation protocol, figures</li>
            <li><strong>Member 4</strong>, AG News experiments, error analysis, report writing</li>
          </ul>
          <p class="foot-note">Course, EE6405, Information Retrieval and Text Mining, Final Project</p>
        </article>

        <article class="card">
          <h3>Reproducibility</h3>
          <ul>
            <li>Deterministic seeds and fixed tokenizer versions</li>
            <li>Unified metric computation across tasks</li>
            <li>Per‑run JSON logs, ready for the dashboard</li>
          </ul>
          <p style="margin-top:6px;">
            See the repo for scripts to export ROC, PR, and confusion matrices that power the dashboard visualizations.
          </p>
        </article>

        <article class="card">
          <h3>Code & Resources</h3>
          <ul>
            <li><a :href="repoUrl" target="_blank" rel="noreferrer">Project repository</a></li>
            <li><a href="#" target="_blank" rel="noreferrer">Slides, coming soon</a></li>
            <li><a href="#" target="_blank" rel="noreferrer">Report, coming soon</a></li>
          </ul>
          <p style="margin-top:6px;">
            Feel free to reuse our training configs and data loaders for new datasets.
          </p>
        </article>
      </div>

      <!-- CTA to dashboard -->
      <RouterLink to="/dashboard" class="cta">View Results Dashboard</RouterLink>
    </section>
  </main>
</template>

<script setup>
import { RouterLink } from 'vue-router'

// Put your public repo URL here
const repoUrl = 'https://github.com/juxni6-eng/EE6405'
</script>

<style scoped>
.hero{ display:grid; place-items:center; min-height: calc(100vh - 120px); padding: 24px; }
.center{ max-width: 1000px; text-align:center; }
.title{ font-size: clamp(28px, 5vw, 44px); margin: 0 0 8px; letter-spacing:-0.02em; }
.subtitle{ color:#4b5563; margin: 0 auto 18px; line-height:1.7; max-width: 860px; }
.cards{ display:grid; grid-template-columns: 1fr; gap: 12px; margin: 18px 0 22px; text-align:left; }
@media (min-width: 900px){ .cards{ grid-template-columns: 1fr 1fr 1fr; } }
.card{ border:1px solid #e5e7eb; border-radius: 14px; padding: 14px; box-shadow: 0 2px 8px rgba(0,0,0,.04); }
.card h3{ margin: 0 0 6px; }
.team .team-list{ margin: 0; padding-left: 18px; }
.foot-note{ color:#6b7280; margin-top: 8px; }
.cta{
  display:inline-block; margin-top: 4px; padding: 12px 16px; border-radius: 999px;
  background:#111827; color:#fff; text-decoration:none; font-weight:700;
  box-shadow: 0 8px 20px rgba(17,24,39,.15);
}
.cta:hover{ transform: translateY(-1px); }
</style>
