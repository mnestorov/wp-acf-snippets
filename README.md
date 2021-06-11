# Useful ACF code snippets

This is a list of useful WordPress ACF plugin snippets.

## Get A Field By Name

```php
/**
 * Get A Field By Name
 */
 
<?php if ( get_field('field_name') ) : ?>
  <?php echo get_field('field_name'); ?>
<?php endif; ?>
```

## Get And Format A Date Field

```php
/**
 * Get And Format A Date Field
 */
 
<?php if ( get_field('field_name') ) : $date = DateTime::createFromFormat('Ymd', get_field('field_name')); ?>
  <?php echo $date->format('d-m-Y'); ?>
<?php endif; ?>
```

## Field Conditional / Also Used for True/False Fields

```php
/**
 * Field Conditional / Also Used for True/False Fields
 */
 
<?php if ( get_field('field_name') ) : ?>
  // some code
<?php endif; ?>
```

## Get A Field By Name Within Repeater/Flexible

```php
/**
 * Get A Field By Name Within Repeater/Flexible
 */
 
<?php if ( get_sub_field('field_name') ) : ?>
  <?php echo get_sub_field('field_name'); ?>
<?php endif; ?>
```

## Image Field With A Return Value Of "Image URL"

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

## File Field With A Return Value Of "File URL"

```php
/**
 * File Field With A Return Value Of "File URL"
 */
<?php if ( get_field('field_name') ) : ?>
  <a href="<?php the_field('field_name'); ?>" >Download File</a>
<?php endif; ?>
```
