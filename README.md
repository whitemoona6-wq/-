/* ===== Сброс и база ===== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --bg: #0a0a0f;
  --bg-card: #12121c;
  --bg-card-2: #0e0e17;
  --border: #1f1f30;
  --border-light: #2a2a3e;
  --border-hover: #3a3a4e;
  --text: #eaeef2;
  --text-muted: #9a9ab0;
  --text-dim: #7a7a90;
  --gold: #d4af37;
  --gold-light: #f0c27b;
  --gold-dark: #b8860b;
}

body {
  font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
}

/* ===== Шапка ===== */
header {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 5%;
  background: rgba(10, 10, 15, 0.85);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border-light);
  flex-wrap: wrap;
  gap: 1rem;
}

.logo {
  font-size: 1.9rem;
  font-weight: 900;
  letter-spacing: 4px;
  background: linear-gradient(135deg, var(--gold-light), var(--gold), var(--gold-dark));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-transform: uppercase;
}

.logo span {
  font-size: 0.7rem;
  display: block;
  letter-spacing: 8px;
  font-weight: 300;
  -webkit-text-fill-color: var(--text-muted);
  background: none;
}

.cart-btn {
  position: relative;
  background: #1e1e2e;
  border: 1px solid var(--border-hover);
  color: var(--text);
  padding: 0.6rem 1.4rem;
  border-radius: 40px;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: 0.2s;
  display: flex;
  align-items: center;
  gap: 8px;
}

.cart-btn:hover {
  background: #2a2a3e;
  border-color: var(--gold);
}

.cart-count {
  background: var(--gold);
  color: var(--bg);
  border-radius: 50%;
  width: 24px;
  height: 24px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 0.85rem;
  font-weight: 700;
}

/* ===== Hero ===== */
.hero {
  padding: 5rem 5% 4rem;
  text-align: center;
  background: radial-gradient(circle at 30% 20%, #1a1a2e, var(--bg) 70%);
  border-bottom: 1px solid var(--border);
}

.hero h1 {
  font-size: clamp(2.2rem, 8vw, 4.5rem);
  font-weight: 900;
  letter-spacing: 4px;
  background: linear-gradient(135deg, #fff, var(--gold), #8b6914);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.8rem;
}

.hero p {
  color: var(--text-muted);
  font-size: 1.2rem;
  max-width: 650px;
  margin: 0 auto;
  letter-spacing: 1px;
}

.gold-line {
  width: 100px;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
  margin: 1.5rem auto 0;
}

/* ===== Фильтры ===== */
.filters {
  display: flex;
  flex-wrap: wrap;
  gap: 0.8rem;
  justify-content: center;
  padding: 2rem 5% 1rem;
  max-width: 1200px;
  margin: 0 auto;
}

.filter-btn {
  background: transparent;
  border: 1px solid var(--border-hover);
  color: var(--text-muted);
  padding: 0.5rem 1.3rem;
  border-radius: 30px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 500;
  transition: 0.25s;
  letter-spacing: 0.5px;
}

.filter-btn:hover {
  border-color: var(--gold);
  color: var(--text);
}

.filter-btn.active {
  background: var(--gold);
  color: var(--bg);
  border-color: var(--gold);
  font-weight: 700;
}

/* ===== Каталог ===== */
.catalog {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
  padding: 2rem 5% 5rem;
  max-width: 1400px;
  margin: 0 auto;
}

.car-card {
  background: linear-gradient(145deg, var(--bg-card), var(--bg-card-2));
  border: 1px solid var(--border);
  border-radius: 20px;
  overflow: hidden;
  transition: 0.3s ease;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
}

.car-card:hover {
  transform: translateY(-6px);
  border-color: var(--gold);
  box-shadow: 0 20px 40px rgba(212, 175, 55, 0.08);
}

.car-image {
  width: 100%;
  height: 210px;
  background: var(--bg);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 5rem;
  border-bottom: 1px solid var(--border);
  position: relative;
}

.car-badge {
  position: absolute;
  top: 14px;
  left: 14px;
  background: rgba(212, 175, 55, 0.9);
  color: var(--bg);
  padding: 0.3rem 0.9rem;
  border-radius: 20px;
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.car-info {
  padding: 1.3rem 1.3rem 1.5rem;
  display: flex;
  flex-direction: column;
  flex: 1;
}

.car-brand {
  font-size: 0.75rem;
  color: var(--gold);
  letter-spacing: 2px;
  text-transform: uppercase;
  font-weight: 600;
  margin-bottom: 4px;
}

.car-name {
  font-size: 1.3rem;
  font-weight: 700;
  margin-bottom: 0.6rem;
  letter-spacing: 0.5px;
}

.car-specs {
  display: flex;
  gap: 1rem;
  font-size: 0.8rem;
  color: var(--text-muted);
  margin-bottom: 1.2rem;
  flex-wrap: wrap;
}

.car-specs span {
  display: flex;
  align-items: center;
  gap: 4px;
}

.car-price-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: auto;
}

.car-price {
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--gold-light);
  letter-spacing: 1px;
}

.car-price small {
  font-size: 0.75rem;
  color: var(--text-dim);
  font-weight: 400;
  display: block;
  letter-spacing: 0;
}

.add-btn {
  background: var(--gold);
  border: none;
  color: var(--bg);
  padding: 0.7rem 1.3rem;
  border-radius: 30px;
  font-weight: 700;
  font-size: 0.9rem;
  cursor: pointer;
  transition: 0.2s;
  letter-spacing: 0.5px;
}

.add-btn:hover {
  background: var(--gold-light);
  transform: scale(1.03);
}

.add-btn:active {
  transform: scale(0.97);
}

.empty-catalog {
  grid-column: 1 / -1;
  text-align: center;
  color: var(--text-dim);
  padding: 4rem 1rem;
  font-size: 1.2rem;
}

/* ===== Модальное окно корзины ===== */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(6px);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 200;
  padding: 1rem;
}

