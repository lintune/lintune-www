---
title: lintune
layout: hextra-home
---

<div class="lintune-hero-layout">
<div class="lintune-hero-content">

{{< hextra/hero-section style="font-size: 62px; line-height: 1.05; letter-spacing: -0.035em;" >}}
Self-hosted identity,<br>mail, and storage<br>for MSPs.
{{< /hextra/hero-section >}}

<div style="margin-top: 1rem;">
{{< hextra/hero-subtitle >}}
One platform. Install Keycloak, Mailcow, and Nextcloud in minutes — wired to single sign-on automatically.
{{< /hextra/hero-subtitle >}}
</div>

<div class="not-prose" style="margin-top: 1.75rem; display: flex; gap: 0.75rem; flex-wrap: wrap;">
  <a href="/docs/admin/installation" style="display:inline-flex; align-items:center; gap:0.45rem; background:#16a34a; color:#fff; font-weight:600; font-size:0.9375rem; padding:0.65rem 1.3rem; border-radius:6px; text-decoration:none;">
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" style="width:1.1rem;height:1.1rem;flex-shrink:0;"><path stroke-linecap="round" stroke-linejoin="round" d="M13 10V3L4 14h7v7l9-11h-7z"/></svg>
    Install in 5 minutes
  </a>
  <a href="/docs" style="display:inline-flex; align-items:center; gap:0.45rem; border:1.5px solid #16a34a; color:#16a34a; font-weight:600; font-size:0.9375rem; padding:0.65rem 1.3rem; border-radius:6px; text-decoration:none;">
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" style="width:1.1rem;height:1.1rem;flex-shrink:0;"><path stroke-linecap="round" stroke-linejoin="round" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/></svg>
    Read the docs
  </a>
</div>

<p class="lintune-stack-links hx:mt-5 hx:text-sm hx:text-gray-500 not-prose">
<a href="https://www.keycloak.org/"><strong>keycloak</strong></a> ·
<a href="https://mailcow.email/"><strong>mailcow</strong></a> ·
<a href="https://github.com/nextcloud/all-in-one"><strong>nextcloud</strong></a> ·
<a href="https://uptimekuma.org/"><strong>uptime-kuma</strong></a> ·
open source · <a href="https://github.com/lintune/lintune-admin/blob/dev/LICENSE"><strong>MIT</strong></a>
</p>

</div>
<div class="lintune-hero-image">
  <img class="hero-light" src="/images/lintune-hero-D.svg" alt="Lintune platform illustration" style="width: 100%; height: auto;" />
  <img class="hero-dark" src="/images/lintune-hero-DK.svg" alt="Lintune platform illustration" style="width: 100%; height: auto;" />
</div>
</div>

<div style="margin-top: 3rem; width: 100%;">
{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    title="Single Sign-On"
    subtitle="Powered by Keycloak. One set of credentials for email, files, and every service in the platform."
    icon="key"
  >}}
  {{< hextra/feature-card
    title="Email"
    subtitle="Full-featured mail stack via Mailcow. SMTP, IMAP, webmail — OIDC-connected at install time."
    icon="inbox"
  >}}
  {{< hextra/feature-card
    title="File Storage"
    subtitle="Nextcloud All-in-One, pre-configured automatically. OIDC login wired during the install wizard."
    icon="folder"
  >}}
  {{< hextra/feature-card
    title="Monitoring"
    subtitle="Uptime Kuma with a custom REST API. All services watched and alerting from day one."
    icon="chart-bar"
  >}}
{{< /hextra/feature-grid >}}
</div>
