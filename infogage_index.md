# InfoGage Single-File App (index.html)

Save this file as **index.html** and open it in any modern browser.  The page will run without any build step. The Google Gemini API key has been inserted for your convenience.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>InfoGage – AI Shopping Assistant</title>

  <!--  FONTS  -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Lora:ital,wght@0,400..700;1,400..700&display=swap" rel="stylesheet" />

  <!--  GLOBAL CSS  -->
  <style>
/* ----------  theme tokens  ---------- */
:root{
  --background:#121212;--surface:#1e1e1e;--primary:#00ff99;--primary-rgb:0,255,153;
  --on-primary:#000;--text-primary:#fff;--text-secondary:#b0b0b0;--border-color:#333;
  --font-family:'Inter',sans-serif;--logo-color:var(--primary);
  /* card accents */
  --accent-analysis:#27ae60;--accent-price:#3498db;--accent-feedback:#f1c40f;
  --accent-alternatives:#9b59b6;--accent-chatbot:#1abc9c;--accent-tradeoff:#e67e22;
  --accent-history:#34495e;--accent-ethical:#00c49f;--accent-lifespan:#8e44ad;
  --accent-value:#e67e22;--accent-nearby:#e67e22;--accent-predictor:#fd79a8;
  --accent-community:#82589F;--accent-carbon:#20bf6b;--accent-style:#ff007f;
  --accent-diy:#8e44ad;--accent-allergy:#e74c3c;--accent-vr:#7e57c2;
  --accent-dumbledore:#d3a625;--accent-sorting-hat:#6a4a3a;
}
/* Harry-Potter skin */
:root.theme-harry-potter{
  --background:#3e332a;--surface:#d8c9b3;--primary:#d3a625;--primary-rgb:211,166,37;
  --text-primary:#372e29;--border-color:#a39480;--font-family:'Lora',serif;
}
/* ----------  base layout  ---------- */
*{box-sizing:border-box;margin:0;padding:0}
html,body,#root{height:100%;width:100%}
body{font-family:var(--font-family);color:var(--text-primary);background:var(--background)}
#root{max-width:480px;margin:0 auto;display:flex;flex-direction:column;position:relative;border-left:1px solid var(--border-color);border-right:1px solid var(--border-color)}
  </style>

  <!--  REACT & FRIENDS VIA CDN  -->
  <script type="importmap">
  {
    "imports": {
      "react":       "https://esm.sh/react@19.1.0",
      "react-dom":   "https://esm.sh/react-dom@19.1.0",
      "@google/genai":"https://esm.sh/@google/genai@1.8.0"
    }
  }
  </script>
  <!--  Babel for JSX/TSX in-browser transpile  -->
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>

<body>
  <div id="root"></div>

  <!--  APP CODE  -->
  <script type="text/babel" data-type="module" data-presets="typescript,react">
    /*---------------------------------------------
      📌 1. CONFIG
    ----------------------------------------------*/
    //  🔑  Gemini API key
    const GEMINI_API_KEY = 'AIzaSyDSYpYRglWaBEVp065kE7mtBUj1bf6Lf4I';

    /*---------------------------------------------
      📌 2. APP COMPONENT – minimal demo
    ----------------------------------------------*/
    import React from 'react';
    import { useEffect, useState } from 'react';
    import { GoogleGenerativeAI } from '@google/genai';

    function App() {
      const [answer, setAnswer]   = useState('');
      const [loading, setLoading] = useState(false);
      const [query, setQuery]     = useState('');

      async function askModel() {
        if (!query.trim()) return;
        setLoading(true);
        const genAI = new GoogleGenerativeAI(GEMINI_API_KEY);
        const model = genAI.getGenerativeModel({ model: 'gemini-1.5-flash' });
        const res   = await model.generateContent(query);
        setAnswer(res.response.text());
        setLoading(false);
      }

      return (
        <main style={{padding:'1rem',display:'flex',flexDirection:'column',gap:'1rem'}}>
          <h1 style={{color:'var(--primary)'}}>InfoGage Demo</h1>

          <input  placeholder="Ask about a product..."
                  value={query} onChange={e=>setQuery(e.target.value)}
                  style={{padding:'0.6rem',borderRadius:6,border:'1px solid var(--border-color)',background:'var(--surface)',color:'inherit'}} />

          <button onClick={askModel} disabled={loading}
                  style={{padding:'0.6rem',borderRadius:6,border:'none',background:'var(--primary)',color:'var(--on-primary)',fontWeight:600,cursor:'pointer'}}>
            {loading ? 'Thinking…' : 'Ask Gemini'}
          </button>

          <section style={{whiteSpace:'pre-wrap',lineHeight:1.4}}>
            {answer}
          </section>
        </main>
      );
    }

    /*---------------------------------------------
      📌 3. MOUNT
    ----------------------------------------------*/
    import { createRoot } from 'react-dom/client';
    const root = createRoot(document.getElementById('root'));
    root.render(<React.StrictMode><App /></React.StrictMode>);

    /*---------------------------------------------
      📌 4. GLOBAL KEYFRAME ANIMATIONS
    ----------------------------------------------*/
    const styleSheet = document.createElement('style');
    styleSheet.textContent = `
      @keyframes spin{100%{transform:rotate(360deg)}}
      @keyframes bounce{0%,80%,100%{transform:scale(0)}40%{transform:scale(1)}}
    `;
    document.head.appendChild(styleSheet);
  </script>
</body>
</html>
```
