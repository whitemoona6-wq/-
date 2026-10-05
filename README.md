<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HYPERCARS | Эксклюзивные гиперкары</title>
  <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>

  <header>
    <div class="logo">
      HYPERCARS
      <span>EXCLUSIVE MOTORS</span>
    </div>
    <button class="cart-btn" id="cartBtn">
      🛒 Корзина <span class="cart-count" id="cartCount">0</span>
    </button>
  </header>

  <section class="hero">
    <h1>ГИПЕРКАРЫ МЕЧТЫ</h1>
    <p>Эксклюзивные автомобили с двигателями мощностью 1000+ л.с. Ограниченные серии. Доставка по всему миру.</p>
    <div class="gold-line"></div>
  </section>

  <div class="filters" id="filters">
    <button class="filter-btn active" data-filter="all">Все</button>
    {% for brand in brands %}
      <button class="filter-btn" data-filter="{{ brand }}">{{ brand }}</button>
    {% endfor %}
  </div>

  <main class="catalog" id="catalog"></main>

  <footer>
    <strong>HYPERCARS</strong> © 2025 — Эксклюзивные автомобили. Все права защищены.
  </footer>

  <div class="modal-overlay" id="modalOverlay">
    <div class="cart-modal">
      <div class="cart-header">
        <h2>🛒 Ваша корзина</h2>
        <button class="close-btn" id="closeModalBtn">&times;</button>
      </div>
      <div class="cart-items" id="cartItems"></div>
      <div class="cart-footer" id="cartFooter">
        <div class="cart-total">
          <span>Итого:</span>
          <span id="cartTotal">$0</span>
        </div>
        <button class="checkout-btn" id="checkoutBtn">Оформить заказ</button>
      </div>
    </div>
  </div>

  <div class="toast" id="toast"></div>

  <script>
    const catalogEl = document.getElementById('catalog');
    const cartCountEl = document.getElementById('cartCount');
    const cartBtn = document.getElementById('cartBtn');
    const modalOverlay = document.getElementById('modalOverlay');
    const closeModalBtn = document.getElementById('closeModalBtn');
    const cartItemsEl = document.getElementById('cartItems');
    const cartTotalEl = document.getElementById('cartTotal');
    const checkoutBtn = document.getElementById('checkoutBtn');
    const toastEl = document.getElementById('toast');
    const filtersEl = document.getElementById('filters');

    let activeFilter = 'all';

    const fmt = n => '$' + n.toLocaleString('en-US');

    function showToast(msg) {
      toastEl.textContent = msg;
      toastEl.classList.add('show');
      clearTimeout(toastEl._t);
      toastEl._t = setTimeout(() => toastEl.classList.remove('show'), 2200);
    }

    // ---- Каталог ----
    async function loadCars(brand = 'all') {
      const res = await fetch(`/api/cars?brand=${brand}`);
      const cars = await res.json();

      if (!cars.length) {
        catalogEl.innerHTML = '<div class="empty-catalog">😔 В этой категории пока нет автомобилей</div>';
        return;
      }

      catalogEl.innerHTML = cars.map(car => `
        <div class="car-card">
          <div class="car-image">
            ${car.emoji}
            <span class="car-badge">${car.badge}</span>
          </div>
          <div class="car-info">
            <div class="car-brand">${car.brand}</div>
            <div class="car-name">${car.name}</div>
            <div class="car-specs">
              <span>⚡ ${car.power} л.с.</span>
              <span>🏁 ${car.speed} км/ч</span>
              <span>📅 ${car.year}</span>
            </div>
            <div class="car-price-row">
              <div class="car-price">
                ${fmt(car.price)}
                <small>без налогов</small>
              </div>
              <button class="add-btn" data-id="${car.id}" data-name="${car.name}">В корзину</button>
            </div>
          </div>
        </div>
      `).join('');
    }

    // ---- Корзина ----
    async function loadCart() {
      const res = await fetch('/api/cart');
      const data = await res.json();
      cartCountEl.textContent = data.count;

      if (!data.items.length) {
        cartItemsEl.innerHTML = '<div class="cart-empty">Корзина пуста<br><span style="font-size:0.85rem;color:#5a5a70;">Добавьте гиперкар мечты</span></div>';
        cartTotalEl.textContent = '$0';
        checkoutBtn.disabled = true;
        return;
      }

      cartItemsEl.innerHTML = data.items.map(item => `
        <div class="cart-item">
          <div class="cart-item-info">
            <div class="cart-item-name">${item.emoji} ${item.name}</div>
            <div class="cart-item-price">${fmt(item.price)} × ${item.qty} = ${fmt(item.item_total)}</div>
          </div>
          <div class="cart-item-qty">
            <button class="qty-btn" data-action="dec" data-id="${item.id}">−</button>
            <span style="min-width:20px;text-align:center;font-weight:600;">${item.qty}</span>
            <button class="qty-btn" data-action="inc" data-id="${item.id}">+</button>
          </div>
          <button class="cart-item-remove" data-action="remove" data-id="${item.id}">✕</button>
        </div>
      `).join('');

      cartTotalEl.textContent = fmt(data.total);
      checkoutBtn.disabled = false;
    }

    // ---- События ----
    catalogEl.addEventListener('click', async (e) => {
      const btn = e.target.closest('.add-btn');
      if (!btn) return;
      const id = Number(btn.dataset.id);
      await fetch('/api/cart/add', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ id })
      });
      showToast(`✅ ${btn.dataset.name} добавлен в корзину`);
      loadCart();
    });

    filtersEl.addEventListener('click', (e) => {
      const btn = e.target.closest('.filter-btn');
      if (!btn) return;
      document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      activeFilter = btn.dataset.filter;
      loadCars(activeFilter);
    });

    cartBtn.addEventListener('click', () => {
      loadCart();
      modalOverlay.classList.add('open');
      document.body.style.overflow = 'hidden';
    });

    closeModalBtn.addEventListener('click', closeModal);
    modalOverlay.addEventListener('click', e => {
      if (e.target === modalOverlay) closeModal();
    });

    function closeModal() {
      modalOverlay.classList.remove('open');
      document.body.style.overflow = '';
    }

    cartItemsEl.addEventListener('click', async (e) => {
      const btn = e.target.closest('button');
      if (!btn) return;
      const action = btn.dataset.action;
      const id = Number(btn.dataset.id);

      if (action === 'inc' || action === 'dec') {
        const delta = action === 'inc' ? 1 : -1;
        await fetch('/api/cart/update', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ id, delta })
        });
      } else if (action === 'remove') {
        await fetch('/api/cart/remove', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ id })
        });
        showToast('🗑️ Автомобиль удалён из корзины');
      }
      loadCart();
    });

    checkoutBtn.addEventListener('click', async () => {
      const res = await fetch('/api/checkout', { method: 'POST' });
      const data = await res.json();
      if (data.success) {
        showToast('🏁 ' + data.message);
        loadCart();
        setTimeout(closeModal, 1200);
      }
    });

    document.addEventListener('keydown', e => {
      if (e.key === 'Escape' && modalOverlay.classList.contains('open')) closeModal();
    });

    // ---- Инициализация ----
    loadCars();
    loadCart();
  </script>

</body>
</html>
