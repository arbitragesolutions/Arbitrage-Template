<script>
  import { ArrowRight, ArrowUpRight, BarChart3, Bell, Bot, ChevronDown, ChevronRight, CircleDollarSign, Database, Filter, Gauge, Menu, Play, Rocket, Search, ShieldCheck, Sparkles, X, Zap } from '@lucide/svelte'

  const navLinks = [
    { label: 'Сканеры', href: '#scanner' },
    { label: 'Возможности', href: '#features' },
    { label: 'DEX', href: '#dex' },
    { label: 'Тарифы', href: '#pricing' },
    { label: 'FAQ', href: '#faq' }
  ]

  const exchanges = ['BINANCE', 'OKX', 'BYBIT', 'KUCOIN', 'MEXC', 'GATE.IO', 'BITGET']
  const networks = ['Ethereum', 'BSC', 'Arbitrum', 'Polygon', 'Base', 'Solana', 'TON', 'Tron', 'Optimism', 'Avalanche']
  const features = [
    ['20+', 'бирж и тысячи монет', 'Собираем ликвидность с главных площадок в одном экране.', BarChart3],
    ['5 сек', 'обновление данных', 'Лента спредов не заставляет ждать ручной перезагрузки.', Zap],
    ['0.01%', 'точность расчёта', 'Видите чистый спред после торговых и сетевых комиссий.', CircleDollarSign],
    ['24/7', 'мониторинг рынка', 'Сигналы приходят, пока вы занимаетесь своими делами.', Bell],
    ['∞', 'гибкие фильтры', 'Сумма, биржи, сети, минимальный спред и доступность вывода.', Filter],
    ['2x', 'проверка ликвидности', 'Не только цена, но и реальный объём для входа и выхода.', Gauge],
    ['13', 'поддерживаемых сетей', 'Сверяем сети перевода до того, как вы нажмёте купить.', ShieldCheck],
    ['API', 'готово для автоматизации', 'Подключайте уведомления и свои торговые сценарии.', Bot],
    ['Smart', 'хеджирование', 'Отмечаем ситуации, где риск можно снизить встречной позицией.', Database]
  ]
  const spreads = [
    { pair: 'INJ / USDT', buy: 'HTX', sell: 'BYBIT', buyPrice: '19.842', sellPrice: '20.126', spread: 1.43, net: 0.98, volume: '380', network: 'ETH', time: '3м 13с' },
    { pair: 'ARB / USDT', buy: 'MEXC', sell: 'BINANCE', buyPrice: '0.7421', sellPrice: '0.7528', spread: 1.44, net: 1.12, volume: '1 240', network: 'ARB', time: '1м 42с' },
    { pair: 'TIA / USDT', buy: 'KUCOIN', sell: 'OKX', buyPrice: '6.184', sellPrice: '6.246', spread: 1.00, net: 0.63, volume: '620', network: 'TIA', time: '4м 08с' },
    { pair: 'WIF / USDT', buy: 'BYBIT', sell: 'GATE.IO', buyPrice: '2.182', sellPrice: '2.241', spread: 2.70, net: 2.18, volume: '890', network: 'SOL', time: '2м 05с' }
  ]
  const plans = [
    { name: 'Межбиржевой', desc: 'Для спокойного старта', monthly: 29, color: 'mint', items: ['Spot–spot сканер', '20+ бирж', 'Фильтры и комиссии'] },
    { name: 'Фьючерсный', desc: 'Больше возможностей', monthly: 49, color: 'blue', items: ['Фьючерсы и funding', 'Хеджирование', 'Telegram-сигналы'] },
    { name: 'DEX–CEX', desc: 'Охват децентрализованных рынков', monthly: 59, color: 'pink', items: ['8 агрегаторов', '13 сетей', 'Проверка газа'] },
    { name: 'Всё включено', desc: 'Максимум контроля', monthly: 89, color: 'rainbow', items: ['Все сканеры', 'Авто-арбитраж', 'Приоритетная поддержка'], recommended: true }
  ]
  const faqs = [
    ['С какой суммы можно начать?', 'Минимальная сумма зависит от выбранной пары, комиссий и доступной ликвидности. Начните с небольшой суммы и проверьте весь маршрут самостоятельно.'],
    ['От чего зависит доходность?', 'От разницы цен, ликвидности, комиссий, скорости перевода и волатильности. Сканер показывает расчёт, но не гарантирует результат.'],
    ['Как часто появляются спреды?', 'Новые возможности появляются постоянно, но живут по-разному: от нескольких секунд до нескольких минут.'],
    ['Какие биржи поддерживаются?', 'Сейчас в демо доступны Binance, OKX, Bybit, KuCoin, MEXC, Gate.io, Bitget и другие площадки.'],
    ['Есть ли обучение?', 'Да, в тарифе есть короткие разборы маршрутов, чек-листы и подсказки по работе с фильтрами.'],
    ['Есть ли чат для пользователей?', 'Да, пользователи получают доступ к закрытому Telegram-чату с обновлениями и поддержкой.'],
    ['Есть ли реферальная программа?', 'Реферальная программа готовится к запуску. Условия появятся в личном кабинете.'],
    ['Как связаться с поддержкой?', 'Напишите нам в Telegram — команда отвечает по вопросам продукта и настройки сканера.']
  ]

  let mobileOpen = $state(false)
  let selectedType = $state('Межбиржевой')
  let query = $state('')
  let minSpread = $state(0.5)
  let exchangeFilter = $state('Все биржи')
  let onlyNet = $state(false)
  let rows = $state(spreads)
  let openFaq = $state(0)
  let activePlan = $state('month')
  let videoOpen = $state(false)
  let selectedVideo = $state('')
  let scrolled = $state(false)

  const scannerTypes = {
    'Межбиржевой': { icon: '↗', title: 'Спот–спот', text: 'Классический маршрут: купить дешевле на одной бирже и продать дороже на другой.', points: ['Чистый спред после комиссий', 'Проверка сети перевода', 'Расчёт рабочего объёма'] },
    'Фьючерсный': { icon: '◈', title: 'Фьючерс–фьючерс', text: 'Сравнивайте ставки финансирования и цену контрактов без вывода средств.', points: ['Funding и basis', 'Спред входа и выхода', 'Отметка для хеджа'] },
    'DEX–CEX': { icon: '✦', title: 'DEX–CEX', text: 'Находите разницу между агрегаторами ликвидности и централизованными биржами.', points: ['Газ и проскальзывание', '8 агрегаторов', '13 сетей'] },
    'DEX–фьючерс': { icon: '◎', title: 'DEX–фьючерс', text: 'Сопоставляйте цену токена в сети с фьючерсной позицией на CEX.', points: ['Мосты и время', 'Funding', 'Лимиты ликвидности'] }
  }
  const filteredRows = $derived(rows.filter((row) => row.pair.toLowerCase().includes(query.toLowerCase()) && row.net >= minSpread && (exchangeFilter === 'Все биржи' || row.buy === exchangeFilter || row.sell === exchangeFilter) && (!onlyNet || row.network !== '—')))
  const priceMultiplier = $derived(activePlan === 'year' ? 0.7 : activePlan === 'quarter' ? 0.85 : 1)

  $effect(() => {
    const timer = window.setInterval(() => {
      rows = rows.map((row) => ({ ...row, spread: +(row.spread + (Math.random() - 0.5) * 0.08).toFixed(2), net: +(row.net + (Math.random() - 0.5) * 0.07).toFixed(2) }))
    }, 5000)
    const onScroll = () => scrolled = window.scrollY > 24
    window.addEventListener('scroll', onScroll)
    return () => { window.clearInterval(timer); window.removeEventListener('scroll', onScroll) }
  })

  function goTo(href) { mobileOpen = false; document.querySelector(href)?.scrollIntoView({ behavior: 'smooth' }) }
  function showVideo(title) { selectedVideo = title; videoOpen = true }
