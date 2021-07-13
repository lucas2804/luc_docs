1) Instal pg 0.18.1 , puma 4.3.3 old versions

```bash
gem install pg -v '0.18.1' -- --with-cflags="-Wno-error=implicit-function-declaration"
gem install puma -v '4.3.3' -- --with-cflags="-Wno-error=implicit-function-declaration"
```

2) install mailcatcher

```bash
gem install mailcatcher -- --with-cflags="-Wno-error=implicit-function-declaration"

# run mailcatcher 
mailcatcher
```

3) Start ngrok `~/.ngrok2/ngrok.yml`
    + install ngrok
    + edit ~/.ngrok2/ngrok.yml
    + point ngrok port 80 -> rails server port 3000
    + authtoken find in ngrok profile

```yaml
authtoken: authtoken_example_4cioZiEArvJYrb_Z7XSTofZHXjP3Sf1qcY5
inspect_db_size: 200000000
region: ap
tunnels:
  judgeluc:
    proto: http
    hostname: judgeluc.ap.ngrok.io
    addr: 127.0.0.1:3000
  judgeshopify:
    proto: http
    hostname: judgeshopifyluc.ap.ngrok.io
    addr: 127.0.0.1:3000
```

- SET UP local side

```bash
rake db:create
rake db:migrate
rake db:seed
ngrok start judgeluc
rails s
mailcatcher
sidekiq
brew services start mongodb-community@4.2; brew services start postgresql; brew services start redis; ~/elasticsearch-6.0.0/bin/elasticsearch
```


4) Regis a `shop-partner` on shopify eg: ```https://judgelucshop.myshopify.com/admin/apps```

5) Create an `shop-test` on shopify
   ![image shop-test](./images/shop-test.png)

6) Create an `app-test`
    + APP URL: should include index `https://judgeluc.ap.ngrok.io/index` (ngrok_https_url + /index)
    + Allowed redirection URL(s): https://judgeluc.ap.ngrok.io/auth/shopify/callback (ngrok_https_url + /auth/shopify/callback)
![create-app-test.png](./images/create-app-test.png)
   
7) Create product on `shop-test`

![create-active-product.png](./images/create-active-product.png)

8) Install `app-test` on `shop-test`

![install-app-on-shop-test.png](./images/install-app-on-shop-test.png)

9) Install theme debut then PUBLIC

![install-debut-theme.png](./images/install-debut-theme.png)

10) Access to product page of shop-test

![access-to-product-page.png](./images/access-to-product-page.png)

11) If review badge are not displayed,  shopify blocked your `judgeme_core.liquid` and `judgeme_widgets.liquid` files.

+ View Page Source or Inspect element
+ Find string `<!-- "snippets/judgeme_core.liquid" was not rendered, the associated app was uninstalled -->`
+ If exist `the associated app was uninstalled`
+ That's mean shopify blocked your `judgeme_core.liquid` and `judgeme_widgets.liquid` files.

12) To fix issue  shopify blocked your `judgeme_core.liquid` and `judgeme_widgets.liquid` files.

+ Edit code of current theme (Debut)

![edit-code-of-current-theme.png](./images/edit-code-of-current-theme.png)

+ Clone 2 files which blocked by shopify
+ `judgeme_core.liquid` -> `judgeme_core_v2.liquid`
+ `judgeme_widgets.liquid` -> `judgeme_widgets_v2.liquid`

+ Edit file names to `judgeme_core_v2.liquid` `judgeme_widgets_v2.liquid` in files below:
+ `theme.liquid`
+ `product-card-grid.liquid`
+ `product-card-grid.liquid`
+ `product-template.liquid`

+ Save then refresh page in 3 minutes

13) Add key action


