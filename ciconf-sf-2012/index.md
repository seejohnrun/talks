!SLIDE other first
# Hello!

!SLIDE other incremental
# Who am I?
* John Crepezzi
* [@seejohnrun](http://twitter.com/seejohnrun)
* [seejohncode.com](http://seejohncode.com)

!SLIDE other incremental
# Topics
* Database
* PHP
* JavaScript
* CSS

* _Breaks for Kittens as Needed_

!SLIDE center other
![Kitten](kitten1.jpeg)

!SLIDE database first
# Database

!SLIDE database
# Constantly consider denormalization
## Object-Relational Impedence Mismatch
## Think - what are you asking

	SELECT sum(value)
	FROM table

!SLIDE database
# Avoid single-table-inheritance

	@@@ sql
	SELECT id, name, type
	FROM table

## Prefer a strategy

	@@@ php
	if ($result->type === "Lamborghini") {
		$result->drive();
	}

!SLIDE database
# Don't
## Try to abstract away the domain

	@@@ sql
	SELECT f1, f2, f3
	FROM t1
	WHERE t1.f2 = "Person"
	AND t1.f3 = "John"

!SLIDE database
# Bulk INSERT

	@@@ sql
	INSERT INTO table
	  (f1, f2) VALUES (v1, v2);

	INSERT INTO table
	  (f1, f2) VALUES (v3, v4);

!SLIDE database
# Bulk INSERT

	@@@ sql
	INSERT INTO table
	  (f1, f2) VALUES (v1, v2), (v3, v4);

!SLIDE database
# Bulk REALLY?

	            1 column    5 columns
	100 rows    90 / 50     95 / 55
	1000 rows   450 / 60    460 / 80
	10000 rows  4120 / 210  4204 / 380

!SLIDE database
# SELECT that shizzy

	@@@ sql
	SELECT *
	FROM table_with_100_columns
	WHERE <condition>

!SLIDE database
# Indexes, indexes, indexes

	@@@ sql
	SELECT *
	FROM table
	WHERE nic = 24;

	SELECT *
	FROM table
	ORDER BY nic DESC
	LIMIT 20

!SLIDE database
# Examine the slow query log

	log-slow-queries = slow.log
	long_query_time = 20

!SLIDE database
# Examine the slow query log

	log-slow-queries = slow.log
	long_query_time = 20
	log-queries-not-using-indexes

!SLIDE database
# Use SQL grouping operators

	@@@ sql
	SELECT *
	FROM table
	WHERE field = value;

	SELECT count(name)
	FROM table;

	SELECT count(*)
	FROM table

!SLIDE database
# Checking existince

	@@@ sql
	SELECT count(*)
	FROM table
	WHERE field = value;

	SELECT 1
	FROM table
	WHERE field = value

!SLIDE database
# Use ON DUPLICATE KEY UPDATE

	@@@ sql
	INSERT INTO table
	(name, age) VALUES ("john", 26)
	ON DUPLICATE KEY UPDATE age = 26;

!SLIDE database
# Remove any superfluous indices
## Increase write performance

* Index on parent, name
* Index on parent

!SLIDE database
# Pagination is a total bitch
## consider using FOUND_ROWS

	@@@ sql
	SELECT SQL_CALC_FOUND_ROWS * FROM users WHERE condition;
	SELECT FOUND_ROWS();

!SLIDE database
# If possible - always use NOT NULL

!SLIDE database incremental
# ENUMs are AWESOME
## Especially for state machines

* Especially good for state machines
* Quicker to use than any int when you know the states beforehand
* Changing the ENUM is non-trivial at large data volume

!SLIDE database
# Referential integrity slows things down
## But its worth it, especially with triggers

!SLIDE database important
# Most important
## Databases are data structures.
## Relational databases are a jack of all trades

* Many representations
* Transactions
* Referential integrity

!SLIDE center other
![Kitten](kitten2.jpeg)

!SLIDE php
# PHP

!SLIDE php important
# Upgrade PHP

!SLIDE php
# No SQL queries within a loop

	@@@ php
	for ($posts as $post) {
		$post->getComments();
	}

!SLIDE php
# Favor built-in functions to writing your own

	@@@ php
	array_flip();
	array_chunk($arr, $size);
	soundex($str);
	str_getcsv($str, $delim, $enc, $escape);
	str_word_count($really);

!SLIDE php
# Don't try to be smart with pass-by-reference

	@@@ php
	function somethingThatReads(&$str) {
		substr($str, 0, 10);
	}

!SLIDE php
# use require, include over require_once
## And use full paths!

!SLIDE php
# Avoid @ error suppression

	@@@ php
	@potenially_terrifying_operation();

!SLIDE php
# functions in the conditional of a for-loop

	@@@ php
	// noooooo
	for ($i = 0; $i < pricyCount($obj); $i++) { }

	// yessssss
	$count = pricyCount($obj);
	for ($i = 0; $i < $count; $i++) { }

!SLIDE php
# ' instead of " - more as a readability concern

	@@@ php
	$bad1 = "go ahead and $ look for some";
	$bad2 = 'no real $variables in here!';

!SLIDE php important
# Don't fall into the Active Record death trap

!SLIDE center other
![Kitten](kitten3.jpeg)

!SLIDE javascript first
# JavaScript

!SLIDE javascript
# Don't use String#concat

	@@@ javascript
	// no!
	var name = 'hello, '.concat(name);

	// yes!
	var name = 'hello, ' + name;

!SLIDE javascript
# Sparing use of globals - even predefined ones

	@@@ javascript
	for (var i = 0; i < 1000; i++) {
		window.location.anchor = 'i#' + i;
	}

!SLIDE javascript
# Use var a lot

	@@@ javascript
	Something.prototype.function () {
		name = 'bobwehadababyitsaboy';
	};

!SLIDE javascript
# Avoid repeated expressions inside of loops

	@@@ javascript
	for (var i = 0; i < 20; i++) {
	  $('#content').find('a').html('hey ' + i);
	}

	// still counts
	setInterval(function () {
	  $('#content').find('a').html('Hey!');
	}, 20);

!SLIDE javascript
# Avoid repeated expressions inside of loops

	@@@ javascript
	$content_a = $('#content').find('a');

	for (var i = 0; i < 20; i++) {
	  $content_a.html('hey ' + i);
	}

	setInterval(function () {
	  $content_a.html('Hey!');
	}, 20);

!SLIDE javascript
# Be aware of rendering that happens as a result of calls

	@@@ javascript
	var $temp = $('<div>');
	for (var i = 0; i < 20; i++) {
		$li = $('<li>').html('element' + i);
		$temp.append($li);
	}

!SLIDE javascript
# Creating 50 of the same element

	@@@ javascript
	var elements = Array.new(50);
	for (var i = 0; i < 50; i++) {
		elements[i] = $('<div>');
	}
	elements.html();

!SLIDE javascript
# Creating 50 of the same element

	@@@ javascript
	Array.new(50).join('<div></div>');

!SLIDE javascript
# Write CSS in bulk

	@@@ javascript
	$elem.css('color', '#fff');
	$elem.css('background', '#000');
	$elem.css('text-decoration', 'none');

	$elem.css({
		color: '#fff',
		background: '#000',
		'text-decoration': 'none
	});

!SLIDE javascript
# Load scripts async

	@@@ javascript
	var load = function (url, callback) {
	  var script = document.
	    createElement('script')
	  script.type = 'text/javascript';
	  script.src = url;
	  script.onload = callback; // no IE
	  document.
	    getElementsByTagName('head')[0].
	    appendChild(script);
	};

!SLIDE javascript
# Garbage collection hinting

	@@@ javascript
	this.obj = loadObject();

	for (var i = 0; i < 20; i++) {
		this.obj[i];
	}

	this.otherFunc();

	this.obj = null;

!SLIDE javascript
# Don't emulate inheritance
## And know what your CoffeeScript is doing

!SLIDE javascript
# Compress & GZip

* http://yuilibrary.com/projects/yuicompressor/
* https://github.com/mishoo/UglifyJS/

!SLIDE javascript important
# Faster renderers
## Are no excuse to be lazy :-P

!SLIDE center other
![Kitten](kitten4.jpeg)


!SLIDE css first
# CSS

!SLIDE css
# right-to-left evaluation

!SLIDE css
# right-to-left evaluation

	@@@ css
	/* pretty bad */
	* {
		font-family: serif;
	}

!SLIDE css
# right-to-left evaluation

	@@@ css
	/* pretty bad */
	* {
		font-family: serif;
	}

	/* :( */
	#content [title="home"] {
		font-family: serif;
	}


!SLIDE css
# Don't qualify IDs

!SLIDE
# Don't qualify IDs

	@@@ css
	a#header {
		display: block;
		background: url(heyman.png);
	}

