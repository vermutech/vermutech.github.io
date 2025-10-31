# AGENTS.md - Guia per a AI Agents

## 📋 Informació del Projecte

**Nom:** Vermutech Newsletter Landing Page  
**Tipus:** Web estàtica (HTML/CSS/JavaScript)  
**Hosting:** GitHub Pages  
**URL:** https://vermutech.com  
**Repositori:** vermutech.github.io  

## 🏗️ Arquitectura

### Estructura de Fitxers
```
/
├── index.html          # Pàgina principal (tot el codi inline)
├── sw.js               # Service Worker per cache
├── CNAME               # Domini personalitzat
├── assets/
│   ├── logo.ico        # Favicon (87KB)
│   ├── logo.png        # Logo original (594KB) - NO USAR
│   ├── logo-200.png    # Logo optimitzat petit (56KB)
│   ├── logo-400.png    # Logo optimitzat mitjà (172KB)
│   └── me.jpeg         # Foto perfil (56KB)
└── AGENTS.md           # Aquest fitxer
```

### Tecnologies Utilitzades
- **HTML5** semàntic
- **CSS3** inline (dins `<style>`)
- **JavaScript** vanilla (sense frameworks)
- **Service Worker** per cache i rendiment
- **Google Fonts:** Roboto, Raleway
- **SVG Icons** inline (sense dependències externes)
- **Insights.io** per analytics
- **Substack iframe** amb lazy loading

## 🚀 Optimitzacions de Rendiment Aplicades

### 1. Imatges Responsives i WebP
- Format **WebP** amb fallback PNG per compatibilitat
- Utilitzem `<picture>` i `srcset` amb múltiples mides
- `loading="lazy"` per imatges below the fold
- `width` i `height` explícits per evitar layout shifts
- Compressions aplicades amb ImageMagick

**IMPORTANT:** 
- Sempre usa imatges WebP (`logo-200.webp`, `logo-400.webp`)
- MAI `logo.png` (594KB - massa gran)
- Reducció: PNG 56-172KB → WebP 12-22KB (78-87% estalvi)

### 2. Preconnect i DNS-Prefetch
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="dns-prefetch" href="https://vermutech.substack.com">
```

### 3. Scripts Asíncrons i Lazy Loading
- Analytics: carregat després del `window.load`
- Navigation scripts: amb `defer`
- Substack iframe: lazy loading amb Intersection Observer
- Icones SVG inline (sense dependències externes)

### 4. Service Worker Cache
- Cache de recursos estàtics
- Estratègia: Cache First amb Network Fallback

## ⚙️ Service Worker - Flux de Treball

### Quan Fer Canvis

**CADA VEGADA** que modifiquis `index.html`, imatges o qualsevol recurs cacheat:

1. **Incrementa la versió del cache** a `sw.js`:
```javascript
const CACHE_NAME = 'vermutech-v3'; // Incrementa el número
```

2. **Actualitza la llista de recursos** si n'afegeixes de nous:
```javascript
const urlsToCache = [
  '/',
  '/index.html',
  '/assets/logo-200.png',
  '/assets/logo-400.png',
  // ... afegeix aquí nous recursos
];
```

3. **Commit tots els canvis junts:**
```bash
git add index.html sw.js assets/
git commit -m "Actualització de contingut (cache v3)"
git push
```

### Per Què És Important
- Els navegadors mantenen la cache antiga fins que detecten un nou `sw.js`
- Canviar `CACHE_NAME` força l'esborrat de caches velles
- Si NO canvies la versió, els usuaris veuran contingut vell

## 🎨 Modificacions de Contingut

### Colors de Marca (CSS Variables)
```css
--vermutech-orange: #ee7835;
--vermutech-deep: #b14d14;
--vermutech-cream: #fff3e1;
--text-dark: #2b1e16;
```

### Seccions de la Pàgina
1. **Header/Hero:** Logo, títol, CTAs principals
2. **#newsletter:** Estructura del newsletter
3. **#benefits:** Per què funciona
4. **#about:** Sobre l'autor (Eduard Carreras)
5. **#community:** Canal Telegram
6. **Footer:** Contacte i xarxes

### CTAs Principals
- Botó subscripció: https://vermutech.substack.com
- Arxiu: https://vermutech.substack.com/archive
- Telegram: https://t.me/vermutech

## 🔧 Limitacions de GitHub Pages

**NO POTS:**
- Usar `.htaccess` (servidor Apache)
- Configurar Nginx
- Modificar headers HTTP del servidor
- Usar server-side rendering
- Usar variables d'entorn en runtime

**SÍ POTS:**
- Service Workers
- Client-side JavaScript
- Fitxers estàtics
- CNAME per domini personalitzat

## 📊 Analytics

### Insights.io Setup
- Només s'executa a `vermutech.com` (no en localhost)
- Events trackeats:
  - `go-to-subscribe`: Click botó subscripció
  - `view-archive`: Click arxiu
  - `go-to-telegram`: Click Telegram
  - Page views automàtic

### Modificar Tracking
Si afegeixes nous botons amb IDs, actualitza:
```javascript
['subscribe-button', 'archive-button', 'telegram-button', 'nou-button-id']
```

## 🖼️ Optimitzar Noves Imatges

### Amb ImageMagick (convert)
```bash
# PNG
convert imatge.png -strip -quality 85 -resize 400x400 imatge-400.png

