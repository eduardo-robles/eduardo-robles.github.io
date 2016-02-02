---
layout: post
title: CKEditor and FCKFinder How To
date: '2015-06-24'
---


**CKEditor and FCKFinder How To...**

**Intergrate with single sessions in PHP.**

I recently had the opportunity to work with [CKEditor](http://www.ckeditor.com) for a project at my job. CKEditor had already been incorporated into a web project and was quite popular with the staff who used it. Though the current installed version was lacking some features that members of our staff needed.

The features they requested included: a file browser and enhanced text editing (left, right, center, justify). Some of these features were simple to install all I needed
to do was install a plugin. Luckily CKEditor has a great "create your own editor" feature. All I did was chose the plugins I wanted and I was able to download a custom build of CKEditor!

The place where I ran into some problems was incorporating a file browser. CKEditor has there own file broswer (CKFinder) which looks great though you have to buy a yearly license to use. Before I could take a plunge I wanted to use something to test out the same features that CKFinder had. Luckily this is where [KCFinder](http://kcfinder.sunhater.com/install) comes in to fill in the gap. Intergrating KCFinder into CKEditor was simple enough all I had to do was add these lines of PHP code to the CKEditor initalizer.

~~~ HTML

fileBroswerUploadURL: '/location/of/KCFinder/browse.php',

fileImageUploadURL: '/location/of/KCFinder/upload.php'

~~~

These two lines of code go inside your CKEditor instance like so...

~~~ HTML
CKEditor.replace(nameOfEditorInstance, {

  fileBroswerUploadURL: '/location/of/KCFinder/browse.php',

  fileImageUploadURL: '/location/of/KCFinder/upload.php'
};)

~~~


You could also skip adding these line to the CKEditor instance and configure your `config.js` file but that is completely optional. After adding these lines to your CKEditor instance you should be able click on the "image" button and browse the images on your server. It didn't quite work the first time I tried it so it was giving me a "permissions denied" error. I did some reading on the [KCFinder docs](http://kcfinder.sunhater.com/integrate#session) and found I missed a step. Now that I had called the FCKFinder in the CKEditor instance I had to make sure the session permission were enabled.

This was an important step because without these permissions enabled, users will not be able to browse files or directories on the server. FCKFinder does have a feature where you can "globally" enable any user to browse the files and directories on the server but of course this extremely dangerous and not advisable. Luckily there is another way to ensure that your trusted users can browser files and directories on the server and all you need is one line of code.

Simply add this the file in which your CKEditor instance is located...

~~~

$_SESSION['KCFINDER'] = array(
    'disabled' => false
);

~~~

This one line of PHP code is stating that for this one session allow this specific user to browse files and directories on the server. Many times you would have created a specific file which manages users login you can also go ahead and add this line to that file so you can expand the priviledges your users have on the system.

After setting everything up I was able to browser files and directories on the server. KCFinder also comes with a setting that allow you to restrict which directories and files can be viewed on the server but that is something that should taken of by you or the server administrator.

In conclusion I think this setup works for the staff at my job. I look forward to working with other technologies like CKEditor and FCKFinder.

-Eduardo
