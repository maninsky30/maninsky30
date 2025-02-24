
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grocery Store Inventory</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
        }
        .product-list {
            margin-top: 20px;
        }
        .product {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Grocery Store Inventory</h2>
        <input type="text" id="productName" placeholder="Product Name">
        <input type="number" id="productQuantity" placeholder="Quantity">
        <input type="number" id="purchasePrice" placeholder="Purchase Price">
        <input type="number" id="sellingPrice" placeholder="Selling Price">
        <button onclick="addProduct()">Add Product</button>
        
        <h3>Products In Stock</h3>
        <div class="product-list" id="inStock"></div>
        
        <h3>Out of Stock</h3>
        <div class="product-list" id="outOfStock"></div>

        <h3>Financial Summary</h3>
        <p>Total Capital: $<span id="totalCapital">0</span></p>
        <p>Potential Profit: $<span id="totalProfit">0</span></p>
    </div>
    
    <script>
        let inventory = [];

        function addProduct() {
            let name = document.getElementById('productName').value;
            let quantity = parseInt(document.getElementById('productQuantity').value);
            let purchasePrice = parseFloat(document.getElementById('purchasePrice').value);
            let sellingPrice = parseFloat(document.getElementById('sellingPrice').value);

            if (name && !isNaN(quantity) && !isNaN(purchasePrice) && !isNaN(sellingPrice)) {
                inventory.push({ name, quantity, purchasePrice, sellingPrice });
                updateInventory();
            }
        }

        function updateInventory() {
            let inStockDiv = document.getElementById('inStock');
            let outOfStockDiv = document.getElementById('outOfStock');
            let totalCapital = 0;
            let totalProfit = 0;

            inStockDiv.innerHTML = '';
            outOfStockDiv.innerHTML = '';
            
            inventory.forEach((item, index) => {
                let productDiv = document.createElement('div');
                productDiv.className = 'product';
                productDiv.innerHTML = `${item.name} (x${item.quantity}) - Cost: $${item.purchasePrice}, Price: $${item.sellingPrice} 
                <button onclick="removeProduct(${index})">Remove</button>`;
                
                if (item.quantity > 0) {
                    inStockDiv.appendChild(productDiv);
                    totalCapital += item.quantity * item.purchasePrice;
                    totalProfit += item.quantity * (item.sellingPrice - item.purchasePrice);
                } else {
                    outOfStockDiv.appendChild(productDiv);
                }
            });

            document.getElementById('totalCapital').innerText = totalCapital.toFixed(2);
            document.getElementById('totalProfit').innerText = totalProfit.toFixed(2);
        }

        function removeProduct(index) {
            inventory.splice(index, 1);
            updateInventory();
        }
    </script>
</body>
</html>
