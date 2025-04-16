
## [Implementing Stripe Payments in Rails: A Quick and Easy Tutorial](https://medium.com/@umerqaisar/implementing-stripe-payments-in-rails-a-quick-and-easy-tutorial-5e78b9211d58)
- ### []
```
# credentials 
EDITOR='subl --wait' ./bin/rails credentials:edit

stripe:
  test_publishable_key: pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B
  test_secret_key: sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F
  webhook_secret: whsec_6856dc192142555126bb021b6a9e4ea56bca247adb76a71438d4bd6d4f55c6c4

sendgrid_mailer:
  user_name: apikey
  api_key_secret: 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
  mail_sender: support@naught4profit.co.uk
  domain: naught4profit.co.uk

# gem
bundle add stripe

# initializer
vi config/initializers/stripe.rb
--
Rails.configuration.stripe = {
  publishable_key: Rails.application.credentials.dig(:stripe, :test_publishable_key),
  secret_key: Rails.application.credentials.dig(:stripe, :test_secret_key)
}
Stripe.api_key = Rails.application.credentials.dig(:stripe, :test_secret_key)

--

#############################
# Payment Form
#
vi app/views/users/stripe_payment.html.erb
--
<%= form_with url: charges_path do |f| %>
  <div class="card-header">
    <strong>Credit Card</strong>
    <small>enter your card details</small>
  </div>

  <div class="card-body">
    <div class="form-group">
      <label for="amount">Amount</label>
      <%= f.number_field :amount, class: 'form-control', id: 'amount', min: 0, placeholder: 'Enter your amount in $' %>
    </div>

    <div class="form-group">
      <label for="ccnumber">Credit Card Number</label>
      <%= f.text_field :card_number, class: 'form-control', id: 'cardNumber', placeholder: '0000 0000 0000 0000' %>
    </div>

    <div class="form-group">
      <label for="ccmonth">Month</label>
      <%= f.text_field :month, class: 'form-control', id: 'ccmonth', placeholder: 'MM' %>
    </div>

    <div class="form-group">
      <label for="ccyear">Year</label>
      <%= f.text_field :year, class: 'form-control', id: 'ccyear', placeholder: 'YY' %>
    </div>

    <div class="form-group">
      <label for="cvv">CVV/CVC</label>
      <%= f.text_field :cvc, class: 'form-control', id: 'cvv', placeholder: '123' %>
    </div>
  </div>

  <div class="card-footer">
    <%= button_to charges_path, class: 'btn btn-sm btn-success float-right' do %>
      <i class="mdi mdi-gamepad-circle"></i> Continue
    <% end %>
    <%= link_to root_path, class: 'btn btn-sm btn-danger' do %>
      <i class="mdi mdi-lock-reset"></i> Reset
    <% end %>
  </div>
<% end %>

--

####################
# charges Controller
#
rails g controller charges create

vi app/controllers/charges_controller.rb
--
class ChargesController < ApplicationController
  rescue_from Stripe::CardError, Stripe::InvalidRequestError, with: :catch_exception
  before_action :set_params

  def create
    charge = Stripe::Charge.create(amount: (@amount * 100), source: 'tok_visa', currency: 'usd')
    # charge = Stripe::Charge.create(amount: (@amount * 100), source: stripe_token.id, currency: 'usd')
    flash[:notice] = charge[:paid] == true ? success_message(charge) : failure_message
    redirect_back fallback_location: root_path
  end

  private

  def set_params
    @card_number = params[:card_number] || 0
    @card_month = params[:month] || 0
    @card_year = params[:year] || 0
    @cvc = params[:cvc] || 0
    @amount = params[:amount]&.to_i || 0
  end

  def stripe_token
    Stripe::Token.create(card: { number: @card_number, exp_month: @card_month, exp_year: @card_year, cvc: @cvc })
  end

  def catch_exception(exception)
    flash[:alert] = exception.message
    redirect_back fallback_location: root_path
  end

  def success_message(charge)
    "Your payment of amount: #{@amount} has been successfully processed.
     You can take your receipt from here: #{charge[:receipt_url]}"
  end

  def failure_message
    "We are sorry. We are unable to proceed with your request. Please try again later."
  end
end

--

########
# routes
#
vi config/reoutes.rb
--
  # root "application#stripe_payment"
  resources :charges, only: :create
  get :stripe_payment, to: 'application#stripe_payment'

--

```

## [testing: simulating payments](https://docs.stripe.com/testing)
### Secret and publishable keys
All accounts have a total of four API keys by default—two in a sandbox, and two in live mode:

- **Sandbox secret key**: Use this key to authenticate requests on your server when you’re testing in a sandbox. By default, you can use this key to perform any API request without restriction.
  - sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F
- **Sandbox publishable key**: Use this key for testing purposes in your web or mobile app’s client-side code.
  - pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B
- **Live mode secret key**: Use this key to authenticate requests on your server when in live mode. By default, you can use this key to perform any API request without restriction.
- **Live mode publishable key**: Use this key, when you’re ready to launch your app, in your web or mobile app’s client-side code.

#### Testing and development
Use only your test API keys for testing and development. This ensures that you don’t accidentally modify your live customers or charges.

### Test card
```
4242 4242 4242 4242
12/34
123


```

## TroubleShoot

### [Sending credit card numbers directly to the Stripe API is generally unsafe. We suggest you use test tokens that map to the test card you are using](https://stackoverflow.com/questions/76583126/sending-credit-card-numbers-directly-to-the-stripe-api-is-generally-unsafe-we-s)
Issue is in Test and caused by the 'source' not using a test credit card. Fix by seeting the 'souce' to 'tok_visa'
```
vi app/controllers/charges_controller.rb
--
    charge = Stripe::Charge.create(amount: (@amount * 100), source: 'tok_visa', currency: 'usd')
--
```

## execute
rails s


