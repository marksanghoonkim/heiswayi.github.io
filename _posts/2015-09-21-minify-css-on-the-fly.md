---
layout: post
title: Minify CSS On The Fly
description: Simple trick to minify CSS on-the-fly using PHP.
keywords: web optimization, css compression, css minification
---

> Did you know that if you're developing a website in PHP, you can minify your CSS code on the fly?

CSS minification or compression is one of the rules for optimizing a web page loading performance. Here's a simple trick that we can do to minify a bunch of CSS files on the fly by using a single PHP file.

Assuming I have a bunch of CSS files such as **grid.css**, **typography.css**, **button.css**, **form.css**, **table.css**, etc. in a directory called `\assets\css\`. Then I create a PHP file in the same directory and name it as `minified.css.php`.

**minified.css.php**

```php
<?php
header('Content-type: text/css');
ob_start("compress");

  function compress($buffer) {
    /* remove comments */
      $buffer = preg_replace('!/\*[^*]*\*+([^/][^*]*\*+)*/!', '', $buffer);

      /* remove tabs, spaces, newlines, etc. */
      $buffer = str_replace(array("\r\n", "\r", "\n", "\t", '  ', '    ', '    '), '', $buffer);

      return $buffer;
  }

  /* css files for compression */
  include('grid.css');
  include('typography.css');
  include('button.css');
  include('form.css');
  include('table.css');
  include('backgrounds.css');
  include('pagination.css');
  include('breadcrumbs.css');
  include('font.css');
  include('helpers.css');
  include('print.css');
  include('animation.css');
  include('responsive.css');

ob_end_flush();
?>
```

The PHP code above will include all those CSS files, minify them, and output itself as a functional CSS-compressed file.

This is how we put **minified.css.php** file in our HTML `<head>` section:

```HTML
<html lang="en">
  <head>
    <title>My Website</title>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=9">

    <link href="assets/css/minified.css.php" rel="stylesheet">

  </head>
  <body>
    <!-- website content -->
  </body>
</html>
```

Have a try and good luck!
