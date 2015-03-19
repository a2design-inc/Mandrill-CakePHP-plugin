# Mandrill CakePHP plugin

### Usage

This plugin uses [CakeEmail class](http://book.cakephp.org/2.0/en/core-utility-libraries/email.html), and works almost the same.

Basic example:

```php
App::uses('CakeEmail', 'Network/Email');
$email = new CakeEmail('mandrill');

$email->to('recipient@domain.com');
$email->subject('Test email via Mandrill');
$email->send('Message');
```

More advanced example:

```php
App::uses('CakeEmail', 'Network/Email');

$email = new CakeEmail(array(
    'transport' => 'Mandrill.Mandrill',
    'from' => 'from@example.com',
    'fromName' => 'FromName',
    'timeout' => 30,
    'api_key' => 'YOUR_API_KEY',
    'emailFormat' => 'both',
));

$viewVars = array(
    'var1' => 'some1',
    'var2' => 'some2'
);

$to = array(
    'to1@example.com',
    'to2@example.com',
);

$email->addHeaders(array(
    'tags' => array('test',),
    'subAccount' => 'YOUR_SUBACCOUNT',
    'preserve_recipients' => false,
    'global_merge_vars' => array(
        array(
            'name' => 'date',
            'content' => date('Y-m-d'),
        ),
    ),
    'merge_vars' => array(
        array(
            'rcpt' => 'to1@example.com',
            'vars' => array(
                array(
                    'name' => 'email',
                    'content' => 'to1@example.com'
                ),
                array(
                    'name' => 'name',
                    'content' => 'to1Name'
                ),
            )
        ),
        array(
            'rcpt' => 'to2@example.com',
            'vars' => array(
                array(
                    'name' => 'email',
                    'content' => 'to2@example.com'
                ),
                array(
                    'name' => 'name',
                    'content' => 'to2Name'
                ),
            )
        ),
    ),
));

$email->addAttachments(array(
    'example.pdf' => array(
        'file' => APP . '/webroot/files/readme.pdf',
        'mimetype' => 'application/pdf',
    ),
));

$email->template('test', 'default');
$email->viewVars($viewVars);
$email->to($to);
$email->subject('Mandrill CakePHP Plugin Test');

$email->send();
```

Part of email template:

```html
<p>Global vars example. Here the current date: *|date|*</p>
<p>Email sent for *|name|* (*|email|*)</p>
```

About sub accounts you can read [here](http://help.mandrill.com/forums/22476178-Subaccounts-Basics).

The syntax of all parameters is the same as the default [CakeEmail class](http://book.cakephp.org/2.0/en/core-utility-libraries/email.html).

### Installation

You can clone the plugin into your project:

```
cd path/to/app/Plugin
git clone git@github.com:a2design-company/Mandrill-CakePHP-plugin.git Mandrill
```

Also you can use composer for install this plugin. Just add new requirement to your composer.json

```
"require": {
    ...,
    "a2design-company/mandrill-cakephp-plugin": "*"
},
```

Bootstrap the plugin in app/Config/bootstrap.php:

```php
CakePlugin::load('Mandrill');
```

### Configuration

Create the file app/Config/email.php with the class EmailConfig.

```php
<?php
class EmailConfig {
	public $mandrill = array(
        'transport' => 'Mandrill.Mandrill',
        'from' => 'from@example.com',
        'fromName' => 'FromName',
        'timeout' => 30,
        'api_key' => 'YOUR_API_KEY',
        'emailFormat' => 'both',
    );
}
```


### Requirements

CakePHP 2.0+

### License

Licensed under The MIT License

Developed by [A2 Design Inc.](http://www.a2design.biz)
