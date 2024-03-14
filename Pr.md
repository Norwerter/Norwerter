//App.js
import React, { useState, createContext } from 'react';
import './App.css';
import Header from './Header';
import Footer from './Footer';
import Product from './Product';
import Cart from './Cart';

export const CartContext = createContext();

function App() {
  const [cartItems, setCartItems] = useState([]);

  const handleBuy = (product) => {
    setCartItems([...cartItems, product]);
  };

  return (
    <div className="App">
      <CartContext.Provider value={{ cartItems, handleBuy }}>
      <Header />
      <Cart items={cartItems} />
      <main>
        <Product name="Товар 1" price={100} />
        <Product name="Товар 2" price={150} />
        <Product name="Товар 3" price={200} onBuy={handleBuy} /> {/* Без useContext в Product отсылающий к 19 строчке onBuy={handleBuy} должен быть, иначе не будет работак кнопка купить и выдет ошибку*/}
      </main>
      </CartContext.Provider>
      <Footer />
    </div>
  );
}

export default App;

// Product.js
import React, { useState, useContext, useEffect } from 'react';
import './App.css';
import { CartContext } from './App';

function Product({ name, price }) {
  const { handleBuy } = useContext(CartContext);
  const [quantityLeft, setQuantityLeft] = useState(3);

  useEffect(() => {
    if (quantityLeft === 0) {
      console.log(`Товар ${name} закончился`);
    }
  }, [quantityLeft, name]);

  const handleBuyClick = () => {
    if (quantityLeft > 0) {
      setQuantityLeft(quantityLeft - 1);
      handleBuy({ name, price });
    }
  };

  return (
    <div className="product">
      <div className="product-info">
        <h2>{name}</h2>
        <p>Цена: {price}</p>
        {quantityLeft > 0 ? (
          <p>Осталось: {quantityLeft} шт.</p>
        ) : (
          <p>Товар закончился</p>
        )}
        <button onClick={handleBuyClick} disabled={quantityLeft === 0}>
          Купить
        </button>
      </div>
    </div>
  );
}

export default Product;

//App.css
.App {
  text-align: center;
}

.App-logo {
  height: 40vmin;
  pointer-events: none;
}

@media (prefers-reduced-motion: no-preference) {
  .App-logo {
    animation: App-logo-spin infinite 20s linear;
  }
}

.header {
  background-color: #ccc; /* Серый фон */
  padding: 20px; /* Добавим немного отступов */
}

.footer {
  background-color: #ccc; /* Серый фон */
  padding: 20px; /* Добавим немного отступов */
}
.App-link {
  color: #61dafb;
}

.product {
  display: inline-block;
  width: 200px;
  height: 200px;
  background-color: #ccc;
  margin: 10px;
  padding: 10px;
}

.product-info {
  text-align: center;
}
.cart {
  text-align: center;
  margin-bottom: 20px;
}
@keyframes App-logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

//Footer.js
import React from 'react';
import './App.css';

function Footer() {
  return (
    <footer className="footer">
      <p>&copy; {new Date().getFullYear()} Мой сайт</p>
    </footer>
  );
}

export default Footer;

//Cart.js
import React from 'react';
import './App.css';

function Cart({ items }) {
  const totalItems = items.reduce((acc, item) => acc + 1, 0);

  return (
    <div className="cart">
      <h2>Корзина</h2>
      <p>Количество товаров: {totalItems}</p>
    </div>
  );
}

export default Cart;

//Header.js
import React from 'react';
import './App.css';

function Header() {
  return (
    <header className="header">
      <h1>Заголовок сайта</h1>
      {/* Здесь могут быть дополнительные элементы заголовка */}
    </header>
  );
}

export default Header;
