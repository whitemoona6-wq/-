<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HYPERCARS | Магазин гиперкаров</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet">
</head>
<body>
    <!-- Header -->
    <header class="header">
        <div class="container header-content">
            <div class="logo">
                <span class="logo-icon">⚡</span>
                <span class="logo-text">HYPERCARS</span>
            </div>
            <nav class="nav">
                <a href="#catalog" class="nav-link">Каталог</a>
                <a href="#about" class="nav-link">О нас</a>
                <a href="#contact" class="nav-link">Контакты</a>
            </nav>
            <button class="cart-btn" id="cartBtn">
                <span class="cart-icon">🛒</span>
                <span class="cart-count" id="cartCount">0</span>
            </button>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="hero">
        <div class="hero-overlay"></div>
        <div class="container hero-content">
            <h1 class="hero-title">СКОРОСТЬ БЕЗ ГРАНИЦ</h1>
            <p class="hero-subtitle">Эксклюзивные гиперкары со всего мира. Скорость, мощь, роскошь.</p>
            <a href="#catalog" class="hero-btn">Смотреть каталог</a>
        </div>
    </section>

    <!-- Stats Section -->
    <section class="stats">
        <div class="container stats-grid">
            <div class="stat-item">
                <div class="stat-value">8+</div>
                <div class="stat-label">Моделей в наличии</div>
            </div>
            <div class="stat-item">
                <div class="stat-value">531</div>
                <div class="stat-label">км/ч максимальная скорость</div>
            </div>
            <div class="stat-item">
                <div class="stat-value">1914</div>
                <div class="stat-label">л.с. максимальная мощность</div>
            </div>
            <div class="stat-item">
                <div class="stat-value">1.85</div>
                <div class="stat-label">сек до 100 км/ч</div>
            </div>
        </div>
    </section>

    <!-- Catalog Section -->
    <section class="catalog" id="catalog">
        <div class="container">
            <div class="section-header">
                <h2 class="section-title">КАТАЛОГ ГИПЕРКАРОВ</h2>
                <div class="filter-controls">
                    <label for="sortSelect">Сортировка:</label>
                    <select id="sortSelect" class="sort-select">
                        <option value="default">По умолчанию</option>
                        <option value="price_asc">Цена: по возрастанию</option>
                        <option value="price_desc">Цена: по убыванию</option>
                        <option value="speed">Макс. скорость</option>
                        <option value="acceleration">Разгон</option>
                    </select>
                </div>
            </div>
            
            <div class="cars-grid" id="carsGrid">
                {% for car in hypercars %}
                <div class="car-card" data-id="{{ car.id }}">
                    <div class="car-image">
                        <img src="{{ car.image }}" alt="{{ car.name }}" loading="lazy">
                        <div class="car-year">{{ car.year }}</div>
                        <div class="car-country">{{ car.country }}</div>
                    </div>
                    <div class="car-info">
                        <h3 class="car-name">{{ car.name }}</h3>
                        <p class="car-description">{{ car.description }}</p>
                        
                        <div class="car-specs">
                            <div class="spec">
                                <span class="spec-icon">🏁</span>
                                <span class="spec-value">{{ car.top_speed }}</span>
                                <span class="spec-label">км/ч</span>
                            </div>
                            <div class="spec">
                                <span class="spec-icon">⚡</span>
                                <span class="spec-value">{{ car.acceleration }}</span>
                                <span class="spec-label">сек</span>
                            </div>
                            <div class="spec">
                                <span class="spec-icon">🔥</span>
                                <span class="spec-value">{{ car.power }}</span>
                                <span class="spec-label">л.с.</span>
                            </div>
                        </div>
                        
                        <div class="car-engine">{{ car.engine }}</div>
                        
                        <div class="car-footer">
                            <div class="car-price">
                                <span class="price-label">Цена</span>
                                <span class="price-value">${{ "{:,}".format(car.price) }}</span>
                            </div>
                            <button class="buy-btn" onclick="addToCart({{ car.id }})">
                                В корзину
                            </button>
                        </div>
                    </div>
                </div>
                {% endfor %}
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section class="about" id="about">
        <div class="container">
            <div class="about-content">
                <div class="about-text">
                    <h2 class="section-title">О НАС</h2>
                    <p>HYPERCARS — это эксклюзивный магазин гиперкаров, где мечты становятся реальностью. Мы предлагаем самые быстрые и мощные автомобили в мире от ведущих производителей.</p>
                    <p>Каждый автомобиль в нашем каталоге — это произведение инженерного искусства, сочетающее в себе невероятную мощность, передовые технологии и роскошь.</p>
                    <ul class="about-list">
                        <li>✓ Официальный дилер ведущих брендов</li>
                        <li>✓ Полное сопровождение сделки</li>
                        <li>✓ Доставка по всему миру</li>
                        <li>✓ Персональный менеджер</li>
                    </ul>
                </div>
                <div class="about-image">
                    <img src="https://images.unsplash.com/photo-1503376780353-7e6692767b70?w=800" alt="О нас">
                </div>
            </div>
        </div>
    </section>

    <!-- Contact Section -->
    <section class="contact" id="contact">
        <div class="container">
            <h2 class="section-title">СВЯЖИТЕСЬ С НАМИ</h2>
            <div class="contact-grid">
                <div class="contact-item">
                    <div class="contact-icon">📍</div>
                    <h3>Адрес</h3>
                    <p>Москва, ул. Автомобильная, 1</p>
                </div>
                <div class="contact-item">
                    <div class="contact-icon">📞</div>
                    <h3>Телефон</h3>
                    <p>+7 (800) 555-35-35</p>
                </div>
                <div class="contact-item">
                    <div class="contact-icon">✉️</div>
                    <h3>Email</h3>
                    <p>info@hypercars.ru</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
        <div class="container">
            <p>&copy; 2024 HYPERCARS. Все права защищены.</p>
        </div>
    </footer>

    <!-- Cart Modal -->
    <div class="cart-modal" id="cartModal">
        <div class="cart-modal-content">
            <div class="cart-header">
                <h2>Корзина</h2>
                <button class="close-btn" id="closeCart">&times;</button>
            </div>
            <div class="cart-items" id="cartItems">
                <p class="empty-cart">Корзина пуста</p>
            </div>
            <div class="cart-footer" id="cartFooter">
                <div class="cart-total">
                    <span>Итого:</span>
                    <span id="cartTotal">$0</span>
                </div>
                <button class="checkout-btn" id="checkoutBtn">Оформить заказ</button>
            </div>
        </div>
    </div>

    <!-- Notification -->
    <div class="notification" id="notification"></div>

    <script>
        // Корзина
        let cart = [];

        // Загрузка корзины при старте
        async function loadCart() {
            try {
                const response = await fetch('/api/cart');
                cart = await response.json();
                updateCartUI();
            } catch (error) {
                console.error('Error loading cart:', error);
            }
        }

        // Добавление в корзину
        async function addToCart(carId) {
            try {
                const response = await fetch('/api/cart', {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({car_id: carId})
                });
                const data = await response.json();
                if (data.success) {
                    cart = data.cart;
                    updateCartUI();
                    showNotification('Автомобиль добавлен в корзину!', 'success');
                }
            } catch (error) {
                console.error('Error adding to cart:', error);
                showNotification('Ошибка при добавлении', 'error');
            }
        }

        // Удаление из корзины
        async function removeFromCart(carId) {
            try {
                const response = await fetch('/api/cart', {
                    method: 'DELETE',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({car_id: carId})
                });
                const data = await response.json();
                if (data.success) {
                    cart = data.cart;
                    updateCartUI();
                    showNotification('Автомобиль удалён из корзины', 'info');
                }
            } catch (error) {
                console.error('Error removing from cart:', error);
            }
        }

        // Обновление UI корзины
        function updateCartUI() {
            const cartCount = document.getElementById('cartCount');
            const cartItems = document.getElementById('cartItems');
            const cartTotal = document.getElementById('cartTotal');
            const cartFooter = document.getElementById('cartFooter');

            cartCount.textContent = cart.length;

            if (cart.length === 0) {
                cartItems.innerHTML = '<p class="empty-cart">Корзина пуста</p>';
                cartFooter.style.display = 'none';
                return;
            }

            cartFooter.style.display = 'block';
            cartItems.innerHTML = cart.map(car => `
                <div class="cart-item">
                    <img src="${car.image}" alt="${car.name}">
                    <div class="cart-item-info">
                        <h4>${car.name}</h4>
                        <p>$${car.price.toLocaleString()}</p>
                    </div>
                    <button class="remove-btn" onclick="removeFromCart(${car.id})">&times;</button>
                </div>
            `).join('');

            const total = cart.reduce((sum, car) => sum + car.price, 0);
            cartTotal.textContent = '$' + total.toLocaleString();
        }

        // Уведомления
        function showNotification(message, type = 'info') {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type} show`;
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Оформление заказа
        document.getElementById('checkoutBtn').addEventListener('click', async () => {
            try {
                const response = await fetch('/api/checkout', {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'}
                });
                const data = await response.json();
                
                if (data.success) {
                    cart = [];
                    updateCartUI();
                    closeCartModal();
                    showNotification('Заказ успешно оформлен! Наш менеджер свяжется с вами.', 'success');
                } else {
                    showNotification(data.error || 'Ошибка оформления', 'error');
                }
            } catch (error) {
                console.error('Checkout error:', error);
                showNotification('Ошибка при оформлении заказа', 'error');
            }
        });

        // Управление модальным окном корзины
        const cartModal = document.getElementById('cartModal');
        const cartBtn = document.getElementById('cartBtn');
        const closeCart = document.getElementById('closeCart');

        cartBtn.addEventListener('click', () => {
            cartModal.classList.add('show');
        });

        closeCart.addEventListener('click', closeCartModal);

        function closeCartModal() {
            cartModal.classList.remove('show');
        }

        cartModal.addEventListener('click', (e) => {
            if (e.target === cartModal) {
                closeCartModal();
            }
        });

        // Сортировка
        document.getElementById('sortSelect').addEventListener('change', async (e) => {
            const sortBy = e.target.value;
            try {
                const response = await fetch(`/api/hypercars?sort=${sortBy}`);
                const cars = await response.json();
                renderCars(cars);
            } catch (error) {
                console.error('Error sorting:', error);
            }
        });

        // Рендер карточек автомобилей
        function renderCars(cars) {
            const grid = document.getElementById('carsGrid');
            grid.innerHTML = cars.map(car => `
                <div class="car-card" data-id="${car.id}">
                    <div class="car-image">
                        <img src="${car.image}" alt="${car.name}" loading="lazy">
                        <div class="car-year">${car.year}</div>
                        <div class="car-country">${car.country}</div>
                    </div>
                    <div class="car-info">
                        <h3 class="car-name">${car.name}</h3>
                        <p class="car-description">${car.description}</p>
                        <div class="car-specs">
                            <div class="spec">
                                <span class="spec-icon">🏁</span>
                                <span class="spec-value">${car.top_speed}</span>
                                <span class="spec-label">км/ч</span>
                            </div>
                            <div class="spec">
                                <span class="spec-icon">⚡</span>
                                <span class="spec-value">${car.acceleration}</span>
                                <span class="spec-label">сек</span>
                            </div>
                            <div class="spec">
                                <span class="spec-icon">🔥</span>
                                <span class="spec-value">${car.power}</span>
                                <span class="spec-label">л.с.</span>
                            </div>
                        </div>
                        <div class="car-engine">${car.engine}</div>
                        <div class="car-footer">
                            <div class="car-price">
                                <span class="price-label">Цена</span>
                                <span class="price-value">$${car.price.toLocaleString()}</span>
                            </div>
                            <button class="buy-btn" onclick="addToCart(${car.id})">
                                В корзину
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Плавная прокрутка
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function(e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({behavior: 'smooth', block: 'start'});
                }
            });
        });

        // Анимация появления при скролле
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -50px 0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, observerOptions);

        document.querySelectorAll('.car-card, .stat-item, .contact-item').forEach(el => {
            observer.observe(el);
        });

        // Инициализация
        loadCart();
    </script>
</body>
</html>
