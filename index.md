---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: opencode + OpenRouter 教材庫
routeAlias: home
style: |
  .slidev-layout p,
  .slidev-layout li,
  .slidev-layout td,
  .slidev-layout th,
  .slidev-layout div {
    font-size: max(16px, 1em);
  }
  table {
    width: 100%;
    margin: 1rem 0;
    border-collapse: collapse;
  }
  th, td {
    padding: 8px !important;
    border: 1px solid #e2e8f0 !important;
  }
  .index-table td {
    text-align: center;
    font-family: monospace;
  }
---

<style>
.chapter-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  width: 100%;
  max-width: 960px;
  margin-top: 1.2rem;
}
.chapter-card {
  display: block;
  background: #f0faf9;
  border: 2px solid #5eada0;
  border-radius: 12px;
  padding: 1.2rem 0.8rem;
  text-decoration: none !important;
  color: #1a5c5c !important;
  transition: all 0.2s ease;
}
.chapter-card:hover {
  background: #5eada0;
  color: white !important;
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(94, 173, 160, 0.35);
}
.chapter-card:hover .chapter-subtitle {
  color: rgba(255,255,255,0.85) !important;
}
.chapter-num {
  font-size: 1.6rem;
  font-weight: 900;
  margin-bottom: 0.3rem;
}
.chapter-subtitle {
  font-size: max(13px, 0.88rem);
  color: #4a7c7c;
  margin-top: 0.3rem;
}
</style>

<div class="flex flex-col items-center h-full" style="background: #ffffff; overflow-y: auto; padding: 1.5rem 0;">
  <p style="color: #5eada0; font-size: 1rem; font-weight: 600; letter-spacing: 0.2em; text-transform: uppercase; margin-bottom: 1rem;">AI Coding Agent Masterclass</p>
  <h1 style="color: #1a5c5c; font-size: 2.8rem; font-weight: 900; line-height: 1.2; margin-bottom: 0.5rem;">課程目錄</h1>
  <div style="height: 4px; width: 240px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 0.5rem;"></div>
  <p style="color: #9dc4c4; font-size: 0.9rem; margin-bottom: 0;">點擊章節卡片開始學習</p>
  <div class="chapter-grid">
    <Link to="ch01" class="chapter-card">
      <div class="chapter-num">Ch 1</div>
      <div>opencode 下載安裝與使用</div>
      <div class="chapter-subtitle">Install & Getting Started</div>
    </Link>
    <Link to="ch02" class="chapter-card">
      <div class="chapter-num">Ch 2</div>
      <div>OpenRouter 介紹與申請</div>
      <div class="chapter-subtitle">OpenRouter & API Key</div>
    </Link>
    <Link to="ch03" class="chapter-card">
      <div class="chapter-num">Ch 3</div>
      <div>opencode + OpenRouter</div>
      <div class="chapter-subtitle">Connect & Use Models</div>
    </Link>
    <Link to="ch04" class="chapter-card">
      <div class="chapter-num">Ch 4</div>
      <div>Agent Skills 介紹與使用</div>
      <div class="chapter-subtitle">Skills</div>
    </Link>
    <Link to="ch05" class="chapter-card">
      <div class="chapter-num">Ch 5</div>
      <div>MCP 介紹與使用</div>
      <div class="chapter-subtitle">Model Context Protocol</div>
    </Link>
    <Link to="ch06" class="chapter-card">
      <div class="chapter-num">Ch 6</div>
      <div>深入理解 AI Agent</div>
      <div class="chapter-subtitle">Principles & Harness Engineering</div>
    </Link>
    <Link to="ch07" class="chapter-card">
      <div class="chapter-num">Ch 7</div>
      <div>上下文工程</div>
      <div class="chapter-subtitle">Context Engineering</div>
    </Link>
    <Link to="ch08" class="chapter-card">
      <div class="chapter-num">Ch 8</div>
      <div>使用者記憶與知識庫</div>
      <div class="chapter-subtitle">Memory & Knowledge Base</div>
    </Link>
    <Link to="ch09" class="chapter-card">
      <div class="chapter-num">Ch 9</div>
      <div>工具設計</div>
      <div class="chapter-subtitle">Tool Design & ACI</div>
    </Link>
    <Link to="ch10" class="chapter-card">
      <div class="chapter-num">Ch 10</div>
      <div>Coding Agent 與程式碼生成</div>
      <div class="chapter-subtitle">Code as Meta-Capability</div>
    </Link>
    <Link to="ch11" class="chapter-card">
      <div class="chapter-num">Ch 11</div>
      <div>互動：觀察與動作空間</div>
      <div class="chapter-subtitle">Real-Time Interaction</div>
    </Link>
    <Link to="ch12" class="chapter-card">
      <div class="chapter-num">Ch 12</div>
      <div>Agent 的評估</div>
      <div class="chapter-subtitle">Evaluation</div>
    </Link>
    <Link to="ch13" class="chapter-card">
      <div class="chapter-num">Ch 13</div>
      <div>模型後訓練</div>
      <div class="chapter-subtitle">Post-Training: SFT & RL</div>
    </Link>
    <Link to="ch14" class="chapter-card">
      <div class="chapter-num">Ch 14</div>
      <div>Agent 的持續進化</div>
      <div class="chapter-subtitle">Continuous Evolution</div>
    </Link>
    <Link to="ch15" class="chapter-card">
      <div class="chapter-num">Ch 15</div>
      <div>多 Agent 協作</div>
      <div class="chapter-subtitle">Multi-Agent Collaboration</div>
    </Link>
  </div>
</div>

---
src: ./ch01-opencode-intro.md
---

---
src: ./ch02-openrouter-intro.md
---

---
src: ./ch03-opencode-openrouter.md
---

---
src: ./ch04-opencode-skill.md
---

---
src: ./ch05-opencode-mcp.md
---

---
src: ./ch06-ai-agent-principles.md
---

---
src: ./ch07-context-engineering.md
---

---
src: ./ch08-memory-and-knowledge.md
---

---
src: ./ch09-tool-design.md
---

---
src: ./ch10-coding-agent.md
---

---
src: ./ch11-interaction.md
---

---
src: ./ch12-evaluation.md
---

---
src: ./ch13-post-training.md
---

---
src: ./ch14-continuous-evolution.md
---

---
src: ./ch15-multi-agent.md
---
