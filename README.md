# woocommerce-settings
Here are a few settings to get you started developing with woocommerce.


## Notes
I hope this helps. Here are some hooks that have helped me in the past when theming/developing for WooCommerce.

Please see the (WooCommerce documentation)[https://docs.woocommerce.com/documentation/plugins/woocommerce/] for more details.


# Here is the code to copy or download the repository:

```
<?php

function namespace_here_woocommerce_scripts() {

	$font_path   = WC()->plugin_url() . '/assets/fonts/';
	$inline_font = '@font-face {
			font-family: "star";
			src: url("' . $font_path . 'star.eot");
			src: url("' . $font_path . 'star.eot?#iefix") format("embedded-opentype"),
				url("' . $font_path . 'star.woff") format("woff"),
				url("' . $font_path . 'star.ttf") format("truetype"),
				url("' . $font_path . 'star.svg#star") format("svg");
			font-weight: normal;
			font-style: normal;
		}';

	wp_add_inline_style( 'namespace_here-woocommerce-style', $inline_font );
}
add_action( 'wp_enqueue_scripts', 'namespace_here_woocommerce_scripts' );

/**
 * Add 'woocommerce-active' class to the body tag.
 *
 * @param  array $classes CSS classes applied to the body tag.
 * @return array $classes modified to include 'woocommerce-active' class.
 */
function namespace_here_add_body_class( $classes ) {
	$classes[] = 'woocommerce-active';

	return $classes;
}
add_filter( 'body_class', 'namespace_here_add_body_class' );


/**
 * Change number of products that are displayed per page (shop page)
 */
function new_loop_shop_per_page( $cols ) {
  // $cols contains the current number of products per page based on the value stored on Options -> Reading
  // Return the number of products you wanna show per page.
  $cols = 4;
  return $cols;
}
add_filter( 'loop_shop_per_page', 'new_loop_shop_per_page', 20 );

/**
 * Remove default WooCommerce wrapper.
 */
remove_action( 'woocommerce_before_main_content', 'woocommerce_output_content_wrapper', 10 );
remove_action( 'woocommerce_after_main_content', 'woocommerce_output_content_wrapper_end', 10 );

if ( ! function_exists( 'namespace_here_woocommerce_wrapper_before' ) ) {
	/**
	 * Before Content.
	 *
	 * Wraps all WooCommerce content in wrappers which match the theme markup.
	 *
	 * @return void
	 */
	function namespace_here_woocommerce_wrapper_before() {
		?>
    <div id="woo" class="padding-seven-tb bg-white">
    		<div class="container">
    				<div class="row">
    					<div class="col-md-12">

			<?php
	}
}
add_action( 'woocommerce_before_main_content', 'namespace_here_woocommerce_wrapper_before' );

if ( ! function_exists( 'namespace_here_woocommerce_wrapper_after' ) ) {
	/**
	 * After Content.
	 *
	 * Closes the wrapping divs.
	 *
	 * @return void
	 */
	function namespace_here_woocommerce_wrapper_after() {
			?>
    </div>
    </div>
    </div>
    </div>

		<?php
	}
}
add_action( 'woocommerce_after_main_content', 'namespace_here_woocommerce_wrapper_after' );

// add open div to shop loop
function woocommerce_product_loop_start() {
	 $cols = 4;
	 echo '<ul class="margin-seven-half-tb products columns-'. $cols . '">';
}
// add closing div to shop loop
function woocommerce_product_loop_end() {
	 echo '</ul>';
 }



```
