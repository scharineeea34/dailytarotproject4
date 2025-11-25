<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daily Tarot</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Georgia', 'Times New Roman', serif;
      height: 100%;
      width: 100%;
      overflow-x: hidden;
    }

    * {
      box-sizing: border-box;
    }

    .app-container {
      min-height: 100%;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 40px 20px;
    }

    .content-wrapper {
      max-width: 600px;
      width: 100%;
      text-align: center;
    }

    .title {
      font-size: 48px;
      font-weight: bold;
      margin-bottom: 12px;
      letter-spacing: 2px;
    }

    .subtitle {
      font-size: 18px;
      margin-bottom: 40px;
      opacity: 0.9;
    }

    .card-container {
      perspective: 1000px;
      margin-bottom: 40px;
      display: flex;
      justify-content: center;
    }

    .card {
      width: 280px;
      height: 420px;
      position: relative;
      transform-style: preserve-3d;
      transition: transform 0.8s cubic-bezier(0.4, 0.0, 0.2, 1);
      cursor: pointer;
    }

    .card.flipped {
      transform: rotateY(180deg);
    }

    .card-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      border-radius: 20px;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 30px;
    }

    .card-back {
      background: linear-gradient(135deg, #fff 0%, #ffe4f0 100%);
      border: 3px solid;
    }

    .card-back-pattern {
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 80px;
    }

    .card-front {
      transform: rotateY(180deg);
      padding: 40px 30px;
    }

    .card-icon {
      font-size: 80px;
      margin-bottom: 20px;
    }

    .card-name {
      font-size: 28px;
      font-weight: bold;
      margin-bottom: 20px;
      letter-spacing: 1px;
    }

    .card-meaning {
      font-size: 16px;
      line-height: 1.6;
      opacity: 0.9;
    }

    .draw-button {
      padding: 18px 48px;
      font-size: 20px;
      font-weight: 600;
      border: none;
      border-radius: 50px;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
      font-family: 'Georgia', 'Times New Roman', serif;
      letter-spacing: 1px;
    }

    .draw-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
    }

    .draw-button:active {
      transform: translateY(0);
    }

    @media (max-width: 768px) {
      .title {
        font-size: 36px;
      }

      .subtitle {
        font-size: 16px;
      }

      .card {
        width: 240px;
        height: 360px;
      }

      .card-icon {
        font-size: 60px;
      }

      .card-name {
        font-size: 24px;
      }

      .card-meaning {
        font-size: 14px;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="app-container">
   <div class="content-wrapper">
    <h1 class="title" id="mainTitle">Daily Tarot</h1>
    <p class="subtitle" id="subtitle">Discover your card of the day</p>
    <div class="card-container">
     <div class="card" id="tarotCard">
      <div class="card-face card-back">
       <div class="card-back-pattern">
        ✨
       </div>
      </div>
      <div class="card-face card-front" id="cardFront">
       <div class="card-icon" id="cardIcon">
        🌙
       </div>
       <div class="card-name" id="cardName">
        The Moon
       </div>
       <div class="card-meaning" id="cardMeaning">
        Intuition, dreams, and the unconscious mind guide your path today.
       </div>
      </div>
     </div>
    </div><button class="draw-button" id="drawButton">Draw Your Card</button>
   </div>
  </div>
  <script>
    const defaultConfig = {
      background_color: "#ffc4e1",
      surface_color: "#ffffff",
      text_color: "#4a2c3e",
      primary_action_color: "#ff69b4",
      border_color: "#ff1493",
      main_title: "Daily Tarot",
      subtitle: "Discover your card of the day",
      button_text: "Draw Your Card",
      font_family: "Georgia",
      font_size: 16
    };

    const tarotCards = [
      { icon: "🌟", name: "The Star", meaning: "Hope, inspiration, and serenity illuminate your journey today." },
      { icon: "🌙", name: "The Moon", meaning: "Intuition, dreams, and the unconscious mind guide your path today." },
      { icon: "☀️", name: "The Sun", meaning: "Joy, success, and vitality shine brightly upon you today." },
      { icon: "⚖️", name: "Justice", meaning: "Balance, fairness, and truth are your companions today." },
      { icon: "💪", name: "Strength", meaning: "Inner courage and compassion empower you today." },
      { icon: "🎡", name: "The Wheel", meaning: "Change and destiny turn in your favor today." },
      { icon: "💝", name: "The Lovers", meaning: "Love, harmony, and meaningful choices await you today." },
      { icon: "👑", name: "The Empress", meaning: "Abundance, creativity, and nurturing energy surround you today." },
      { icon: "🔮", name: "The Magician", meaning: "Manifestation and willpower are yours to command today." },
      { icon: "🌸", name: "The Fool", meaning: "New beginnings and fresh adventures call to you today." }
    ];

    let isFlipped = false;

    function onConfigChange(config) {
      const baseSize = config.font_size || defaultConfig.font_size;
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontStack = 'Georgia, Times New Roman, serif';

      document.querySelector('.app-container').style.background = `linear-gradient(135deg, ${config.background_color || defaultConfig.background_color} 0%, ${config.surface_color || defaultConfig.surface_color} 100%)`;
      
      document.getElementById('mainTitle').textContent = config.main_title || defaultConfig.main_title;
      document.getElementById('mainTitle').style.color = config.text_color || defaultConfig.text_color;
      document.getElementById('mainTitle').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.getElementById('mainTitle').style.fontSize = `${baseSize * 3}px`;

      document.getElementById('subtitle').textContent = config.subtitle || defaultConfig.subtitle;
      document.getElementById('subtitle').style.color = config.text_color || defaultConfig.text_color;
      document.getElementById('subtitle').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.getElementById('subtitle').style.fontSize = `${baseSize * 1.125}px`;

      const cardBack = document.querySelector('.card-back');
      cardBack.style.background = `linear-gradient(135deg, ${config.surface_color || defaultConfig.surface_color} 0%, #ffe4f0 100%)`;
      cardBack.style.borderColor = config.border_color || defaultConfig.border_color;

      const cardFront = document.getElementById('cardFront');
      cardFront.style.background = config.surface_color || defaultConfig.surface_color;
      cardFront.style.color = config.text_color || defaultConfig.text_color;
      cardFront.style.borderColor = config.border_color || defaultConfig.border_color;
      cardFront.style.border = `3px solid ${config.border_color || defaultConfig.border_color}`;

      document.getElementById('cardName').style.color = config.text_color || defaultConfig.text_color;
      document.getElementById('cardName').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.getElementById('cardName').style.fontSize = `${baseSize * 1.75}px`;

      document.getElementById('cardMeaning').style.color = config.text_color || defaultConfig.text_color;
      document.getElementById('cardMeaning').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.getElementById('cardMeaning').style.fontSize = `${baseSize}px`;

      const drawButton = document.getElementById('drawButton');
      drawButton.textContent = config.button_text || defaultConfig.button_text;
      drawButton.style.background = config.primary_action_color || defaultConfig.primary_action_color;
      drawButton.style.color = config.surface_color || defaultConfig.surface_color;
      drawButton.style.fontFamily = `${customFont}, ${baseFontStack}`;
      drawButton.style.fontSize = `${baseSize * 1.25}px`;
    }

    function mapToCapabilities(config) {
      return {
        recolorables: [
          {
            get: () => config.background_color || defaultConfig.background_color,
            set: (value) => {
              config.background_color = value;
              window.elementSdk.setConfig({ background_color: value });
            }
          },
          {
            get: () => config.surface_color || defaultConfig.surface_color,
            set: (value) => {
              config.surface_color = value;
              window.elementSdk.setConfig({ surface_color: value });
            }
          },
          {
            get: () => config.text_color || defaultConfig.text_color,
            set: (value) => {
              config.text_color = value;
              window.elementSdk.setConfig({ text_color: value });
            }
          },
          {
            get: () => config.primary_action_color || defaultConfig.primary_action_color,
            set: (value) => {
              config.primary_action_color = value;
              window.elementSdk.setConfig({ primary_action_color: value });
            }
          },
          {
            get: () => config.border_color || defaultConfig.border_color,
            set: (value) => {
              config.border_color = value;
              window.elementSdk.setConfig({ border_color: value });
            }
          }
        ],
        borderables: [],
        fontEditable: {
          get: () => config.font_family || defaultConfig.font_family,
          set: (value) => {
            config.font_family = value;
            window.elementSdk.setConfig({ font_family: value });
          }
        },
        fontSizeable: {
          get: () => config.font_size || defaultConfig.font_size,
          set: (value) => {
            config.font_size = value;
            window.elementSdk.setConfig({ font_size: value });
          }
        }
      };
    }

    function mapToEditPanelValues(config) {
      return new Map([
        ["main_title", config.main_title || defaultConfig.main_title],
        ["subtitle", config.subtitle || defaultConfig.subtitle],
        ["button_text", config.button_text || defaultConfig.button_text]
      ]);
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities,
        mapToEditPanelValues
      });
    }

    const card = document.getElementById('tarotCard');
    const drawButton = document.getElementById('drawButton');

    function drawCard() {
      if (isFlipped) {
        card.classList.remove('flipped');
        isFlipped = false;
        setTimeout(() => drawNewCard(), 400);
      } else {
        drawNewCard();
      }
    }

    function drawNewCard() {
      const randomCard = tarotCards[Math.floor(Math.random() * tarotCards.length)];
      
      document.getElementById('cardIcon').textContent = randomCard.icon;
      document.getElementById('cardName').textContent = randomCard.name;
      document.getElementById('cardMeaning').textContent = randomCard.meaning;
      
      setTimeout(() => {
        card.classList.add('flipped');
        isFlipped = true;
      }, 100);
    }

    drawButton.addEventListener('click', drawCard);
    card.addEventListener('click', () => {
      if (isFlipped) {
        card.classList.remove('flipped');
        isFlipped = false;
      }
    });

    onConfigChange(window.elementSdk ? window.elementSdk.config : defaultConfig);
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a41e61684c0c691',t:'MTc2NDA4MTYwMS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
