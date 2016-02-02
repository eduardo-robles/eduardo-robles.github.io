---
layout: post
title: Working With PHP Functions
date: '2015-08-08'
---

**Working with PHP functions**

Recently, I ran across a problem which required a bit more planning and thought. The problem was as follows, there's a website with several pages and on each page I wanted to display a unique "hero" image
at the top. This way every page you visited had it's own hero image and was unique. Now as of yet this doesn't sound all that complicated, it's actually incredibily easy to solve. But things got a bit
trickier when you to the mix the fact that on this website I was using a custom PHP templating system.

Basically I built a template for several components of the website, for example the _header_
was a template. This makes changing a link or image easy because you change it one area and the changes are made across the website where the template is used.

So as you can guess my "hero" image lived inside the header template, so changing it there would prove to futile because it would change everywhere else it appeared. I wasn't going to create a new header template
for every page that needed a unique hero image either. My only solution was to come up with a method to

1. Figure out on which page I was on.
2. Display the appropriate image for that page.

I know there are many methods do this in but since I wanted to practice my PHP knowledge I decided to write a PHP function to do the job.

**_First_:**

I needed to find a way to determine on which page I was in. That was easy, because, in my template system I created a variable that would change the page title. This is how the variable works

_In my template script_
`<?php $page['title'] = 'Homepage' ?>`

*This is example, ideally you would make this match the main title of your website.

_And when I want to call it_
`<title><?php echo $page['title']; ?></title>`

All I needed to do was pass this variable into a function which would then do something with it.

**_Second_:**

 I needed to write some kind of control structure to determine which image to use based on the page. It would look like this in pseudo code...

~~~

If variable $page is set to "homepage" then do this

  print the image to page

if variable $page is set to this "page 1"

  then do this instead
  print the image to page

Else if variable $page is not set to anything then

  print this image to page

~~~

Ok, so I decided to use a switch statement instead of `if/elseif/else` statements because I felt it gave me more control. So my code ended up looking this...

~~~

function imageSwitch($pageImg){

switch ($pageImg) {
  case 'homepage':
    echo "<img src="img/homepage.jpg" />";
    break;
  case 'page 1':
    echo "<img src="img/page1.jpg" />";
    break;
 case 'page 2':
    echo "<img src="img/page2.jpg" />";
    break;
  default:
    echo "<img src="img/default.jpg" />";
    break;
}//end switch

}//end function imageSwitch

~~~


As you can see the `switch` statement based on what variable `$pageImg` is prints out the specific image that I want. Now, I decided to
name the variable `$page['title']` to `$pageImg` just to keep things separate and clean.

**_Third_:**

Now, that I have my script I need to call it. To do so I simply did the following...

~~~

<div id="header">

  <div id="wrap">
    <?php include='imageSwitcher.php'; echo imageSwitch($page['title']);?>
  </div>

</div><!--end header-->

~~~

The `<div>` statements are to give clarity but the main idea is in the PHP snippet. I call my function I have to include the
PHP file in which it resides. Now there several ways to "include" a PHP script/file you can either use a _require_, _require once_, or _include_.
Which one you use depends on what your are trying to accomplish so study up on those function if you are unsure on which to use.
After I include my PHP script I can now call my function with `echo` followed by the name of the function. As you can see I passed
a variable into my function, that variable being the `$page['title']`. This is what I allows me to use the `$page['title']` variable in
my function. Which I simply set to the `$pageImg` variable in my PHP script like so `$pageImg =  $page['title']`.

**_Conclusion_**

To wrap up, the script works just fine and I was able to accomplish the 2 task I needed to complete. The first one to determine on
which page I was on and the second to display the correct image based on what page I was on. In future I plan making some slight
improvement to the script so I won't have to print out raw HTML but for now this is quick solution for the problem I wanted
to solve.