!SLIDE css
# Understand SASS

!SLIDE css
# Understand SASS

	@@@ css
	.sidebar {
		#header {
			a {
				font-family: serif;
			}
		}
	}

!SLIDE css
# Understand SASS

	@@@ css
	.sidebar {
		#header {
			a {
				font-family: serif;
			}
		}
	}

	/* after */
	.sidebar #header a {
		font-family: serif;
	}

!SLIDE css
# CSS Spriting to reduce number of image requests

	@css
	#nav a.item1 {
		background-image: url('img');
		background-position: 0px 0px;
	}

	#nav a.item2 {
		background-image: url('img');
		background-position: 0px -100px;
	}

!SLIDE css
# CSS Spriting to reduce number of image requests

	@css
	#nav a { background-image: url('img'); }

	#nav a.item2 { background-position: 0px 0px; }
	#nav a.item2 { background-position: 0px -100px; }

!SLIDE css
# Reduce CSS assets - number of files should line up with how they're used
## Use an asset manager

!SLIDE css
# Use CSS shorthands

	@@@ css
	.thing {
		margin: 10px;
		margin: 10px 5px;
		margin: 10px 5px 3px 1px;
	}

!SLIDE css
# Use CSS shorthands

	@@@ css
	.thing {
		font: small 24px/1.2em "Helvetica", sans-serif;
	}

!SLIDE css important
# Selectors are right-to-left!

!SLIDE css important
# Compress & GZip
* http://yuilibrary.com/projects/yuicompressor/

!SLIDE other
# 

!SLIDE other
# Thank You!
## https://github.com/seejohnrun/talks
