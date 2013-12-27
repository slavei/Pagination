SugiPHP\Pagination
==================

SugiPHP Pagination is a simple to use class that provides pagination links for your app. You can customize the look and feel of the
pages links by writing a custom renderer or use one of the available ones like Twitter's Bootstrap (TODO) or extend them.

Basic usage
-----------

Pagination class by it's own does not have any renders, instead it's primary goal is to give an array of items (links). This can be done easily:

```
<?php

use SugiPHP\Pagination\Pagination;

$pagination = new Pagination();
$pagination->setItems(45); // Set the total number of records (items)

?>
```

If the web page's URL is http://example.com/index.php?page=3 Pagination will guess that the current page is 3 and will return something like this:

```
<?php

var_dump($pagination->toArray());

// some parameters are removed for better readability
[
	'prev' => [
		'page' => 2,
		'uri' => '/index.php?page=1',
		'isDisabled' => false
	],
	1 => [
		'page' => 1,
		'uri' => '/index.php?page=1',
		'isCurrent' => false,
	],
	2 => [
		'page' => 2,
		'uri' => '/index.php?page=2',
		'isCurrent' => false,
	],
	3 => [
		'page' => 3,
		'uri' => '/index.php?page=3',
		'isCurrent' => true,
		'isDisabled' => true,
	],
	'next' => [
		'page' => 2,
		'uri' => '#',
		'isDisabled' => true
	]
]
?>
```

Getters
-------

```
<?php

// Returns current page number if it is set, or will try to guess it from the URL address. Default 1
$pagination->getPage();

// Returns the maximum number of items viewed in a single page. Default 20
$pagination->getItemsPerPage();

// Returns URI pattern that is used to generate links and to guess current page. Default is 'page={page}'
$pagination->getPattern();

// Returns total number of pages based on total items and items per page settings.
$pagination->getTotalPages();

// Returns first item's index in a page we are on. Used in SQLs (e.g. LIMIT 20 OFFSET 60)
$pagination->getOffset();

?>
```

Setters
-------
```
<?php

// Total number of items. This one MUST be set.
$pagination->setItems(int);

// Sets the number of items (lines) in a single page.
$pagination->setItemsPerPage(int);

// Sets the page number manually.
$pagination->setPage(int);

/*
 * Sets the URI pattern for creating links for pages.
 * Default pattern is "page={page}"  (URLs like /posts/show?page=5)
 * Can be set for example to "p={page}" or anything else for $_GET parameter
 * Can be set also to "page/{page}" for friendly URLs which will result with URLs like: /posts/show/page/5
 */
$pagination->setPattern();

/*
 * Sets the current URI. Default is $_SERVER["REQUEST_URI"]
 * Handy for unit tests.
 */
$pagination->setUri($uri)

?>
```

Basic renderer
--------------
```
<?php

$pages = $pagination->toArray();

// Twitter Bootstrap 3 pagination
$items = "";
foreach ($pages as $key => $page) {
	// link
	$href = $page["uri"];

	// label
	if ($key === "prev") {
		$label = "&laquo;";
	} elseif ($key === "next") {
		$label = "&raquo;";
	} elseif ($key === "less" or $key === "more") {
		$label = "...";
	} else {
		$label = $page["page"];
	}

	// class
	if ($page["isCurrent"]) {
		$class = "active";
	} elseif ($page["isDisabled"]) {
		$class = "disabled";
	} else {
		$class = "";
	}
	$items .= '<li class="'.$class.'"><a href="'.$href.'">'.$label.'</a></li>';
}

echo '<ul class="pagination">' . $items . '</ul>';

?>
```

TODO
----
 - Set option for maximum number of links to display (currently 11 + previous and next buttons)
 - Some basic renders

