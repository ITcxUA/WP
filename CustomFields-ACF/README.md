# Вывод произвольных полей ACF
Рассмотрим вывод произвольных полей, созданных в Advanced Custom Fields для WordPress. А так же, как вывести произвольные поля для терминов таксономии, например, «Рубрики» в шаблоне category.php


Содержание:
- Текст, число, область текста, файл, медиа;
- Изображение:
- Ссылка;
- Id;
Массив;
Галерея;
Повторитель;
Объект записи;
Группа;
Гибкое содержание;
Ссылка;
Вывод поля, если оно заполнено;
Вывод полей для другой страницы;
Вывод полей для терминов таксономии;
Вывод полей для профиля пользователя;
Страница с опциями ACF;
вывод полей в нужном шаблоне;
дочерние страницы.
несколько страниц с опциями;
иконка для пункта меню опций.
Типы полей — текст, число, область текста, файл, медиа

Чтобы вывести текст, область текста, число, файл или медиа, воспользуйтесь кодом ниже.
<?= get_field("имя_поля"); ?>
Тип поля — «Изображение»


## Ссылка на изображение (URL)
> Данный способ вывода изображения не позволяет вывести дополнительные данные об изображении. Только URL. Используйте следующий код.


<img src="<?php the_field('имя_поля_с_изображением'); ?>" />
ID изображения

При выборе формата вывода — ID изображения, используется функция wp_get_attachment_image().


<?php
$id_image = get_field("имя_поля");
$size_image = "full"; // (thumbnail, medium, large, full или свой)

if ($id_image) {
    echo wp_get_attachment_image($id_image, $size_image);
}
Ещё пример:


<?php $image_by_id = wp_get_attachment_image_src(get_field("имя_поля_изображения"), "medium"); ?>

<img 
    src="<?= $image_by_id[0]; ?>" 
    alt="<?= get_the_title(get_field('имя_поля_изображения')); ?>" 
/>
Массив изображения


<?php if ($image_array = get_field("имя_поля_изображения")) { ?>
    <img 
        src="<?= esc_url($image_array['url']); ?>" 
        alt="<?= esc_attr($image_array['alt']); ?>"
        width="<?= esc_attr($image_array['width']); ?>"
        height="<?= esc_attr($image_array['height']); ?>"
        loading="lazy"
    />
<?php } ?>
Вывод нужно размера изображения, заголовка или всплывающей подсказки.


<?= esc_url($image_array["url"]) ?> - вывод URL изображения (полный размер);
<?= esc_url($image_array["sizes"]["thumbnail"]) ?> - thumbnail, medium, large или свой;
<?= esc_attr($image_array["alt"]) ?> - вывод alt изображения;
<?= esc_attr($image_array["title"]) ?> - вывод заголовка изображения;
<?= esc_html($image_array["caption"]) ?> - вывод подписи изображения.
Тип поля — «Галерея»

Рассмотрим возвращаемый формат — массив изображения.


<?php if ($img_gallery = get_field("имя_галереи")) : ?>
    <?php foreach ($img_gallery as $img) : ?>
        <?php if ($img) : ?>
            <?= "<img 
                src="<?= esc_url($img['sizes']['thumbnail']) ?>" 
                alt="<?= esc_attr($img['alt']) ?>"
                loading="lazy"
                width="<?= esc_attr($image_array['width']) ?>"
                height="<?= esc_attr($image_array['height']) ?>"
            />" ?>
        <?php endif; ?>
    <?php endforeach; ?>
<?php endif; ?>
Использование очень похоже на вывод изображений, за исключением использования цикла foreach().

Тип поля — «Повторитель»

Для вывода повторителя (repeater), используйте код ниже.


<?php if (have_rows("имя_поля_повторителя")) : ?>
    <?php while (have_rows("имя_поля_повторителя")) : the_row(); ?>
        <?php if (get_sub_field('имя_поля_в_повторителе')) : ?>
            <?php the_sub_field("имя_поля_в_повторителе") ?>
        <?php endif; ?>
    <?php endwhile; ?>
<?php endif; ?>
Вывод индекса строки:


<?= get_row_index(); ?>
Тип поля — «Объект записи»

Для вывода нескольких значений «Объекта записи» используйте следующий код.


<?php if ($post_objects = get_field("имя_поля_объект_записи")) : ?>
    <?php // переменная должна называться $post (ВАЖНО)
    foreach ($post_objects as $post) : setup_postdata($post); ?>
        <?php if ($post) : ?>

            <?php the_title(); ?>
            <?php the_excerpt(); ?>
            <?php the_field("имя_поля"); ?>
            <?php the_permalink(); ?>

        <?php endif; ?>
    <?php endforeach; ?>
    <?php wp_reset_postdata(); ?>

