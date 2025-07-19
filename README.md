# WordPress Disable Emojis

This comprehensive code snippet allows you to **completely disable WordPress's emoji functionality across your entire site**, including both the frontend and the admin area. WordPress loads JavaScript and CSS for emoji support by default. If you don't use emojis or prefer to control them through other means, disabling them can help improve page load times and reduce unnecessary requests.

## Why disable emojis?

* **Performance Optimization:** Removes unnecessary JavaScript and CSS files, leading to faster page load times and fewer HTTP requests.
* **Reduced Bloat:** Declutters your site's code by removing features you might not need or use.
* **Control over Aesthetics:** Ensures that emojis are not rendered in a way that might conflict with your site's design or custom fonts.
* **Simplicity:** For minimalist sites, removing emoji functionality contributes to a cleaner and more streamlined experience.

## Features

* **Prevents emoji detection script from loading:** Stops `wp-emoji-release.min.js` (or similar) from being added to `wp_head`.
* **Removes emoji styles:** Prevents `wp-emoji-styles.min.css` (or similar) from being loaded.
* **Disables emoji functionality in RSS feeds, comments, and emails:** Ensures emojis are not processed in these areas.
* **Disables smilies conversion:** Prevents WordPress from converting text-based emoticons (like `:)` or `:P`) into emoji images.
* Applies to both the public-facing frontend and the WordPress admin dashboard.

## Installation

There are two primary methods to implement this code:

### Method 1: As a Standalone Plugin (Recommended)

Creating a small, dedicated plugin is the most robust way to add this functionality. It ensures your modification remains active regardless of theme changes.

1.  Create a new folder named `wp-no-emojis` inside your WordPress site's `wp-content/plugins/` directory.
2.  Inside this new folder, create a file named `wp-no-emojis.php`.
3.  Copy and paste the following code into `wp-no-emojis.php`:

    ```php
    <?php
    /**
     * Plugin Name: WordPress Disable Emojis
     * Description: Completely disables WordPress's emoji functionality across the frontend and admin.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    // Filters for frontend and admin
    add_filter( 'emoji_svg_url', '__return_false' ); // Disable SVG emojis

    // Main function to remove emoji-related actions and filters
    function herminal_hook_disable_emojies() {
        remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
        remove_action( 'wp_print_styles', 'print_emoji_styles' );
        remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
        remove_action( 'admin_print_styles', 'print_emoji_styles' );
        remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
        remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
        remove_filter( 'embed_head', 'print_emoji_detection_script' );
        remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
        // Also disable old text smilies to image conversion
        add_filter( 'option_use_smilies', '__return_false' );
    }
    add_action( 'init', 'herminal_hook_disable_emojies' );

    // These actions are also added directly to ensure they are removed if init runs later
    remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
    remove_action( 'wp_print_styles', 'print_emoji_styles' );
    remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
    remove_action( 'admin_print_styles', 'print_emoji_styles' );
    ?>
    ```

4.  Go to your WordPress admin dashboard, navigate to **"Plugins"**, and **activate** the "WordPress Disable Emojis" plugin.

### Method 2: Adding to your Theme's functions.php File

You can add this code directly to your active theme's `functions.php` file. **Before doing so, it's highly recommended to back up your `functions.php` file.**

1.  Navigate to `wp-content/themes/YourThemeName/` (replace `YourThemeName` with the actual name of your active theme).
2.  Open the `functions.php` file.
3.  Add the following code to the end of the file (before the closing `?>` tag, if one exists):

    ```php
    add_filter( 'emoji_svg_url', '__return_false' );

    function herminal_hook_disable_emojies() {
        remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
        remove_action( 'wp_print_styles', 'print_emoji_styles' );
        remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
        remove_action( 'admin_print_styles', 'print_emoji_styles' );
        remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
        remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
        remove_filter( 'embed_head', 'print_emoji_detection_script' );
        remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
        add_filter( 'option_use_smilies', '__return_false' );
    }
    add_action( 'init', 'herminal_hook_disable_emojies' );

    // Redundant but harmless calls if init already ran the above removals
    remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
    remove_action( 'wp_print_styles', 'print_emoji_styles' );
    remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
    remove_action( 'admin_print_styles', 'print_emoji_styles' );
    ```

## Important Considerations

