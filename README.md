from flask import Flask, render_template, jsonify, request
from datetime import datetime

app = Flask(__name__)

# Данные о гиперкарах
hypercars = [
    {
        'id': 1,
        'name': 'Bugatti Chiron Super Sport 300+',
        'price': 3900000,
        'top_speed': 490,
        'acceleration': 2.3,
        'power': 1600,
        'engine': '8.0L W16 Quad-Turbo',
        'year': 2021,
        'country': 'France',
        'image': 'https://images.unsplash.com/photo-1544636331-e26879cd4d9b?w=800',
        'description': 'Легендарный гиперкар, преодолевший барьер 300 миль/ч'
    },
    {
        'id': 2,
        'name': 'Koenigsegg Jesko Absolut',
        'price': 3400000,
        'top_speed': 531,
        'acceleration': 2.5,
        'power': 1600,
        'engine': '5.0L V8 Twin-Turbo',
        'year': 2023,
        'country': 'Sweden',
        'image': 'https://images.unsplash.com/photo-1614200187524-dc4b892acf16?w=800',
        'description': 'Самый быстрый автомобиль Koenigsegg с потенциалом 531 км/ч'
    },
    {
        'id': 3,
        'name': 'Hennessey Venom F5',
        'price': 2100000,
        'top_speed': 500,
        'acceleration': 2.6,
        'power': 1817,
        'engine': '6.6L V8 Twin-Turbo',
        'year': 2022,
        'country': 'USA',
        'image': 'https://images.unsplash.com/photo-1552519507-da3b142c6e3d?w=800',
        'description': 'Американский гиперкар с мощностью 1817 л.с.'
    },
    {
        'id': 4,
        'name': 'Rimac Nevera',
        'price': 2400000,
        'top_speed': 412,
        'acceleration': 1.85,
        'power': 1914,
        'engine': 'Electric Quad-Motor',
        'year': 2023,
        'country': 'Croatia',
        'image': 'https://images.unsplash.com/photo-1617788138017-80ad40651399?w=800',
        'description': 'Электрический гиперкар с рекордным разгоном'
    },
    {
        'id': 5,
        'name': 'Pagani Huayra R',
        'price': 3100000,
        'top_speed': 350,
        'acceleration': 2.8,
        'power': 850,
        'engine': '6.0L V12 Naturally Aspirated',
        'year': 2022,
        'country': 'Italy',
        'image': 'https://images.unsplash.com/photo-1544829099-b9a0c07fad1a?w=800',
        'description': 'Трековый гиперкар с атмосферным V12 от Mercedes-AMG'
    },
    {
        'id': 6,
        'name': 'McLaren Speedtail',
        'price': 2250000,
        'top_speed': 403,
        'acceleration': 2.9,
        'power': 1070,
        'engine': '4.0L V8 Hybrid',
        'year': 2020,
        'country': 'UK',
        'image': 'https://images.unsplash.com/photo-1621135802920-133df287f89c?w=800',
        'description': 'Трёхместный гиперкар с гибридной силовой установкой'
    },
    {
        'id': 7,
        'name': 'Lamborghini Revuelto',
        'price': 608000,
        'top_speed': 350,
        'acceleration': 2.5,
        'power': 1015,
        'engine': '6.5L V12 Hybrid',
        'year': 2024,
        'country': 'Italy',
        'image': 'https://images.unsplash.com/photo-1544636331-e26879cd4d9b?w=800',
        'description': 'Новое поколение флагмана Lamborghini'
    },
    {
        'id': 8,
        'name': 'Aston Martin Valkyrie',
        'price': 3200000,
        'top_speed': 402,
        'acceleration': 2.5,
        'power': 1160,
        'engine': '6.5L V12 Hybrid',
        'year': 2022,
        'country': 'UK',
        'image': 'https://images.unsplash.com/photo-1605559424843-9e4c228bf1c2?w=800',
        'description': 'Гиперкар, созданный при участии Red Bull Racing'
    }
]

# Корзина (в реальном приложении должна быть в базе данных)
cart = []

@app.route('/')
def index():
    return render_template('index.html', hypercars=hypercars)

@app.route('/api/hypercars')
def get_hypercars():
    # Поддержка сортировки
    sort_by = request.args.get('sort', 'default')
    sorted_cars = hypercars.copy()
    
    if sort_by == 'price_asc':
        sorted_cars.sort(key=lambda x: x['price'])
    elif sort_by == 'price_desc':
        sorted_cars.sort(key=lambda x: x['price'], reverse=True)
    elif sort_by == 'speed':
        sorted_cars.sort(key=lambda x: x['top_speed'], reverse=True)
    elif sort_by == 'acceleration':
        sorted_cars.sort(key=lambda x: x['acceleration'])
    
    return jsonify(sorted_cars)

@app.route('/api/cart', methods=['GET', 'POST', 'DELETE'])
def manage_cart():
    global cart
    
    if request.method == 'GET':
        return jsonify(cart)
    
    elif request.method == 'POST':
        data = request.json
        car_id = data.get('car_id')
        car = next((c for c in hypercars if c['id'] == car_id), None)
        
        if car:
            cart.append(car)
            return jsonify({'success': True, 'cart': cart})
        return jsonify({'success': False, 'error': 'Car not found'}), 404
    
    elif request.method == 'DELETE':
        data = request.json
        car_id = data.get('car_id')
        global cart
        cart = [c for c in cart if c['id'] != car_id]
        return jsonify({'success': True, 'cart': cart})

@app.route('/api/checkout', methods=['POST'])
def checkout():
    global cart
    if not cart:
        return jsonify({'success': False, 'error': 'Cart is empty'}), 400
    
    total = sum(car['price'] for car in cart)
    cart = []
    return jsonify({
        'success': True,
        'message': 'Заказ успешно оформлен!',
        'total': total,
        'order_date': datetime.now().isoformat()
    })

if __name__ == '__main__':
    app.run(debug=True)
