<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>JQ Tab Example</title>
    <style>
        ul.link-wapper {
            list-style-type: none;
            margin: 20px;
            padding: 0;
            text-align: center;
        }

        ul.link-wapper li {
            display: inline-block;
            padding: 10px;
        }

        ul.link-wapper li a {
            text-decoration: none;
            color: black;
            padding: 10px;
        }

        ul.link-wapper li a.active{
            color: #0037ff;
            border-bottom: 1px solid red;
        }

        div.content {
            max-width: 1200px;
            background-color: orange;
            padding: 20px 50px;
            margin: 0 auto 10px;
            display: none;
            max-height: 500px;
        }
        .homeimg{
            width: 500px;
            height: 400px;
            margin-right: 80px;
        }
        .homeimg1{
            width: 500px;
            height: 400px;
            margin-left: 80px;
        }
        #home h3{
            text-align: center;
        }
        #about h3{
            text-align: center;
        }
        #contact h3{
            text-align: center;
        }
    </style>
    <script src="jquery.min.js"></script>
    <script src="//cdn.jsdelivr.net/npm/sweetalert2@11"></script>

  <script type="text/javascript">
    // Your Script Here!
    // $(document).ready(function (argument) {

    $(function() {
        $('.content').first().slideDown();

        $('ul.link-wapper li a').on("click",function (event) { 
            var current_tab = $(this);
            $('ul.link-wapper li a').removeClass('active');
            current_tab.addClass('active');

            $('.content').slideUp();
            current_tab_href = current_tab.attr('href'); // #home
            $(current_tab_href).slideDown(); // $('#home')

            event.preventDefault();
        })

        // LocalStorage
        $('.atc').on("click",function () { 
            let id = $(this).data('id');
            let name = $(this).data('name');
            let price = $(this).data('price');
            let item = {
                id: id,
                name: name,
                price: price,
                qty: 1
            }
            // console.log(item)
            
            let cart = localStorage.getItem('cart');
           // let myArr = '';
            let cartArr = []
            
            if(cart != null){
                cartArr = JSON.parse(cart);
            }else{
               
                cartArr = []
            }
            var totalqty = 1
            let totalqtystr = ""
            for(row of cartArr){
                totalqty += row.qty
                
            }
            totalqtystr += `<h5> Cart Items: ${totalqty}</h5>`
            $(".totalqty").html(totalqtystr)
            console.log(totalqty)

            // homework
            let status=0;
            for(row of cartArr){ // 1,2
                if(row.id == id){ // 3
                    row.qty++;
                    status = 1;
                }
            } 

            if(status == 0){
                cartArr.push(item)
            }
            Swal.fire({
                position: 'top-middle',
                icon: 'success',
                title: 'Item has been added to cart',
                showConfirmButton: false,
                 timer: 1500
                })
            // JS Object => JSON String
            let myArrString = JSON.stringify(cartArr);
            console.log(myArrString)

            localStorage.setItem('cart',myArrString);
           getdata();
        })

    //     // Get Data From LS
        function getdata() { 
            let cart = localStorage.getItem('cart');
            if(cart != null){
                let cartArr = JSON.parse(cart);
                let row = ''
                let total = 0;
                let i = 0;
                for([index,item] of cartArr.entries()){
                    total += (item.qty*item.price);
                    row += `<tr>
                            <td>
                                <button class="remove" data-id="${index}">Remove</button> 
                                ${++i}
                            </td>
                            <td>${item.name}</td>
                            <td>${item.price}</td>
                            <td>
                                <input type="number" min="1" value="${item.qty}" data-id="${index}" class="updateqty">
                            </td>
                            <td>${item.qty*item.price} Ks</td>
                        </tr>`
                }

                row += `<tr>
                            <td colspan="4">Total Amount</td>
                            <td>${total} Ks</td>
                        </tr>`

                $('tbody').html(row)
            }else{
                $('tbody').html(`<tr><td colspan="5">Your Cart is empty!</td></tr>`);
            }
        }
        getdata();

        // Remove 
        $('tbody').on("click",".remove",function () { 
            let index = $(this).data('id');
            //console.log(index)
            
            let cart = localStorage.getItem('cart');
            let cartArr = JSON.parse(cart);
           let removeqty = cartArr[index].qty
           console.log(removeqty)
            let totalqty=0
            for(row of cartArr){
                totalqty += row.qty
                afterremove = totalqty-removeqty
            }
            
            let totalqtystr=""
            totalqtystr += `<h5>Cart Items: ${afterremove}</h5>`
            $(".totalqty").html(totalqtystr)
            
            console.log(afterremove)
            // Delete one row
            cartArr.splice(index,1)
            // JS Object => JSON String
            let myArrString = JSON.stringify(cartArr);
            // console.log(myArrString)
            localStorage.setItem('cart',myArrString);
            getdata();
        })
       
        // increase - decrease Qty
        $('tbody').on("change",".updateqty",function () { 
            let index = $(this).data('id');
            // console.log(index)
            let qty = $(this).val();
            let cart = localStorage.getItem('cart');
            let cartArr = JSON.parse(cart);

           cartArr[index].qty = qty
            

            let changeqty = cartArr[index].qty
            console.log(changeqty)
            // totalqty = $("h5.totalqty").val()
            // console.log(totalqty)
            // var totalqty = 0 
            // for(row of cartArr){
            //     totalqty += row.qty
            // }
            // console.log(totalqty)
            // let totalqty = 0
            // let myh5 = $('.totalqty h5').val()
            // console.log(myh5)
            // if(qty> changeqty){
            //     afterremove = totalqty-qty
            // }
            // else{
            //     afterremove = totalqty+qty
            // }
            
            // let totalqtystr=""
            // totalqtystr += `<h5>${afterremove}</h5>`
            // $(".totalqty").html(totalqtystr)
            
            // console.log(afterremove)
           
            // JS Object => JSON String
            let myArrString = JSON.stringify(cartArr);
            // console.log(myArrString)
            localStorage.setItem('cart',myArrString);
            getdata();
        })

    //     // checkout
        $('.checkout').on("click",function () { 
          
            Swal.fire({
                        title: 'Are you sure?',
                        //text: "You won't be able to revert this!",
                        icon: 'warning',
                        showCancelButton: true,
                        confirmButtonColor: '#3085d6',
                        cancelButtonColor: '#d33',
                        confirmButtonText: 'Yes, check out it!'
                        }).then((result) => {
                        if (result.isConfirmed) {
                            Swal.fire(
                            'Checked out!',
                            'Items from your cart has been checked out.',
                            'success',
                            localStorage.removeItem('cart'),
                            totalqty = 0,
                            totalqtystr="",
                            totalqtystr += `<h5>${totalqty}</h5>` ,
                             $(".totalqty").html(totalqtystr),
                             getdata() 
                           
                            )
                        }
                        })
                        
        })
        //cart total qty noti
        // let cart = localStorage.getItem('cart');
        //     let cartArr = JSON.parse(cart);
     
    })
    
  </script>

