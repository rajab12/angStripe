# AngStripe

Step 1:- Setup a new project with ng new <project name>
Step 2:- Once the creation process is over go inside the folder i.e., cd <project name>
Step 3:- Type ng serve and enter
Step 4:- Once the status is "Compiled Successfully" in the prompt, hit http://localhost:4200
Step 5:- Stripe offer two way to interact with Stripe server using JS.

           a. Default Stripe form.
           b. Custom Stripe Form.
         Default Stripe Form which is the objective of this exercise gives us the easiest and safest way to create a token.
Step 6:- Insert <script src="https://checkout.stripe.com/checkout.js"></script> in index.html. We can include via component as well. In this method the stripe will load when we really need it.
Step 7:- In app.component.ts, include 

loadStripe() {
      
    if(!window.document.getElementById('stripe-script')) {
      var s = window.document.createElement("script");
      s.id = "stripe-script";
      s.type = "text/javascript";
      s.src = "https://checkout.stripe.com/checkout.js";
      window.document.body.appendChild(s);
    }
}

Step 8:- Add ngOnInit

ngOnInit() {
    this.loadStripe();
}

In this way loadStripe method will add the script dynamically when the component loads.

Step 9:- Add pay method which will open the stripe payment form

pay(amount: number) {    
  
    var handler = (<any>window).StripeCheckout.configure({
      key: 'pk_test_aeUUjYYcx4XNfKVW60pmHTtI',
      locale: 'auto',
      token: function (token: any) {
        // You can access the token ID with `token.id`.
        // Get the token ID to your server-side code for use.
        console.log(token)
        alert('Token Created!!');
      }
    });
  
    handler.open({
      name: 'Demo Site',
      description: '2 widgets',
      amount: amount * 100
    });
  
}

Step 10:- Open app.component.html and replace the placeholder markup with 

<div class="container mt-5">
  <h2>Stripe Checkout</h2>
  <div class="row mt-5">
    <div class="col-md-4">
      <button (click)="pay(20)" class="btn btn-primary btn-block">Pay $20</button>
    </div>
    <div class="col-md-4">
      <button (click)="pay(30)" class="btn btn-success btn-block">Pay $30</button>
    </div>
    <div class="col-md-4">
      <button (click)="pay(50)" class="btn btn-info btn-block">Pay $50</button>
    </div>    
  </div>
  <p class="mt-5">
      Try it out using the test card number <b>4242 4242 4242 4242</b>, a random three-digit CVC number, any expiration date in the future, and a random five-digit U.S. ZIP code.
  </p>
</div>

Step 11:- app.component.ts should look this

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

export class AppComponent implements OnInit {
  title = 'angStripe';
  
  constructor() { }
  handler:any = null;
  ngOnInit() {
  
    this.loadStripe();
  }

  pay(amount: number) {    
  
    var handler = (<any>window).StripeCheckout.configure({
      key: 'pk_test_aeUUjYYcx4XNfKVW60pmHTtI',
      locale: 'auto',
      token: function (token: any) {
        // You can access the token ID with `token.id`.
        // Get the token ID to your server-side code for use.
        console.log(token)
        alert('Token Created!!');
      }
    });
  
    handler.open({
      name: 'Demo Site',
      description: '2 widgets',
      amount: amount * 100
    });
  
  }

  loadStripe() {
      
    if(!window.document.getElementById('stripe-script')) {
      var s = window.document.createElement("script");
      s.id = "stripe-script";
      s.type = "text/javascript";
      s.src = "https://checkout.stripe.com/checkout.js";
      s.onload = () => {
        this.handler = (<any>window).StripeCheckout.configure({
          key: 'pk_test_aeUUjYYcx4XNfKVW60pmHTtI',
          locale: 'auto',
          token: function (token: any) {
            // You can access the token ID with `token.id`.
            // Get the token ID to your server-side code for use.
            console.log(token)
            alert('Payment Success!!');
          }
        });
      }
        
      window.document.body.appendChild(s);
    }
  }
}

Step 12:- If ng serve is still running when doing these updates, keep verifying if there is any break any errors in the prompt. Else Compiled Successfully, will be seen.

Step 13:- The updated UI in the browser will have, three options for $20, $30 and $50 as declared in app.component.html.
Step 14:- Click one of the options and the dialog opens with the request for credit card no, card expiry date and security code of the card. Sample credit card no is 4242 4242 4242 4242. The card expiry date and the security code is the user's choice. 
Step 15:- Once the inputs are keyed in and submitted, the user get's an alert saying Token created.