* **Emoji Display:** After implementing this, emojis might display as simple text characters or be rendered by the user's operating system/browser, potentially with varying appearances. They will no longer be styled or converted by WordPress's emoji scripts.
* **Font Compatibility:** Ensure your site's fonts support a wide range of Unicode characters if you expect emojis to still show up (even if unstyled).
* **No Functionality Loss:** Disabling emojis generally does not cause any critical functionality loss for most websites, unless your site heavily relies on WordPress's specific emoji rendering features.

## Contributing

Contributions are welcome! If you have suggestions or improvements for this code, feel free to open a "Pull Request" or report an "Issue."

## License

This project is licensed under the GPL-2.0-or-later License.

---

# غیرفعال‌سازی اموجی‌ها در وردپرس

این قطعه کد جامع به شما امکان می‌دهد تا **قابلیت اموجی وردپرس را در سراسر سایت شما، از جمله فرانت‌اند و ناحیه مدیریت، به طور کامل غیرفعال کنید**. وردپرس به طور پیش‌فرض جاوااسکریپت و CSS برای پشتیبانی از اموجی‌ها را بارگذاری می‌کند. اگر از اموجی‌ها استفاده نمی‌کنید یا ترجیح می‌دهید آن‌ها را از طریق روش‌های دیگر کنترل کنید، غیرفعال کردن آن‌ها می‌تواند به بهبود زمان بارگذاری صفحه و کاهش درخواست‌های غیرضروری کمک کند.

## چرا اموجی‌ها را غیرفعال کنیم؟

* **بهینه‌سازی عملکرد:** فایل‌های جاوااسکریپت و CSS غیرضروری را حذف می‌کند که منجر به زمان بارگذاری سریع‌تر صفحه و درخواست‌های HTTP کمتر می‌شود.
* **کاهش حجم اضافه:** با حذف ویژگی‌هایی که ممکن است به آن‌ها نیازی نداشته باشید یا از آن‌ها استفاده نکنید، کد سایت شما را خلوت‌تر می‌کند.
* **کنترل بر زیبایی‌شناسی:** تضمین می‌کند که اموجی‌ها به روشی که ممکن است با طراحی سایت یا فونت‌های سفارشی شما تداخل داشته باشند، رندر نشوند.
* **سادگی:** برای سایت‌های مینیمالیستی، حذف قابلیت اموجی به تجربه‌ای تمیزتر و کارآمدتر کمک می‌کند.

## قابلیت‌ها

* **جلوگیری از بارگذاری اسکریپت تشخیص اموجی:** از اضافه شدن `wp-emoji-release.min.js` (یا مشابه) به `wp_head` جلوگیری می‌کند.
* **حذف استایل‌های اموجی:** از بارگذاری `wp-emoji-styles.min.css` (یا مشابه) جلوگیری می‌کند.
* **غیرفعال کردن قابلیت اموجی در فیدهای RSS، دیدگاه‌ها و ایمیل‌ها:** تضمین می‌کند که اموجی‌ها در این مناطق پردازش نمی‌شوند.
* **غیرفعال کردن تبدیل شکلک‌ها:** از تبدیل شکلک‌های متنی (مانند `:)` یا `:P`) به تصاویر اموجی توسط وردپرس جلوگیری می‌کند.
* در هر دو بخش فرانت‌اند (قابل مشاهده برای عموم) و داشبورد مدیریت وردپرس اعمال می‌شود.

## نصب

برای پیاده‌سازی این کد، دو روش اصلی وجود دارد:

### روش ۱: به عنوان یک افزونه مستقل (توصیه شده)

ایجاد یک افزونه کوچک و اختصاصی، قوی‌ترین راه برای افزودن این قابلیت است. این تضمین می‌کند که تغییر شما صرف‌نظر از تغییر قالب، فعال باقی بماند.