# JPEG
convert imatge.jpg -strip -quality 85 -resize 800x800 imatge-800.jpg

# WebP (PREFERIT - millor compressió)
convert imatge.png -quality 85 imatge.webp
convert imatge.png -quality 85 -resize 200x200 imatge-200.webp
convert imatge.png -quality 85 -resize 400x400 imatge-400.webp
```

**Sempre genera WebP + PNG com a fallback**

### Mides Recomanades
- **Logos/Icones:** 200px, 400px
- **Fotos perfil:** 220px, 440px
- **Imatges contingut:** 800px, 1200px

## 🧪 Testing Local

### Provar Service Worker
```bash
# Servir amb Python
python3 -m http.server 8000

# O amb Node.js
npx serve
```

⚠️ **Service Workers NOMÉS funcionen:**
- En HTTPS
- En `localhost`

### Validar HTML
```bash
# Si tens validator instal·lat
html5validator index.html

# O online: https://validator.w3.org/
```

## 📝 Workflow Recomanat

### Per Canvis de Contingut
1. Edita `index.html`
2. Incrementa versió a `sw.js`
3. Test local
4. Commit + push
5. Espera ~1-2 minuts (deploy GitHub Pages)
6. Verifica a https://vermutech.com

### Per Noves Imatges
1. Optimitza amb `convert`
2. Afegeix a `/assets/`
3. Actualitza `urlsToCache` a `sw.js`
4. Incrementa versió cache
5. Commit + push

### Per Canvis CSS/JS
1. Modifica inline al `<style>` o `<script>`
2. Incrementa versió cache
3. Commit + push

## 🐛 Debugging

### Cache Issues
Si els usuaris veuen contingut vell:
1. Verifica que `CACHE_NAME` s'ha incrementat
2. Comprova que `sw.js` s'ha pujat correctament
3. Hard refresh: `Ctrl+Shift+R` (o `Cmd+Shift+R`)

### Service Worker No Es Registra
1. Comprova consola del navegador (F12)
2. Verifica que estàs en HTTPS o localhost
3. Mira la secció Application > Service Workers a DevTools

### Imatges No Es Veuen
1. Verifica que el path és correcte: `/assets/imatge.png`
2. Comprova que el fitxer existeix al repositori
3. Espera deploy de GitHub Pages

## 📚 Referències Útils

- **Lighthouse:** Per auditar rendiment
- **PageSpeed Insights:** https://pagespeed.web.dev/
- **Service Worker API:** https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- **GitHub Pages Docs:** https://docs.github.com/pages

## 🎯 Mètriques Objectiu

**Core Web Vitals:**
- **LCP (Largest Contentful Paint):** < 2.5s
- **FID (First Input Delay):** < 100ms
- **CLS (Cumulative Layout Shift):** < 0.1

**Performance Score:** > 90/100 a Lighthouse

---

**Última actualització:** 2024-10-31  
**Versió Cache Actual:** v4  
**Contacte:** hola@vermutech.com

## 🎨 Icones SVG

Les icones són SVG inline definides dins d'un `<svg style="display:none">` al `<head>`. Per usar-les:

```html
<svg class="icon"><use href="#icon-bell"></use></svg>
```

**Icones disponibles:**
- `#icon-bell` - Notificacions
- `#icon-envelope` - Email
- `#icon-telegram` - Telegram
- `#icon-linkedin` - LinkedIn
- `#icon-github` - GitHub
- `#icon-mastodon` - Mastodon

Per afegir noves icones, afegeix un nou `<symbol>` dins del SVG ocult i usa'l amb `<use>`.

## ♿ Accessibilitat

### Links amb només icones
Sempre afegeix `aria-label` als links que només tenen icones:

```html
<a href="..." aria-label="Descripció clara del destí">
    <svg class="icon"><use href="#icon-..."></use></svg>
</a>
```

### Imatges
- Sempre afegeix `alt` descriptiu
- Afegeix `width` i `height` per evitar CLS
- Usa `loading="lazy"` per imatges below the fold
