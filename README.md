# Useful ACF code snippets

This is a list of useful WordPress ACF plugin snippets and functions that I often reference to enhance or clean up my sites. 

**Note:** Please be careful and make backups!

**WordPress ACF**

***Basic Fields***

- [Get A Field By Name](#get-a-field-by-name)
- [Get And Format A Date Field](#get-and-format-a-date-field)
- [Field Conditional Also Used for True or False Fields](#field-conditional-also-used-for-true-or-false-fields)
- [Get A Field By Name Within Repeater (Flexible)](#get-a-field-by-name-within-repeater-flexible)

***Image Fields***

- [Image Field With A Return Value Of "Image URL"](#image-field-with-a-return-value-of-image-url)
- [Image Field With A Return Value Of "Image ID"](#image-field-with-a-return-value-of-image-id)
- [Image Field With A Return Value Of "Image Object"](#image-field-with-a-return-value-of-image-object)

***File Fields***

- [File Field With A Return Value Of "File URL"](#file-field-with-a-return-value-of-file-url)
- [File Field With A Return Value Of "File ID"](#file-field-with-a-return-value-of-file-id)
- [File Field With A Return Value Of "File Object"](#file-field-with-a-return-value-of-file-object)
- [Flexible Content Basic Field Returns 1 Row Deep](#flexible-content-basic-field-returns-1-row-deep)
- [Flexible Content Nested Field Returns](#flexible-content-nested-field-returns)
- [Flexible Content Basic Field Returns 1 Row Deep](#flexible-content-basic-field-returns-1-row-deep)

***Relationship Field***

- [Get A Relationship Field And Loop Over All Returned Posts](#get-a-relationship-field-and-loop-over-all-returned-posts)

***Location Fields***

- [Get The Street Address From A Location Field](#get-the-street-address-from-a-location-field)
- [Get A Location Field And Convert It To A Static Google Map](#get-a-location-field-and-convert-it-to-a-static-google-map)
- [Get A Location Field And Convert It To An Interactive Google Map](#get-a-location-field-and-convert-it-to-an-interactive-google-map)

***Gravity Form Field***

- [Display A Gravity Form](#display-a-gravity-form)

***Repeater Field***

- [Get And Loop Over A Repeater Field](#get-and-loop-over-a-repeater-field)
- [Loop Over A Repeater Filed And Seperate Results Into Rows](#loop-over-a-repeater-filed-and-seperate-results-into-rows)

***Queries***

- [Query A Post Type On A Field Value And Loop Over Posts](#query-a-post-type-on-a-field-value-and-loop-over-posts)

## Basic Fields

### Get A Field By Name

```php
/**
 * Get A Field By Name
 */
 
<?php if ( get_field('field_name') ) : ?>
  <?php echo get_field('field_name'); ?>
<?php endif; ?>
```

### Get And Format A Date Field

```php
/**
 * Get And Format A Date Field
 */
 
<?php if ( get_field('field_name') ) : $date = DateTime::createFromFormat('Ymd', get_field('field_name')); ?>
  <?php echo $date->format('d-m-Y'); ?>
<?php endif; ?>
```

### Field Conditional Also Used for True or False Fields

```php
/**
 * Field Conditional / Also Used for True/False Fields
 */
 
<?php if ( get_field('field_name') ) : ?>
  // some code
<?php endif; ?>
```

### Get A Field By Name Within Repeater (Flexible)

```php
/**
 * Get A Field By Name Within Repeater/Flexible
 */
 
<?php if ( get_sub_field('field_name') ) : ?>
  <?php echo get_sub_field('field_name'); ?>
<?php endif; ?>
```

## Image Fields

### Image Field With A Return Value Of "Image URL"

```php
/**
 * Image Field With A Return Value Of "Image URL"
 */
 
<?php if ( get_field('field_name') ) : ?>
    <img src="<?php the_field('field_name'); ?>" alt="<?php the_field(''); ?>">
<?php endif; ?>
```

## Image Field With A Return Value Of "Image ID"

```php
/**
 * Image Field With A Return Value Of "Image ID"
 */
 
<?php
  if ( get_field('field_name') ) {
    $attachment_id = get_field('field_name');
    $size = "full"; // (thumbnail, medium, large, full or custom size)
    wp_get_attachment_image( $attachment_id, $size );
  }
?>
```

## Image Field With A Return Value Of "Image Object"

```php
/**
 * Image Field With A Return Value Of "Image Object"
 */
 
<?php if ( get_field('field_name') ) : $image = get_field('field_name'); ?>

  <!-- Full size image -->
  <img src="<?php echo $image['url']; ?>" alt="<?php echo $image['alt']; ?>"/>

  <!-- Thumbnail image -->
  <img src="<?php echo $image['sizes']['thumbnail']; ?>" alt="<?php echo $image['alt']; ?>"/>

<?php endif; ?>
```

## File Fields

### File Field With A Return Value Of "File URL"

```php
/**
 * File Field With A Return Value Of "File URL"
 */
<?php if ( get_field('field_name') ) : ?>
  <a href="<?php the_field('field_name'); ?>" >Download File</a>
<?php endif; ?>
```

### File Field With A Return Value Of "File ID"

```php
/**
 * File Field With A Return Value Of "File ID"
 */
<?php
  if ( get_field('field_name') ) :
    $attachment_id = get_field('field_name');
    $url = wp_get_attachment_url( $attachment_id );
    $title = get_the_title( $attachment_id );
?>
  <a href="<?php echo $url; ?>" ><?php echo $title; ?></a>
<?php endif; ?>
```

### File Field With A Return Value Of "File Object"

```php
/**
 * File Field With A Return Value Of "File Object"
 */
<?php if ( get_field('field_name') ) : $file = get_field('field_name'); ?>
  <a href="<?php echo $file['url']; ?>"><?php echo $file['title']; ?></a>
<?php endif; ?>
```

### Flexible Content Basic Field Returns 1 Row Deep

```php
/**
 * Flexible Content Basic Field Returns 1 Row Deep
 */
<?php if ( have_rows( 'field_name' ) ) : ?>
    <?php while ( have_rows('field_name' ) ) : the_row();
        if ( get_row_layout() == 'layout_field' ) : ?>
            <div class="class">
                <?php the_sub_field( 'field_name' ); ?>
            </div>
        <?php endif; ?>
    <?php endwhile; ?>
<?php endif; ?>
```

### Flexible Content Nested Field Returns

```php
/**
 * Flexible Content Nested Field Returns
 */
<?php if( have_rows('field_name') ): ?>
    <?php while ( have_rows('field_name') ) : the_row(); ?>
        <?php if( get_row_layout() == 'layout_field' ): ?>
            <?php if( have_rows('layout_rows') ): ?>
                <ul>
                    <?php while ( have_rows('layout_rows') ) : the_row(); $image = get_sub_field('sub_field'); ?>
                        <li><img src="<?php echo $image['url']; ?>" alt="<?php echo $image['alt']; ?>" /></li>
                    <?php endwhile; ?>
                </ul>
            <?php endif; ?>
        <?php endif; ?>
    <?php endwhile; ?>
<?php endif; ?>
```

### Flexible Content Basic Field Returns 1 Row Deep

```php
/**
 * Flexible Content Basic Field Returns 1 Row Deep
 */
<?php if ( have_rows( 'field_name' ) ) : ?>
    <?php while ( have_rows('field_name' ) ) : the_row();
        if ( get_row_layout() == 'layout_field' ) : ?>
            <div class="class">
                <?php the_sub_field( 'field_name' ); ?>
            </div>
        <?php endif; ?>
    <?php endwhile; ?>
<?php endif; ?>
```

## Relationship Field

### Get A Relationship Field And Loop Over All Returned Posts

```php
/**
 * Get A Relationship Field And Loop Over All Returned Posts
 */
<?php $posts = get_field('field_name'); ?>
<?php if ( $posts ): ?>
  <ul>
    <?php foreach ( $posts as $post ) : setup_postdata( $post ); ?>
      <li>
        <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
      </li>
    <?php endforeach; wp_reset_postdata(); ?>
  </ul>
<?php endif; ?>
```

## Location Fields

### Get The Street Address From A Location Field

```php
/**
 * Get The Street Address From A Location Field
 */
<?php if ( get_field('field_name') ) :
  $location = get_field('field_name'); ?>
  <?php echo $location['address']; ?>
<?php endif; ?>
```

### Get A Location Field And Convert It To A Static Google Map

```php
/**
 * Get A Location Field And Convert It To A Static Google Map
 */
<?php if ( get_field('field_name') ) :
  $location = get_field('field_name');
  $coordinates = isset( $location['coordinates'] ) ? $location['coordinates'] : $location ; ?>
  <img src="http://maps.google.com/maps/api/staticmap?markers=<?php echo $coordinates; ?>&size=500x300&sensor=false" alt="">
<?php endif; ?>
```

### Get A Location Field And Convert It To An Interactive Google Map

```php
/**
 * Get a location field and convert it to an interactive Google Map. 
 * Also adds a marker to the location. The CSS is used to prevent rendering 
 * issues with map controls caused by most responsive CSS grids.
 */
<?php if ( get_field('field_name') ) :
  $location = get_field('field_name');
  $coordinates = isset( $location['coordinates'] ) ? $location['coordinates'] : $location ; ?>

  <script src="https://maps.googleapis.com/maps/api/js?v=3.exp&sensor=false"></script>
  <script>
    google.maps.event.addDomListener(window, 'load', function() {
      var map = new google.maps.Map(document.getElementById('map-canvas'), {
        zoom: 16,
        center: new google.maps.LatLng(<?php echo $coordinates; ?>),
        mapTypeId: google.maps.MapTypeId.ROADMAP,
        scrollwheel: false
      });

      new google.maps.Marker({
          position: new google.maps.LatLng(<?php echo $coordinates; ?>),
          map: map
      });
    });
  </script>

  <style>
  #map-canvas img {
    max-width: inherit;
  }
  </style>

  <div id="map-canvas" style="width:500px;height:300px;"></div>

<?php endif; ?>
```

## Gravity Form Field

### Display A Gravity Form

```php
/**
 * Display a Gravity form
 * The parameters for gravity_form() are outlined in the Gravity Forms documentation
 */
<?php if ( get_field('field_name') ) :
  $location = get_field('field_name');
  $coordinates = isset( $location['coordinates'] ) ? $location['coordinates'] : $location ; ?>
  <img src="http://maps.google.com/maps/api/staticmap?markers=<?php echo $coordinates; ?>&size=500x300&sensor=false" alt="">
<?php endif; ?>
```

## Repeater Field

### Get And Loop Over A Repeater Field

```php
/**
 * Get And Loop Over A Repeater Field
 */
<?php if ( have_rows('field_name') ) : ?>

  <?php while( have_rows('field_name') ) : the_row(); ?>

    <?php the_sub_field('sub_field_name'); ?>

  <?php endfor; ?>

<?php endif; ?>
```

### Loop Over A Repeater Filed And Seperate Results Into Rows

```php
/**
 * Loop Over A Рepeater Filed And Seperate Results Into Rows. 
 * The second tabstop is the row length.
 */
<?php if ( get_field('field_name') ) : ?>

  <div class="items">
    <?php foreach ( array_chunk(get_field('field_name'), 2) as $row): ?>

      <div class="row">
        <?php foreach ($row as $item): ?>

          <div class="item">
            <?php echo $item['field_name']; ?>$5
          </div>

        <?php endforeach; ?>
      </div>

    <?php endforeach; ?>
  </div>

<?php endif; ?>
```

## Queries

### Query A Post Type On A Field Value And Loop Over Posts

```php
/**
 * Query A Post Type On A Field Value And Loop Over Posts
 */
<?php

$args = array(
  'numberposts' => 10,
  'post_type' => 'post',
  'meta_key' => 'field_name',
  'meta_value' => 'field_value'
);

$query = new WP_Query( $args );

?>

<?php if( $query->have_posts() ) : ?>
  <ul>
  <?php while ( $query->have_posts() ) : $query->the_post(); ?>
    <li>
      <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
    </li>
  <?php endwhile; ?>
  </ul>
<?php endif; ?>

<?php wp_reset_query(); ?>
```
