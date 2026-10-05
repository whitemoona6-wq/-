from flask import Flask, render_template, request, jsonify, session
import secrets

app = Flask(__name__)
app.secret_key = secrets.token_hex(32)

# ---- База данных гиперкаров (в реальном проекте — БД) ----
CARS = [
    {"id": 1, "brand": "Bugatti", "name": "Chiron Super Sport 300+",
     "price": 3900000, "power": 1600, "speed": 490, "year": 2023,
     "emoji": "🏎️", "badge": "Limited"},
    {"id": 2, "brand": "Koenigsegg", "name": "Jesko Absolut",
     "price": 3400000, "power": 1600, "speed": 531, "year": 2024,
     "emoji": "⚡", "badge": "New"},
    {"id": 3, "brand": "Pagani", "name": "Huayra R",
     "price": 3100000, "power": 850, "speed": 380, "year": 2023,
     "emoji": "🔥", "badge": "Track"},
    {"id": 4, "brand": "Rimac", "name": "Nevera",
     "price": 2400000, "power": 1914, "speed": 412, "year": 2024,
     "emoji": "⚡", "badge": "Electric"},
    {"id": 5, "brand": "McLaren", "name": "Speedtail",
     "price": 2250000, "power": 1070, "speed": 403, "year": 2022,
     "emoji": "🏁", "badge": "Hybrid"},
    {"id": 6, "brand": "Bugatti", "name": "Bolide",
     "price": 4700000, "power": 1850, "speed": 500, "year": 2024,
     "emoji": "🏎️", "badge": "Track Only"},
    {"id": 7, "brand": "Koenigsegg", "name": "Gemera",
     "price": 1700000, "power": 1700, "speed": 400, "year": 2024,
     "emoji": "👑", "badge": "Hybrid"},
    {"id": 8, "brand": "Pagani", "name": "Utopia",
     "price": 2500000, "power": 864, "speed": 370, "year": 2024,
     "emoji": "✨", "badge": "New"},
    {"id": 9, "brand": "Rimac", "name": "Nevera R",
     "price": 2800000, "power": 2107, "speed": 412, "year": 2025,
     "emoji": "⚡", "badge": "Electric"},
]

BRANDS = ["Bugatti", "Koenigsegg", "Pagani", "Rimac", "McLaren"]


def get_cart():
    """Получить корзину из сессии."""
    return session.setdefault("cart", {})


def car_by_id(car_id):
    """Найти машину по id."""
    return next((c for c in CARS if c["id"] == car_id), None)


# ---------- Страницы ----------
@app.route("/")
def index():
    return render_template("index.html", cars=CARS, brands=BRANDS)


# ---------- API ----------
@app.route("/api/cars")
def api_cars():
    """Все машины с учётом фильтра по бренду."""
    brand = request.args.get("brand", "all")
    if brand == "all":
        return jsonify(CARS)
    return jsonify([c for c in CARS if c["brand"] == brand])


@app.route("/api/cart", methods=["GET"])
def api_cart_get():
    """Текущая корзина с полными данными."""
    cart = get_cart()
    items = []
    total = 0
    for car_id_str, qty in cart.items():
        car = car_by_id(int(car_id_str))
        if not car:
            continue
        item_total = car["price"] * qty
        total += item_total
        items.append({**car, "qty": qty, "item_total": item_total})
    return jsonify({
        "items": items,
        "total": total,
        "count": sum(cart.values()),
    })


@app.route("/api/cart/add", methods=["POST"])
def api_cart_add():
    """Добавить машину в корзину."""
    data = request.get_json(silent=True) or {}
    car_id = data.get("id")
    car = car_by_id(car_id)
    if not car:
        return jsonify({"error": "Автомобиль не найден"}), 404

    cart = get_cart()
    key = str(car_id)
    cart[key] = cart.get(key, 0) + 1
    session.modified = True
    return jsonify({"success": True, "count": sum(cart.values())})


@app.route("/api/cart/update", methods=["POST"])
def api_cart_update():
    """Изменить количество (+1 / -1)."""
    data = request.get_json(silent=True) or {}
    car_id = str(data.get("id"))
    delta = int(data.get("delta", 0))

    cart = get_cart()
    if car_id not in cart:
        return jsonify({"error": "Позиция не найдена"}), 404

    cart[car_id] += delta
    if cart[car_id] <= 0:
        del cart[car_id]
    session.modified = True
    return jsonify({"success": True, "count": sum(cart.values())})


@app.route("/api/cart/remove", methods=["POST"])
def api_cart_remove():
    """Полностью удалить позицию."""
    data = request.get_json(silent=True) or {}
    car_id = str(data.get("id"))
    cart = get_cart()
    cart.pop(car_id, None)
    session.modified = True
    return jsonify({"success": True, "count": sum(cart.values())})


@app.route("/api/cart/clear", methods=["POST"])
def api_cart_clear():
    """Очистить корзину."""
    session["cart"] = {}
    session.modified = True
    return jsonify({"success": True, "count": 0})


@app.route("/api/checkout", methods=["POST"])
def api_checkout():
    """Оформить заказ."""
    cart = get_cart()
    if not cart:
        return jsonify({"error": "Корзина пуста"}), 400
    total_items = sum(cart.values())
    session["cart"] = {}
    session.modified = True
    return jsonify({
        "success": True,
        "message": f"Заказ на {total_items} авто оформлен! Менеджер свяжется с вами.",
    })


if __name__ == "__main__":
    app.run(debug=True)
