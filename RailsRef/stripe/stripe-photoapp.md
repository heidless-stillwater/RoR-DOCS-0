
# [stripe testing](https://docs.stripe.com/testing)
```
4242 4242 4242 4242
2/34
123

```


# add stripe functionality to rails-fin-track-7
# credentials 
```
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
vi app/views/application/stripe_payment.html.erb
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

########################
# application controller
#
vi app/controllers/application_controller.rb
--
...
  def stripe_payment; end
...
--


########
# routes
#
vi config/routes.rb
--
  root "application#stripe_payment"
  resources :charges, only: :create
  get :stripe_payment, to: 'application#stripe_payment'

--

```

##############
# test
```
rails s

http://127.0.0.1:3000/stripe_payment

```