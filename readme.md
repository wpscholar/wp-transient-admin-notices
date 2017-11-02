# WordPress Transient Admin Notices
Handles the display of admin notices in WordPress after a page redirect or reload.

## Details

When creating a new instance of `TransientAdminNotices`, you will need to provide a transient name. This is used as either the cache key, when caching is enabled, or the database option name, if caching isn't an option. If you're notice is user-specific, be sure to scope your transient name to the user.

Ensure that you instantiate the class on the `admin_init` hook. This ensures that the class can automatically setup a hook to render your notices on the `admin_notices` hook and that it only runs in the WordPress admin. Note that you must create an instance, even if you are not adding new notices, in order for notices from a previous page load to display.

All notices added to your instance require a key, the message you want to display, and the notice type. The key can be any string you want and can be used to later get or remove that specific notice from the queue. 


## Notice Types

These notice types are the types provided by WordPress, but can be passed in to the `add()` method to customize how your notices will display. 

- `success` - Displays with a green bar 
- `info` - Displays with a blue bar (default)
- `warning` - Displays with an orange bar
- `error` - Displays with a red bar

## Sample Usage

```php
<?php 

use wpscholar\WordPress\TransientAdminNotices;

add_action( 'admin_init', function() {
	
    $transient_name = md5( 'my_plugin' . get_current_user_id() );
    $notices = new TransientAdminNotices( $transient_name );
    
    if( ! $user_has_account ) {
        $notices->add( 'msg1', 'Looks like you have not created an account yet!', 'warning' );
    }
    
} );
```