```ruby
key_action_data = [{"name"=>"Import Existing Reviews", "sentence"=>"Quickly transfer your review history by importing reviews from another app (like Shopify, Yotpo or Stamped), from E-commerce platforms like AliExpress, or directly from your customers.", "box_id"=>"/import", "action_text"=>"Import Reviews", "positive_confirm_text"=>"", "key_index"=>999.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>nil, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Request new reviews from customers", "sentence"=>"Collect new email, photo and video reviews by setting up automatic requests and reminders for customers to review your products.", "box_id"=>"Request Timing", "action_text"=>"Request Reviews", "positive_confirm_text"=>"", "key_index"=>998.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>0, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Publicize reviews on social media", "sentence"=>"Connect your Facebook business page and Twitter account to automatically share your reviews with a wider audience on social media.", "box_id"=>"Facebook Authentication", "action_text"=>"Share Reviews", "positive_confirm_text"=>"", "key_index"=>997.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>0, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Reward reviewers with coupons", "sentence"=>"Encourage your customers to review products quickly by offering them a coupon with a discount code. Reward customers and get repeat sales!", "box_id"=>"Generated Coupon Code", "action_text"=>"Create Coupons", "positive_confirm_text"=>"", "key_index"=>996.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>nil, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Group reviews across products and shops", "sentence"=>"Increase your reviews by creating product groups and shop groups and synchronizing the reviews across these products or shop groups.", "box_id"=>"Product Groups", "action_text"=>"Group Reviews", "positive_confirm_text"=>"", "key_index"=>996.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>nil, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Get community answers about your products", "sentence"=>"Allow your customers to show their expertise by adding a Q&A section or  gather more detailed product feedback using custom forms.", "box_id"=>"Questions and Answers", "action_text"=>"Add Q&A", "positive_confirm_text"=>"", "key_index"=>995.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>nil, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Boost your SEO with Google rich snippets", "sentence"=>"Check out your Google Rich Snippets (we create them automatically) and add reviews to Google shopping using your Google Product Review Feed.", "box_id"=>"SEO - Rich Snippets", "action_text"=>"Share Reviews", "positive_confirm_text"=>"", "key_index"=>994.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>nil, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Integrate with other apps", "sentence"=>"Integrate with PushOwl, Klaviyo, Shoelace, Help Scout, Aftership, Fomo, Recently, Nextopia, Searchanise, EasyTabs, EasyAccordian, as well as many JSON-LD and loyalty apps.", "box_id"=>"EasyTabs / EasyAccordion", "action_text"=>"See All Integrations", "positive_confirm_text"=>"", "key_index"=>993.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>nil, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil}, {"name"=>"Add and customize other widgets", "sentence"=>"Showcase  multiple product reviews with the Review Carousel and All Reviews Page, as well as build trust with the Preview Badge and Verified Review Count Badge.", "box_id"=>"Reviews Carousel Installation", "action_text"=>"Add More Widgets", "positive_confirm_text"=>"", "key_index"=>996.0, "active"=>true, "negative_confirm_text"=>"", "awesome"=>false, "setting_name"=>"", "delay_time"=>0, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil},{"name"=>"Customize Review Widget", "sentence"=>"Customize the [position](https://judge.me/settings?jump_to=review+widget+installation) and [design](https://judge.me/settings?jump_to=widget+star+color) of your Review Widget on your product pages so it fits in perfectly with your current theme, and choose when to publish reviews automatically.", "box_id"=>"Widget Star Color", "action_text"=>"Customize Widget", "positive_confirm_text"=>nil, "key_index"=>1000.0, "active"=>true, "negative_confirm_text"=>nil, "awesome"=>false, "setting_name"=>nil, "delay_time"=>0, "platforms_to_apply"=>"", "version"=>"2019", "locale"=>"en", "icon_name"=>nil},{"name"=>"install_review_widget", "sentence"=>"Where should we install the Review Widget?", "box_id"=>"Review Widget Installation", "action_text"=>"Save Settings & Next", "positive_confirm_text"=>"Widget automatically added to the bottom of the product page. Our support team will manually install the widget {{ action_reply }} and get back to you as soon as possible.", "key_index"=>1000.0, "active"=>true, "negative_confirm_text"=>"Ok, we will not install the widget for you.", "awesome"=>false, "setting_name"=>"", "delay_time"=>0, "platforms_to_apply"=>"", "version"=>nil, "locale"=>"en", "icon_name"=>nil}]
key_action_data.each do |key_action_datum|
  KeyAction.where(key_index: key_action_datum['key_index'], version: key_action_datum['version']).first_or_create(key_action_datum)
end

shop = Shop.first
HtmlMiracleServices::VersionFactory.new.create_and_attach_to_shops(version: 'v2', order: 1, shops: Shop.none)
HtmlMiracle.last
Shopify::Session.start(shop)
ShopifyAPI::Metafield.where(namespace: :judgeme).last
HtmlMiracleServices::BuildMetafields.new(shop).publish
HtmlMiracleJobs::PublishJob.new.perform(shop.id)

# Elasticsearch reindex
Product.reindex
Review.reindex
Question.reindex
Order.reindex
Message.reindex
DoneSearch.reindex
Shop.reindex
```

- Force display review badge with params `?judgeme_badge=show&judgeme_token=123`
- Clear asset cache: `rm -r tmp/cache && rake assets:clean assets:clobber`