1.  یک پوشه جدید با نام `wp-no-emojis` در مسیر `wp-content/plugins/` سایت وردپرسی خود ایجاد کنید.
2.  در داخل این پوشه جدید، یک فایل با نام `wp-no-emojis.php` ایجاد کنید.
3.  کد زیر را در `wp-no-emojis.php` کپی و جایگذاری کنید:

    ```php
    <?php
    /**
     * Plugin Name: WordPress Disable Emojis
     * Description: Completely disables WordPress's emoji functionality across the frontend and admin.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    // Filters for frontend and admin
    add_filter( 'emoji_svg_url', '__return_false' ); // Disable SVG emojis

    // Main function to remove emoji-related actions and filters
    function herminal_hook_disable_emojies() {
        remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
        remove_action( 'wp_print_styles', 'print_emoji_styles' );
        remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
        remove_action( 'admin_print_styles', 'print_emoji_styles' );
        remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
        remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
        remove_filter( 'embed_head', 'print_emoji_detection_script' );
        remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
        // Also disable old text smilies to image conversion
        add_filter( 'option_use_smilies', '__return_false' );
    }
    add_action( 'init', 'herminal_hook_disable_emojies' );

    // این اکشن‌ها همچنین به طور مستقیم اضافه شده‌اند تا اطمینان حاصل شود که اگر init قبلاً حذف‌های فوق را اجرا کرده باشد، حذف می‌شوند
    remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
    remove_action( 'wp_print_styles', 'print_emoji_styles' );
    remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
    remove_action( 'admin_print_styles', 'print_emoji_styles' );
    ?>
    ```

4.  وارد پنل مدیریت وردپرس خود شوید، به بخش **"افزونه‌ها"** بروید و افزونه **"غیرفعال‌سازی اموجی‌ها در وردپرس"** را **فعال کنید**.

### روش ۲: اضافه کردن به فایل functions.php قالب شما

می‌توانید این کد را مستقیماً به فایل `functions.php` قالب فعال خود اضافه کنید. **پیشنهاد اکید می‌شود قبل از انجام این کار، از فایل `functions.php` خود یک پشتیبان (backup) تهیه کنید.**

1.  به مسیر `wp-content/themes/YourThemeName/` بروید (به جای `YourThemeName` نام واقعی قالب فعال خود را قرار دهید).
2.  فایل `functions.php` را باز کنید.
3.  کد زیر را به انتهای فایل (قبل از تگ بستن `?>`، در صورت وجود) اضافه کنید:

    ```php
    add_filter( 'emoji_svg_url', '__return_false' );

    function herminal_hook_disable_emojies() {
        remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
        remove_action( 'wp_print_styles', 'print_emoji_styles' );
        remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
        remove_action( 'admin_print_styles', 'print_emoji_styles' );
        remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
        remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
        remove_filter( 'embed_head', 'print_emoji_detection_script' );
        remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
        add_filter( 'option_use_smilies', '__return_false' );
    }
    add_action( 'init', 'herminal_hook_disable_emojies' );

    // این اکشن‌ها همچنین به طور مستقیم اضافه شده‌اند تا اطمینان حاصل شود که اگر init قبلاً حذف‌های فوق را اجرا کرده باشد، حذف می‌شوند (زیانی ندارند)
    remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
    remove_action( 'wp_print_styles', 'print_emoji_styles' );
    remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
    remove_action( 'admin_print_styles', 'print_emoji_styles' );
    ```

## ملاحظات مهم

* **نمایش اموجی:** پس از پیاده‌سازی این کد، اموجی‌ها ممکن است به صورت کاراکترهای متنی ساده نمایش داده شوند یا توسط سیستم عامل/مرورگر کاربر رندر شوند، که احتمالاً با ظاهر متفاوتی همراه خواهد بود. آن‌ها دیگر توسط اسکریپت‌های اموجی وردپرس استایل‌دهی یا تبدیل نخواهند شد.
* **سازگاری فونت:** اطمینان حاصل کنید که فونت‌های سایت شما از طیف وسیعی از کاراکترهای یونیکد پشتیبانی می‌کنند، اگر انتظار دارید اموجی‌ها همچنان نمایش داده شوند (حتی اگر بدون استایل باشند).
* **عدم از دست رفتن قابلیت:** غیرفعال کردن اموجی‌ها به طور کلی برای اکثر وب‌سایت‌ها هیچ از دست رفتن قابلیت حیاتی را به همراه ندارد، مگر اینکه سایت شما به شدت به ویژگی‌های رندر اموجی خاص وردپرس متکی باشد.

## مشارکت (Contributing)

مشارکت شما خوشایند است! اگر پیشنهاد یا بهبودهایی برای این کد دارید، می‌توانید یک "Pull Request" ایجاد کنید یا "Issue" جدیدی را گزارش دهید.

## مجوز (License)

این پروژه تحت مجوز GPL-2.0-or-later منتشر شده است.
