<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spend Bill Gates' Money</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f7;
        }
        
        .header {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 10;
            background-color: #1c1c1e;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 1rem 0;
        }
        
        .balance {
            font-size: 2.5rem;
            font-weight: bold;
            color: #46c93a;
            text-shadow: 0 0 10px rgba(70, 201, 58, 0.5);
        }
        
        .content {
            margin-top: 160px;
            padding-bottom: 2rem;
        }
        
        .item-card {
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            transition: transform 0.2s;
        }
        
        .item-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
        }
        
        .item-image {
            height: 160px;
            background-size: cover; /* Changed to cover for better image fit */
            background-position: center;
            background-repeat: no-repeat;
            background-color: #f9f9f9;
        }
        
        .item-name {
            font-weight: 600;
            font-size: 1.1rem;
        }
        
        .item-price {
            color: #0070f3;
            font-weight: 700;
        }
        
        .counter {
            width: 60px;
            text-align: center;
            font-weight: 600;
            background-color: #f9f9f9;
        }
        
        .btn-sell {
            background-color: #ff4d4f;
            color: white;
            border-radius: 8px 0 0 8px;
            transition: all 0.2s;
        }
        
        .btn-sell:hover:not(:disabled) {
            background-color: #ff2a2d;
        }
        
        .btn-sell:disabled {
            background-color: #ffcccb;
            cursor: not-allowed;
        }
        
        .btn-buy {
            background-color: #46c93a;
            color: white;
            border-radius: 0 8px 8px 0;
            transition: all 0.2s;
        }
        
        .btn-buy:hover:not(:disabled) {
            background-color: #3aad30;
        }
        
        .btn-buy:disabled {
            background-color: #c5e8c0;
            cursor: not-allowed;
        }
        
        /* Format for currency */
        .currency::before {
            content: "$";
        }
    </style>