</head>
<body>
    <div class="tab"> 
        <ul class="link-wapper">
            <li>
                <a href="#home" class="active">Home</a>
            </li>

            <li>
                <a href="#about">About</a>
            </li>
            
            <li>
                <a href="#contact">Contact</a>
            </li>

            <li>
                <a href="#shop">Shop</a>
            </li>

            <li>
                <a href="#cart">Cart</a>
            </li>
            <li class="totalqty">
                
            </li>
        </ul>

        <div class="content" id="home">
            <h3>Blk & Bold Coffe Bean Shop</h3>
            <img src="./images/home.jpg" alt="" class="homeimg">
            <img src="./images/best-coffee-beans.jpg" alt="" class="homeimg1">
           
        </div>

        <div class="content" id="about">
            <h3>Our story</h3>
            <p>
                A lot has changed since '63, but our philosophy never has. We're passionate about delivering the best handcrafted products and take pride in the journey from seed to cup. <br>
                Welcome to The Bean Shop, where we take great pride in sourcing the best speciality coffee beans, roasting them with care and skill then sending them out to you quickly so you can experience the superb aromas and distinctive flavours of our exceptional coffee beans.  
<br><br>
You can order online for post/courier delivery or pickup from The Bean Shop or you can also just turn up at The Bean Shop and order from the doorstep.

<br><br>
Opening hours - Mon - Sat, 9:30-5:30. 
<br><br> We look forward to seeing you!
            </p>
        </div>

        <div class="content" id="contact">
            <h3>Get in touch!</h3>
            <p>
              Blk & Bold <br>
                The Bean Shop <br>
                67 George Street ,PH1 5LB ,Scotland <br>
                 Open Mon-Sat 9.30-5.30 <br>
                
                 01738 449955 <br>
                
                 hello@blk&boldthebeanshop.com <br>
                
                 
            </p>
        </div>

        <div class="content" id="shop">
            <h3>Shop Section</h3>
            
            <div class="shop">
                <div class="item">
                    <img src="./images/item1 (1).jpg" alt="Where" width="100px">
                    <p>Volcanica coffee bean</p>
                    <strong>3500 Ks</strong>
                    <button class="atc" data-id="1" data-name="Item One" data-price="500">Add To Cart</button>
                </div>

                <div class="item">
                    <img src="images/item1 (2).jpg" alt="Where" width="100px">
                    <p>Dark Roast Blend</p>
                    <strong>4000 Ks</strong>
                    <button class="atc" data-id="2" data-name="Item Two" data-price="1000">Add To Cart</button>
                </div>

                <div class="item">
                    <img src="images/item3.jpg" width="100px">
                    <p>The warm up: Estern Conference</p>
                    <strong>2000 Ks</strong>
                    <button class="atc" data-id="3" data-name="Item Three" data-price="200">Add To Cart</button>
                </div>
            </div>
        </div>

        <div class="content" id="cart">
            <h3>Cart Section</h3>
            <div class="mycart">
                <table border="1" cellspacing="0" cellpadding="10" width="100%" style="margin-bottom: 10px;">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Name</th>
                            <th>Price</th>
                            <th>Qty</th>
                            <th>Subtotal</th>
                        </tr>
                    </thead>
                    <tbody>
                        
                    </tbody>
                </table>

                <button class="checkout">Checkout</button>
            </div>
        </div>
    </div>
</body>
</html>

