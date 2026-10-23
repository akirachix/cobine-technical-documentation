---
hide:
  - navigation
  - toc
---

<div class="tw-home">
  <!-- The video asset and overlay background layer -->
  <video class="tw-home-video" autoplay muted loop playsinline>
    <source src="assets/images/background-video.mp4" type="video/mp4" />
  </video>
  <div class="tw-home-overlay"></div>
  
  <!-- Content Inner Wrapper -->
  <div class="tw-home-inner">
    <div class="tw-home-eyebrow">TerraWatch Rwanda · Technical Documentation</div>
    <h1 class="tw-home-title">TerraWatch <span>Rwanda</span></h1>
    <p class="tw-home-subtitle">Landslide-risk intelligence platform for disaster-management decision support in Rwanda.</p>
    <a class="md-button md-button--primary" href="overview/">Explore the platform</a>
  </div>

  <div class="tw-card-wrap">
    <div class="tw-card-grid">
      <a class="tw-card" href="overview/">
        <div class="tw-card-icon">O</div><strong>Overview</strong><span>What TerraWatch Rwanda does and how it works.</span>
      </a>
      <a class="tw-card" href="architecture/">
        <div class="tw-card-icon">A</div><strong>Architecture</strong><span>Platform structure, data flow, and service boundaries.</span>
      </a>
      <a class="tw-card" href="data-pipelines/">
        <div class="tw-card-icon">D</div><strong>Data Pipelines</strong><span>Rainfall, terrain, and landslide ingestion and storage.</span>
      </a>
      <a class="tw-card" href="ai-risk-assessment/">
        <div class="tw-card-icon">AI</div><strong>AI Risk Assessment</strong><span>Random Forest classification, confidence, and explanations.</span>
      </a>
      <a class="tw-card" href="analyst-workspace/">
        <div class="tw-card-icon">W</div><strong>Analyst Workspace</strong><span>Risk map, alerts, incidents, and operational learning.</span>
      </a>
      <a class="tw-card" href="security-compliance/">
        <div class="tw-card-icon">S</div><strong>Security &amp; Compliance</strong><span>Authentication, RBAC, encryption, privacy, and audit.</span>
      </a>
      <a class="tw-card" href="glossary/">
        <div class="tw-card-icon">G</div><strong>Glossary</strong><span>Acronyms and domain terms.</span>
      </a>
    </div>
  </div>
</div>

<!-- PROBLEM STATEMENT SECTION -->
<section class="tw-section">
  <div class="tw-section-container">
    <div class="tw-section-eyebrow">The Challenge</div>
    <h2 class="tw-section-heading">Problem Statement</h2>
    
    <div class="tw-problem-box">
      <div class="tw-problem-quote-icon">“</div>
      <p>Relevant information about landslide risk is distributed across different data sources, institutions, and communication channels, making it difficult to turn changing environmental conditions into geographically specific, timely, understandable, and actionable institutional decisions.</p>
    </div>

    <p class="tw-section-text">
      Rwanda experiences recurring rainfall-induced landslides, particularly in mountainous regions such as <strong>Western Rwanda</strong>, where steep terrain, intense rainfall, and human activity increase environmental vulnerability.
    </p>
    <p class="tw-section-text">
      Although meteorological observations, satellite rainfall data, disaster-management institutions, and historical hazard information already exist, risk information is often fragmented across different systems, datasets, and organizations. As a result, emerging threats may not be identified quickly, evidence may be difficult to interpret, and decision-makers may lack geographic context.
    </p>
    <p class="tw-section-text tw-text-highlight">
      TerraWatch bridges the gap between environmental evidence and disaster-management decision making by continuously evaluating environmental conditions and highlighting locations requiring attention.
    </p>
  </div>
</section>

<!-- PRODUCT GOALS SECTION (Styled Timeline/Process Track */ -->
<section class="tw-section tw-section-alt">
  <div class="tw-section-container">
    <div class="tw-section-eyebrow">Strategic Roadmap</div>
    <h2 class="tw-section-heading">Product Goals</h2>
    
    <div class="tw-goals-timeline">
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">01</div>
        <div class="tw-timeline-content">
          <strong>Information Continuity</strong>
          <span> geospatial cataloging, polygon registration, timeline analysis, and change-alert handling.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">02</div>
        <div class="tw-timeline-content">
          <strong>Anomaly Recognition</strong>
          <span>Support earlier recognition of environmental anomalies during automated observation schedules.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">03</div>
        <div class="tw-timeline-content">
          <strong>Targeted Intervention</strong>
          <span>Drive appropriate intervention strategies instead of simply displaying raw map overlays.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">04</div>
        <div class="tw-timeline-content">
          <strong>Reduced Fragmentation</strong>
          <span>Deliver highly structured, lightweight vector alerts and payload reports.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">05</div>
        <div class="tw-timeline-content">
          <strong>Data Protection</strong>
          <span>Strict multi-tier permissions, spatial access boundary logic, and thorough audit trails.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">06</div>
        <div class="tw-timeline-content">
          <strong>Team Accountability</strong>
          <span>Create team operational accountability through organizational administrative workflows.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">07</div>
        <div class="tw-timeline-content">
          <strong>High Usability</strong>
          <span>Performant spatial load handling via edge-optimized compression and tileset tiling.</span>
        </div>
      </div>
      <div class="tw-timeline-item">
        <div class="tw-timeline-num">08</div>
        <div class="tw-timeline-content">
          <strong>Decision Boundary</strong>
          <span>Keep AI within a supportive decision boundary rather than absolute autonomous verdicts.</span>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- SYSTEM SCOPE SECTION (Side-by-Side Split Grid) */ -->
<section class="tw-section">
  <div class="tw-section-container">
    <div class="tw-section-eyebrow">Project Guardrails</div>
    <h2 class="tw-section-heading">System Scope</h2>
    
    <div class="tw-scope-split">
      <!-- In Scope Box -->
      <div class="tw-scope-box tw-scope-in">
        <h3>In Scope</h3>
        <ul>
          <li>Analyst-centric geospatial boundaries</li>
          <li>Target monitoring areas & custom polygon records</li>
          <li>Regional visual history timelines</li>
          <li>Automated change-detection risk levels</li>
          <li>Alert notifications & intervention payloads</li>
          <li>Identity verification & platform security bounds</li>
          <li>Explicit spatial permission structures</li>
          <li>Multi-tiered tenant routing</li>
          <li>Comprehensive structural system audit loops</li>
          <li>Secure geospatial vector asset uploads</li>
          <li>Strategic analytical engine integrations</li>
          <li>Performant client web dashboards</li>
          <li>Scalable central processing APIs and storage backends</li>
        </ul>
      </div>
      
      <!-- Out of Scope Box -->
      <div class="tw-scope-box tw-scope-out">
        <h3>Out of Scope</h3>
        <ul>
          <li>Legally claim or resolve cross-border land disputes</li>
          <li>Dictate regional environmental enforcement policies independently</li>
          <li>Guarantee real-time sub-meter video coverage under cloud obscuration</li>
          <li>Directly execute on-the-ground physical cleanups</li>
          <li>Solve systemic hardware satellite launch or orbital degradation delays</li>
        </ul>
      </div>
    </div>
  </div>
</section>
