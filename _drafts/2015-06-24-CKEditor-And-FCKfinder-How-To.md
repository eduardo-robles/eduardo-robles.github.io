---
layout: post
title: CKEditor and FCKFinder How To
date: '2015-06-24'
---


##CKEditor and FCKFinder How To...

###Intergrate with single sessions in PHP.

I recently had the opportunity to work with CKEditor for a project at my job. CKEditor had already been incorporated into a web project and was quite
popular with the staff who used it. Though the currenct installed
version was lacking some features that members of our staff needed.

The features they requested included: a file broswer and enhanced text editing (left, right, center, justify). Some of these features were simple to install all I needed
to do was install a plugin. Luckily CKEditor has a great "create your own editor" feature. All I did was chose the plugins
I wanted and I was able to download a custom build of CKEditor!

The place where I ran into some problems was incorporating a file browser. CKEditor has there own file broswer (CKFinder) which
looks great though you have to buy a yearly license to use. Before I could take a plunge I wanted to use something to test out
the same features that CKFinder had. Luckily this is where FCKFinder comes in to fill in the gap. Intergrating FCKFinder into CKEditor
was simple enough all I had to do was add these lines of code to the CKEditor initalizer.

`fileBroswerUploadURL: '/location/of/FCKFinder/browse.php',`
`fileImageUploadURL: '/location/of/FCKFinder/upload.php'`

`CKEditor.something(nameOfEditorInstance, {`
` 	fileBroswerUploadURL: '/location/of/FCKFinder/browse.php',`
`	fileImageUploadURL: '/location/of/FCKFinder/upload.php'`
`};)`


