# AquaRoute · Bottled Water Booking & Supply

An installable, standalone WordPress plugin with customer ordering and supplier order and delivery management.

## Install and configure

1. Open **Plugins → Add New Plugin → Upload Plugin** in WordPress.
2. Upload **bottled-water-booking.zip**, then install and activate it.
3. Open **Water Supply → Settings** and replace the sample products, prices, business name and currency. Prices use a fixed two-decimal scale: **500 = 5.00**. Set the delivery fee, lead time, booking window and delivery slots.
4. Set the supplier notification email. Customer emails use your existing WordPress mail transport; configure it and send a trial order before opening bookings.
5. Create a WordPress page and add a **Shortcode** block:

   ```text
   [bwb_order_form]
   ```

6. Publish the page and link it from your menu. It includes both booking and tracking. For a separate tracking page, use:

   ```text
   [bwb_order_tracking]
   ```

7. In **Users**, assign staff the **Water Supplier** role. They can handle every water order on the site. Only administrators can change settings.

Requires WordPress 6.2+ and PHP 7.4+. No WooCommerce or JavaScript build process is required. On multisite, activate separately on each site.

## Customer flow

Select products and quantities, enter contact and delivery details, choose a delivery window, and place the order. Payment is **cash on delivery**. The server determines prices and totals from the catalogue. Confirmation shows the order reference, private tracking key and a link that the customer can save. A customer email includes the same details when enabled.

The tracking key grants access to the order's status and summary. It does not expose contact information or internal delivery notes. Only a hash of the key is stored, so a supplier cannot retrieve a lost key. Customers contact the supplier for changes or cancellations.

## Supplier flow

Open **Water Supply → Orders** to view totals, search by reference/customer/phone, and filter by status or delivery date. Open an order to inspect contact details and products, assign a driver by name, edit internal delivery notes, reschedule, and record payment.

The normal delivery sequence is:

```text
Pending → Confirmed → Out for delivery → Delivered
```

Active orders can be cancelled. An out-for-delivery order can return to confirmed if dispatch needs to be retried. Delivered and cancelled orders cannot be reopened or rescheduled. Payment and internal notes remain editable. Concurrent edits are checked so a stale screen cannot silently overwrite a newer save.

Use **Export CSV** for filtered order records. The export is limited to 10,000 rows; use date/status filters for larger histories. Revenue reports include delivered orders marked paid, in the current configured currency. Past orders preserve their original prices and currency.

## Launch checklist

- Replace the sample catalogue and verify the business name, currency and minor-unit prices.
- Set the WordPress timezone under **Settings → General**; booking dates use that timezone.
- Confirm your delivery area, cancellation terms, taxes and bottle-return policies on the site. This version does not calculate taxes, deposits, stock, service areas or delivery capacity.
- Test a customer booking, email delivery, tracking, supplier updates and mobile layout.
- Configure HTTPS and your normal WordPress backups and access controls.
- Exclude the nonce refresh request and `/wp-json/bwb/v1/*` from API/CDN caching. The form itself can be page-cached.
- Review **Settings → Privacy** and set a retention policy. IP-based anti-abuse counters retain a salted hash temporarily; order records stay until erased or explicitly removed.

## Scope

This is a single-business, manual-fulfillment plugin. It includes individual orders, driver names, delivery scheduling, cash on delivery, private tracking, CSV exports and WordPress email notifications. It does not include recurring subscriptions, multiple independent vendors, stock or bottle-return management, card payments, GPS route planning, SMS or WhatsApp.

## Personal data and removal

WordPress **Tools → Export Personal Data / Erase Personal Data** includes these orders. Erasure removes contact/address data and notes and revokes tracking access while retaining transaction totals. Resolve active deliveries before approving erasure, as their delivery address will be removed too.

Deactivation and deletion preserve data by default. If you intend to permanently delete the plugin data, back up first, then add this line to `wp-config.php` before deleting the plugin through WordPress:

```php
define( 'BWB_DELETE_DATA_ON_UNINSTALL', true );
```

Remove that line afterward. On multisite, this opt-in deletion removes the plugin's data across **every site in the network**.

## Package contents

- `bottled-water-booking.zip`: upload directly in WordPress.
- `SETUP-GUIDE.md`: this guide.
- `TEST-REPORT.md`: verification results and environment details.
- Preview images: the customer form and supplier dashboard captured from the local WordPress test site.