</script>

<svelte:head>
  <title>Arbitrage Bro Scan — сканер криптоарбитража</title>
  <meta name="description" content="Arbitrage Bro Scan помогает находить криптоарбитражные спреды между биржами, DEX и фьючерсами в реальном времени." />
  <meta property="og:title" content="Arbitrage Bro Scan — следующий спред ближе, чем кажется" />
  <meta property="og:description" content="Мониторинг спредов, ликвидности и комиссий в одном пространстве." />
</svelte:head>

<header class:scrolled class="topbar">
  <a class="brand" href="#top" onclick={(event) => { event.preventDefault(); goTo('#top') }}><span class="brand-mark"><ArrowUpRight size={18} /></span><span>Arbitrage Bro <b>Scan</b></span></a>
  <nav class="desktop-nav" aria-label="Основная навигация">{#each navLinks as link}<a href={link.href} onclick={(event) => { event.preventDefault(); goTo(link.href) }}>{link.label}</a>{/each}</nav>
  <div class="nav-actions"><a class="login-link" href="#pricing">Войти</a><a class="button button-small" href="#pricing">Начать <ArrowRight size={15} /></a><button class="menu-button" aria-label="Открыть меню" aria-expanded={mobileOpen} onclick={() => mobileOpen = !mobileOpen}>{#if mobileOpen}<X size={21} />{:else}<Menu size={21} />{/if}</button></div>
  {#if mobileOpen}<div class="mobile-menu">{#each navLinks as link}<a href={link.href} onclick={(event) => { event.preventDefault(); goTo(link.href) }}>{link.label}<ArrowRight size={16} /></a>{/each}<a class="button" href="#pricing" onclick={() => mobileOpen = false}>Начать сканировать</a></div>{/if}
</header>

<main id="top">
  <section class="hero shell reveal">
    <div class="stars"></div><div class="nebula nebula-one"></div><div class="nebula nebula-two"></div>
    <div class="hero-orbit orbit-one"></div><div class="hero-orbit orbit-two"></div>
    <div class="rocket-wrap" aria-hidden="true">
      <!-- TODO: replace this illustrated rocket with static/hero-rocket.webp when the 3D render is ready. -->
      <div class="flame flame-back"></div><div class="flame flame-front"></div>
      <svg class="rocket" viewBox="0 0 120 260" fill="none"><path d="M60 8C84 34 94 66 91 119L78 190H42L29 119C26 66 36 34 60 8Z" fill="url(#rocket)" stroke="#fff" stroke-opacity=".35"/><path d="M29 120 8 147l34-5M91 120l21 27-34-5" fill="#7C3AED" stroke="#fff" stroke-opacity=".25"/><circle cx="60" cy="80" r="17" fill="#111827" stroke="#A5F3FC" stroke-width="4"/><circle cx="60" cy="80" r="8" fill="#67E8F9"/><path d="M44 190h32l-7 37H51l-7-37Z" fill="#F472B6"/><defs><linearGradient id="rocket" x1="30" y1="20" x2="90" y2="180" gradientUnits="userSpaceOnUse"><stop stop-color="#F9A8D4"/><stop offset=".45" stop-color="#818CF8"/><stop offset="1" stop-color="#22D3EE"/></linearGradient></defs></svg>
    </div>
    <div class="hero-content">
      <!-- TODO: demo social proof; replace with real customer avatars and count. -->
      <div class="proof"><div class="avatars"><span>А</span><span>М</span><span>К</span><span class="avatar-plus">+</span></div><div><div class="stars-rating">★★★★★</div><small>Уже используют 1 000+ трейдеров</small></div></div>
      <h1>Находи арбитражные<br /><em>спреды</em> быстрее рынка</h1>
      <p>Мониторинг спредов в реальном времени между 20+ биржами: спот, фьючерсы и DEX — с учётом комиссий, сетей и ликвидности.</p>
      <div class="hero-actions"><a class="button" href="#scanner">Попробовать сканер <Sparkles size={17} /></a><a class="button button-ghost" href="#videos"><Play size={16} fill="currentColor" /> Смотреть демо</a></div>
    </div>
    <div class="hero-exchanges"><span>ДАННЫЕ С ВЕДУЩИХ БИРЖ</span><div>{#each exchanges as exchange}<b>{exchange}</b>{/each}</div></div>
  </section>

  <section id="scanner" class="section scanner-section reveal"><div class="section-kicker">LIVE SCANNER <span></span> 00:05</div><div class="section-heading-row"><div><h2>Рынок, <em>разложенный</em><br />по полочкам</h2><p class="lead">Демо-лента показывает, как быстро находить маршрут с положительным чистым спредом.</p></div><div class="live-pill"><span></span> Данные обновляются</div></div>
    <div class="scanner-card glass"><div class="scanner-toolbar"><div class="filter-chips"><label>Сумма <select><option>1 000 USDT</option><option>5 000 USDT</option></select></label><label>Мин. спред <select bind:value={minSpread}><option value="0.5">0.5%</option><option value="1">1.0%</option><option value="2">2.0%</option></select></label><label>Биржа <select bind:value={exchangeFilter}><option>Все биржи</option>{#each exchanges as exchange}<option>{exchange}</option>{/each}</select></label><button class:active={onlyNet} onclick={() => onlyNet = !onlyNet}><ShieldCheck size={15} /> Только сеть доступна</button></div><div class="demo-note">Демо-данные. Не является торговой рекомендацией.</div></div><div class="table-scroll"><table><thead><tr><th>Монета</th><th>Купить на</th><th>Продать на</th><th>Цена покупки</th><th>Цена продажи</th><th>Спред</th><th>Чистый спред</th><th>Объём</th><th>Сеть</th><th>Перевод</th></tr></thead><tbody>{#each filteredRows as row}<tr><td class="pair">{row.pair}</td><td>{row.buy}</td><td>{row.sell}</td><td class="mono">{row.buyPrice}</td><td class="mono">{row.sellPrice}</td><td class="positive mono">+{row.spread}%</td><td><strong class="positive mono">+{row.net}%</strong></td><td class="mono">{row.volume}</td><td><span class="network">{row.network}</span></td><td class="muted">{row.time}</td></tr>{:else}<tr><td colspan="10" class="empty">По этим фильтрам спредов нет — попробуйте снизить порог.</td></tr>{/each}</tbody></table></div></div>
  </section>

  <section class="section reveal"><div class="section-heading"><div class="section-kicker">SCAN MODES</div><h2>Один рынок.<br /><em>Четыре</em> сценария.</h2><p class="lead">Выберите тип арбитража и смотрите только те возможности, которые подходят вашей стратегии.</p></div><div class="type-tabs">{#each Object.keys(scannerTypes) as type}<button class:active={selectedType === type} onclick={() => selectedType = type}><span>{scannerTypes[type].icon}</span>{type}<ChevronRight size={17} /></button>{/each}</div><div class="type-detail glass"><div class="type-icon">{scannerTypes[selectedType].icon}</div><div><div class="section-kicker">{scannerTypes[selectedType].title}</div><h3>{selectedType}</h3><p>{scannerTypes[selectedType].text}</p><div class="point-list">{#each scannerTypes[selectedType].points as point}<span><ShieldCheck size={15} /> {point}</span>{/each}</div></div><a href="#features" class="text-link">Подробнее <ArrowUpRight size={17} /></a></div></section>

  <section id="features" class="section reveal"><div class="section-heading centered"><div class="section-kicker">WHY BRO SCAN</div><h2>Инструменты для <em>ясных</em><br />решений</h2><p class="lead">Меньше ручной рутины. Больше контекста вокруг каждой возможности.</p></div><div class="feature-grid">{#each features as feature}<article class="feature-card glass"><div class="feature-icon"><Sparkles size={19} /></div><strong>{feature[0]}</strong><h3>{feature[1]}</h3><p>{feature[2]}</p><ArrowUpRight class="card-arrow" size={18} /></article>{/each}</div></section>

  <section class="section chart-section reveal"><div class="chart-copy"><div class="section-kicker">SPREAD INTELLIGENCE</div><h2>Видь не только<br /><em>цифру</em>, но и риск</h2><p class="lead">График собирает картину сделки: как меняется спред, где стоит ликвидность и сколько съедают комиссии.</p><div class="metric-list"><div><span>Спред входа / выхода</span><strong class="positive">1.43% <small>/ 0.98%</small></strong></div><div><span>Funding прогноз</span><strong>+0.018%</strong></div><div><span>Комиссии и сеть</span><strong>0.45 USDT</strong></div><div><span>Объём стакана</span><strong>38 420 USDT</strong></div></div></div><div class="chart-card glass"><div class="chart-top"><span>INJ / USDT · 1H</span><b class="positive">+1.43%</b></div><svg viewBox="0 0 600 260" preserveAspectRatio="none" aria-label="График спреда"><defs><linearGradient id="chartFill" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#C026D3" stop-opacity=".35"/><stop offset="1" stop-color="#C026D3" stop-opacity="0"/></linearGradient><linearGradient id="chartLine"><stop stop-color="#22D3EE"/><stop offset=".55" stop-color="#C026D3"/><stop offset="1" stop-color="#FB923C"/></linearGradient></defs><path d="M0 210 C45 180 68 205 105 156 S168 190 205 131 S261 113 298 147 S345 98 382 119 S430 175 462 89 S525 95 600 42V260H0Z" fill="url(#chartFill)"/><path d="M0 210 C45 180 68 205 105 156 S168 190 205 131 S261 113 298 147 S345 98 382 119 S430 175 462 89 S525 95 600 42" fill="none" stroke="url(#chartLine)" stroke-width="3"/><line x1="0" y1="65" x2="600" y2="65" stroke="#fff" stroke-opacity=".12" stroke-dasharray="5 7"/><line x1="0" y1="150" x2="600" y2="150" stroke="#fff" stroke-opacity=".08" stroke-dasharray="5 7"/></svg><div class="orderbook"><div><span class="sell-bar" style="width: 60%">0.014 · 320</span><span class="sell-bar" style="width: 43%">0.011 · 210</span><span class="sell-bar" style="width: 28%">0.008 · 124</span></div><div class="book-price">20.126 <small>best ask</small></div><div><span class="buy-bar" style="width: 38%">0.010 · 180</span><span class="buy-bar" style="width: 52%">0.016 · 270</span><span class="buy-bar" style="width: 70%">0.021 · 390</span></div></div></div></section>

  <section id="dex" class="section dex-section reveal"><div class="dex-glow"></div><div class="section-kicker">BEYOND CEX</div><div class="dex-layout"><div><h2>Данные из<br /><em>глубины</em> рынка</h2><p class="lead">Сверяем CEX с DEX и учитываем то, что обычно скрыто в мелком шрифте: сеть, газ, проскальзывание и время маршрута.</p><a class="button button-ghost" href="#pricing">Исследовать DEX <ArrowRight size={16} /></a></div><div class="dex-stats"><div><strong>13</strong><span>сетей</span></div><div><strong>10+</strong><span>CEX</span></div><div><strong>8</strong><span>агрегаторов</span></div></div></div><div class="network-cloud">{#each networks as network}<span>{network}</span>{/each}</div></section>

  <section class="section auto-section reveal"><div class="section-heading centered"><div class="section-kicker">AUTOMATION LAYER</div><h2>От сигнала к действию —<br /><em>под твоим контролем</em></h2><p class="lead">Настройте правила, а терминал покажет, как выглядит сценарий исполнения.</p></div><div class="flow glass"><div class="flow-step"><span>01</span><Zap size={22} /><h3>Спред достиг 1.2%</h3><p>Фильтры подтвердили объём и сеть</p></div><div class="flow-line"></div><div class="flow-step"><span>02</span><ArrowUpRight size={22} /><h3>Открытие позиций</h3><p>Две биржи, один рассчитанный маршрут</p></div><div class="flow-line"></div><div class="flow-step"><span>03</span><ShieldCheck size={22} /><h3>Закрытие при 0.4%</h3><p>Правило срабатывает по заданному уровню</p></div></div><div class="risk-note">Риск не исчезает от автоматизации. Используйте лимиты, тестируйте небольшим объёмом и учитывайте задержки сети.</div></section>

  <section class="section telegram-banner glass reveal"><div class="phone"><div class="phone-screen"><div class="telegram-head">BRO SCAN <span>● live</span></div><div class="telegram-message"><small>ARBITRAGE ALERT · 19:52</small><strong>INJ / USDT</strong><span>HTX → BYBIT</span><b>Чистый спред <i>+0.98%</i></b><small>Объём 380 USDT · ETH</small></div></div></div><div><div class="section-kicker">TELEGRAM BOTS</div><h2>Спреды приходят<br /><em>сами</em></h2><p class="lead">Получайте понятные алерты по межбиржевому, фьючерсному и DEX-арбитражу прямо в Telegram.</p><a class="button" href="#pricing" onclick={(event) => event.preventDefault()}>Открыть бота <ArrowUpRight size={16} /></a><small class="todo-label">TODO: подключить реальную ссылку на Telegram-бота</small></div></section>

  <section id="videos" class="section reveal"><div class="section-heading-row"><div><div class="section-kicker">START HERE</div><h2>Пять минут до <em>первого</em> сигнала</h2></div><a class="text-link" href="#faq">Все материалы <ArrowRight size={17} /></a></div><div class="video-row">{#each ['Как читать чистый спред', 'Фильтры без лишнего шума', 'DEX-маршрут за 3 шага', 'Funding и хеджирование'] as video, i}<button class="video-card" onclick={() => showVideo(video)}><div class="video-art art-{i + 1}"><span><Play size={22} fill="currentColor" /></span><small>0{i + 2}:4{i}</small></div><strong>{video}</strong><span>Разбор для старта <ArrowUpRight size={15} /></span></button>{/each}</div></section>

  <section id="pricing" class="section reveal"><div class="section-heading centered"><div class="section-kicker">PLANS FOR DIFFERENT PACE</div><h2>Выбери свой<br /><em>темп</em> арбитража</h2><p class="lead">Начните с одного сценария и расширяйтесь, когда появится уверенность.</p><div class="period-switch"><button class:active={activePlan === 'month'} onclick={() => activePlan = 'month'}>1 месяц</button><button class:active={activePlan === 'quarter'} onclick={() => activePlan = 'quarter'}>3 месяца <b>−15%</b></button><button class:active={activePlan === 'year'} onclick={() => activePlan = 'year'}>12 месяцев <b>−30%</b></button></div></div><div class="pricing-grid">{#each plans as plan}<article class:recommended={plan.recommended} class="price-card glass"><div class="price-top"><div><span class="plan-dot {plan.color}"></span><h3>{plan.name}</h3></div>{#if plan.recommended}<b class="recommend-badge">Выгодно</b>{/if}</div><p>{plan.desc}</p><div class="price"><span>$</span>{Math.round(plan.monthly * priceMultiplier)}<small>/ мес</small></div><div class="price-divider"></div><ul>{#each plan.items as item}<li><ShieldCheck size={15} /> {item}</li>{/each}</ul><a class:button={plan.recommended} class="plan-link" href="#top">Выбрать план <ArrowRight size={16} /></a></article>{/each}</div><p class="todo-label centered">TODO: заменить демо-цены на актуальные условия тарифа и подключить оплату</p></section>

  <section class="exchange-strip"><div class="strip-label">LIVE MARKET COVERAGE</div><div class="marquee">{#each [...exchanges, ...exchanges] as exchange}<span>{exchange}</span>{/each}</div><div class="marquee reverse">{#each [...networks, ...networks] as network}<span>{network}</span>{/each}</div></section>

  <section id="faq" class="section faq-section reveal"><div><div class="section-kicker">QUESTIONS, ANSWERED</div><h2>Осталось<br /><em>уточнить?</em></h2><p class="lead">Честно отвечаем о возможностях, ограничениях и рисках.</p><a class="text-link" href="#top">Вернуться к началу <ArrowUpRight size={16} /></a></div><div class="faq-list">{#each faqs as faq, i}<div class:open={openFaq === i} class="faq-item"><button aria-expanded={openFaq === i} onclick={() => openFaq = openFaq === i ? -1 : i}><span>{faq[0]}</span><ChevronDown size={18} /></button>{#if openFaq === i}<div class="faq-answer"><p>{faq[1]}</p></div>{/if}</div>{/each}</div></section>

  <section class="final-cta shell reveal"><div class="cta-stars"></div><div class="cta-cloud"></div><div class="section-kicker">READY WHEN YOU ARE</div><h2>Поймаешь следующий<br /><em>спред?</em></h2><p>Начни с демо-ленты и посмотри, сколько контекста можно увидеть за один экран.</p><a class="button" href="#scanner">Начать сейчас <Sparkles size={16} /></a><a class="cta-telegram" href="#faq">Telegram-канал <ArrowUpRight size={15} /></a></section>
</main>

<footer class="footer"><div class="footer-main"><a class="brand" href="#top"><span class="brand-mark"><ArrowUpRight size={18} /></span><span>Arbitrage Bro <b>Scan</b></span></a><p>Сканер, который переводит шум рынка<br />в понятные возможности.</p><div class="footer-links">{#each navLinks as link}<a href={link.href}>{link.label}</a>{/each}</div><a class="footer-support" href="#faq">Telegram поддержки <ArrowUpRight size={15} /></a></div><div class="footer-bottom"><span>© 2026 Arbitrage Bro Scan</span><span>Криптовалютный арбитраж связан с рисками. Информация не является инвестиционной рекомендацией.</span><span><a href="#top">Политика</a> · <a href="#top">Условия</a></span></div></footer>

{#if videoOpen}<div class="modal-backdrop" role="presentation" onclick={(event) => { if (event.currentTarget === event.target) videoOpen = false }}><div class="video-modal" role="dialog" aria-modal="true" aria-label={selectedVideo}><button class="modal-close" aria-label="Закрыть" onclick={() => videoOpen = false}><X size={20} /></button><div class="embed-placeholder"><Play size={34} fill="currentColor" /><span>Здесь появится видео «{selectedVideo}»</span><small>TODO: добавить YouTube embed ID</small></div></div></div>{/if}
