# loja-de-receitas<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loja de Receitas</title>
    <script src="https://www.paypal.com/sdk/js?client-id=SEU_CLIENT_ID_AQUI&currency=BRL"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f8f8f8;
            text-align: center;
        }
        header {
            background-color: #ff6347;
            color: white;
            padding: 20px;
            font-size: 24px;
        }
        .container {
            max-width: 800px;
            margin: 20px auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        }
        .product {
            margin-bottom: 20px;
            padding: 15px;
            border-bottom: 1px solid #ddd;
        }
        .product img {
            width: 100%;
            border-radius: 5px;
        }
        .buy-button {
            background-color: #28a745;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        #cart {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            margin-top: 20px;
        }
        #cart-items {
            list-style: none;
            padding: 0;
        }
    </style>
</head>
<body>
    <header>Loja de Receitas</header>
    <div class="container">
        <div class="product" data-name="Ebook: Receitas de Bolos" data-price="19.90">
            <h2>Ebook: Receitas de Bolos</h2>
            <img src="https://via.placeholder.com/600x300" alt="Ebook de Bolos">
            <p>Aprenda a fazer os melhores bolos com receitas detalhadas.</p>
            <button class="buy-button" onclick="addToCart(this)">Comprar por R$19,90</button>
        </div>
        <div class="product" data-name="Ebook: Massas Artesanais" data-price="29.90">
            <h2>Ebook: Massas Artesanais</h2>
            <img src="https://via.placeholder.com/600x300" alt="Ebook de Massas">
            <p>Descubra os segredos das massas italianas feitas em casa.</p>
            <button class="buy-button" onclick="addToCart(this)">Comprar por R$29,90</button>
        </div>
        <div class="product" data-name="Ebook: Sobremesas Fáceis" data-price="24.90">
            <h2>Ebook: Sobremesas Fáceis</h2>
            <img src="https://via.placeholder.com/600x300" alt="Ebook de Sobremesas">
            <p>Receitas simples e deliciosas para adoçar seu dia.</p>
            <button class="buy-button" onclick="addToCart(this)">Comprar por R$24,90</button>
        </div>
    </div>
    <div id="cart" class="container">
        <h2>Carrinho</h2>
        <ul id="cart-items"></ul>
        <p>Total: R$<span id="cart-total">0.00</span></p>
        <div id="paypal-button-container"></div>
    </div>
    <script>
        let cart = [];
        function addToCart(button) {
            const product = button.parentElement;
            const name = product.getAttribute('data-name');
            const price = parseFloat(product.getAttribute('data-price'));
            cart.push({ name, price });
            updateCart();
        }
        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            cartItems.innerHTML = '';
            let total = 0;
            cart.forEach(item => {
                total += item.price;
                const li = document.createElement('li');
                li.textContent = `${item.name} - R$${item.price.toFixed(2)}`;
                cartItems.appendChild(li);
            });
            document.getElementById('cart-total').textContent = total.toFixed(2);
            renderPayPalButton(total);
        }
        function renderPayPalButton(total) {
            document.getElementById('paypal-button-container').innerHTML = '';
            paypal.Buttons({
                createOrder: function(data, actions) {
                    return actions.order.create({
                        purchase_units: [{
                            amount: {
                                value: total.toFixed(2)
                            }
                        }]
                    });
                },
                onApprove: function(data, actions) {
                    return actions.order.capture().then(function(details) {
                        alert('Pagamento concluído por ' + details.payer.name.given_name);
                        cart = [];
                        updateCart();
                    });
                }
            }).render('#paypal-button-container');
        }
    </script>
</body>
</html>
