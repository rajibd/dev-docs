<div><h3 class="sub-docs-type" id="bigcommerce-for-wordpress">BigCommerce for WordPress</h3>

# Site Launch Checklist

<div class="otp" id="no-index">

### On this page
- [Required steps](#required-steps)
- [Optional steps](#optional-steps)
- [Resources](#resources)

</div>

This document contains our recommended steps before launching any BigCommerce for WordPress site. It assumes that you know how to install the plugin and have a reasonable understanding of wordPress and BigCommerce. If that is not the case, check out the overview of BigCommerce and the plugin here:

- [BigCommerce Overview](https://support.bigcommerce.com/s/article/Getting-Started-with-the-New-Control-Panel)
    - [Store profile](https://support.bigcommerce.com/s/article/Store-Profile-Settings)
    - [Taxes](https://support.bigcommerce.com/s/article/Tax-Overview)
    - [Shipping](https://support.bigcommerce.com/s/article/Shipping-Setup)
    - [Payment methods](https://support.bigcommerce.com/s/article/Online-Payment-Methods)
- [BigCommerce for WordPress Overview](https://developer.bigcommerce.com/bigcommerce-for-wordpress/getting-started/introduction) 

## Required steps

### Set up WordPress host to meet recommended server requirements

While you can use BigCommerce for WordPress on the vast majority of WordPress hosts, it is best to use the latest stable versions of PHP, MySQL, and WordPress to ensure the most performant, secure experience.

* PHP: 7.3+
    * With `intl` extension enabled for better currency formatting
* MySQL: 5.6+
* WordPress: 5.3+
* SSL

### Connect BigCommerce for WordPress to BigCommerce using API credentials

BigCommerce for WordPress connects to the BigCommerce API with a set of API credentials. These credentials are for a single BigCommerce app. 

For production sites, you will need to create a separate set of API credentials to ensure proper rate limits are applied. 

This is absolutely required if you want more than one WordPress channel configured in your account, as using the standard **connect to BC** flow in the plugin will disconnect previous app connections from your other sites. 

For instructions on how to create additional API credentials, see [Multisite Setup](https://developer.bigcommerce.com/bigcommerce-for-wordpress/setup/multi-site).

Additionally, API credentials can be set using constants in the site's `wp-config.php` file. Values set here will override options in the database.

```
define( 'BIGCOMMERCE_API_URL', 'https://api.bigcommerce.com/stores/xxxxxxxxxx/v3/' );
define( 'BIGCOMMERCE_CLIENT_ID', 'xxxxxxxxxx' );
define( 'BIGCOMMERCE_CLIENT_SECRET', 'xxxxxxxxxx' );
define( 'BIGCOMMERCE_ACCESS_TOKEN', 'xxxxxxxxxx' );
```

### Configure BigCommerce for WordPress product sync

The product sync will occur every 5 minutes, by default, and any products that are new in BigCommerce, and not on the WordPress site, will be automatically listed on the site.

These settings can be configured to match business requirements. The frequency of new products being added and whether all of the catalog should be for sale online determines the right settings. 

The plugin uses the standard [WP-Cron](https://developer.wordpress.org/plugins/cron/) system, which needs traffic to process. If you would like to process regardless of if there are visitors to the site or not, we recommend setting an a cron job on the server to force processing. Information on how to use the System Track Scheduler for WP-Cron can be found in the [WordPress Handbook](https://developer.wordpress.org/plugins/cron/hooking-wp-cron-into-the-system-task-scheduler/).

Product, inventory, and checkout webhook events are automatically set up when the plugin is installed. There actions in BigCommerce will automatically trigger a future partial sync for only the affected products. 

### Select cart type

Within BigCommerce for WordPress, there are multiple ways to configure how the cart interactions work. Take the time to select the right experience:

**Mini-cart**
* Keep the shopper on the page after they have added an item.
* When the cart menu item is clicked, show a mini-cart overview as an overlay on the page, which details the items, total, and links to view cart or checkout.
*  Also available as a widget to built into other parts of the template, such as display a persistent, dynamic sidebar cart.

**Ajax add-to-cart**
* Keep the shopper on the page after they have added an item.
* When the cart menu item is clicked, navigate to cart page.

**Redirect to cart**
* Navigate to the cart page when the shopper adds and item to cart.

### Select Checkout Type

The checkout type is important as it determines which payment methods and checkout customizations are available to the site.

Embedded Checkout creates a more deeply integrated experience, where the user stays on the WP site. However, there are less supported payment methods available to this checkout type. If you want to enable hosted wallets like Amazon Pay, for example, you won’t be able to use Embedded Checkout.

Redirected Checkout is what unlocks the full checkout experience of BigCommerce, including hosted wallets and access to customize the checkout layout through the Stencil framework and Checkout SDK. You will want to set up a subdomain like checkout.your-domain.com for the best shopper experience, so the hand off from WP to BC is seamless. For details on how to change your store domain, see [Changing Domains](https://support.bigcommerce.com/s/article/Changing-Domains).

Note that in either case, the links within checkout like register account and edit cart will be automatically set by the plugin to link back to the correct pages on the WP site using the [Sites and Routes API](https://developer.bigcommerce.com/api-reference/cart-checkout/sites-routes-api). 

### Enable user registration on WordPress

To enable BigCommerce for WordPress to handle registration of new customers, the WordPress site first needs to be set up to allow membership of new users.

Enable this in the Settings > General section of the WordPress admin by turning on ‘Anyone can register’ for the Membership option. 

### Ensure dynamic pages are not cached

Customer specific pages like Cart, My Account, and Checkout should not be cached by any plugins or the WordPress host.

### Customize the layout and color scheme to best fit theme

In the WordPress Customizer (Appearance -> Customize), there is a BigCommerce section once BigCommerce for WordPress is installed. This allows you to change, without code, the following options:

**Buttons**
* "Add to Cart" Button Label
* "Pre-Order Add to Cart" Button Label
* "Buy Now" Button Label
* "Pre-Order Now" Button Label
* "Choose Options" Button Label
* "View Product" Button Label

**Colors & Theme**
* Use plugin CSS or disable built-in styles
* Button Color
* Button Text Color
* Sale Price Color
* Sale Icon Text Color
* Product Availability Color
* Production Condition Color
* Product Condition Color

**Product Single** 
* Related Products to display
* Default Product Image
* Control how default prices display while waiting for Pricing API responses
* Inventory display logic

**Product Catalog** 
* Catalog Page Title
* Catalog Page Slug
* Catalog Page Description
* Enabled Sorting Options
    * Featured
    * Newest
    * Best Selling
    * Product Title A–Z
    * Product Title Z–A
    * Reviews
    * Price (low to high)
    * Price (high to low)
* Filters 
    * Categories
    * Brands
    * Columns in Product Grid
    * Products per Page
    * Whether Quick View is enabled or not

**Cart** 
* Enable mini-cart or not

**Embedded Checkout** 
* Background Color
* Text Color
* Link Color
* Error Color

## Optional steps 

### Create staging WordPress instance

One of the benefits of using BigCommerce for WordPress is it’s very simple to set up a staging site on WordPress.

To do this in a way that doesn’t affect the main site, you want to set up an additional Channel for the staging site. This will ensure you can test the shopper experience end-to-end with all the URLs linking back to the staging site and an independent set of products listed.

The quickest way to do this is by installing BigCommerce for WordPress on a second WordPress site using the API credentials connection method and choosing to create a new Channel during the setup process.

### Hide WordPress admin bar for customers

The default setup of WordPress will show an “admin” bar at the top of every page when logged in, for every user. It provides a quick link back to the main site, the user’s name, and the ability to log out. This makes sense for sites that are mainly content and might have additional custom actions in the bar, but for commerce-focused sites you may want to hide it.

For a BigCommerce for WordPress child plugin that hides the bar for customers, see [BC4WP plugin](https://github.com/becomevocal/bc4wp-hide-wp-admin-bar-for-customers).

### Stop unnecessary webhooks

BigCommerce contains a number of webhooks that your site may or may not benefit from. If there are unneeded webhooks it is recommended to disable them to enhance performance.

You can tune this by using the [Webhooks API](https://developer.bigcommerce.com/api-docs/store-management/webhooks/overview) and disabling the webhooks you do not need, by updating the `is_active` fields to false.

Webhooks are associated by client ID, so you want to make sure you are using the same API credentials to make these Webhook API calls as BigCommerce for WordPress is using.

### Connect multiple channels into BigCommerce for WordPress

More than one Channel can be connected to BC4WP to drive multi-region and other complex shopper experiences. For example, you could set up Channels for US, FR, and UK regions that reference different names, descriptions, and price lists for each, and then have them activate when certain subdirectories are accessed or you could have a B2B Channel that activates only when someone has a certain cookie set.

This requires activation of ‘multi-channel’ mode, which is a one-liner you add to your WordPress code, and some customization to tell WordPress when to use each Channel.

`add_filter( 'bigcommerce/channels/enable-multi-channel', '__return_true' );`

For more details on how to build a multi-region storefront with BigCommerce for WordPress, see [Building a Multi-Region Storefront](https://medium.com/bigcommerce-developer-blog/build-a-multi-region-storefront-with-bigcommerce-for-wordpress-3-1-0-80cf56c3e8e9).

### CLI setup instructions

The import process will run faster and more reliably if run from the command line.

1. Ensure that your host has [WP-CLI](https://wp-cli.org/) installed and configured to manage your WordPress site.
2. The initial import will likely take longer than subsequent imports. Run the following command for the first import: 
`wp bigcommerce import products`
3. Set up a system cron job to run the import at your desired frequency. For example, to run every 10 minutes:
`*/10 * * * * /usr/bin/wp bigcommerce import products --quiet > /dev/null 2>&1`

Note that this does not disable the WordPress cron-based import process. But so long as the CLI import runs more frequently than the import configured on the settings screen, the cron-based import should never be triggered. In the event that both run are running concurrently, safeguards are in place to ensure that only one process runs at a time.

![Product Sync](https://raw.githubusercontent.com/bigcommerce/dev-docs/master/assets/images/cli-setup-instructions.png)

Subsequent imports will only update products that have changed since the last import. To force an update on all products, add the `--force` flag to the import command:
`wp bigcommerce import products --force`

### Set up and recommendations for Cron

WordPress sites will generally operate more efficiently if WordPress's default cron is disabled and replaced with a server-side cron job.

To disable WordPress's default cron, set a constant in your 
`wp-config.php: define( 'DISABLE_WP_CRON', true );`

Then set up a system cron job to run WordPress's cron events from the command line. Running the job every minute is generally recommended.
`* * * * * /usr/bin/wp cron event run --due-now --quiet > /dev/null 2>&1`

### Details on how to debug

If the site is not importing as expected, enable diagnostic logging to produce a detailed log of the import process.

![Diagnostic logging](https://raw.githubusercontent.com/bigcommerce/dev-docs/master/assets/images/details-on-how-to-debug.png)

After the next import has run, click "Get Diagnostics" to display data from the logs (along with other information about the site).

If running an import from the command line, add the `--debug` flag to get additional information as the import runs, including API error messages.

`wp bigcommerce import products --debug`

If network latency is causing your API requests to timeout, the timeout duration can be increased with a filter:

```js
add_filter( 'bigcommerce/api/timeout', function () {
	return 60;
} );
```

### Expectations for performance

The duration of the import process depends on a number of factors, including network latency, system load, number of products, and the volatility of the catalog.

For an initial import, one can presume roughly 1-2 seconds per product. So a catalog of 30 products may take around a minute to import, whereas 10,000 products may take several hours.

Subsequent imports only update products that have changed in some way. This can reduce the overall processing time to less than a second per product, which includes some overhead to determine which products have changed.

It is important to note changes that will cause a product to be updated:
* Changes to the title/description/categories/price/etc. in the BigCommerce admin.
* Sales of the product, which change the "total_sold" property.
* New product reviews.
* Changes in inventory level, including variants.

### Hosting specs

The BigCommerce for WordPress plugin should be able to run in any environment supported by the current version of WordPress.
* PHP: 5.6.20+, but 7.3+ recommended.
* MySQL: 5.6+
* WordPress: the current major release (5.3.x) or the prior two major versions (5.1+) (note that WordPress considers the first two sequences when identifying a "major" version).
* SSL is required for OAuth and Embedded Checkout (and recommended in general).

Extend BigCommerce with compatible plugins

There is a growing list of plugins that support BigCommerce either through added support to their core plugin or additional child plugins. This enables you to add more complex functionality like faceted search without coding it yourself.

Keep up to date with the supported plugins through the ‘Resources’ section in BigCommerce for WordPress.


## Resources

[Customization Guide](https://developer.bigcommerce.com/bigcommerce-for-wordpress/extending-the-plugin/customization-guide)
[Proxy REST API Endpoints](https://developer.bigcommerce.com/bigcommerce-for-wordpress/extending-the-plugin/proxy-rest-api-endpoints)
[BigCommerce Plugin Code Reference](https://bigcommerce.moderntribe.qa/)
[BigCommerce for WordPress Changelog](https://github.com/bigcommerce/bigcommerce-for-wordpress/releases) (GitHub)