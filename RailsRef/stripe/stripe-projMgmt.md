
################
# Account
- ### [Stripe SignIn](https://dashboard.stripe.com/login?redirect=%2Ftest%2Fpayments)
```
########
# SIGNIN
#
Email:
lockhart.r@gmail.com
3ym)-hjh:-U&:@*


############
# DEV CONFIG
#
Developers->API Keys
--
Publishable key:
pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B

Secret key:
sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F

```



#############
# CONFIG
#
```
#############
# credentials 
#
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

```

###########
# APP SETUP
# NB: Creating 'Tarrif' for plactice with the setup. 
# 'Tarrif' belomgs to Organization rather than User.
#
```
rails g model Tarrif email:string token:string organization:belongs_to

# app/models/tarrif.rb
--
class Payment < ActiveRecord::Base
  belongs_to :organization
  attr_accessor :card_number, :card_cvv, :card_expires_month, :card_expires_year
  
  def self.month_options
    Date::MONTHNAMES.compact.each_with_index.map { |name,i| ["#{i+1} - #{name}", i+1] }
  end

  def self.year_options
    (Date.today.year..(Date.today.year+10)).to_a
  end
  
  def process_payment
    customer = Stripe::Customer.create email: email, card: token

    Stripe::Charge.create customer: customer.id,
                          amount: 1000,
                          description: 'Premium',
                          currency: 'usd'
  end
end
--


# app/models/organization.rb
--
class Organization < ApplicationRecord
...
  has_one :tarrif
  accepts_nested_attributes for :tarrif

...

--

# app/views/application.html.erb
--
    <%= javascript_include_tag "https://js.stripe.com/v2" %>
--

# app/views/devise/registrations/new
--
<!-- TOP OF FILE -->
<script language="Javascript">
  Stripe.setPublishableKey("<%= Rails.application.credentials.dig(:stripe, :test_publishable_key) %>");
</script>

<!-- STRIPE TARRIF FORM -->
      
      <div class="form-group ms-3 mt-2">
        <%= fields_for( :tarrif ) do |p| %>
          <div class="row col-md-12">
            <div class="form-group col-md-4 no-left-padding">
              <%= p.label :card_number, "Card Number", data: { stripe: 'label' } %>
              <%= p.text_field :card_number, class: "form-control", required: true, data: { stripe: 'number' } %>
            </div>
            <div class="form-group col-md-2">
              <%= p.label :card_cvv, "Card CVV", data: { stripe: 'label' } %>
              <%= p.text_field :card_cvv, class: "form-control", required: true, data: { stripe: 'cvc' } %>
            </div>
            <div class="form-group d-flex row col-md-6">
              <div class="col-md-12">
                <%= p.label :card_expires, "Card Expires", data: { stripe: 'label' } %>
              </div>
              <div class="col-md-3">
                <%= p.select :card_expires_month, options_for_select(Tarrif.month_options),
                                    { include_blank: 'Month' }, 
                                    "data-stripe" => "exp-month", 
                                    class: "form-control", required: true %>    
              </div>
              <div class="col-md-3">
                <%= p.select :card_expires_year, options_for_select(Tarrif.year_options.push),
                                    { include_blank: 'Year' }, 
                                    class: "form-control", 
                                    data: { stripe: "exp-year" },
                                    required: true %>    
              </div>
            </div>
          </div>
        <% end %>      
      </div>

```

############## Javascript FORM ################
# Javascript informed from Stripe Documentation
#
### [Developer Documentstion](https://docs.stripe.com/)
- ### [Payments](https://docs.stripe.com/payments)




# app/javascript/credit_card_form.js
--

function getURLParameter(sParam) {
  var sPageURL = window.location.search.substring(1);
  var sURLVariables = sPageURL.split('&');
  for (var i = 0; i < sURLVariables.length; i++) 
  {
      var sParameterName = sURLVariables[i].split('=');
      if (sParameterName[0] === sParam)  
      {
          return decodeURIComponent(sParameterName[1]);
      }
  }
};

$(document).on('ready turbolinks:load', function() {
  var show_error, stripeResponseHandler, submitHandler;

  // function to handle the submit of the form and intercept the default event
  submitHandler = function (event) {
    var $form = $(event.target);
    $form.find("input[type=submit]").prop("disabled", true);

    // check if stripe is loaded
    if(Stripe){
      Stripe.card.createToken($form, stripeResponseHandler);
    } else {
      show_error("Failed to load credit card processing functionality. Please reload this page in your browser.")
    }
    return false;
  };

// initiate submit handler for the form with class cc_form 
  $(".cc_form").on('submit', submitHandler);

  // handle event of plan dropdown changing
  var handlePlanChange = function (plan_type, form) {
    var $form = $(form);

    if (plan_type == undefined) {
      plan_type = $('#organization_plan :selected').val();
    }

    if (plan_type === "premium") {
      $('[data-stripe]').prop('required', true);
      $form.off('submit');
      $form.on('submit', submitHandler);
      $('[data-stripe]').show();
    } else {
      $('[data-stripe]').hide();
      $form.off('submit');
      $('[data-stripe]').removeProp('required');
    }
  };

  // setup plan change event listener for #organization_plan id in the forms for class cc_form 
  $('#organization_plan').on('change', function (event) {
    handlePlanChange($('#organization_plan :selected').val(), ".cc_form");
  });

  // call plan change handler so that the plan is set correctly in the drop down when the page loads
  handlePlanChange(getURLParameter('plan'), "cc_form");

  // function to handle the token received from stripe and remove credit card fields
  stripeResponseHandler = function (status, response) {
    var token, $form;

    $form = $('.cc_form');

    if (response.error) {
      console.log(response.error.message);
      show_error(response.error.message);
      $form.find("input[type=submit]").prop("disabled", false);
    } else {
      token = response.id;
      $form.append($("<input type=\"hidden\" name=\"payment[token]\" />").val(token));
      $("[data-stripe=number]").remove();
      $("[data-stripe=cvc]").remove();
      $("[data-stripe=exp-year]").remove();
      $("[data-stripe=exp-month]").remove();
      $("[data-stripe=label]").remove();
      $form.get(0).submit();
    }
    return false;
  };

  // function to show errors when stripe functionality returns an error
  show_error = function (message) {
    if($("#flash-messages").size() < 1) {
      $('div.container.main div:first').prepend("<div id='flash-messages'></div>")
    }
    $("#flash-messages").html('<div class="alert alert-warning"><a class="close" data-dismiss="alert">x</a><div id="flash_alert">' + message + '</div></div>');
    $('.alert').delay(5000).fadeOut(3000);
    return false;
  };
});

--

--

```