</head>
<body>
    <header class="header">
        <div class="container mx-auto px-4">
            <h1 class="text-center text-white text-3xl font-bold mb-2">Spend Bill Gates' Money</h1>
            <div class="balance-container bg-white rounded-lg py-3 px-4 shadow-lg">
                <p class="balance text-center" id="balance">100,000,000,000</p>
            </div>
        </div>
    </header>

    <main class="content container mx-auto px-4">
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6" id="items-container"></div>
    </main>

    <script>
        // Items data
        const items = [
            { id: 1, name: "Big Mac", price: 2, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/burger.png" },
            { id: 2, name: "Flip Flops", price: 3, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/shoes.png" },
            { id: 3, name: "Coca-Cola Pack", price: 5, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/soda.png" },
            { id: 4, name: "Movie Ticket", price: 12, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/ticket.png" },
            { id: 5, name: "Book", price: 15, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/book.png" },
            { id: 6, name: "Lobster Dinner", price: 45, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/lobster.png" },
            { id: 7, name: "Video Game", price: 60, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/game.png" },
            { id: 8, name: "Amazon Echo", price: 99, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/speaker.png" },
            { id: 9, name: "Year of Netflix", price: 100, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/tv.png" },
            { id: 10, name: "Air Jordans", price: 125, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/sneakers.png" },
            { id: 11, name: "Airpods", price: 199, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/earbuds.png" },
            { id: 12, name: "Gaming Console", price: 299, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/console.png" },
            { id: 13, name: "Drone", price: 350, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/drone.png" },
            { id: 14, name: "Smartphone", price: 699, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/phone.png" },
            { id: 15, name: "Bike", price: 800, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/bike.png" },
            { id: 16, name: "Kitten", price: 1500, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/cat.png" },
            { id: 17, name: "Puppy", price: 1500, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/dog.png" },
            { id: 18, name: "Auto Rickshaw", price: 2300, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/rickshaw.png" },
            { id: 19, name: "Horse", price: 2500, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/horse.png" },
            { id: 20, name: "Acre of Farmland", price: 3000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/farm.png" },
            { id: 21, name: "Designer Handbag", price: 5500, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/handbag.png" },
            { id: 22, name: "Hot Tub", price: 6000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/hottub.png" },
            { id: 23, name: "Luxury Wine", price: 7000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/wine.png" },
            { id: 24, name: "Diamond Ring", price: 10000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/ring.png" },
            { id: 25, name: "Jet Ski", price: 12000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/jetski.png" },
            { id: 26, name: "Rolex", price: 15000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/watch.png" },
            { id: 27, name: "Ford F-150", price: 30000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/truck.png" },
            { id: 28, name: "Tesla", price: 75000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/tesla.png" },
            { id: 29, name: "Monster Truck", price: 150000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/monstertruck.png" },
            { id: 30, name: "Ferrari", price: 250000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/sportscar.png" },
            { id: 31, name: "Single Family Home", price: 300000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/house.png" },
            { id: 32, name: "Gold Bar", price: 700000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/gold.png" },
            { id: 33, name: "McDonalds Franchise", price: 1500000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/franchise.png" },
            { id: 34, name: "Superbowl Ad", price: 5250000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/ad.png" },
            { id: 35, name: "Yacht", price: 7500000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/yacht.png" },
            { id: 36, name: "M1 Abrams", price: 8000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/tank.png" },
            { id: 37, name: "Formula 1 Car", price: 15000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/f1.png" },
            { id: 38, name: "Apache Helicopter", price: 31000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/helicopter.png" },
            { id: 39, name: "Mansion", price: 45000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/mansion.png" },
            { id: 40, name: "Make a Movie", price: 100000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/movie.png" },
            { id: 41, name: "Boeing 747", price: 148000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/airplane.png" },
            { id: 42, name: "Mona Lisa", price: 780000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/monalisa.png" },
            { id: 43, name: "Skyscraper", price: 850000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/skyscraper.png" },
            { id: 44, name: "Cruise Ship", price: 930000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/cruiseship.png" },
            { id: 45, name: "NBA Team", price: 2120000000, image: "https://cdn.jsdelivr.net/gh/octoshrimpy/blokkblokk@main/resources/basketball.png" }
        ];

        // Application state
        let state = {
            balance: 100000000000, // $100 billion
            purchases: {}
        };

        // Initialize purchases with 0 quantity for each item
        items.forEach(item => {
            state.purchases[item.id] = 0;
        });

        // Format number as currency
        function formatCurrency(amount) {
            return new Intl.NumberFormat('en-US').format(amount);
        }

        // Update the displayed balance
        function updateBalance() {
            document.getElementById('balance').innerText = formatCurrency(state.balance);
        }

        // Create item cards and add them to the container
        function renderItems() {
            const container = document.getElementById('items-container');
            
            items.forEach(item => {
                const itemCard = document.createElement('div');
                itemCard.className = 'item-card flex flex-col';
                
                itemCard.innerHTML = `
                    <div class="item-image" style="background-image: url('${item.image}');"></div>
                    <div class="p-4 flex-grow">
                        <h3 class="item-name text-center mb-2">${item.name}</h3>
                        <p class="item-price text-center mb-4 currency">${formatCurrency(item.price)}</p>
                        <div class="flex items-center justify-between">
                            <button class="btn-sell py-2 px-4 w-1/3" 
                                id="sell-${item.id}" 
                                disabled
                                onclick="sellItem(${item.id})">Sell</button>
                            <div class="counter py-2">${state.purchases[item.id]}</div>
                            <button class="btn-buy py-2 px-4 w-1/3" 
                                id="buy-${item.id}" 
                                onclick="buyItem(${item.id})">Buy</button>
                        </div>
                    </div>
                `;
                
                container.appendChild(itemCard);
            });
        }

        // Buy an item
        function buyItem(itemId) {
            const item = items.find(i => i.id === itemId);
            
            // Check if we can afford it
            if (state.balance >= item.price) {
                // Update state
                state.balance -= item.price;
                state.purchases[itemId]++;
                
                // Update UI
                updateBalance();
                updateItemQuantity(itemId);
                updateButtonStates();
            }
        }

        // Sell an item
        function sellItem(itemId) {
            const item = items.find(i => i.id === itemId);
            
            // Check if we have the item to sell
            if (state.purchases[itemId] > 0) {
                // Update state
                state.balance += item.price;
                state.purchases[itemId]--;
                
                // Update UI
                updateBalance();
                updateItemQuantity(itemId);
                updateButtonStates();
            }
        }

        // Update the quantity displayed for an item
        function updateItemQuantity(itemId) {
            const quantityElement = document.querySelector(`#buy-${itemId}`).previousElementSibling;
            quantityElement.textContent = state.purchases[itemId];
        }

        // Update the enabled/disabled state of all buttons
        function updateButtonStates() {
            items.forEach(item => {
                const buyButton = document.getElementById(`buy-${item.id}`);
                const sellButton = document.getElementById(`sell-${item.id}`);
                
                // Disable buy button if not enough money
                buyButton.disabled = state.balance < item.price;
                
                // Disable sell button if quantity is 0
                sellButton.disabled = state.purchases[item.id] === 0;
            });
        }

        // Initialize the application
        function init() {
            renderItems();
            updateBalance();
            updateButtonStates();
        }

        // Start the application when the page loads
        window.onload = init;
    </script>
</body>
</html>
