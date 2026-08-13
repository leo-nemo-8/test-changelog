---
title: Test
---
<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>SecPod Developers</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=IBM+Plex+Sans:wght@400;500&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --paper:#F6F7FA;
    --panel:#FFFFFF;
    --ink:#12161C;
    --muted:#5B6472;
    --line:#DBE0E6;
    --brand:#2F5FFF;
    --signal:#1FA97A;
    --alert:#E8871E;
  }

\*{ box-sizing:border-box; }
html,body{ margin:0; padding:0; }

body{
ckground:var(--paper);
lor:var(--ink);
nt-family:'IBM Plex Sans', sans-serif;
ebkit-font-smoothing:antialiased;


a{ color:inherit; text-decoration:none; }
a:focus-visible, button:focus-visible{





.mono{ font-family:'IBM Plex Mono', monospace; }

.wrap{
-width:1080px;
gin:0 auto;
ding:0 24px;


/ _---------- top bar ----------_ /
header{
er-bottom:1px solid var(--line);

.topbar{
ay:flex;
-items:center;
fy-content:space-between;
ng:20px 0;

.logo-container {

nter;


.custom-logo {
;

contain;

.wordmark{
mily:'Space Grotesk', sans-serif;
ight:700;
ze:18px;
spacing:0.01em;

.wordmark span{ color:var(--brand); }
nav.toplinks{
ex;


nav.toplinks a{
px;
muted);
olor .15s ease;

nav.toplinks a:hover{ color:var(--ink); }

@media (max-width:640px){
none; }


/ _---------- hero ----------_ /
.hero{
ition:relative;
ding:88px 0 56px;
rflow:hidden;

.hero::before{

solute;
:-20%;


linear-gradient(90deg, transparent, var(--brand) 40%, transparent);
;
weep 7s linear infinite;

@keyframes sweep{
translateX(0); }
m:translateX(220%); }

@media (prefers-reduced-motion:reduce){
ay:none; }


.eyebrow{
ize:12px;
-spacing:.14em;
var(--brand);
eight:500;
-bottom:18px;


h1{
font-family:'Space Grotesk', sans-serif;
font-weight:600;
font-size:clamp(34px, 5vw, 52px);
line-height:1.08;
letter-spacing:-0.01em;
margin:0 0 20px;
max-width:16ch;


.lede{
t-size:17px;
e-height:1.55;
or:var(--muted);
-width:46ch;
gin:0 0 32px;


.cta-row{
y:flex;
px;
rap:wrap;
-bottom:56px;

.btn{
nt-size:14px;
nt-weight:500;
dding:11px 20px;
rder-radius:6px;
ansition:transform .15s ease, background .15s ease, border-color .15s ease;

.btn-primary{
:var(--ink);
--paper);

.btn-primary:hover{ background:var(--brand); transform:translateY(-1px); }
.btn-secondary{
olid var(--line);
ink);

.btn-secondary:hover{ border-color:var(--ink); transform:translateY(-1px); }

/ _---------- status ledger ----------_ /
.ledger{
ay:flex;
wrap:wrap;
8px;
ng:16px 20px;
round:var(--panel);
r:1px solid var(--line);
r-radius:8px;
size:13px;

.ledger-item{
ex;
s:center;

--muted);

.dot{
dth:7px; height:7px;
rder-radius:50%;
ckground:var(--signal);
ex-shrink:0;

.dot.amber{ background:var(--alert); }
.ledger-item b{ color:var(--ink); font-weight:500; }

/ _---------- grid ----------_ /
.section{
g:64px 0 96px;
-top:1px solid var(--line);

.grid{
play:grid;
d-template-columns:1fr 1fr;
:1px;
kground:var(--line);
der:1px solid var(--line);
der-radius:10px;
rflow:hidden;

@media (max-width:720px){
olumns:1fr; }


.card{
kground:var(--panel);
ding:32px;
play:flex;
x-direction:column;
:14px;
nsition:background .15s ease;

.card:hover{ background:#FBFCFE; }

.card-icon {
x;
px;
t:contain;


.card-label{
:11px;
acing:.1em;
(--brand);
ht:500;

.card h2{
amily:'Space Grotesk', sans-serif;
ize:20px;
eight:600;
:0;

.card p{
size:14.5px;
height:1.55;
:var(--muted);
n:0;
grow:1;

.card-link{
e:14px;
ght:500;
r(--ink);
inline-flex;
ems:center;


.card-link .arrow{ transition:transform .15s ease; }
.card:hover .card-link .arrow{ transform:translateX(3px); }

/ _---------- footer ----------_ /
footer{
er-top:1px solid var(--line);
ing:28px 0;

.footrow{
y:flex;
y-content:space-between;
items:center;
rap:wrap;
px;
ize:13px;
var(--muted);

.footlinks{ display:flex; gap:20px; }
.footlinks a:hover{ color:var(--ink); } </style>

</head>
<body>

<header>
  <div class="wrap topbar">
    <div class="logo-container">
      <img src="https://files.readme.io/c9a6c2b2b51cd6364ba2182aef3f95e00f2664b7d97f8d21feedad9b11215e10-__.png" alt="Logo" class="custom-logo" />
      <div class="wordmark">SEC<span>POD</span></div>
    </div>
    <nav class="toplinks mono">
      <a href="/api-docs">API</a>
      <a href="/sdk">SDK</a>
      <a href="/mcp">MCP</a>
      <a href="/contributing">Contributing</a>
    </nav>
  </div>
</header>

<section class="hero">
  <div class="wrap">
    <div class="eyebrow mono">DEVELOPER HUB</div>
    <h1>Build with SecPod's vulnerability data.</h1>
    <p class="lede">API reference, SDK, and MCP tooling for teams integrating vulnerability management into their own products and workflows.</p>
    <div class="cta-row">
      <a href="/api-docs" class="btn btn-primary">Read the API docs</a>
      <a href="/contributing" class="btn btn-secondary">View on GitHub</a>
    </div>

    <div class="ledger mono">
      <div class="ledger-item"><span class="dot"></span>API <b>Operational</b></div>
      <div class="ledger-item"><span class="dot"></span>SDK v1 <b>Published</b></div>
      <div class="ledger-item"><span class="dot amber"></span>MCP Server <b>Beta</b></div>
      <div class="ledger-item"><span class="dot"></span>Docs <b>Synced</b></div>
    </div>
  </div>
</section>

<section class="section">
  <div class="wrap">
    <div class="grid">

      <a class="card" href="/api-docs">
        <img src="https://files.readme.io/c9a6c2b2b51cd6364ba2182aef3f95e00f2664b7d97f8d21feedad9b11215e10-__.png" alt="REST API Icon" class="card-icon" />
        <div class="card-label mono">REST API</div>
        <h2>API Reference</h2>
        <p>Authenticate, query vulnerability data, and manage assets through SecPod's REST API.</p>
        <span class="card-link">Read the docs <span class="arrow">→</span></span>
      </a>

      <a class="card" href="/sdk">
        <div class="card-label mono">PYTHON</div>
        <h2>SDK</h2>
        <p>Install <code>saner-sdk</code> and start scripting against SecPod without touching raw HTTP.</p>
        <span class="card-link">View SDK docs <span class="arrow">→</span></span>
      </a>

      <a class="card" href="/mcp">
        <div class="card-label mono">MCP</div>
        <h2>MCP Server</h2>
        <p>Give your AI agents direct, structured access to SecPod's vulnerability data.</p>
        <span class="card-link">Connect via MCP <span class="arrow">→</span></span>
      </a>

      <a class="card" href="/contributing">
        <div class="card-label mono">OPEN SOURCE</div>
        <h2>Contributing</h2>
        <p>Report issues, propose changes, or contribute code on GitHub.</p>
        <span class="card-link">See guidelines <span class="arrow">→</span></span>
      </a>

    </div>
  </div>
</section>

</body>
</html>