.modal-overlay.open {
  display: flex;
}

.cart-modal {
  background: var(--bg-card);
  border: 1px solid var(--border-light);
  border-radius: 24px;
  max-width: 600px;
  width: 100%;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.8);
  animation: fadeIn 0.25s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px) scale(0.97); }
  to { opacity: 1; transform: translateY(0) scale(1); }
}

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.3rem 1.5rem;
  border-bottom: 1px solid var(--border-light);
}

.cart-header h2 {
  font-size: 1.3rem;
  letter-spacing: 1px;
  font-weight: 700;
}

.close-btn {
  background: none;
  border: none;
  color: var(--text-muted);
  font-size: 1.8rem;
  cursor: pointer;
  transition: 0.2s;
  line-height: 1;
  padding: 0 6px;
}

.close-btn:hover {
  color: var(--gold);
}

.cart-items {
  padding: 1rem 1.5rem;
  overflow-y: auto;
  flex: 1;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.9rem 0;
  border-bottom: 1px solid var(--border);
  gap: 1rem;
}

.cart-item:last-child {
  border-bottom: none;
}

.cart-item-info {
  flex: 1;
}

.cart-item-name {
  font-weight: 600;
  font-size: 1rem;
}

.cart-item-price {
  color: var(--gold);
  font-size: 0.9rem;
  font-weight: 600;
}

.cart-item-qty {
  display: flex;
  align-items: center;
  gap: 10px;
}

.qty-btn {
  background: #1e1e2e;
  border: 1px solid var(--border-hover);
  color: var(--text);
  width: 30px;
  height: 30px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.2s;
}

.qty-btn:hover {
  border-color: var(--gold);
  color: var(--gold);
}

.cart-item-remove {
  background: none;
  border: none;
  color: #7a3a3a;
  cursor: pointer;
  font-size: 1.2rem;
  transition: 0.2s;
  padding: 4px;
}

.cart-item-remove:hover {
  color: #e74c3c;
}

.cart-empty {
  text-align: center;
  color: var(--text-dim);
  padding: 2.5rem 1rem;
  font-size: 1.1rem;
}

.cart-footer {
  padding: 1.3rem 1.5rem;
  border-top: 1px solid var(--border-light);
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.cart-total {
  display: flex;
  justify-content: space-between;
  font-size: 1.2rem;
  font-weight: 700;
}

.cart-total span:last-child {
  color: var(--gold-light);
}

.checkout-btn {
  background: linear-gradient(135deg, var(--gold), var(--gold-dark));
  border: none;
  color: var(--bg);
  padding: 0.9rem;
  border-radius: 40px;
  font-weight: 800;
  font-size: 1rem;
  letter-spacing: 1px;
  cursor: pointer;
  transition: 0.25s;
  text-transform: uppercase;
}

.checkout-btn:hover {
  opacity: 0.9;
  transform: scale(1.01);
}

.checkout-btn:disabled {
  opacity: 0.35;
  cursor: not-allowed;
  transform: none;
}

/* ===== Уведомление ===== */
.toast {
  position: fixed;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: #1e1e2e;
  border: 1px solid var(--gold);
  color: var(--text);
  padding: 0.9rem 2rem;
  border-radius: 40px;
  font-weight: 600;
  z-index: 300;
  opacity: 0;
  transition: 0.35s ease;
  pointer-events: none;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
  white-space: nowrap;
}

.toast.show {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

/* ===== Футер ===== */
footer {
  text-align: center;
  padding: 2.5rem 5%;
  border-top: 1px solid var(--border);
  color: #5a5a70;
  font-size: 0.85rem;
  letter-spacing: 1px;
}

footer strong {
  color: var(--gold);
  font-weight: 600;
}

/* ===== Скроллбар ===== */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: var(--bg);
}

::-webkit-scrollbar-thumb {
  background: var(--border-light);
  border-radius: 10px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--gold);
}

/* ===== Адаптивность ===== */
@media (max-width: 600px) {
  header { padding: 0.8rem 4%; }
  .logo { font-size: 1.4rem; letter-spacing: 2px; }
  .logo span { letter-spacing: 4px; font-size: 0.55rem; }
  .hero { padding: 3rem 4% 2.5rem; }
  .catalog {
    grid-template-columns: 1fr;
    padding: 1.5rem 4% 3rem;
    gap: 1.5rem;
  }
  .cart-modal { max-height: 90vh; }
  .cart-item { flex-wrap: wrap; }
  .toast {
    white-space: normal;
    text-align: center;
    width: 90%;
    font-size: 0.9rem;
  }
}
