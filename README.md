# php native Jade wrapper (for Laravel)

You need to have jade install using npm(if you don't,download from www.nodejs.com).
```
	sudo npm install -g jade
```
And it (should) not work on windows without the aid of cygwin or the like.


## Normal Usage:

first change the variables in jadeView.php
```php
	<?php
    $jade_bin = "/usr/bin/jade"; // if you don't know what it is type `which jade` in your terminal
    $jade_tpl_path = app_path() . '/jade/'; //enter your template path here,defaulted for laravel at app/jade/
```

php file(index.php):
```php
	<?php
	require "./jadeView.php";
	$data = [
	"hello" => "You are welcome.",
	"welcome"=>true,
	"list"=>["item1,item2,item3"],
	"escapetxt"=>"<b>bold tags</b>"
	];
	echo Jade::render("path/to/your/jadefile",$data);
```
jade file($jade_tpl_path . path/to/your/jadefile.jade):
```jade
	if welcome
		ul
			for item in list
				li item
		h1 #{hello}
		p= escapetxt
		p!= escapetxt
	else
		h1 PLEASE LEAVE
```

output on browser:
```html
	<ul>
		<li>item 1</li>
		<li>item 2</li>
		<li>item 3</li>
	</ul>
	<h1>You are welcome.</h1>
	<p>&lt;p&gt;bold tags&lt;/p&gt</p>
	<p><b>bold tags</b></p>
```
in case of syntax error on jade,the function will render the error message.

## extends layout
```
	extends in jade are set to relative to $jade_tpl_path instead of the $jadefile's path
	in most cases,this make more sense
```


## Extra steps for Laravel:

```php
	<?php
	// edit file that will be preloaded,such as app/start/global.php,or simplely put jadeView.php to 	model,that shoulda work too

	// prepend this line.
	require app_path()."./jadeView.php";
```

Then in your controller
```	php
	<?php
	//...
	public function index(){
		$data = ["hello" => "You are welcome.","welcome"=>true];
		return Jade::render("path/to/your/jadefile",$data);
	}
```

## Roadmap
- add composer package support
- get controller class from laravel to make some MVC sense