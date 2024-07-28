# vscode-snippet-and-custom-wpdebug

## Asennus
1. Avaa Visual Studio Code
2. Valitse hiirellä File->Preferences->Configure User Snippets
3. Valitse New Global Snippets File
4. Kirjoita jokin nimi Snippetsille esim. custom-snippet
5. Kopioi tämän repositoryn tiedostosta vscode-snippet sisältö ja korvaa äsken luotu tiedosto tällä
6. Kirjoita haluamasi lisäosan alkuun wpdebug ja valitse listalta lisäämäsi snippets

## Vaikutus
Snippets luo wpdebug koodin joka helpottaa php rivien debuggaamista. Tiedosto wp-custom.log tallennetaan wordpressin uploads kansioon.

```php
if (!function_exists("wpdebug")) {
    function wpdebug($msg, $js_console = false) {
        $upload_dir = wp_upload_dir();
        $log_file_prefix = 'wp-custom';
        $log_file = $log_file_prefix . '.log';

        if (wp_get_environment_type() !== "production") {
            if (is_array($msg) || is_object($msg)) {
                $message_with_meta =  date('Y-m-d H:i:s') . '| ' . print_r($msg, true)  . "\n";
            } else {
                $message_with_meta = date('Y-m-d H:i:s') . '| ' . $msg  . "\n";
            }
            // This is logged under the uploads directory
            error_log($message_with_meta, 3, $upload_dir['basedir'] . '/' .  $log_file);
            // For JavaScript console
            if ($js_console) {
                $js_code = 'console.log(' . json_encode($msg, JSON_HEX_TAG) . ');';
                echo "<script>" . $js_code . "</script>";
            }
        }
    }
}

wpdebug('lokitetaan tämä teksti');
wpdebug('lokitetaan tämä teksti myös JavaScript consoleen', true);
