# Lintune — 13A Asset Pack

Direction 13A: three black blocks symbolize **Keycloak**, **Mailcow**, **Nextcloud**.
The green stub is **Lintune** — the binding layer.

## Contents

```
mark/
  lintune-mark-light.svg          Mark on transparent bg, dark ink + green
  lintune-mark-dark.svg           Mark on transparent bg, paper ink + bright green

lockup/
  lintune-lockup-light.svg        Horizontal lockup (mark + wordmark), light theme
  lintune-lockup-dark.svg         Horizontal lockup, dark theme
  lintune-lockup-stacked-light.svg
  lintune-lockup-stacked-dark.svg

wordmark/
  lintune-wordmark-light.svg      Wordmark only, dark ink
  lintune-wordmark-dark.svg       Wordmark only, paper ink

favicon/
  favicon-light.svg               32px tight-cropped favicon, light theme
  favicon-dark.svg                32px tight-cropped favicon, dark theme
  favicon-light-16.svg / -48.svg
  favicon-dark-16.svg  / -48.svg

website-example/
  lintune-light.html              Standalone HTML preview (light)
  lintune-dark.html               Standalone HTML preview (dark)
```

## Color tokens

| Use      | Light theme | Dark theme |
|----------|-------------|------------|
| Ink      | #0e0f12     | #fafaf7    |
| Paper    | #fafaf7     | #0e0f12    |
| Green    | #16a34a     | #22c55e    |

All SVGs have transparent backgrounds. Wordmarks fall back from Geist → Inter → system sans
if Geist isn't installed locally.

## Logo geometry (viewBox 0 0 64 64)

```
rect 14,10  14×12  ink     ← Keycloak
rect 14,24  14×12  ink     ← Mailcow
rect 14,38  14×12  ink     ← Nextcloud
rect 30,38  20×12  green   ← Lintune (binding stub)
```
