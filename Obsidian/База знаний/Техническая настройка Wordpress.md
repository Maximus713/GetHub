#Wordpress
[[Настройка сайта на Wordpress]]
[[Техническая настройка сайта blogfoto.ru]]
Настройка:
Robot
.htaccess
Elementor
[[Вставка кода на Wordpress|Вставка кода]]
[[Настройка производительности]]


### Изменение параметров Open Server
- Для изменения параметра "Time Limit"  в файле "wp-config" после строчки - define( 'WP_DEBUG', false ); была сделана запись:  set_time_limit(600);
- Для изменения параметра: PHP Max Input Vars, в файле .htaccess была сделана запись: php_value max_input_vars 5000
- Для изменения параметра WP Memory Limit, в файле: "wp-config", после строки: "define( 'WP_DEBUG', false ); ", сделать запись: "define( 'WP_MEMORY_LIMIT', '256M' );"
- Для изменения "Post Size (post_max_size)", в файле ".htaccess", сделать запись:  "php_value post_max_size 1000M"

Другие записи для ".htaccess"
php_value upload_max_filesize 1000M
php_value post_max_size 1000M
php_value max_execution_time 300
php_value max_input_time 300