---
layout: default
title: Eksport użytkowników z WordPress do CSV
---

# Eksport użytkowników z WordPress do CSV

## functions.php
```php
require_once __DIR__ . '/includes/partials/export-users-data/createCSV.php';

add_filter( 'bulk_actions-users', 'export_user_data' );

function export_user_data( $new_bulk_actions ) {
    $new_bulk_actions[ 'export_user_data' ] = __( 'Export', 'export_user_data');
    
    return $new_bulk_actions;
}

add_filter('handle_bulk_actions-users', 'export_user_data_handler', 10, 3);

function export_user_data_handler($redirect_to, $do_action, $object_ids) {
    if ($do_action !== 'export_user_data') {
        return $redirect_to;
    }

    foreach ($object_ids as $user_id) {
        $na_retailer_info = maybe_unserialize(get_user_meta($user_id, 'na_retailer_info', true));
        $pw_user_status = get_user_meta($user_id, 'pw_user_status', true);

        if(!$na_retailer_info) {
            continue;
        }

        if ($pw_user_status) {
            $user_data['status'] = $pw_user_status;
        }
        
        if ($na_retailer_info['Company']) {
            $user_data['company'] = $na_retailer_info['Company'];
        }
        
        if ($na_retailer_info['Country']) {
            $user_data['country'] = $na_retailer_info['Country'];
        }
        
        if ($na_retailer_info['VAT_Identification']) {
            $user_data['VAT_Identification'] = $na_retailer_info['VAT_Identification'];
        }
        
        if ($na_retailer_info['field_2']) {
            $user_data['county'] = $na_retailer_info['field_2'];
        }

        $users_data[] = $user_data;
	}

    $headers = 'Status;Company;Country;VAT_Identification;County;';
    $fileName = 'exported_users_data';
    createCSV($headers, $users_data, $fileName);
}
```

## createCSV.php
```php
<?php
function createCSV(string $headers, array $data, string $fileName) {
    header('Content-Description: File Transfer');
    header('Content-Disposition: attachment; filename='. $fileName .'.csv');
    header('Content-Type: text/csv; charset=' . get_option('blog_charset'), true);

    echo $headers;
    echo PHP_EOL;

    foreach ($data as $values) {
        foreach ($values as $key => $value) {
            echo $value;
            echo ';';
        }
        
        echo PHP_EOL;
    }
}
```